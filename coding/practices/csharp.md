# C# Coding Best Practices

This document outlines coding guidelines for an ASP .NET Core service. AI agents should follow these guidelines when generating or reviewing code written in C# language. 
No need to mention the guidelines in comments while generating any code.

1.  **Type Inference (`var`)**:
    *   Use `var` when the type of the variable is **obvious** from the right-hand side of the assignment (e.g., `var user = new User();` or `var items = new List<string>();`).
    *   Use **explicit type names** when the type is not immediately apparent, improves readability, or is required by the language (e.g., fields, properties, method parameters, return types). Example: `IEnumerable<User> activeUsers = GetActiveUsers();`.

2.  **LINQ and Collections**:
    *   Prefer simple, readable LINQ queries.
    *   Break down complex LINQ queries involving multiple non-trivial operations (e.g., multiple `Where`, `Select`, `GroupBy`, `OrderBy` clauses) into separate statements using intermediate variables if it significantly improves readability.
    *   Avoid excessively long or deeply nested LINQ chains that are difficult to parse mentally.
    *   Use LINQ methods like `.FirstOrDefault()`, `.Any()`, `.Single()`, `.Count()` where appropriate for clarity.
    *   Use collection interfaces (IList<T>, IReadOnlyList<T>, etc.) in parameter and return types
        * Use the minimal required interface; avoid interfaces with unnecessary capabilities (e.g., prefer `IReadOnlyList<T>` over `IList<T>` if modification isn't needed; If only enumeration is required then IEnumerable<T> should be used).
    *   Prefer collection initialization syntax when appropriate
    *   Avoid unnecessary `ToList` calls to avoid unnecessary allocations 

3.  **Code Structure & Readability**:
    *   Keep methods concise and focused on a single responsibility (preferably under 30 lines).
    *   Limit method parameters (preferably 3 or fewer, use parameter objects for more).
    *   Add comments to explain *why* something is done, not *what* it does, especially for complex logic, workarounds, or non-obvious decisions.
    *   Use `#region` sparingly, if at all, primarily for organizing large files or generated code.
    *   Use separate files for distinct classes and interfaces.

4. **Performance Considerations**:
    *   **`StringBuilder`**: Prefer using `System.Text.StringBuilder` for concatenating multiple strings, especially within loops or complex conditional logic, to avoid excessive string allocations. Initialize `StringBuilder` with an estimated capacity when possible (e.g., `new StringBuilder(128)`).
    *   **`Span<T>` and `ReadOnlySpan<T>`**: Utilize `Span<T>` and `ReadOnlySpan<T>` for high-performance scenarios involving parsing, formatting, or manipulating sequences of data (like strings or byte arrays) without allocating new memory, where applicable.

5. **String Manipulation**:
    *   **Joining**: Use `string.Join()` for simple concatenation of collection elements. The delimiter character depends on the context (e.g., `,` is used for joining key IDs in logging). Use `StringBuilder.Append()` or `StringBuilder.AppendLine()` for more complex multi-part string construction.
    *   **Constants**: Define constants (`const string`) or static readonly fields for recurring strings like logger categories, route templates, header names, policy names, error messages, claim types, etc., instead of using magic strings or numbers directly in the code. This improves maintainability and reduces errors. 
