# Raft Consensus Algorithm

Raft is a distributed consensus algorithm that allows a group of servers to agree on the same sequence of operations, even when servers crash or the network is unreliable. It is designed around a strong leader and a replicated log, making the algorithm easier to understand and implement than many earlier consensus protocols.

## Server roles

Every Raft server is in one of three roles:

| Role | Responsibility |
| --- | --- |
| Leader | Accepts client commands, appends them to its log, and replicates log entries to followers |
| Follower | Responds to requests from leaders and candidates |
| Candidate | Requests votes during a leader election |

A healthy Raft cluster normally has one leader per term. All other servers are followers.

## Terms

A **term** is a logical election epoch. Terms increase monotonically and help servers recognize stale leaders and requests.

For example:

| Term | Event |
| --- | --- |
| 1 | Server A becomes leader |
| 2 | Server A fails and Server B becomes leader |
| 3 | A later election makes Server C the leader |

Every server stores its `currentTerm`. When a server observes a higher term, it updates `currentTerm` and becomes a follower if necessary.

## How Raft works

Raft divides consensus into leader election, log replication, and safety rules that keep the replicated state machines consistent.

### 1. Leader election

All servers begin as followers. The leader periodically sends heartbeats to prevent new elections.

If a follower does not receive a heartbeat before its randomized election timeout expires, it:

1. Increments its current term.
2. Becomes a candidate.
3. Votes for itself.
4. Sends `RequestVote` RPCs to the other servers.

The candidate becomes leader after receiving votes from a majority of the cluster. Randomized election timeouts reduce the chance that several followers repeatedly become candidates at the same time.

### Majority quorum

A decision requires more than half of the voting servers. With five servers, a candidate needs at least three votes to become leader. The same majority principle allows the cluster to continue operating when a minority of servers is unavailable.

### 2. Log replication

Suppose Server 1 is the leader and a client submits:

```text
SET x = 10
```

The replication flow is:

1. The leader appends the command to its local log.
2. The leader sends `AppendEntries` RPCs to its followers.
3. Followers append the entry and acknowledge it.
4. After the entry is safely replicated, the leader advances its commit index.
5. Each server applies the committed entry to its state machine in log order.

For a three-server cluster, the leader and one follower form a majority. The third server can be temporarily unavailable without preventing progress.

Clients may contact any server, but only the leader accepts new commands for the Raft log. A follower commonly redirects the client to the known leader, although some implementations proxy the request.

> A leader can use majority replication to commit an entry from its current term. Entries from older terms become committed indirectly when a current-term entry is committed.

### 3. Leader failure

When a leader fails:

1. Followers stop receiving heartbeats.
2. One follower's election timeout expires.
3. That follower becomes a candidate and requests votes.
4. A candidate that wins a majority becomes the new leader.
5. The new leader resumes log replication and repairs inconsistent follower logs.

Raft's voting rules compare the last log term and index of the candidate with those of each voter. A voter rejects a candidate whose log is less up to date than its own. This prevents an outdated server from replacing a leader when doing so could lose committed entries.

## The replicated log

The Raft log is an ordered sequence of commands. Each entry contains:

- **Index**: The entry's position in the log.
- **Term**: The leader term in which the entry was created.
- **Command**: The operation for the state machine.

An example log is:

| Log index | Term | Command |
| --- | --- | --- |
| 1 | 1 | `SET x = 10` |
| 2 | 1 | `SET y = 20` |
| 3 | 2 | `SET x = 30` |

The index and term together identify the history before an entry. During replication, the leader includes the preceding index and term in `AppendEntries`. A follower rejects the request if its log does not contain that matching previous entry, allowing the leader to find and repair the divergence.

## Replicated state machine

Raft separates agreement from execution:

- The **Raft log** records commands that the cluster has agreed to execute.
- The **state machine** applies committed commands in the same order on every server.

```text
Raft log                 State machine
[SET x = 10]  -------->  x = 10
```

Only committed entries are applied. If every server applies the same deterministic commands in the same order, their state machines converge to the same state.

## Raft safety properties

Raft's election and log rules provide several important guarantees:

- **Election safety**: At most one leader can be elected in a term.
- **Leader append-only**: A leader never overwrites or deletes entries in its own log.
- **Log matching**: If two logs contain an entry with the same index and term, all preceding entries are identical.
- **Leader completeness**: A committed entry appears in the logs of leaders elected in later terms.
- **State machine safety**: Servers never apply different commands at the same log index.

These properties preserve consistency across elections, failures, and log repairs.

## Raft and Paxos

| Feature | Raft | Paxos |
| --- | --- | --- |
| Primary goal | Distributed consensus | Distributed consensus |
| Leadership | Explicit, strong leader | Depends on the variant; Multi-Paxos commonly uses a stable leader |
| Log replication | Defined as a central part of the algorithm | Commonly built with Multi-Paxos |
| Leader election | Included in the protocol | Usually provided by the implementation |
| Design emphasis | Understandability and decomposition | Minimal consensus mechanism and formal foundations |

Both protocols can provide fault-tolerant consensus. Raft packages leader election, log replication, and safety into a particularly approachable design.

## Where Raft is used

Raft is used by distributed systems that need consistent replicated metadata or state, including:

- **etcd**, the distributed key-value store used by Kubernetes.
- **Consul**, for service discovery and configuration.
- **TiKV**, a distributed transactional key-value database.

For example, Kubernetes stores cluster metadata in etcd, while etcd uses Raft to replicate that metadata across its members.
