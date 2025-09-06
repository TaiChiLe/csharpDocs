In C#, **`dynamic`** is a special type that tells the compiler:  
👉 _“Don’t check this variable’s type at compile time. Figure it out at runtime.”_

## 🔹 Example

```
dynamic value = 10; 
Console.WriteLine(value.GetType()); // System.Int32

value = "Hello";   
Console.WriteLine(value.GetType()); // System.String  

value = DateTime.Now;   
Console.WriteLine(value.GetType()); // System.DateTime
```

- Here, `value` can hold **different types** over time.
    
- Unlike `var` (fixed at compile-time), `dynamic` is **flexible but unsafe**.
---

## 🔹 Key Features

1. **No compile-time type checking**
    
    `dynamic obj = "hello"; obj.NonExistentMethod(); // ❌ No error at compile time // Runtime error: method doesn’t exist`
    
2. **Can call members that don’t exist (until runtime)**  
    Useful when working with:
    
    - Reflection
        
    - COM objects
        
    - JSON / dynamic APIs
        
    - Interop with dynamic languages (Python, JavaScript)
        
3. **Converts to any type at runtime**
    
    `dynamic num = 5; int result = num + 10; // Works`
    

---

## 🔹 `var` vs `dynamic` vs `object`

|Keyword|Type Resolution|Checked at|Reassign to other types?|
|---|---|---|---|
|`var`|Compile-time|Compile-time|❌ (fixed type)|
|`dynamic`|Runtime|Runtime|✅ (can change type)|
|`object`|Compile-time|Compile-time|✅ but needs casting|

Example:

`var a = "text";       // string, fixed dynamic b = "text";   // dynamic, can change object c = "text";    // object, needs casting to use members`

---

## 🔹 When to Use `dynamic`

✅ Good for:

- Working with **data of unknown type** (e.g., parsing JSON).
    
- **Interop** with dynamic languages / COM.
    
- Quick prototyping.
    

❌ Avoid for:

- Core logic where type safety matters → because errors will only appear at **runtime**.