### Exclusive vs shared locks

Exclusive lock - lock for writing. Once it is obtained, no other workers can read/update the resource until the exclusive lock is released.
**synchronized** keyword obtains exclusive lock over the object

Shared lock - lock for reading. There might be many read locks obtained at the same time. Exclusive lock cannot be obtained while there are any shared locks.

---
Every Java object can implicitly act as a lock for purposes of synchronization; these built-in locks are called **intrinsic locks** or **monitor locks**

***Intrinsic locks are reentrant***
Reentrancy is implemented by associating with each lock an acquisition count and an owning thread

---
Intrinsic locks in Java act as **mutexes** *(or mutual exclusion locks)*, which means that at most one thread may own the lock. When thread A attempts to acquire a lock held by thread B, A must wait, or block, until B releases it. If B never releases the lock, A waits forever.

---
