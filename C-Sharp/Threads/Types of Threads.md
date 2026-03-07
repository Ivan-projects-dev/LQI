#OS #SoftDev #CSharp 
 When an app starts, it automatically creates a foreground [[Thread]] (the main $1$). All the threads are background threads by default. We can change the type of [[Thread]] by setting the `Background` property of the [[Thread]].
 
 **Foreground [[Thread]]** continues to exec even after the main [[Thread]] completes its exec. It ensures that the app does not terminate until all foreground threads have completed their assigned tasks. Foreground threads are critical when a task must complete before the app exits.
	- [[Thread]] continues exec until its work is finished, & only then will the program exit. App will wait for the foreground [[Thread]] to complete before exiting.
	- Foreground threads can be started/paused/stopped explicitly.
	- Useful when we need to perform a task that is critical to the operation of the app & must be completed before the app can terminate (update UI/perform important calcs).

**Background [[Thread]]** is terminated as soon as the main [[Thread]] finishes its exec (lifespan of a background [[Thread]] depends on the main [[Thread]]). These threads are used for non-critical tasks that can be abandoned when the app shuts down.
	- If app finishes exec, background threads will be killed automatically, even if they are still running. 
	- App will close once all user threads have been completed, even if there are still background threads running.