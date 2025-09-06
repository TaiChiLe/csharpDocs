Let’s dig into **exceptions you can use in a `try-catch` in C#**.

---

# 📌 1. General Rule

In C#, **you can catch any type that derives from `System.Exception`**.

- The base class: **`Exception`**
    
- Specialized exceptions: `ArgumentNullException`, `InvalidOperationException`, etc.
    

You **cannot** catch things like `string` or `int` — only `Exception` objects.

---

# 📌 2. Common Exception Types in C#

Here are the most frequently used ones:

### **General purpose**

- `Exception` → base type (catches everything, but usually avoided by itself)
    
- `SystemException` → base for system-generated exceptions
    

### **Argument / parameter issues**

- `ArgumentException` → invalid argument
    
- `ArgumentNullException` → argument is `null` when it shouldn’t be
    
- `ArgumentOutOfRangeException` → argument not in allowed range
    

### **Invalid operations**

- `InvalidOperationException` → method call not valid for current state
    
- `NotImplementedException` → feature/method not yet implemented
    
- `NotSupportedException` → method/property not supported
    

### **I/O and external issues**

- `IOException` → general I/O errors
    
- `FileNotFoundException` → file missing
    
- `DirectoryNotFoundException` → directory missing
    
- `UnauthorizedAccessException` → no permission
    

### **Collections / indexing**

- `IndexOutOfRangeException` → index outside array bounds
    
- `KeyNotFoundException` → dictionary key doesn’t exist
    

### **Type conversion / casting**

- `InvalidCastException` → invalid type conversion
    
- `FormatException` → string not in correct format
    
- `OverflowException` → arithmetic overflow
    

### **Concurrency / threading**

- `ThreadAbortException` → thread forced to stop
    
- `TaskCanceledException` → async task canceled
    
- `OperationCanceledException` → operation canceled (with `CancellationToken`)
    

---

# 📌 3. Example `try-catch`

```csharp
try
{
    int[] numbers = { 1, 2, 3 };
    Console.WriteLine(numbers[5]); // throws IndexOutOfRangeException
}
catch (IndexOutOfRangeException ex)
{
    Console.WriteLine("Index was out of bounds: " + ex.Message);
}
catch (Exception ex)
{
    // fallback: catches all other exceptions
    Console.WriteLine("Something went wrong: " + ex.Message);
}
```

---

# 📌 4. Best Practices

- **Catch specific exceptions first**, then `Exception` last as a fallback.
    
- Avoid catching `Exception` unless you need a _global safety net_.
    
- Use **`finally`** for cleanup (closing files, releasing resources).
    

---

✅ **In short:** You can `catch` **any class derived from `System.Exception`**, and C# provides many built-in exceptions for common error scenarios.

---
