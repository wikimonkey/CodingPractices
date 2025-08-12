# C# Coding Rules

This document outlines coding guidelines for an ASP .NET Core service. AI agents should follow these guidelines when generating or reviewing code written in C# language. 
No need to mention the guidelines in comments while generating any code.

1.  **Error Handling & Exceptions**:
    *   Throw specific exception types (e.g., `ArgumentNullException`, `InvalidOperationException`, custom exceptions like `TenantNotFoundException`, `AzconfigException`) rather than generic `System.Exception`.
    *   Catch specific exceptions you expect and can handle. Never catch generic `System.Exception`.
    *   Use `try-finally` to ensure cleanup code runs even if exceptions occur.
    *   Use `throw;` instead of `throw ex;` to preserve the original stack trace.
    *   Document all exceptions thrown by public methods with XML comments

2.  **Asynchronous Programming (`async`/`await`)**:
    *   **CancellationToken:** All `async Task` or `async ValueTask` methods **must** accept a `CancellationToken` as the last parameter.
        *   Example: `public async Task<ResultType> DoSomethingAsync(RequestData data, CancellationToken cancellationToken)`
        *   Provide a default value (`= default`) only if cancellation is genuinely optional (e.g., simple helpers, test utilities). Controller actions typically receive the token via model binding from the framework.
        *   Pass the `cancellationToken` down the call stack to all subsequent asynchronous operations (database calls, HTTP requests, etc.).
    *   **Return Types:** Prefer `Task<T>` or `Task` for asynchronous methods. Use `ValueTask<T>` or `ValueTask` only when the method is expected to complete synchronously frequently, to avoid heap allocation in those cases.

3.  **Naming Conventions**:
    *   Follow standard C# naming conventions:
        *   `PascalCase` for classes, interfaces, enums, methods, properties, events, constants, and static readonly.
        *   `camelCase` for local variables and method parameters.
    *   Prefix private fields with an underscore (`_`). Example: `private readonly ILogger _logger;`.
    *   Choose descriptive names for variables, methods, and classes.
    *   Use `const string` for logger categories or route templates where applicable.
    *   Use readonly for fields that don't change after initialization.

4.  **Null Handling**:
    *   **Argument Validation:** Rigorously validate arguments in public methods and constructors. Use explicit `if (argument == null) throw new ArgumentNullException(nameof(argument));` checks at the beginning of methods/constructors, especially for injected dependencies.
    *   **Null-Forgiving Operator (`!`):** Avoid using the null-forgiving operator (`!`) unless you are absolutely certain through prior checks or logic flow that the value cannot be null at that point. Prefer explicit null checks or null-conditional operators (`?.`).
    *   **Multiple Null Check Operator on Same Variable:** Avoid using the null check operator on the same variable twice in a code block.
        * Example:
          Below bad code
          ``` csharp
            a = obj?.A;
            b = obj?.B;
          ```
          Should be 
          ``` csharp
            if (obj != null)
            {
                a = obj.A;
                b = obj.B;
            }
          ```

5.  **Dependency Injection (DI)**:
    *   Use **constructor injection** as the primary way to obtain dependencies (e.g., `ILoggerFactory`, `IOptions<T>`, `IHttpClientFactory`, custom services like `ICosmosStorageProvider`, `ICryptoService`).
    *   Store injected dependencies in `private readonly` fields.
    *   Validate injected dependencies for null in the constructor.
    *   Register services with appropriate lifetimes (Singleton, Scoped, Transient) in `Program.cs` or `Startup.cs`.

6.  **Resource Management (`IDisposable`)**:
    *   Wrap instances of types implementing `IDisposable` (like `HttpClient`, `CosmosClient`, `CancellationTokenSource`) in `using` statements (or `await using` for `IAsyncDisposable`) to ensure proper disposal.
    *   Implement `IDisposable` or `IAsyncDisposable` correctly if your class manages unmanaged resources or holds references to disposable objects that need cleanup (e.g., `CosmosStorageProvider`).

7. **Singleton Pattern**: Implement singletons primarily through the Dependency Injection container using `services.AddSingleton<TService, TImplementation>()` or `services.AddSingleton<TService>()`. Avoid manual static singleton implementations unless there's a compelling reason and it doesn't interfere with testability.

8. **Using directives**: Always remove unused `using` statements and sort the `using` statements. System directives are not required to be listed first.