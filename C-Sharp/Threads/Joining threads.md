#OS #CSharp #SoftDev 
In C#, multiple threads can be created & run concurrently to perform parallel tasks. Sometimes it is necessary to wait for one [[Thread]] to finish its exec before proceeding or for all threads to complete before continuing further. 

[[Thread]]`.Join()` method waits for the [[Thread]] to complete its exec before continuing with the exec of the calling [[Thread]]. This method blocks the calling [[Thread]] until the target [[Thread]] has completed its exec.
```csharp
public void Join(); //blocks calling thread until target thread terminates
public bool Join(int millisecondsTimeout);
public bool Join(TimeSpan timeout);
```
If $t$ is a [[Thread]] object, `t.Join()` causes the current [[Thread]] to pause its exec until the [[Thread]] represented by $t$ terminates.