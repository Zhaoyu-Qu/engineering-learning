
# Asynchronous Programming Notes

## C# and .NET

### Awaitables
An awaitable is any object that can be awaited using the `await` keyword.

#### Task
In .NET, `Task` and `Task<T>` are the most common types of awaitable objects. These are awaitable because they implement the required pattern (they have a `GetAwaiter()` method). Other types can also be awaitable if they implement the same pattern (e.g., `ValueTask`, custom awaitables).

A `Task` (or awaitable object) is not something you execute directly; it represents an operation that is already running or scheduled to run. The `Task` is created as a result of starting the asynchronous operation. You use `await` to asynchronously wait for its completion, but the `Task` itself is generated when the operation begins, not when you await it. If you do not use `await` (or otherwise observe the `Task`), the operation still runs, but you won't be notified when it completes, nor will you be able to get its result or handle exceptions. Awaiting the `Task` simply allows you to asynchronously wait for its completion and retrieve the result or exception.

### Async Methods and Await
To wait asynchronously for an operation to finish, use the `await` keyword.

To use `await` inside a method, the method must be marked with the `async` keyword in its signature.

An async method always returns an awaitable object. Even if you return a regular object, the async method will still wrap it in an awaitable object for consistency. In this case, the task will be a completed `Task`, and anyone examining the `Task` will see that it is already finished and the result is immediately available. This is a common pattern for synchronous results in async APIs: the consumer just sees a `Task` that is already completed with the result.

---

*More sections for other programming languages can be added below.*