#SoftDev #OS #CSharp 
**Task Parallel Library (TPL)** is a collection of APIs that provides $>$ control over parallel & async programming. TPL simplifies [[Multithreading]] by managing [[Thread]] scheduling & exec efficiently. It includes features like data parallelism, task scheduling, & supportive cancellation. TPL provides a higher level of abstraction over traditional threading mechanisms, making it easier to write, read, & maintain parallel code.
- TPL reduces CPU overheating in long-running tasks.
- It makes [[Thread]] Management easier & doesn't worry about race conditions.
- Provide an efficient way to cancel a task co-operatively through `CancellationToken`.
- TPL is used for its better CPU utilization, enhancing performance, & is a better alternative to the traditional [[Thread]] class.
```csharp
static void Main() {
	Task task1 = Task.Run(() => Printnums());
    Task task2 = Task.Run(() => Printnums());
	Task.WhenAll(task1, task2).Wait();
}

static void Printnums() {
    for (int i = 0; i < 3; i++) Console.WriteLine(i);
} //res 0 1 2 0 1 2 simultaneously
```