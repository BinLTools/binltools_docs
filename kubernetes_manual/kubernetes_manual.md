# Components

- Native Components
    
    ![截屏2024-09-29 17.36.39.png](/kubernetes_manual/images/截屏2024-09-29%2017.36.39.png)
    

## API Server (Master)

### Function

1.  The API Server is the only component that communicates with etcd; any component that needs to interact with etcd must do so through the API Server
2.  It provides a RESTful API-based CRUD interface for querying and modifying cluster state, which is stored in etcd

### Process

1.  Authentication
    - Authentication is performed by inspecting HTTP requests
    - The authentication plugin retrieves: username, user ID, and the user’s group
2.  Authorization
    - The authorization plugin verifies whether the user has permission to perform the requested action on the resource
    - e.g., RBAC
3.  Admission
    - When a request is a Create/Update/Delete operation, it enters the Admission Control plugins
    - These plugins can fill in missing spec values with defaults or override the spec; they can modify the relevant resource
    - Examples of Admission Control plugins
        - AlwaysPullImages: Overrides the `imagePullPolicy` in the pod to `Always`
        - ServiceAccount: Applies the default ServiceAccount to pods that do not declare a ServiceAccount
        - NamespaceLifecycle: Prevents pods from being created in a namespace that is being deleted or does not exist
        - ResourceQuota: Ensures the amount of CPU and memory used by a pod
4.  Validation
    - The API Server validates the object, stores it in etcd, and returns a response to the client

### Features

1.  The API Server can have multiple instances, enabling parallel processing
2.  The API Server can be deployed directly on the system or run as a Pod

### Connect to the API Server via [localhost](http://localhost)

```go
// 获取 API Server 的 IP Addr，此时无法直接访问（无权限）
kubectl cluster-info 

// 开启代理，proxy 拥有权限
kubectl proxy

// 访问地址，可以连接了
curl 127.0.0.1:8001
```

## etcd (Master)

### Features

1.  etcd is a fast, distributed, and reliable key-value storage component
2.  Storage Method
    - v2
        - A hierarchical keyspace that makes key-value pairs resemble a file system.
        - Each key acts as a directory, containing other keys or values
            
            ```yaml
            $ etcdctl ls /registry
            /registry/configmaps
            /registry/daemonsets
            /registry/deployments
            /registry/events
            /registry/namespaces
            /registry/pods
            ...
            ```
            
        - Uses a round-robin mode
            - HTTP/1.x + JSON
            - The client uses a persistent HTTP/1.1 connection to periodically poll (watch) the server for the latest data change events (e.g., a client such as a scheduler)
            - Disadvantages: If there are too many Watchers, it consumes a significant amount of memory, sockets, and other resources
    - v3
        - Using the push model
            - Uses Protobuf (similar to JSON; structured data with small footprint, but binary and less human-readable)
            - Communicates with the server using gRPC over HTTP/2
            - Supports bidirectional data flow; the server can push data
            - Supports multiplexing to reduce load

### Features

1.  etcd can have multiple instances, enabling parallel operation
2.  etcd can be deployed directly on the system or run as a Pod
3.  Only the API Server communicates with etcd
    1.  Robust optimistic locking system
    2.  Validation
4.  Typically, the number of etcd instances is odd
    - Clusters with an even number of nodes carry a higher risk of unavailability, as there is a higher probability of a tie during the leader election process, thereby triggering the next round of elections
        - A member that times out without receiving heartbeats from the current leader initiates an election
        - Leader: Processes all operations **requiring consensus** (writes, linearizable reads), replicates the Raft log to followers
    - Clusters with an odd number of nodes and those with an even number (odd number + 1) can tolerate the same number of node failures; clusters with an even number of nodes waste resources
    - Majority: The number of nodes that accept the vote, exceeding half of the total

## Scheduler (Master)

### Function

- The Scheduler monitors pods that are not bound to a node, then updates the pod’s definition to bind the pod to a corresponding node (here, the Scheduler does not cause the node to run the pod; this requires the kubelet)
- How the Scheduler Selects Nodes
    
    ![截屏2024-10-08 11.39.29.png](/kubernetes_manual/images/截屏2024-10-08%2011.39.29.png)
    
    - When selecting the optimal node, if there is a tie, Round-Robin is used
    - Criteria for selecting a node
        - Hardware resources
        - Label matches the node selector
        - Binding to a specified host port
        - Volumes of a specified type
        - Tolerate taints
        - Node and/or pod affinity or anti-affinity rules

### Features

1.  Only one instance of the Scheduler can run at a time
2.  The Scheduler can be deployed directly to the system or run as a Pod

## Controller Manager (Master)

### Functionality

1.  Controllers monitor resource changes provided by the API Server and perform operations for each change
2.  Controllers also periodically perform a re-list operation to ensure that no changes are missed
3.  Controllers run a reconciliation loop to align the actual state with the desired state

### Characteristics

1.  Controllers have no communication with one another; they are unaware of each other’s existence or that of the kubelet
2.  Controllers only update resources on the API Server
3.  Only one instance of the Controller Manager can run at a time
4.  The Controller Manager can be deployed directly to the system or run as a Pod

### Controllers Cooperate

![截屏2024-10-11 13.43.51.png](/kubernetes_manual/images/截屏2024-10-11%2013.43.51.png)

### Controller Examples

- Replication Manager
    
    ![截屏2024-10-08 17.39.58.png](/kubernetes_manual/images/截屏2024-10-08%2017.39.58.png)
    
- ReplicaSet, DaemonSet, and Job controllers
    - Similar to Replication Manager
- Deployment controller
    - Adds Rollout functionality to Replication Manager
- StatefulSet controller
    - Similar to ReplicaSet
- Node controller
    - Manages worker nodes
    - Ensures that the list of nodes matches the nodes actually running on the physical machines
    - Monitors node health and evicts pods from unreachable nodes
- Service controller
    - When creating or deleting a LoadBalancer Service, the Service controller is responsible for requesting and releasing load balancers from the infrastructure.
- Endpoint Controller
    - A Service does not connect directly to pods; instead, it maintains a list of endpoints (IPs and ports).
    - The Endpoint Controller is responsible for ensuring that the endpoint list is up to date (pod IPs and ports match the label selector).
    - The Endpoint Controller monitors both Service resources and Pod resources and updates the Endpoint resources accordingly.
        
    ![截屏2024-10-08 17.39.58.png](/kubernetes_manual/images/截屏2024-10-08%2017.39.58.png)
        
- Namespace Controller
    - When a namespace resource is deleted, the Namespace Controller ensures that all resources within that namespace are deleted
- PersistentVolume Controller
    - The PersistentVolume Controller helps created PVCs find suitable PVs
    - When a PVC is deleted, it also unmounts the PV and takes action based on the reclaim policy
- etc.

## Kubelet (Worker Node)

### Function

1.  Creates a Node resource in the API Server
2.  Listens to the API Server to ensure pods have been scheduled to this node, then starts the pod’s containers (by notifying the configured container runtime (Docker…))
3.  Monitors running containers and reports their status, events, and resource consumption to the API Server
4.  Runs container liveness probes and restarts containers when probes fail
5.  Terminate the container when the pod is terminated
6.  You can create pods directly using a local pod manifest without going through the API Server

### Features

1.  Kubelet can only be deployed directly on the system
2.  Kubelet is responsible for everything on the worker node

## kube-proxy (Worker Node)

 Full name: Kubernetes Service Proxy

### Functionality

1.  Kube-proxy ensures that connections made to a Service’s IP and port ultimately reach one of the pods supporting that Service. When a service is supported by multiple pods, the proxy performs load balancing across these pods. Kube-proxy maintains network rules on the node to load balance traffic destined for the Service (via ClusterIP and port) to the correct backend pods.
2.  iptables proxy mode: Uses iptables rules to redirect packets to random pods, rather than routing them through an actual proxy server
    - Instead of kube-proxy itself listening on a port and forwarding packets, it installs NAT rules so the **kernel** directly rewrites and forwards packets.
    
    ![截屏2024-10-09 09.59.51.png](/kubernetes_manual/images/截屏2024-10-09%2009.59.51.png)
    
3.  userspace mode: Legacy version. Waits for connections and opens a new connection for each one

### Features

1.  kube-proxy can be deployed directly on the system or run as a Pod
2.  Acts as an actual proxy

## Container Runtime (Worker Node)

- Pulls the container image from a registry.
- Creates the container filesystem from the image.
- Sets up cgroups (CPU/memory limits) and namespaces (isolation).
- Starts the container process.

## CSI (Add-on)

 View [Kubernetes CSI](https://www.notion.so/Kubernetes-CSI-7ab6ca25a6ab416cae8482c5b7d280cb?pvs=21) 

## CNI (Add-on)

## DNS server (Add-on)

1.  All pods use the cluster's internal DNS server by default, which allows pods to look up the IP address of a Service by name
2.  The IP addresses of Services are recorded in the /etc/resolv.conf file of each container
3.  The kube-dns pod uses the API Server’s watch mechanism to monitor updates to Services and Endpoints, then updates the DNS records

## Dashboard (Add-on)

## Ingress Controller (Add-on)

1.  Provides a reverse proxy server
2.  Ensures the server is always up to date (requires monitoring updates to Ingress, Services, and Endpoints)
3.  The Ingress Controller is responsible for resolving Ingress reverse proxy rules. After the Ingress Controller receives a request, it forwards the request to the corresponding Service’s Pod based on the Ingress rules

## Heapster (Add-on)

## Provisioner (Add-on)

 Used to create PVs and actual Volumes (Disks)

 Determines which volume to use to provision the PV

![image.png](/kubernetes_manual/images/image.png)

### ProvisionController

- Provides **dynamic provisioning**
    
    ![截屏2024-09-24 14.08.11.png](/kubernetes_manual/images/截屏2024-09-24%2014.08.11.png)
    
    - Static provisioning: Requires manually creating a storage volume on the server, then creating a PV, and finally creating a PVC to bind to the PV
    - Dynamic provisioning: Users do not need to concern themselves with creating PV resources or storage volumes on the storage server. The ProvisionController automatically creates storage volumes and PV resources based on the information in the StorageClass resource and the requested capacity of the PVC

### CapacityController

![截屏2024-09-24 14.14.04.png](/kubernetes_manual/images/截屏2024-09-24%2014.14.04.png)

- After the ProvisionController completes the PV creation operation, the CapacityController updates the consumed capacity in CSIStorageCapacity
- After the ProvisionController completes the PV deletion operation, the CapacityController updates the CSIStorageCapacity to release the freed capacity

### **CloningProtectionController**

 When cloning a storage volume, the ProvisionController sets cloning protection on the PVC pointed to by the dataSource.

 Subsequently, the CloningProtectionController monitors the PVCs under cloning protection. If it detects that the target PVC has completed the cloning operation, it deletes the cloning finalizer member. This prevents the source PVC from being deleted while the cloned volume is being created.

**Finalizer**: A finalizer is a namespace-scoped key that instructs Kubernetes to wait until specific conditions are met before completely deleting resources marked for deletion. Finalizers prompt [controllers](https://kubernetes.io/zh-cn/docs/concepts/architecture/controller/) to clean up resources owned by the deleted objects.

# Resources

![截屏2024-09-23 11.39.28.png](/kubernetes_manual/images/截屏2024-09-23%2011.39.28.png)

## Pod

![未命名.png](/kubernetes_manual/images/未命名.png)

 All containers within a Pod share a single Linux namespace and network.

### Create a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-manual
spec:
  containers:
  - image: luksa/kubia
    name: kubia
    ports:
    - containerPort: 8080
      protocol: TCP
```

### Pause Container

- A Pod must contain a Pause Container, and it starts up before any other containers in the Pod
- The Pause Container ensures that all containers in this Pod share a single network and a single Linux namespace
    - The Pause container starts first and "acquires" the namespaces (Network, UTS, IPC).
    - All your actual containers then join the namespaces owned by the Pause container.
    - This is the Pause Container code:
    
    ```bash
    #include <unistd.h>
    #include <signal.h>
    
    int main() {
        // Ignore signals or handle them to reap "zombie" processes
        for (;;) {
            pause(); 
        }
    }
    ```
    
- If the Pause Container is killed, the kubelet will recreate the Pause Container and all other containers

### Pods communicate within the same Node

- Virtual Ethernet Interface Pair (veth pair) serves the containers
- In a veth pair, one interface remains in the host’s namespace; the other is in the container’s network namespace
- These two virtual interfaces act like the two ends of a pipe: what goes in one end comes out the other
- Network packets can enter through the container’s interface, reach the bridge, and ultimately be delivered to any container connected to the bridge

![截屏2024-10-11 16.11.30.png](/kubernetes_manual/images/截屏2024-10-11%2016.11.30.png)

### Pod Communication Across Nodes

- Within the cluster, Pod IP addresses must be unique, so there are no IP address conflicts
- The Node’s physical network interface must be connected to the bridge
- The routing table on the Node must ensure that the corresponding IP address is routed to the target Node

![截屏2024-10-11 16.21.58.png](/kubernetes_manual/images/截屏2024-10-11%2016.21.58.png)

### Pod CPU Binding

- Kubelet uses the CFS (Completely Fair Scheduler) algorithm to allocate CPU resources to Pods
- Kubelet uses Linux cpusets to enable Pod CPU exclusivity (i.e., CPU pinning)
- This prevents competition for CPU resources with other Pods, thereby avoiding performance degradation
- Procedure:
    1.  Drain the Node: `kubectl drain <NODE_NAME>` 
    2.  Stop Kubelet: `systemctl stop kubelet`
    3.  Modify Kubelet parameters: `--cpu-manager-policy="static"`
        - --cpu-manager-policy:
            - [`none`](https://kubernetes.io/docs/tasks/administer-cluster/cpu-management-policies/#none-policy): Default policy, which enforces CPU usage limits for Guaranteed and Burstable pods using Linux’s default CFS quota.
            - [`static`](https://kubernetes.io/docs/tasks/administer-cluster/cpu-management-policies/#static-policy): Allows *Guaranteed* Pods with integer CPU `requests` to exclusively use the node’s CPU, implemented via Linux cpuset cgroups.
    4.  Delete the old CPU manager state file: `rm /var/lib/kubelet/cpu_manager_state`
    5.  Start Kubelet: `systemctl start kubelet`

## Namespace

### Create a namespace

```yaml

apiVersion: v1
kind: Namespace
metadata:
  name: custom-namespace
```

 Alternatively: **`kubectl create namespace custom-namespace`**

## ReplicationController

### Procedure:

![截屏2024-09-23 11.30.59.png](/kubernetes_manual/images/截屏2024-09-23%2011.30.59.png)

### Internal Structure:

![截屏2024-09-23 11.32.59.png](/kubernetes_manual/images/截屏2024-09-23%2011.32.59.png)

 Determine labels, verify that the number of replicas matches the requirement, and create new pods using the pod template

## ReplicaSet

### Create a ReplicaSet

```yaml
apiVersion: apps/v1beta2
kind: ReplicaSet
metadata:
  name: kubia
spec:
  replicas: 3
  selector:
    matchLabels:
      app: kubia
  template:
    metadata:
      labels:
        app: kubia
    spec:
      containers:
      - name: kubia
        image: luksa/kubia
```

## Deployment

 When a Deployment is launched, it launches a ReplicaSet

![截屏2024-09-29 15.47.57.png](/kubernetes_manual/images/截屏2024-09-29%2015.47.57.png)

### Deployment vs. ReplicaSet

1.  A Deployment has a rolling update mechanism, whereas a ReplicaSet does not
2.  A Deployment can be rolled back, but a ReplicaSet cannot

### Rolling Back and Revisions

- When changes are made to a Deployment’s `.spec.template`, a Revision is created
- Revisions are used for rolling back; you can roll back to a previous revision
- Changing the `revisionHistoryLimit` can adjust the number of revisions stored

### Rolling Update

- rollingUpdate Strategy
    
    ```yaml
    spec:
      strategy:
        rollingUpdate:
          maxSurge: 1  # 允许 pod 数量超 Replica 设置的个数
          maxUnavailable: 0  # 允许 Unavaliable Pod 的数量，默认 25% pod 数量
        type: RollingUpdate
    ```
    

### Create a Deployment

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubia
spec:
  replicas: 3  # 副本数量
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - image: luksa/kubia:v1
        name: nodejs
```

## DaemonSet

![截屏2024-09-23 17.22.17.png](/kubernetes_manual/images/截屏2024-09-23%2017.22.17.png)

- A DaemonSet ensures that each Node has a specific Pod
- Use `a NodeSelector` to specify the Nodes where Pods should be placed (by adding labels to the Nodes)

### Create a DaemonSet

```yaml
apiVersion: apps/v1beta2
kind: DaemonSet
metadata:
  name: ssd-monitor
spec:
  selector:
    matchLabels:
      app: ssd-monitor
  template:
    metadata:
      labels:
        app: ssd-monitor
    spec:
      nodeSelector:
        disk: ssd
      containers:
      - name: main
        image: luksa/ssd-monitor
```

## StatefulSet

### StatefulSet vs. Deployment, ReplicaSet, and ReplicationController

- Pods within a StatefulSet are stateful, whereas Pods in the other three resources are stateless
    - "Stateful" means that a StatefulSet maintains the Pod's information and state
    - When a Pod is lost, the StatefulSet creates a new Pod and ensures that the new Pod retains the information and state of the original Pod
    - In contrast, the Pods associated with the other three resources have no information or state; they create a new, random Pod
    - Advantages of pods having state:
        - A **StatefulSet** ensures:
            - Pods are assigned **stable names** ( `pod-0`, `pod-1`, …).
            - Each Pod gets its **own PersistentVolumeClaim (PVC)** that remains associated with it.
            - Pods are created and terminated in **order** (important for clustered databases, Zookeeper, Kafka, etc.).
        - Without a StatefulSet, if you used a Deployment:
            - Pods would have random names (e.g., `mysql-abc123` ),
            - Storage would be lost or reassigned,
            - and cluster membership would be disrupted.
- StatefulSet ensures that each Pod has a stable network identity by appending a sequence number to the Pod name
    - and when a Pod disappears, it creates a new Pod that retains the name of the disappeared Pod
    
    ![截屏2024-09-29 16.50.52.png](/kubernetes_manual/images/截屏2024-09-29%2016.50.52.png)
    
- StatefulSet Scaling
    - Scaling Up: The sequence number for the newly created Pod will be the next unused number
    - Scaling Down: Pods are deleted in descending order of their sequence numbers
- StatefulSet has Volume Claim Templates
    - You can create PVCs with different requirements for each Pod

### Creating a StatefulSet

```yaml
kind: List
apiVersion: v1
items:
- apiVersion: v1
  kind: PersistentVolume
  metadata:
    name: pv-a
  spec:
    capacity:
      storage: 1Mi
    accessModes:
      - ReadWriteOnce
    persistentVolumeReclaimPolicy: Recycle
    gcePersistentDisk:
pdName: pv-a
      fsType: nfs4
- apiVersion: v1
  kind: PersistentVolume
  metadata:
name: pv-b
...
```

## Job

### Create a Job

```yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: batch-job
spec:
  template:
    metadata:
      labels:
        app: batch-job
    spec:
      restartPolicy: OnFailure  # 不可以是 Always
			completions: 5  # 设置完成次数
		  parallelism: 2  # 设置并行 pods 数量
      containers:
      - name: main
        image: luksa/batch-job
```

## CronJob

### Create a CronJob

```yaml
apiVersion: batch/v1beta1
kind: CronJob
metadata:
API group is batch, version is v1beta1
  name: batch-job-every-fifteen-minutes
spec:
  schedule: "0,15,30,45 * * * *“  # 分钟、小时、几号、几月、周几。
																	# 这里是每天每小时的 0、15、30、45 分
  jobTemplate:
    spec:
      template:
        metadata:
          labels:
            app: periodic-batch-job
        spec:
          restartPolicy: OnFailure
          containers:
          - name: main
            image: luksa/batch-job
```

## Service

### Features

- A Service has its own static IP address and port for clients to connect to (typically a Pod)
- This IP address is virtual (VIP) and is not physically connected to any network port; it is not listed as a Destination IP Addr
- When a Service is created, the VIP is immediately assigned to it
- The API Server notifies all kube-proxy instances running on Worker Nodes that the Service has been created
- kube-proxy ensures that the Service’s IP address is reachable by configuring iptables rules

### Creating a Service

```yaml
apiVersion: v1
kind: Service
metadata:
  name: kubia
spec:
  ports:
  - port: 80
    targetPort: 8080
  selector:
		app: kubia  # 所有 app=kubia 的 pod 才能使用这个 svc
```

### Internal access to the Service from a Pod

![截屏2024-09-23 17.47.48.png](/kubernetes_manual/images/截屏2024-09-23%2017.47.48.png)

### Service Types

1.  ClusterIP (default): Assigns an IP address from an IP address pool
2.  NodePort: Assigns an additional port to allow external access to the service
3.  LoadBalancer: External requests first pass through the load balancer; traffic from the load balancer is directly redirected to the various backend Pods, with the cloud platform determining how to perform load balancing
    - Each Service requires its **own external load balancer** (which can be expensive).
    - Works only at **Layer 4 (TCP/UDP)**: no URL routing, no domain-based routing.
    - Multiple services can be exposed by a single Ingress
        
        ![截屏2024-09-23 18.21.50.png](/kubernetes_manual/images/截屏2024-09-23%2018.21.50.png)
        
        - An **API object + controller (e.g., Nginx, HAProxy, Traefik, Istio Gateway)**.
        - Provides **L7 (HTTP/HTTPS)** routing:
            - Path-based ( `/api` → service A, `/blog` → service B).
            - Host-based ( `foo.example.com` → service A, `bar.example.com` → service B).
        - Typically runs behind **a single LoadBalancer** or NodePort → so you don’t need a separate load balancer for each service.
        - Supports additional features:
            - TLS termination (HTTPS)
            - URL rewrites, request/response manipulation
            - Centralized routing configuration

### **Service (L4: TCP/UDP) vs. Ingress (L7: HTTP/HTTPS)**

**L4:**

- Operates at the **Transport layer** (IP + port).
- Balances traffic across Pods based on **port only**.
- Types: ClusterIP, NodePort, LoadBalancer.
- **Pros**
    - Simple and lightweight.
    - Fast (no HTTP parsing).
    - Works for **any protocol** (HTTP, gRPC, MySQL, Redis, custom TCP/UDP).
    - Built-in Kubernetes primitive (no extra controller needed).
- **Cons**
    - Basic load balancing (round-robin, no content awareness).
    - Cannot route based on URL path, host header, or cookies.
    - No TLS termination, rewrites, or advanced policies.
    - If exposed via NodePort or LoadBalancer, can become expensive or complex.

**L7:**

- Operates at the **application layer** (understands HTTP and gRPC).
- Requires an **Ingress Controller** (Nginx, Traefik, Istio Gateway, etc.).
- Can route traffic based on **Host, Path, Headers, and Cookies**.
- **Pros**
    - Smart routing:
        - `/api` → backend API Pods
        - `/static` → CDN service
        - `foo.example.com` → Team A app
        - `bar.example.com` → Team B app
    - TLS termination (manage HTTPS certificates centrally, e.g., cert-manager).
    - Can implement rate limiting, authentication, caching, etc. via a controller.
    - Cleaner than exposing multiple NodePorts/LoadBalancers.
- **Cons**
    - Only works for **HTTP/HTTPS/gRPC** (L7 protocols).
    - Requires an additional component (Ingress Controller) → increased complexity.
    - More overhead (must parse HTTP, manage configurations).
    - Advanced features vary between controllers (Nginx vs. Traefik vs. Istio).

## Endpoint

![image.png](/kubernetes_manual/images/image%20(1).png)

## Volume (Pod / PersistentVolume)

### Non-persistent Storage

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fortune
spec:
containers:
- image: luksa/fortune
  name: html-generator
  volumeMounts:
  - name: html
    mountPath: /var/htdocs
- image: nginx:alpine
  name: web-server
  volumeMounts:
  - name: html
    mountPath: /usr/share/nginx/html
		readOnly: true
  ports:
  - containerPort: 80
    protocol: TCP
volumes:  # Volume 直接建立在 pod 内，会随着 pod 的消失而消失
- name: html
  emptyDir: {}
```

### Persistent Storage

```yaml
apiVersion: v1
kind: PersistentVolume
metadata:
  name: mongodb-pv
spec:
  capacity:
    storage: 1Gi  # 定义 PV 的大小
	accessModes:
	- ReadWriteOnce  # 允许单个用户读写
	- ReadOnlyMany  # 或者允许多个用户读
	persistentVolumeReclaimPolicy: Retain
	gcePersistentDisk:
		pdName: mongodb
		fsType: ext4
```

- A PV does not belong to any namespace; pods in all namespaces can access it

### Access Modes

- RWO - ReadWriteOnce
- ROX - ReadOnlyMany
- RWX - ReadWriteMany
- RWOP - ReadWriteOncePod

## PersistentVolumeClaim

![截屏2024-09-24 13.46.55.png](/kubernetes_manual/images/截屏2024-09-24%2013.46.55.png)

### Create PVC

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: mongodb-pvc
spec:
  resources:
    requests:
      storage: 1Gi  # 请求了 1GB 资源
  accessModes:
  - ReadWriteOnce  # 允许单个用户读写
  storageClassName: ""  # 指名 sc
												# 如果是空，则被视为要请求的是没有设置存储类的 PV 卷，因此这一 PVC 申领只能绑定到未设置存储类的 PV 卷
```

## StorageClass

- Different types may map to different service levels or backup policies, equivalent to configuration profiles (SSD/HDD/100T/500GB…)
- Must include `the provisioner`, `parameters`, and `reclaimPolicy` fields
- You can mark a specific StorageClass as the cluster’s default storage class
- When a PVC does not specify a ` `storageClassName` `, the default StorageClass is used
    - To set as default: ` `kubectl patch storageclass <your-class-name> -p '{"metadata {"annotations":{"[storageclass.kubernetes.io/is-default-class":"true](http://storageclass.kubernetes.io/is-default-class%22:%22true)"}}}'``

### Create a StorageClass

```yaml
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: low-latency
  annotations:
    storageclass.kubernetes.io/is-default-class: "false"
provisioner: csi-driver.example-vendor.example
reclaimPolicy: Retain # 默认值是 Delete
allowVolumeExpansion: true
mountOptions:
  - discard # 这可能会在块存储层启用 UNMAP/TRIM
volumeBindingMode: WaitForFirstConsumer
parameters:
  guaranteedReadWriteLatency: "true" # 这是服务提供商特定的
```

## Lease

### Node Heartbeats

 Each Node has a Lease object with a matching name in the `kube-node-lease` Namespace. Based on this, each kubelet heartbeat is an **update** request to that `Lease` object, updating the `spec.renewTime` field of the Lease.

### Leader Election

 Ensures that only one instance of a component is running.

 Typically used for Controllers or Schedulers.

### View Lease YAML

```yaml
apiVersion: coordination.k8s.io/v1
kind: Lease
metadata:
  name: example-lease
  namespace: default
spec:
  holderIdentity: "" # 当前持有该 Lease 的实体
  leaseDurationSeconds: 15 # 超过 15 秒没有反应 lease 失效，表示持有者不再存活
  acquireTime: null # 租约被创建的时间
  renewTime: null # 租约的最后一次创建时间
  leaseTransitions: 0 # 租约持有者的更变次数
```

## Secret

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: test-secret
data:
  username: test
  password: 123456
```

### ConfigMap, Secret, emptyDir Graph

![截屏2024-09-27 16.42.17.png](/kubernetes_manual/images/截屏2024-09-27%2016.42.17.png)

## ServiceAccount

### Features

- A Pod can only use a ServiceAccount from the same namespace
- If no ServiceAccount is declared for a Pod, the default ServiceAccount in the namespace is used by default
- Used to bind to a Role or ClusterRole

## Role

### Characteristics

- Defines verbs and the resources they target

### Create a Role

```yaml
apiVersion: rbac.authorization.k8s.io/v1kind: Role
metadata:
  namespace: foo
  name: service-reader
rules:
- apiGroups: [""]
  verbs: ["get", "list"]
  resources: ["services"]
```

```bash
$ kubectl create -f service-reader.yaml -n foo

$ kubectl create role service-reader --verb=get --verb=list --resource=services -n bar
```

## RoleBinding

![截屏2024-10-25 11.27.28.png](/kubernetes_manual/images/截屏2024-10-25%2011.27.28.png)

- Bind Roles/ClusterRoles to users/groups/ServiceAccounts

### Create a RoleBinding

```bash
$ kubectl create rolebinding test --role=service-reader --serviceaccount=foo:default -n foo
```

## Downward API (Volume)

```yaml

apiVersion: v1
kind: Pod
metadata:
  name: downward
spec:
  containers:
  - name: main
    image: busybox
    command: ["sleep", "9999999"]
    resources:
      requests:
        cpu: 15m
        memory: 100Ki
      limits:
				cpu: 100m
        memory: 4Mi
       env:
       - name: POD_NAME
				 valueFrom:
				   fieldRef:
				     fieldPath: metadata.name  # referencing, not absolute value
			 - name: POD_NAMESPACE
				 valueFrom:
				   fieldRef:
				     fieldPath: metadata.namespace
			 - name: POD_IP
				 valueFrom:
				   fieldRef:
				     fieldPath: status.podIP
			 - name: NODE_NAME
				 valueFrom:
				   fieldRef:
				     fieldPath: spec.nodeName
			 - name: SERVICE_ACCOUNT
				 valueFrom:
				   fieldRef:
				     fieldPath: spec.serviceAccountName
			 - name: CONTAINER_CPU_REQUEST_MILLICORES
				 valueFrom:
				   resourceFieldRef:
				     resource: requests.cpu
			       divisor: 1m
			 - name: CONTAINER_MEMORY_LIMIT_KIBIBYTES
			   valueFrom:
		       resourceFieldRef:
				     resource: limits.memory
				  	 divisor: 1Ki
```

# Field

## Label (labels can be customized)

![截屏2024-09-23 10.55.42.png](/kubernetes_manual/images/截屏2024-09-23%2010.55.42.png)

- app: Category
- rel: Current version status (stable, beta, canary)
- app and rel are key-value labels that can be customized.

 Label a node: **`kubectl label node test1 gpu=true`**

**Create a pod based on the node's labels (nodeSelector):**

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: kubia-gpu
spec:
  nodeSelector:
    gpu: "true"
  containers:
  - image: luksa/kubia
name: kubia
```

## Label vs Taint

- Label
    - Apply labels to a node and specify `a nodeSelector` in `the YAML`
    - The pod will only be deployed on the corresponding node
- Taint
    - When a node is `tainted`, the YAML must include `a tolerance` to deploy a Pod to that node
    - Taints = repelling mechanism. By default, pods are *blocked* unless they tolerate the taint.

## Annotation

 Unlike Labels, there is no functionality to search or filter using the `command` field.

 Used to add a description

## Liveness probe

 Detects pod health in three ways:

1.  HTTP GET
2.  TCP Socket
3.  Exec (sends a command and checks if the return code is 0)

### Create a Liveness Probe

```yaml
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: registry.k8s.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -f /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5  # 执行第一次探测前应该等待 5 秒
      periodSeconds: 5  # 每 5 秒执行一次
```

## Readiness Probe

### Create Readiness Probe

```yaml
readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

## Command & Arguments

### e.g.

```yaml
kind: Pod
  spec:
    containers:
    - image: some/image
      command: ["/bin/command"]
      args: ["arg1", "arg2", "arg3"]
```

### command

- Equivalent to Docker's ENTRYPOINT; executes an executable file inside the container

### args

- Equivalent to Docker's CMD; parameters passed to the executable

## Env

```yaml
kind: Pod
spec:
 containers:
 - image: luksa/fortune:env
   env:
   - name: INTERVAL
		 value: "30"
   name: html-generator
...
```

## ConfigMap

### Create a ConfigMap

1.  Create based on file contents
    
    **`kubectl create configmap my-config --from-file=config-file.conf`**
    
    ![截屏2024-09-24 14.53.57.png](/kubernetes_manual/images/截屏2024-09-24%2014.53.57.png)
    
2.  Create from a YAML file
 **`kubectl create -f fortune-config.yaml`**

### Pods Using ConfigMaps

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: fortune-env-from-configmap
spec:
  containers:
  - image: luksa/fortune:env
		env:
		- name: INTERVAL  # 环境变量名称
			valueFrom:
			  configMapKeyRef:
			    name: fortune-config  # ConfigMap 名称
			    key: sleep-interval  # 对应 ConfigMap 中的 key
...
```

# Process

## Create resources

### Create a Deployment by loading a YAML file via kubectl

![截屏2024-10-11 13.48.17.png](/kubernetes_manual/images/截屏2024-10-11%2013.48.17.png)

### Create a Service

- Pod A wants to send a packet to the Service
- The packet’s destination is initially set to the Service’s IP and port (172.30.0.1:80)
- The packet is first processed by Node A’s kernel according to iptables rules
- If the packet matches a rule in iptables, its destination IP and port are replaced with those of a randomly selected Pod (in this example, changed to 10.1.2.1:8080)

![截屏2024-10-12 10.50.04.png](/kubernetes_manual/images/截屏2024-10-12%2010.50.04.png)

# Instructions

```shell
// 创建新资源（重复配置会报错）（需要yaml/yml/json文件）
kubectl create -f <file's name>

// 配置应用于资源（重复配置不会报错）（需要yaml/yml/json文件）
kubectl apply -f <file's name> 

// 创建容器镜像
kubectl run <name> --image=<image's name> 

// 列出pods/nodes/ingresses/deployments/secrets/namespaces/services...
kubectl get pods / nodes / ingresses （ing） / deployments / secrets / namespaces (ns) / services (svc) ... 

// 列出所有pods
kubectl get pods -A 

// 列出pods并显示详细信息
kubectl get pods -o wide 

// 查看 yaml 文件
kubectl get po <name> -o yaml

// 列出指定namespace的pod
kubectl get pods --ns=<namespace> 

// 实时查看pods的信息
kubectl get pods --watch

// 查看 pod 并显示 label
kubectl get po --show-labels

// 查看带有 creation_method 和 env 标签的 pod
kubectl get po -L creation_method,env

// 查看标签有 creation_method=manual 的 pod
kubectl get po -l creation_method=manual

// 查看 k8s 组件状态
kubectl get componentstatuses

// 删除目标pod
kubectl delete pod <pod's name> 

// 显示pod详细信息
kubectl describe pod <pod's name> 

// 干进行，只会在客户端执行命令，但不会向Kubernetes API 发送实际的请求
-dry-run=client 

// 修改 pod 的镜像
kubectl set image POD/<POD_NAME> <CONTAINER_NAME>=<IMAGE_NAME>:<TAG> 

// help
kubectl explain po.spec

// get pod logs
kubectl logs <pod's name>

// get container logs
kubectl logs <container id>

// pod 端口转发
kubectl port-forward <pod name> 8888:8080

// 回滚到先前的 Deployment 版本
kubectl rollout undo deployment/abc
  
// 检查 Daemonset 的部署状态
kubectl rollout status daemonset/foo
  
// 重启 Deployment
kubectl rollout restart deployment/abc
  
// 重启带有 'app=nginx' 标签的 Deployment
kubectl rollout restart deployment --selector=app=nginx
```
