#OS #SoftDev 
**Semaphore** is a non-negative int var that is shared between various threads. Works upon signaling mechanism, in this a [[Thread]] can be signaled by another [[Thread]]. 
It provides a < restrictive control mechanism. Any [[Thread]] can invoke `signal()` (also known as `release()` or `up()`), & any other [[Thread]] can invoke `wait()` (also known as `acquire()` or `down()`). 
There is no strict ownership in semaphores, meaning the [[Thread]] that signals doesn't necessarily have to be the same $1$ that waited. Semaphores are often used for coordinating signaling between threads. Semaphore uses $2$ atomic operations for process sync: 
- Wait $(P)$
- Signal $(V)$

| Advantages                                                                                         | Disadvantages                                                                         |
| -------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------- |
| Multiple threads can access the critical section at the same time                                  | Has priority inversion                                                                |
| Semaphores are machine-independent                                                                 | Semaphore operations must be implemented in the correct manner to avoid deadlock      |
| Only $1$ process will access the critical section at a time, however, multiple threads are allowed | It leads to a loss of modularity, so semaphores can't be used for large-scale systems |
| Flexible resource management                                                                       | OS has to track all the calls to wait & signal operations                           |
[[Semaphore (sync)]]

