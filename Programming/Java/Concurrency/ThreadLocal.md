 **ThreadLocal**

Thread-Local provides get and set accessor methods that maintain a separate copy of the value for each thread that uses it, so a get returns the most recent value passed to set from the currently executing thread.

The thread-specific values are stored in the Thread object itself; when the thread terminates, the thread-specific values can be garbage collected.

---
PS
ThreadLocal is widely used in implementing application frameworks. For example, J2EE containers associate a transaction context with an executing thread for the duration of an EJB call. This is easily implemented using a static Thread- Local holding the transaction context: when framework code needs to determine what transaction is currently running, it fetches the transaction context from this ThreadLocal.