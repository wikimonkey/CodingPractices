# C# Coding Rules

This document outlines coding guidelines for an ASP .NET Core service. AI agents should follow these guidelines when generating or reviewing code written in C# language. 
No need to mention the guidelines in comments while generating any code.

1.  **Error Handling & Exceptions**:
    *   Throw specific exception types (e.g., `ArgumentNullException`, `InvalidOperationException`, custom exceptions like `TenantNotFoundException`, `AzconfigException`) rather than generic `System.Exception`.
    *   Catch specific exceptions you expect and can handle. Never catch generic `System.Exception`.
    *   Use `try-finally` to ensure cleanup code runs even if exceptions occur.
    *   Use `throw;` instead of `throw ex;` to preserve the original stack trace.
    *   Document all exceptions thrown by public methods with XML comments
    