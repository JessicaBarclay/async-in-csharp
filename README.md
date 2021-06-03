# async-in-csharp-pluralsight

Exercises and notes on Asynchronous Programming from the PluralSight course [Applying Asynchronous Programming in C#](https://app.pluralsight.com/library/courses/applying-asynchronous-programming-c-sharp/table-of-contents)

## What is asynchronous programming(in C#/.NET), and when to use it?

> Asynchronous: not having to wait for one task to finish before starting another.

Asynchronous programming allows an application to run a unit of work separately from the main application thread, allowing tasks to run in parallel. When used correctly, applications can benefit from improved performance and responsiveness.

### Async pattern in .NET

The recommended asynchronous pattern to follow in modern .NET is the [Task-based Asynchronous Programming pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) - otherwise known as TAP.

#### `Task` and `Task<T>`

> Tasks are constructs used to implement what is known as the [Promise Model of Concurrency](https://en.wikipedia.org/wiki/Futures_and_promises). In short, they offer you a "promise" that work will be completed at a later point, letting you coordinate with the promise with a clean API.

* Task represents a single operation that does not return a value.
* Task<T> represents a single operation that returns a value of type T.

#### Naming

Asynchronous methods in TAP include the *Async* suffix after the operation name for methods that return awaitable types, for example, an asynchronous Get operation that returns a Task<String> can be named GetAsync.

The parameters of a TAP method should match the parameters of its synchronous counterpart and should be provided in the same order. However, out and ref parameters are exempt from this rule and should be avoided entirely.

#### Exceptions

When an exception occurs in an async method that has a return type of `Task` or `Task<TResult>`, the exception object is wrapped in an instance of AggregateException and attached to the Task object. If multiple exceptions are thrown, all of them are stored in the Task object.

### I/O vs CPU bounded work

**CPU Bound** The rate at which an application or process progresses is limited by the speed of the CPU. A typical example of a CPU Bound program is one which includes processes that performs lots of calculations. Example program: [Perform a calculation for a game](https://docs.microsoft.com/en-us/dotnet/csharp/async#cpu-bound-example-perform-a-calculation-for-a-game)

**I/O Bound** A task that processes data, from disk or via a network, is likely to be I/O bound(input/output). Example program: [Download data from a web service](https://docs.microsoft.com/en-us/dotnet/csharp/async#io-bound-example-download-data-from-a-web-service)

When optimizing a program, it is good to know if you are CPU bound or I/O bound(memory bound or cache bound), to know where to optimize.
