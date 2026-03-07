#SoftDev #CSharp #OS
Allow devs to write code that performs non-Blocking Operations, such as file I/O, network calls or DB queries, without freezing the main [[Thread]].
	`Async` modifier is applied to a method, lambda or anonymous func.
	`Await` keyword is used inside an async method to pause exec until the awaited task is complete.
	`Async` method usually returns `Task, Task<TResult>` or `void` (only for event handlers).
	Exec does not block the calling thread while awaiting a task.

1. When await is encountered (inside async method), the control is returned to the caller until the awaited task finishes.
2. After completion, exec resumes from the point where it was paused.

`await Task.Delay(time);` simulates a non-blocking pause without freezing the program

1. An async method must have at least one await inside, otherwise, it exec sync.
2. Use `async Task` for methods that perform async operations without returning a result.
3. Use `async Task<TResult>` for methods that return a result.
4. Avoid `async void` except in event handlers.
5. Avoid blocking calls like `.Result` or `.Wait()` on tasks as they may cause deadlock.