There is a branch of research focused on creating non-blocking algorithms for concurrent environments. These algorithms exploit low-level atomic machine instructions such as compare-and-swap (CAS), to ensure data integrity.

A typical CAS operation works on three operands:

1.  The memory location on which to operate (M)
2.  The existing expected value (A) of the variable
3.  The new value (B) which needs to be set

**The CAS operation updates atomically the value in M to B, but only if the existing value in M matches A, otherwise no action is taken.**

In both cases, the existing value in M is returned. This combines three steps – getting the value, comparing the value, and updating the value – into a single machine level operation.

When multiple threads attempt to update the same value through CAS, one of them wins and updates the value. **However, unlike in the case of locks, no other thread gets suspended**; instead, they're simply informed that they did not manage to update the value. The threads can then proceed to do further work and context switches are completely avoided.

One other consequence is that the core program logic becomes more complex. This is because we have to handle the scenario when the CAS operation didn't succeed. We can retry it again and again till it succeeds, or we can do nothing and move on depending on the use case.

The most commonly used atomic variable classes in Java are **AtomicInteger**, **AtomicLong**, **AtomicBoolean**, and **AtomicReference**. These classes represent an _int_, _long_, _boolean,_ and object reference respectively which can be atomically updated. The main methods exposed by these classes are:

-   _get()_ – gets the value from the memory, so that changes made by other threads are visible; equivalent to reading a _volatile_ variable
-   _set()_ – writes the value to memory, so that the change is visible to other threads; equivalent to writing a _volatile_variable
-   _lazySet()_ – eventually writes the value to memory, maybe reordered with subsequent relevant memory operations. One use case is nullifying references, for the sake of garbage collection, which is never going to be accessed again. In this case, better performance is achieved by delaying the null _volatile_ write
-   _compareAndSet()_ – same as described in section 3, returns true when it succeeds, else false
-   _weakCompareAndSet()_ – same as described in section 3, but weaker in the sense, that it does not create happens-before orderings. This means that it may not necessarily see updates made to other variables. As of Java 9, this method has been deprecated in all atomic implementations in favor of _weakCompareAndSetPlain()_. The memory effects of _weakCompareAndSet()_ were plain but its names implied volatile memory effects. To avoid this confusion, they deprecated this method and added four methods with different memory effects such as _weakCompareAndSetPlain()_ or _weakCompareAndSetVolatile()_

Example of correct non-blocking AtomicInteger usage:

```
public class SafeCounterWithoutLock {
    private final AtomicInteger counter = new AtomicInteger(0);
    
    public int getValue() {
        return counter.get();
    }
    public void increment() {
        while(true) {
            int existingValue = getValue();
            int newValue = existingValue + 1;
            if(counter.compareAndSet(existingValue, newValue)) {
                return;
            }
        }
    }
}
```