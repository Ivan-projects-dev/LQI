#SoftDev #CSharp #OS 
When multiple threads access & modify shared data, issues like **race conditions** (simultaneous edit) can occur. For this purpose **sync mechanisms**.
1. **Synchronized blocks** are written using the `lock` keyword on a specific object. Only $1$ [[Thread]] can enter the block at a time. Other threads are blocked until the lock is released.
```csharp
lock(sync_object) { 
	// Access shared resources
}
```
2. **Monitor class** provides a $>$ flexible way of sync threads. It works similarly to lock but offers additional control like `Wait()`, `Pulse()`, & `PulseAll()`.
```csharp
private static int count = 0;
private static readonly object sync = new object();

static void Increment() {
    for (int i = 0; i < 3; i++) {
	    Monitor.Enter(sync);
        try {
            count++;
            Console.WriteLine($"Count = {count}");
        }
        finally {
            Monitor.Exit(sync);
        }
    }
}
```
2. [[Mutex]] is used to sync threads across multiple processes. Unlike `lock`, it works across different apps.
```csharp
private static Mutex mutex = new Mutex();

static void Printnums() {
    mutex.WaitOne();
    for (int i = 1; i <= 3; i++) {
        Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} -> {i}");
        Thread.Sleep(200);
    }
    mutex.ReleaseMutex();
}
```
2. **[[Semaphore]]** controls access to a resource by allowing a specified num of threads to enter at once.
```csharp
private static Semaphore semaphore = new Semaphore(2, 2);

static void Work() {
	semaphore.WaitOne();
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} entered");
    Thread.Sleep(500);
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} exiting");
    semaphore.Release();
} //max 2 threads can work simultaneously. Others wait until a slot is released.
```
5. `SemaphoreSlim` is a lightweight alternative to [[Semaphore]] & is recommended for sync within a single process. It supports async programming & is $>$ efficient when inter-process sync is not required.
```csharp
private static SemaphoreSlim sem = new SemaphoreSlim(2, 2);

static async Task Work() {
	await sem.WaitAsync();
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} entered");
    await Task.Delay(500);
    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} exiting");
    sem.Release();
}
```