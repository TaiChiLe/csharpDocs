In C#, **built-in types** are the **primitive data types** that the C# language provides out-of-the-box. They are really just **aliases for .NET Framework types** in the `System` namespace, but the C# keywords make them easier to use.

---

# 📌 1. Categories of Built-in Types

### **Integral Types**

- Whole numbers (signed and unsigned).
    

|C# Keyword|.NET Type|Size|Range|
|---|---|---|---|
|`sbyte`|`System.SByte`|8-bit|-128 to 127|
|`byte`|`System.Byte`|8-bit|0 to 255|
|`short`|`System.Int16`|16-bit|-32,768 to 32,767|
|`ushort`|`System.UInt16`|16-bit|0 to 65,535|
|`int`|`System.Int32`|32-bit|-2,147,483,648 to 2,147,483,647|
|`uint`|`System.UInt32`|32-bit|0 to 4,294,967,295|
|`long`|`System.Int64`|64-bit|-9.22e18 to 9.22e18|
|`ulong`|`System.UInt64`|64-bit|0 to 1.84e19|
|`char`|`System.Char`|16-bit|Single Unicode character|

---

### **Floating-point & Decimal Types**

- For real numbers and precise decimal calculations.
    

|C# Keyword|.NET Type|Size|Range / Precision|
|---|---|---|---|
|`float`|`System.Single`|32-bit|~7 digits of precision|
|`double`|`System.Double`|64-bit|~15–16 digits of precision|
|`decimal`|`System.Decimal`|128-bit|28–29 significant digits (great for money)|

---

### **Other Primitive Types**

|C# Keyword|.NET Type|Description|
|---|---|---|
|`bool`|`System.Boolean`|Represents `true` or `false`.|
|`string`|`System.String`|Immutable sequence of characters.|
|`object`|`System.Object`|Base type for all types in C#.|
|`dynamic`|`System.Object`|Runtime-typed object (late binding).|

---

### **Special Types**

|C# Keyword|.NET Type|Description|
|---|---|---|
|`void`|N/A|Represents no return value (used in methods).|
|`var`|Compiler-inferred|Type inferred at compile time.|
|`null`|Literal|Represents “no value” for reference types.|

---

# 📌 2. Example Usage

```csharp
class Program
{
    static void Main()
    {
        int age = 25;
        double pi = 3.14159;
        decimal money = 19.99m;
        char grade = 'A';
        bool isActive = true;
        string name = "Alice";

        Console.WriteLine($"{name}, {age} years old, Grade: {grade}, Active: {isActive}, Balance: {money}, Pi: {pi}");
    }
}
```

---

# 📌 3. Key Notes

- **Aliases vs .NET types**:  
    `int` ↔ `System.Int32`, `string` ↔ `System.String`.  
    Both are the same, but C# developers almost always use the **keyword**.
    
- **Value types vs Reference types**:
    
    - Value types: numbers, `bool`, `struct`, `enum` → stored on the **stack**.
        
    - Reference types: `string`, `object`, `dynamic`, arrays, classes → stored on the **heap**.
        

---

✅ **Summary:**

- Built-in types = C# keywords (aliases) for `System` types.
    
- Categories: **integral, floating-point, decimal, boolean, char, string, object, dynamic, void**.
    
- They’re the foundation of all data handling in C#.
    
