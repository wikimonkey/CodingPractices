# C# Coding Rules

This document outlines coding guidelines for an ASP .NET Core service. AI agents should follow these guidelines when generating or reviewing code written in C# language. 
No need to mention the guidelines in comments while generating any code.

1.  **Error Handling & Exceptions**:
    *   Throw specific exception types (e.g., `ArgumentNullException`, `InvalidOperationException`, custom exceptions like `TenantNotFoundException`, `AzconfigException`) rather than generic `System.Exception`.
    *   Catch specific exceptions you expect and can handle. Never catch generic `System.Exception`.
    *   Use `try-finally` to ensure cleanup code runs even if exceptions occur.
    *   Use `throw;` instead of `throw ex;` to preserve the original stack trace.
    *   Document all exceptions thrown by public methods with XML comments


2.  **Naming Conventions**:
    *   Follow standard C# naming conventions:
        *   `PascalCase` for classes, interfaces, enums, methods, properties, events, constants, and static readonly.
        *   `camelCase` for local variables and method parameters.
    *   Prefix private fields with an underscore (`_`). Example: `private readonly ILogger _logger;`.
    *   Choose descriptive names for variables, methods, and classes.
    *   Use `const string` for logger categories or route templates where applicable.
    *   Use readonly for fields that don't change after initialization.