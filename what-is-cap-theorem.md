# CAP Theorem

The `CAP theorem` (also known as Brewer's theorem) is a fundamental principle in distributed computer science. It states that a distributed data store can simultaneously provide at most `two` of three core guarantees: `Consistency`, `Availability`, and `Partition Tolerance`.

---

## The Three Pillars (CAP)

- `Consistency (C):`
  > Every read receives the most recent write or an error—meaning all nodes see the exact same data at the exact same time, regardless of which node a client connects to.
  
- `Availability (A):`
  > Every non-failing node returns a non-error response for every request, without a guarantee that it contains the absolute latest write (the system is always up and responding).
  
- `Partition Tolerance (P):`
  > The system continues to function despite an arbitrary number of messages being dropped or delayed by the network between nodes.

---

### The Inevitable Trade-off

In real-world networking, physical network failures (`partitions`) are unavoidable. You cannot choose to opt-out of partition tolerance because cables cut, routers crash, and packets get dropped.

Therefore, when a network partition happens and nodes can no longer talk to each other, a system must choose between:

1. `CP (Consistency + Partition Tolerance):`
   > The system chooses to stop responding (or return an error) to requests on isolated nodes to prevent serving stale or conflicting data, sacrificing `Availability`

2. `AP (Availability + Partition Tolerance):`
   > The system keeps responding to all requests from any accessible node, even if some nodes are out-of-date and serving stale data, sacrificing `Consistency`.

---

### Practical Examples

- `CP Systems (e.g., MongoDB with strict configurations, HBase, ZooKeeper):`
  > Prioritize data correctness over uptime. If a bank database loses connection to its main replica, it will refuse transactions rather than risk allowing a user to spend money they don't have due to a delayed sync.

- `AP Systems (e.g., Cassandra, CouchDB, DynamoDB):`
  > Prioritize uninterrupted uptime. If you update your profile picture on a global social media app, one friend might see the old picture for a few extra seconds while the network heals, but the app itself never goes down.
