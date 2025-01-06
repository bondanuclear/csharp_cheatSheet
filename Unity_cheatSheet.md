# Unity Awaitables:
### We use them when we want to take into consideration destroying of gameObject, stopping the game time.
### This approach is modern, readable, and integrates well with Unity's main thread operations. Unlike coroutines, async/await uses C#'s asynchronous programming model, which is more versatile for complex operations like waiting for tasks, handling file I/O, or networking. 
### Good candidates for using Unity's Awaitable are any synchronous operations that are likely to cause bottlenecks, like 
- graph algorithms;
- algorithms with cubic time complexity;
- image manipulation tasks;
- time consuming I/O operations.
