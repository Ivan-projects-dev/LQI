#SoftDev #OS #CSharp 
**[[Thread]] Pool** - collection of worker threads managed by the runtime. It reuses threads for multiple tasks, reducing overhead (creating & destroying threads repeatedly consumes resources).
```csharp
class Program {
    static void Print(object msg) { Console.WriteLine("ThreadPool: " + msg); }

    static void Main() {
        ThreadPool.QueueUserWorkItem(Print, "Task 1");
        ThreadPool.QueueUserWorkItem(Print, "Task 2");
    }
}
```
![[Pasted image 20251014090220.png]]