Let’s clarify the difference between **`ref`** and **`out`** in C# — two ways to pass variables **by reference**

---

# 📌 1. **What They Do**

Both `ref` and `out` allow a method to **modify the caller’s variable directly**, instead of working on a copy.

- **ref** → the variable **must be initialized before passing**.
    
- **out** → the variable **does not need to be initialized**, but **must be assigned inside the method**.
    

---

# 📌 2. **ref Example**

```csharp
void Increment(ref int x)
{
    x += 1;
}

int number = 5;
Increment(ref number);
Console.WriteLine(number); // Output: 6
```

**Key Points:**

- `number` **must be initialized** before passing.
    
- Changes inside the method **affect the original variable**.
    

---

# 📌 3. **out Example**

```csharp
void GetValues(out int a, out int b)
{
    a = 10; // Must assign inside method
    b = 20; // Must assign inside method
}

int x, y;            // Not initialized
GetValues(out x, out y);
Console.WriteLine($"{x}, {y}"); // Output: 10, 20
```

**Key Points:**

- `x` and `y` **do not need initialization** before passing.
    
- Method **must assign** a value to all out parameters before returning.
    

---

# 📌 4. **Differences Table**

|Feature|`ref`|`out`|
|---|---|---|
|Initialization required|Must be initialized before passing|No, can be uninitialized|
|Assignment inside method|Optional|Mandatory|
|Purpose|Pass value by reference, can read & modify|Pass value by reference, method must provide value|
|Common usage|Modify existing value|Return multiple values, output results|

---

# 📌 5. **Combined Example**

```csharp
void Calculate(ref int num, out int square)
{
    num += 5;       // modifies original num
    square = num * num; // must assign square
}

int number = 3;
int result;

Calculate(ref number, out result);

Console.WriteLine(number); // 8
Console.WriteLine(result); // 64
```

- `ref` reads & modifies `number`
    
- `out` must be assigned `result`
    

---

✅ **Tip:**

- Use `ref` when you want to **modify an existing value**.
    
- Use `out` when you want a **method to return multiple values** without initializing them first.
