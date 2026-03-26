#OS #SoftDev #CSharp 
1. `Abort()` method throws a `ThreadAbortException` to the [[Thread]] in which it is called, forcing the [[Thread]] to terminate. However, this method is deprecated due to unpredictable behavior & is no longer recommended for use in modern versions of .NET.
	`public void Abort();`
	- **SecurityException**: If the caller does not have the required permission.
	- **ThreadStateException**: If the [[Thread]] that is being aborted is currently suspended.
2. **Abort Object**. This method raises a `ThreadAbortException` in the [[Thread]] on which it is invoked, to begin the process of terminating the [[Thread]] while also providing exception info about the [[Thread]] termination. Not recommended to use.
	`public void Abort(object info);`
3.  `CancellationToken` allows us to politely request that a [[Thread]] should stop its work. It is recommended to use in newer versions. We need to create a `CancellationTokenSource` object & pass its token to the [[Thread]].
```csharp
CancellationTokenSource tokenSource = new(); 
Thread workerThread = new(() => LongRunningOperation(tokenSource.Token));
if (token.IsCancellationRequested)
	return; // Perform cleanup & exit
tokenSource.Cancel();
```
4. **Manual termination**:
```csharp
private volatile bool stopRequested; // Flag to signal thread termination

public void DoWork()
{
	while (!stopRequested) // Check flag periodically
	{ // Thread work here
		Console.WriteLine("Working...");
		Thread.Sleep(100); // Simulate work  
	}
}

public void RequestStop() => stopRequested = true;
```

- **Deadlock** can occur if the [[Thread]] that calls `Abort` methods holds a lock that the aborted [[Thread]] requires.
- If the `Abort` method is called on a [[Thread]] which has not been started, then that [[Thread]] will abort when `Start` is called.
- If the `Abort` method is called on a [[Thread]] which is blocked or is sleeping then the [[Thread]] will get interrupted & after that get aborted.
- Calling `Abort` on a suspended [[Thread]] throws a `ThreadStateException` in the calling [[Thread]] & adds `AbortRequested` to the state of the aborted [[Thread]].
- `ThreadAbortException` is not thrown in the suspended [[Thread]] until Resume is called.
- If the `Abort` method is called on a Managed [[Thread]] which is currently exec unmanaged code then a `ThreadAbortException` is not thrown until the [[Thread]] returns to managed code.
- If $2$ calls to `Abort` come at the same time then it is possible for $1$ call to set the state info & the other call to exec the `Abort`. However,  app cannot detect this situation.
- After `Abort` is invoked on a [[Thread]], its state shows `AbortRequested`. Once terminated due to `Abort`, the state switches to `Stopped`. If permitted, the target [[Thread]] can cancel the abort with `ResetAbort`.