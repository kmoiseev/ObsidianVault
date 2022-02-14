**Replication** means keeping a copy of the same data on multiple machines that are connected via a network.

Reasons for replications:
- To keep data geographically close to users (+*latency*)
- To allow the system to continue working even if fail (+*avialability*)
- Scale out machines serving requests (+*throughput*)

*Single-leader* replication is popular because it is fairly easy to understand and there is no conflict resolution to worry about. *Multi-leader* and *leaderless* replication can be more robust in the presence of faulty nodes, network interruptions, and latency spikes—at the cost of being harder to reason about and providing only very weak consistency guarantees.