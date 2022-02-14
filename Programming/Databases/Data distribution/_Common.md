 There are various reasons why you might want to distribute a database across multiple machines:

**Scalability**

If your data volume, read load, or write load grows bigger than a single machine can handle, you can potentially spread the load across multiple machines.

**Fault tolerance/high availability**

If your application needs to continue working even if one machine (or several machines, or the network, or an entire datacenter) goes down, you can use multiple machines to give you redundancy. When one fails, another one can take over.

**Latency**

If you have users around the world, you might want to have servers at various locations worldwide so that each user can be served from a datacenter that is geographically close to them. That avoids the users having to wait for network packets to travel halfway around the world.

---

#### Vertical scale

Simpliest approach to scale load is to *scale vertically* or *scale up*. 
Disadvantages:
- Cost grows fater than lineary
- Even with hot-swappable components, machine is limited to a single geographic location
 
 ---
 
 #### Horizontal scale
 
*Shared-nothing architectures* (sometimes called *horizontal scaling* or *scaling out*) have gained a lot of popularity. In this approach, each machine or virtual machine running the database software is called a node. Each node uses its CPUs, RAM, and disks independently. Any coordination between nodes is done at the software level, using a conventional network.

No special hardware is required by a shared-nothing system, so you can use whatever machines have the best price/performance ratio.

With cloud deployements of virtual machines, a multi-region distributed architecture is now feasible even for small companies.

---

*Replication* and *partitioning*  are two common ways to distribute data across multiple nodes. Replication - copying the data, partitioning - splitting the data. These both mechanisms often go hand in hand
![[DB_Distribution_Replication_and_Partitioning.png]]