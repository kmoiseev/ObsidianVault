## Reliability

The system should continue to work correctly (performing the correct function at the desired level of performance) even in the face of adversity (hardware or software faults, and even human error).
 *“continuing to work correctly, even when things go wrong.”*
 
 The things that can go wrong are called faults, and systems that anticipate faults and can cope with them are called **fault-tolerant** or **resilient**.
 
#### Fault types:
**Hardware faults** : cpu, hdd, cables, etc

*Examples:*
- Cpu broken
- Hdd broken
- Cables unplugged

*Ho to prevent (mainly redundant components):*
-  RAID
-  Hot-swappable cpu
-  Dual power cable

**Software faults**

*Examples:*
- Bad input 
- Leap second

*How to prevent:*
-  Testing
-  Isolation
-  Auto-restart
-  Monitoring

**Human errors**

*Examples:*
- Bad configuration update

*How to prevent*
- Design systems to minimize opportunities for error. Fore example, admin interface to do only "the right thing"
- Sandbox environments
- Testing

---
 
### Scalability

 As the system grows (in data volume, traffic volume, or complexity), there should be reasonable ways of dealing with that growth.
 *"system’s ability to cope with increased load"*
 Load can be described with *load paramters*. Those parameters depend on the system being designed. For example, for we server that can be requests per second, database read/write ration, simultaneous active users and etc.
 
 Percentile of *response time* is often conisdered as a metric of the system. That time includes network latency and server processing time. Percintile *p50* means the middle in the sorted array of all the response time. *p99* means the one response time which is almost the slowest one. Usually, the more data user has the more response time is fore him. And the more data user has, the more valuable he is. The right way of aggregating response time data is to add the histograms
 
 ---
 
 ### Maintainability
 
 Over time, many different people will work on the system (engineering and oper‐ ations, both maintaining current behavior and adapting the system to new use cases), and they should all be able to work on it productively
 
 Subtopics of maintainability:
 - Operability - make it easy for operations team to keep the system running smoothly.  Good operability means having good visibility into the system’s health, and having effective ways of managing it.
	 - Monitoring
	 - Automation and integration tools
	 - Aboid dependency on individual machines
	 - Good documentation
	 - Self-healing plus manual control
 - Simplicity - make it easy for new engineers to understand the system
	 - Abstraction is the best tool to remove complexity
 - Evolvability - make it easy for engineers to make changes to the system in the future, adapting it for unanticipated use cases as requirements change. 
	 - TDD and good coverage
	 - Good architecture

 
 