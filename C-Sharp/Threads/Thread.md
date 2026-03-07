#OS #SoftDev #CSharp 
**Thread** is the smallest unit of exec in a program. Every C# program starts with a main thread created automatically by the runtime. It is possible to create additional threads to run tasks in parallel with the main thread.
![[Pasted image 20251014090810.png]]![[Pasted image 20251014090829.png]]
![[Pasted image 20251014090902.png]]
![[Pasted image 20251014090915.png]]
Thread object is created with a target method, `Start()` begins exec in parallel with the main thread:
```csharp
class Program {
    static void Printnums() {
        for (int i = 1; i <= 5; i++) {
            Console.WriteLine("Worker Thread: " + i);
            Thread.Sleep(500); // Simulates work
        }
    }

    static void Main() {
        Thread t1 = new Thread(Printnums); 
        t1.Start(); // Start the worker thread
        for (int i = 1; i <= 5; i++) {
            Console.WriteLine("Main Thread: " + i);
            Thread.Sleep(500);
        }
    }
}
```
![[Pasted image 20251014085047.png]]

`ParameterizedThreadStart` allows to pass params to threads:
```csharp
class Program {
    static void PrintMessage(object msg) {
        Console.WriteLine("Message: " + msg);
    }

    static void Main() {
        Thread t = new Thread(PrintMessage);
        t.Start("Hello from Thread");
    }
} //Message: Hello from Thread
```

Instead of separate methods, it is possible to define thread [[Logic]] inline:
```csharp
Thread t = new Thread(() => { 
	for (int i = 1; i <= 3; i++) 
	Console.WriteLine("Lambda Thread: " + i);
});
t.Start();
```
![[Pasted image 20251014085519.png]]

**Foreground Threads**: Keep running until they finish, even if the main thread ends.
**Background Threads**: Terminate when all foreground threads end.
```csharp
Thread t = new Thread(() => Console.WriteLine("Background thread running"));
t.IsBackground = true;
t.Start();
```
If the main thread ends before the background thread completes, the background thread is terminated.