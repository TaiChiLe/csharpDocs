
- C# numeric types (`int`, `byte`, etc.) have **fixed sizes**.
    
- If you try to store a value outside their range → overflow happens.
    

Example:

`int x = int.MaxValue;  // 2,147,483,647 x = x + 1;             // Overflow! Console.WriteLine(x);  // -2,147,483,648 (wraps around)`

By default, **C# doesn’t throw an error** — it just **wraps around** (like circular numbers).

---

# 📌 `checked`

`checked` tells C# → **throw an exception if overflow occurs**.

```
try
{
    int x = int.MaxValue;
    int y = checked(x + 1); // throws OverflowException
}
catch (OverflowException ex)
{
    Console.WriteLine("Overflow detected!");
}
```

Or use a block:

```
checked 
{     
	int x = int.MaxValue;     
	int y = x + 1;  // throws OverflowException 
}
```

---

# 📌 `unchecked`

`unchecked` tells C# → **ignore overflow, allow wrapping** (the default behavior in most cases).

```
unchecked
{
    int x = int.MaxValue;
    int y = x + 1;  // wraps to -2,147,483,648
    Console.WriteLine(y);
}
```

---

# 📌 Use Cases

- **`checked`** → when precision matters (financial, scientific calculations).
    
- **`unchecked`** → when you deliberately want wrapping (e.g., low-level bitwise operations, performance-sensitive code).
    

---

✅ **Summary**

- `checked` → throws error on overflow.
    
- `unchecked` → ignores overflow (wraps around).
    
- You can use them **as expressions or blocks**.