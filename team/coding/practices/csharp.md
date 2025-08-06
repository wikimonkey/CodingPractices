# C# Coding Best Practices

This document outlines coding guidelines for an ASP .NET Core service. AI agents should follow these guidelines when generating or reviewing code written in C# language. 
No need to mention the guidelines in comments while generating any code.

1.  **Asynchronous Programming (`async`/`await`)**:
    *   **CancellationToken:** All `async Task` or `async ValueTask` methods **must** accept a `CancellationToken` as the last parameter.
        *   Example: `public async Task<ResultType> DoSomethingAsync(RequestData data, CancellationToken cancellationToken)`
        *   Provide a default value (`= default`) only if cancellation is genuinely optional (e.g., simple helpers, test utilities). Controller actions typically receive the token via model binding from the framework.
        *   Pass the `cancellationToken` down the call stack to all subsequent asynchronous operations (database calls, HTTP requests, etc.).
    *   **Return Types:** Prefer `Task<T>` or `Task` for asynchronous methods. Use `ValueTask<T>` or `ValueTask` only when the method is expected to complete synchronously frequently, to avoid heap allocation in those cases.
