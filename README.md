# async-in-csharp-pluralsight

Exercises and notes on Asynchronous Programming from the PluralSight course [Applying Asynchronous Programming in C#](https://app.pluralsight.com/library/courses/applying-asynchronous-programming-c-sharp/table-of-contents)

## What is asynchronous programming(in C#/.NET), and when to use it?

> Asynchronous: not having to wait for one task to finish before starting another.

Asynchronous programming allows an application to run a unit of work separately from the main application thread, allowing tasks to run in parallel. When used correctly, applications can benefit from improved performance and responsiveness.

### Async pattern in .NET

The recommended asynchronous pattern to follow in modern .NET is the [Task-based Asynchronous Programming pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) - otherwise known as TAP.

#### `Task` and `Task<T>`

> Tasks are constructs used to implement what is known as the [Promise Model of Concurrency](https://en.wikipedia.org/wiki/Futures_and_promises). In short, they offer you a "promise" that work will be completed at a later point, letting you coordinate with the promise with a clean API.

* `Task` represents a single operation that does not return a value.
* `Task<T>` represents a single operation that returns a value of type `T`.

Word of warning - calling `Result` or `Wait` may cause a deadlock to your application.

#### `async` and `await`
You can use the `await` keyword in C# to asynchronously await `Task` and `Task<T>` objects. When you're awaiting a `Task`, the await expression is of type `void`. When you're awaiting a `Task<T>`, the `await` expression is of type `T`. An `await` expression must occur inside the body of an asynchronous method, marked with the `async` keyword in the method signature.

* The `async` keyword turns a method into an `async` method, which allows you to use the `await` keyword in its body.
* When the `await` keyword is applied, it suspends the calling method and yields control back to its caller until the awaited task is complete.
* `await` can only be used inside an `async` method.

Use `await` with a `Task` or `Task<T>` to retrieve results and validate that it completed successfully.

Continuation
The `await` keyword introduces a continuation - allowing you to get back to the original context, or thread.
Code *after* an await statement, will be executed once a Task completes successfully - then you will be back on the continuation.

NOTE: `async void` is evil. Unless used in the scenario where EventHandlers are called. Otherwise AVOID.

#### Naming

Asynchronous methods in TAP include the `async` suffix after the operation name for methods that return awaitable types, for example, an asynchronous `Get` operation that returns a `Task<String>` can be named `GetAsync`.

The parameters of a TAP method should match the parameters of its synchronous counterpart and should be provided in the same order. However, `out` and `ref` parameters are exempt from this rule and should be avoided entirely.

#### Exceptions

When an exception occurs in an async method that has a return type of `Task` or `Task<T>`, the exception object is wrapped in an instance of `AggregateException` and attached to the `Task` object. If multiple exceptions are thrown, all of them are stored in the `Task` object.

### I/O vs CPU bounded work

**CPU Bound** The rate at which an application or process progresses is limited by the speed of the CPU. A typical example of a CPU Bound program is one which includes processes that performs lots of calculations. Example program: [Perform a calculation for a game](https://docs.microsoft.com/en-us/dotnet/csharp/async#cpu-bound-example-perform-a-calculation-for-a-game)

**I/O Bound** A task that processes data, from disk or via a network, is likely to be I/O bound(input/output). Example program: [Download data from a web service](https://docs.microsoft.com/en-us/dotnet/csharp/async#io-bound-example-download-data-from-a-web-service)

**Two questions you should ask before you write any asynchronous code**

NOTE: Taken from the Microsoft docs https://docs.microsoft.com/en-us/dotnet/csharp/async#recognize-cpu-bound-and-io-bound-work

When optimizing a program, it is good to know if you are CPU bound or I/O bound, to know where to optimize.

* *Will your code be "waiting" for something, such as data from a database?*

If your answer is "yes", then your work is I/O-bound.

* *Will your code be performing an expensive computation?*

If you answered "yes", then your work is CPU-bound.

If the work you have is I/O-bound, use async and await without Task.Run. You should not use the Task Parallel Library. The reason for this is outlined in [Async in Depth](https://docs.microsoft.com/en-us/dotnet/standard/async-in-depth).

If the work you have is CPU-bound and you care about responsiveness, use async and await, but spawn off the work on another thread with Task.Run. If the work is appropriate for concurrency and parallelism, also consider using the [Task Parallel Library](https://docs.microsoft.com/en-us/dotnet/standard/parallel-programming/task-parallel-library-tpl).


### Async vs Parallel vs Concurrent

**Asynchronous** 
Async principles are suitable for I/O operations. With an asynchronous operation, you subscribe to when that operation completes.

Async describes how individual threads are used. 

**Parallelism** 
Parallel programming makes use of as much CPU as possible. The Task Parallel library in .NET allows asynchronous as well as parallel programming, you can also combine the two. Parallelism is great for performing x amount of the same task, dividing the work between multiple threads.

**Concurrency**
In the context of .NET, concurrency is almost always associated with execution on simultaneous threads. Different threads, doing different things at the same time. An example - a web app receives a request, begins processing that request, then another requests comes in and this new request begins to be processed on another thread.