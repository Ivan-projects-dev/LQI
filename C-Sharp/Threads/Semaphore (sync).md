#OS 
In multiprogramming systems, multiple processes may need to access shared resources like files, printers, or memory. To handle this, OSes use [[Semaphore]] as sync mechanism.

[[Semaphore]] works using $2$ fundamental operations:
1. **Wait** ($P$ operation / down)
- Decreases the [[Semaphore]] value.
- If the value becomes negative, the process is blocked until the resource becomes available.
1. **Signal** ($V$ operation / up)
- Increases the [[Semaphore]] value.
- If there are waiting processes, one of them gets unblocked.
![[Pasted image 20251014100051.png]]
**[[Semaphore]]** works by maintaining a counter that controls access to a specific resource, ensuring that no $>$ than the allowed num of processes access the resource at the same time.
To achieve sync, every critical section of code is surrounded by $2$ operations.

Example: Let’s consider processes $P_1$ & $P_2$ sharing a [[Semaphore]] $S$, init to $1$:
- State $1$: Both processes are in their non-critical sections, & $S = 1$.
- State $2$: $P_1$ enters the critical section. It performs wait(S), so $S = 0$. $P_2$ continues in the non-critical section.
- State $3$: If $P_2$ now wants to enter, it cannot proceed since $S = 0$. It must wait until $S > 0$.
- State $4$: When $P_1$ finishes, it performs $signal(S)$, making $S = 1$. Now $P_2$ can enter its critical section & again sets $S = 0$.
![[Pasted image 20251014101336.png]]
This mechanism guarantees mutual exclusion, ensuring that only one process can access the shared resource at a time.

**Counting [[Semaphore]]** is used when multiple instances of a resource exist. Its value can range over an unrestricted domain $(0 to N)$ (managing access to a pool of $5$ printers).
**Binary [[Semaphore]]** - special case of counting [[Semaphore]] with only values $0 or 1$. Works like a **lock**: either the resource is **free $(1)$** or **busy $(0)$** (managing access to a single critical section).