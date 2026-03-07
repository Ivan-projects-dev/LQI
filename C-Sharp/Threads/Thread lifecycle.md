#OS #SoftDev #CSharp 
![[Pasted image 20251014094055.png]]
1. **Unstarted state**: When an instance of a [[Thread]] class is created, it is unstarted, which means the `Start()` method is not called.
2. **Runnable State**: [[Thread]] might be running or ready to run at any instant of time (`Start()` was called). It is the responsibility of the [[Thread]] scheduler to give the [[Thread]], time to run. 
3. **Running State**: [[Thread]] moves to the `Running` state after calling `Start()` & when the CPU scheduler assigns exec time.
4. **WaitSleepJoin State**: A [[Thread]] enters this state when:
- [[Thread]]`.Sleep(milliseconds)` is called.
- [[Thread]]`.Join()` is called, making another [[Thread]] wait for completion.
- It is waiting for lock or I/O operation.
1. **Dead State**: When the [[Thread]] completes its task, the [[Thread]] enters into dead, terminates, abort state.
![[Pasted image 20251014094120.png]]