**Synchronized**
 
Provides exlusive lock over the object it references
If no object used, an object on which method is invoked serves as lock
If called from static, then Class object is used.

Lock is acquired before entering the block and automatically released after leaving normally or cause of an exception

 Synchronization _also_ creates a "happens-before" memory barrier, causing a memory visibility constraint such that anything done up to the point some thread releases a lock _appears_ to another thread subsequently acquiring **_the same lock_** to have happened before it acquired the lock. In practical terms, on current hardware, this typically causes flushing of the CPU caches when a monitor is acquired and writes to main memory when it is released, both of which are (relatively) expensive.