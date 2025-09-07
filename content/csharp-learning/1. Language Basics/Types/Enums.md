Let’s dive into **enums in C#**.

---

# 📌 1. What is an Enum?

- **Enum** (short for _enumeration_) is a **special value type** that represents a **set of named constants**.
    
- It improves **readability** and **type safety** compared to using plain integers or strings.
    

---

# 📌 2. Basic Syntax

```csharp
public enum Day
{
    Sunday,
    Monday,
    Tuesday,
    Wednesday,
    Thursday,
    Friday,
    Saturday
}
```

- By default, the **underlying type is `int`**.
    
- The first value (`Sunday`) is 0, next (`Monday`) is 1, and so on.
    

---

# 📌 3. Using Enums

```csharp
class Program
{
    static void Main()
    {
        Day today = Day.Wednesday;

        Console.WriteLine(today);         // Output: Wednesday
        Console.WriteLine((int)today);    // Output: 3
    }
}
```

- You can **cast enum values to int** to see the underlying number.
    
- You can also cast **integers to enums**:
    

```csharp
Day someDay = (Day)5;
Console.WriteLine(someDay); // Output: Friday
```

---

# 📌 4. Assigning Custom Values

```csharp
public enum ErrorCode
{
    None = 0,
    NotFound = 404,
    ServerError = 500,
    Unauthorized = 401
}
```

- Each constant can have an explicit value.
    
- Useful for mapping to **status codes, flags, or other numeric identifiers**.
    

---

# 📌 5. Enum Underlying Types

- You can specify the underlying type (must be integral):
    

```csharp
public enum ByteEnum : byte
{
    A = 1,
    B = 2
}
```

- Default is `int`.
    
- Other allowed types: `byte`, `sbyte`, `short`, `ushort`, `int`, `uint`, `long`, `ulong`.
    

---

# 📌 6. Flags Enums

- For combinations of values (bitwise operations), use `[Flags]` attribute:
    

```csharp
[Flags]
public enum FileAccess
{
    Read = 1,
    Write = 2,
    Execute = 4
}

class Program
{
    static void Main()
    {
        FileAccess permissions = FileAccess.Read | FileAccess.Write;
        Console.WriteLine(permissions); // Output: Read, Write

        bool canWrite = (permissions & FileAccess.Write) == FileAccess.Write;
        Console.WriteLine(canWrite);    // Output: True
    }
}
```

- `[Flags]` makes the enum printable as a **combination of names** instead of a number.
    

---

# 📌 7. Summary

|Feature|Notes|
|---|---|
|Enum|Named constants|
|Default type|int|
|Custom underlying type|byte, long, etc.|
|Custom values|Yes, assign manually|
|Flags|Combine multiple values with bitwise operators|

---

✅ **In short:**  
Enums make your code **more readable, maintainable, and type-safe** when working with fixed sets of values.  
Use `[Flags]` when you need **combinations of values**.

---

If you want, I can also create a **cheat sheet comparing Enums vs Const vs Static readonly** in C# — it helps decide when to use which.

Do you want me to make that?