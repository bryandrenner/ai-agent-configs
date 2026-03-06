---
name: csharp-async
description: 'Get best practices for C# async programming'
---

# C# Async Programming Best Practices

Your goal is to help me follow best practices for asynchronous programming in C#.

## Naming Conventions

- Use the 'Async' suffix for all async methods
- Match method names with their synchronous counterparts when applicable (e.g., `GetDataAsync()` for `GetData()`)

## Return Types

- Return `Task<T>` when the method returns a value
- Return `Task` when the method doesn't return a value
- Consider `ValueTask<T>` for high-performance scenarios to reduce allocations
- Avoid returning `void` for async methods except for event handlers

## Exception Handling

- Use try/catch blocks around await expressions
- Avoid swallowing exceptions in async methods
- Use `ConfigureAwait(false)` when appropriate to prevent deadlocks in library code
- Propagate exceptions with `Task.FromException()` instead of throwing in async Task returning methods

## Performance

- Use `Task.WhenAll()` for parallel execution of multiple tasks
- Use `Task.WhenAny()` for implementing timeouts or taking the first completed task
- Use `Task.WhenEach()` (.NET 9+) to process tasks as they complete, rather than waiting for all
- Avoid unnecessary async/await when simply passing through task results
- Consider cancellation tokens for long-running operations
- Compose cancellation tokens with `CancellationTokenSource.CreateLinkedTokenSource()` for layered timeout scenarios

## Common Pitfalls

- Never use `.Wait()`, `.Result`, or `.GetAwaiter().GetResult()` in async code
- Avoid mixing blocking and async code
- Don't create async void methods (except for event handlers)
- Always await Task-returning methods

## ConfigureAwait Guidance

- In ASP.NET Core, `ConfigureAwait(false)` has no effect because there is no `SynchronizationContext`; omit it in application code for clarity
- In library code that may run outside ASP.NET Core (WPF, WinForms, legacy ASP.NET), use `ConfigureAwait(false)` to avoid deadlocks
- When in doubt about the consumer, use `ConfigureAwait(false)` in shared libraries

## Async Streams

- Use `IAsyncEnumerable<T>` for producing sequences of values asynchronously
- Accept `[EnumeratorCancellation] CancellationToken` in async iterator methods
- Consume with `await foreach` and pass cancellation via `.WithCancellation(token)`
- Prefer async streams over returning `Task<List<T>>` when results can be yielded incrementally

## Implementation Patterns

- Implement the async command pattern for long-running operations
- Use async streams (IAsyncEnumerable<T>) for processing sequences asynchronously
- Consider the task-based asynchronous pattern (TAP) for public APIs

When reviewing my C# code, identify these issues and suggest improvements that follow these best practices.

<!-- Source: https://github.com/github/awesome-copilot/blob/main/skills/csharp-async/SKILL.md -->
