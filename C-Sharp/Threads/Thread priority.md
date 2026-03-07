#OS #SoftDev #CSharp 
In a multithreaded env, each [[Thread]] has a priority that determines how frequently the [[Thread]] is allocated CPU resources by OS. `Priority` property in C# is used to set or get the priority of a [[Thread]].
- Programmer can explicitly assign a priority to a [[Thread]] using the `Priority` property.
- By default, a [[Thread]]'s priority is set to `Normal`.
- OS handles [[Thread]] scheduling, but priorities influence how threads are exec.
- Setting priorities incorrectly can lead to **[[Thread]] [[Starvation]]**, where lower-priority threads may not get enough CPU time.
- High-priority [[Thread]] does not guarantee that it will exec before a low-priority [[Thread]] because of context switching & other OS-level scheduling mechanisms.
- [[Thread]] priority always depends on the process priority (the parent container).