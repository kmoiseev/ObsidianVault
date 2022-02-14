#### Atomicity
Some platforms writes long/double in two steps. First 32 bits -> second 32 bits.
 it is not safe to use shared mutable long and double variables in multithreaded programs unless they are declared volatile or guarded by a lock.

#### Visibility 
In new memory model it does no