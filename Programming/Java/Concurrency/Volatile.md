**Volatile**

When a field is declared volatile, the compiler and runtime are put on notice that this variable is shared and that operations on it should not be reordered with other memory operations.
 
Volatile variables are not cached in registers or in caches where they are hidden from other processors, so a read of a volatile variable always returns the most recent write by any thread.

Accessing a volatile variable performs no locking and so cannot cause the executing thread to block, making volatile variables a lighter-weight synchronization mechanism than synchronized.

When thread A writes to a volatile variable and subsequently thread B reads that same variable, the values of all variables that were visible to A prior to writing to the volatile variable become visible to B after reading the volatile variable. *Not recommended to rely on it*

Use volatile variables only when all the following criteria are met:

-   Writes to the variable do not depend on its current value, or you can ensure that only a single thread ever updates the value;

-   The variable does not participate in invariants with other state variables; and
    
-   Locking is not required for any other reason while the variable is being accessed.