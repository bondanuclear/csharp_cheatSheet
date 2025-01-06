# Unity Awaitables:
### We use them when we want to take into consideration destroying of gameObject, stopping the game time.
### This approach is modern, readable, and integrates well with Unity's main thread operations. Unlike coroutines, async/await uses C#'s asynchronous programming model, which is more versatile for complex operations like waiting for tasks, handling file I/O, or networking. 
### Good candidates for using Unity's Awaitable are any synchronous operations that are likely to cause bottlenecks, like 
- graph algorithms;
- algorithms with cubic time complexity;
- image manipulation tasks;
- time consuming I/O operations.

#### ! Never use Task.WaitAll, because its synchronous and it will completely freeze the game.
</br>

### Async/Await also doesn't work in WebGL, but Awaitable has fixed this problem.
<img width="623" alt="image" src="https://github.com/user-attachments/assets/3fa3ab80-a8ef-446a-9e0b-12a83f1d1eaa" />
### We can also use Awaitable with all Unity functions that return AsyncOperation (Resource functions)
