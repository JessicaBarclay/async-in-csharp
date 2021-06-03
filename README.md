# async-in-csharp-pluralsight

Exercises and notes on Asynchronous Programming from the PluralSight course [Applying Asynchronous Programming in C#](https://app.pluralsight.com/library/courses/applying-asynchronous-programming-c-sharp/table-of-contents)

## What is asynchronous programming(in C#/.NET), and when to use it?

> Asynchronous: not having to wait for one task to finish before starting another.

Asynchronous programming allows an application to run a unit of work separately from the main application thread, allowing tasks to run in parallel. When used correctly, applications can benefit from improved performance and responsiveness.

### Async pattern in .NET

The recommended asynchronous pattern to follow in modern .NET is the [Task-based Asynchronous Programming pattern](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap) - otherwise known as TAP.

#### Naming

Asynchronous methods in TAP include the *Async* suffix after the operation name for methods that return awaitable types, for example, an asynchronous Get operation that returns a Task<String> can be named GetAsync.

The parameters of a TAP method should match the parameters of its synchronous counterpart and should be provided in the same order. However, out and ref parameters are exempt from this rule and should be avoided entirely.

#### Exceptions

When an exception occurs in an async method that has a return type of `Task` or `Task<TResult>`, the exception object is wrapped in an instance of AggregateException and attached to the Task object. If multiple exceptions are thrown, all of them are stored in the Task object.