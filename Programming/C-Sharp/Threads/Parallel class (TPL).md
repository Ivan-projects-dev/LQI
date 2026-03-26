#OS #CSharp #SoftDev 
**Parallel class** is a part of [[TPL]], provides various looping methods to achieve parallelism & It is an easier way to iterate through the parallel programs. It has powerful abstract methods which are used to perform different operations on parallel tasks. These are the important iterating methods that are mainly used in the parallel class.

`Parallel.For()` used to exec the loops in parallel. It is an efficient way to iterate through the tasks which work independently, & we can use this loop to iterate concurrently which enhances the computational speed & reduce the overhead. Divides the loop iterations into separate threads & runs concurrently.

`Parallel.For(int fromInclusive, int toExclusive, Action<int> body)`
	`fromInclusive`: The starting index of the loop.
	`toExclusive`: The exclusive end index of the loop (one past the last index).
	`body`: We need to pass the `Action<int>` delegate which is the action to be performed.
```csharp
class Geeks {
    static void Main() {
        List<string> data = new List<string> { "Hello", "Geek", "How", "Are", "You" };
    Parallel.ForEach(data, ele => {
        Console.WriteLine($"Data => {ele} on thread:- {System.Threading.Thread.CurrentThread.ManagedThreadId}");
        });
    }
}
```
Given loop separates data in the list in small chunks & runs it on a different [[Thread]].

`Parallel.ForEach()`  also used to iterate parallel loops. It works directly on the collection & we don't need to specify the start & end index instead we just simply pass the collection in the argument directly.
`ForEach<TSource>(IEnumerable<TSource> source, Action<TSource> body)`
	`collection`: We can pass any collection like array, list or set which we want to iterate.
	`body`: An `Action<T>` delegate that defines the work to be done for each item in the collection.

`Parallel.Invoke()` used to run multiple methods or any action concurrently, doesn't require to creation of a loop. We can use this method when we have multiple actions to perform & they are not dependent on each other & can be exec parallelly. It runs the actions at the same time which enhances the performance.
`Parallel.Invoke(Action[] actions);` //actions - tasks to be exec in parallel