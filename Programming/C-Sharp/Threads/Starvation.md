#OS #Algorithm 
**Starvation/indefinite blocking** occurs in **priority scheduling** when a low-priority process keeps waiting indefinitely because higher-priority processes continuously get the CPU.
- This problem arises in heavily loaded systems where resources are always occupied by higher-priority tasks, leaving some processes starved.

**Aging** is a scheduling tech used to prevent starvation by gradually increasing the priority of processes waiting too long in the system.
- Ensures fairness, as long-waiting processes eventually get CPU time.
- Often combined with algorithms like priority scheduling or round-robin to balance short-term efficiency with long-term fairness.