## CRD

A Kubernetes Custom Resource Definition (CRD) is a powerful mechanism for extending the Kubernetes API.

It allows users to create and manage custom resources that are not part of the standard Kubernetes API.

With CRDs, Kubernetes is no longer limited to built-in resources (such as Pods and Services) but can also support user-defined resource types.

## Operator

An **Operator** is a **custom controller** that watches **Custom Resources (CRs)** defined by a **CustomResourceDefinition (CRD)**.

**Operator** = CRD + Controller.

## Process for creating a CRD

### 1. Write a CRD YAML

A CRD tells the API server: *“Here’s a new kind of object you should accept.”*

```yaml
apiVersion: apiextensions.k8s.io/v1
kind: CustomResourceDefinition
metadata:
  name: crontabs.stable.example.com   # Must be <plural>.<group>
spec:
  group: stable.example.com           # API group
  names:
    plural: crontabs                  # CR plural name
    singular: crontab                 # CR singular name
    kind: CronTab                     # Kind (like Pod, Service, etc.)
    shortNames:
      - ct
  scope: Namespaced                   # or Cluster
  versions:
    - name: v1
      served: true
      storage: true
      schema:                         # OpenAPI schema validation
        openAPIV3Schema:
          type: object
          properties:
            spec:
              type: object
              properties:
                cronSpec:
                  type: string
                image:
                  type: string
                replicas:
                  type: integer
```

## 2. Apply the CRD

```bash
kubectl apply -f crontab-crd.yaml
```

Now Kubernetes knows about a new type:

```
CronTab (stable.example.com/v1)
```

Check:

```bash
kubectl get crd
```

## 3. Add a Controller

- The CRD defines **data**.
- A **Controller** (usually you write one with Kubebuilder or Operator SDK) makes that data *do something* (e.g., create Pods, scale Deployments, etc.).
- Controller runtime: High-level
- Client-go: Low-level

### 4.1 Create the project & API

```bash
# Install kubebuilder if you haven't
# https://book.kubebuilder.io/quick-start.html

mkdir crontab-operator && cd crontab-operator
kubebuilder init --domain example.com --owner "Demo"
kubebuilder create api --group stable --version v1 --kind CronTab --resource --controller
```

### 4.2 Define your API (spec & status)

Edit `api/v1/crontab_types.go`:

```go
package v1

import (
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
)

// CronTabSpec defines the desired state of CronTab
type CronTabSpec struct {
    // Schedule in standard cron format, e.g. "*/5 * * * *"
    // +kubebuilder:validation:Pattern=`^(\*|([0-5]?\d)) (\*|([01]?\d|2[0-3])) (\*|([01]?\d|2[0-9]|3[01])) (\*|(1[0-2]|0?[1-9])) (\*|([0-6]))$`
    CronSpec string `json:"cronSpec"`

    // Container image to run
    // +kubebuilder:validation:MinLength=1
    Image string `json:"image"`

    // Optional command to exec (if empty, image entrypoint is used)
    // +optional
    Command []string `json:"command,omitempty"`
}

// CronTabStatus defines the observed state of CronTab
type CronTabStatus struct {
    // LastTime we successfully reconciled the child CronJob
    // +optional
    LastScheduleTime *metav1.Time `json:"lastScheduleTime,omitempty"`
}

//+kubebuilder:object:root=true
//+kubebuilder:subresource:status
type CronTab struct {
    metav1.TypeMeta   `json:",inline"`
    metav1.ObjectMeta `json:"metadata,omitempty"`

    Spec   CronTabSpec   `json:"spec,omitempty"`
    Status CronTabStatus `json:"status,omitempty"`
}

//+kubebuilder:object:root=true
type CronTabList struct {
    metav1.TypeMeta `json:",inline"`
    metav1.ListMeta `json:"metadata,omitempty"`
    Items           []CronTab `json:"items"`
}
```

Then regenerate code:

```bash
make generate
make manifests
```

### 4.3 Implement the controller (reconcile logic)

**Package: Controller Runtime**

Edit `controllers/crontab_controller.go`:

```go
package controllers

import (
    "context"
    "time"

    stablev1 "crontab-operator/api/v1"
    "github.com/go-logr/logr"
    batchv1 "k8s.io/api/batch/v1"
    corev1 "k8s.io/api/core/v1"
    kerrors "k8s.io/apimachinery/pkg/api/errors"
    metav1 "k8s.io/apimachinery/pkg/apis/meta/v1"
    "k8s.io/apimachinery/pkg/types"
    ctrl "sigs.k8s.io/controller-runtime"
    "sigs.k8s.io/controller-runtime/pkg/controller/controllerutil"
    "sigs.k8s.io/controller-runtime/pkg/log"
)

// RBAC for our custom resource and the child CronJob
// +kubebuilder:rbac:groups=stable.example.com,resources=crontabs,verbs=get;list;watch;create;update;patch;delete
// +kubebuilder:rbac:groups=stable.example.com,resources=crontabs/status,verbs=get;update;patch
// +kubebuilder:rbac:groups=stable.example.com,resources=crontabs/finalizers,verbs=update
// +kubebuilder:rbac:groups=batch,resources=cronjobs,verbs=get;list;watch;create;update;patch;delete
// +kubebuilder:rbac:groups="",resources=events,verbs=create;patch

type CronTabReconciler struct {
    ctrl.Client
}

// Reconcile ensures that for each CronTab, a matching CronJob exists and is kept up-to-date.
func (r *CronTabReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, error) {
    logger := log.FromContext(ctx)

    // 1) Fetch the CronTab instance
    var ct stablev1.CronTab
    if err := r.Get(ctx, req.NamespacedName, &ct); err != nil {
        if kerrors.IsNotFound(err) {
            // Object deleted. Nothing to do.
            return ctrl.Result{}, nil
        }
        return ctrl.Result{}, err
    }

    // 2) Build desired CronJob name (stable & unique)
    cronJobName := ct.Name + "-cronjob"

    // 3) Get or create/patch the child CronJob
    var cj batchv1.CronJob
    err := r.Get(ctx, types.NamespacedName{Name: cronJobName, Namespace: ct.Namespace}, &cj)
    if err != nil && kerrors.IsNotFound(err) {
        // Not found — create
        cj = batchv1.CronJob{
            ObjectMeta: metav1.ObjectMeta{
                Name:      cronJobName,
                Namespace: ct.Namespace,
            },
        }
    }

    // Use CreateOrUpdate to reconcile spec & ownerRef idempotently
    op, err := controllerutil.CreateOrUpdate(ctx, r.Client, &cj, func() error {
        // Set owner reference so CronJob is GC’d when CronTab is deleted
        if err := controllerutil.SetControllerReference(&ct, &cj, r.Scheme()); err != nil {
            return err
        }

        // Desired CronJob spec derived from CronTab spec
        cj.Spec.Schedule = ct.Spec.CronSpec
        // Reasonable defaults for demo
        truePtr := true
        cj.Spec.Suspend = nil                 // ensure not suspended
        cj.Spec.SuccessfulJobsHistoryLimit = int32Ptr(3)
        cj.Spec.FailedJobsHistoryLimit = int32Ptr(1)
        // Optionally, start deadline
        cj.Spec.StartingDeadlineSeconds = nil
        cj.Spec.ConcurrencyPolicy = batchv1.ForbidConcurrent

        // Job template (the actual work)
        cj.Spec.JobTemplate.Spec.Template = corev1.PodTemplateSpec{
            ObjectMeta: metav1.ObjectMeta{
                Labels: map[string]string{
                    "app": "crontab-worker",
                    "ct":  ct.Name,
                },
            },
            Spec: corev1.PodSpec{
                RestartPolicy: corev1.RestartPolicyNever,
                Containers: []corev1.Container{
                    {
                        Name:    "worker",
                        Image:   ct.Spec.Image,
                        Command: ct.Spec.Command, // optional
                    },
                },
            },
        }

        return nil
    })
    if err != nil {
        logger.Error(err, "failed to CreateOrUpdate CronJob")
        return ctrl.Result{}, err
    }

    logger.Info("reconciled CronJob", "op", op, "name", cj.Name)

    // 4) Update status (optional but nice)
    now := metav1.NewTime(time.Now())
    ct.Status.LastScheduleTime = &now
    if err := r.Status().Update(ctx, &ct); err != nil {
        // Non-fatal; requeue if you want stronger guarantees
        logger.Error(err, "failed to update CronTab status")
    }

    // 5) Done — no requeue needed unless you want periodic resync
    return ctrl.Result{}, nil
}

func (r *CronTabReconciler) SetupWithManager(mgr ctrl.Manager) error {
    return ctrl.NewControllerManagedBy(mgr).
        For(&stablev1.CronTab{}).
        Owns(&batchv1.CronJob{}).
        Complete(r)
}

func int32Ptr(i int32) *int32 { return &i }
```

**What this does**

- Watches `CronTab` objects.
- Ensures there’s a **child `CronJob`** with the same schedule/image/command.
- Sets an **OwnerReference** so deleting a `CronTab` cleans up its `CronJob`.
- Updates a small **status** field.

### 4.4 Install CRD & run the controller

```bash
# Generate/refresh CRDs and RBAC
make generate
make manifests

# Install CRD into your cluster (kind/minikube/etc.)
make install

# Run the controller locally (uses your kubeconfig)
make run
```

## 4. Create a Custom Resource (CR)

Once the CRD is installed, you can create instances of your new type:

```yaml
apiVersion: stable.example.com/v1
kind: CronTab
metadata:
  name: hello-every-minute
  namespace: default
spec:
  cronSpec: "*/1 * * * *"
  image: busybox
  command: ["sh", "-c", "date; echo Hello from CronTab"]
```

Apply it:

```bash
kubectl apply -f my-crontab.yaml
```

Check it:

```bash
kubectl get crontabs
```
