**Implicit typing** means you let the **compiler figure out the data type** of a variable based on the value you assign to it.  

You use the keyword **`var`** for this.

```
var age = 25;          // Compiler infers int
var name = "Alice";    // Compiler infers string
var price = 19.99;     // Compiler infers double
var numbers = new int[] { 1, 2, 3 }; // Compiler infers int[]
```

Behind the scenes, the compiler **replaces `var` with the actual type**.

So:

`var age = 25;`

is the same as:

`int age = 25;`

# Rules of Implicit Typing
- You must **initialize** the variable immediately.
    
    `var x; // ❌ Error: Compiler can’t infer type var y = 10; // ✅ OK, inferred as int`
    
- Once inferred, the type is **fixed** — you can’t assign another type.
    
    `var z = 100; // int z = "hello"; // ❌ Error: can't change type`
    
- Implicit typing **does not mean dynamic typing**.
    
    - `var` → type decided at **compile time** (strongly typed).
        
    - `dynamic` → type decided at **runtime**.

# When to use

✅ Good for:

- Long or complex types (like LINQ queries).
    
- When the type is obvious from the right-hand side.
    

`var customers = new Dictionary<int, string>();`

❌ Avoid when:

- It makes code less readable.
    

`var x = GetData(); // ❌ unclear what type x is`

---

⚡ In short: **`var` lets you write less, but the type is still checked at compile time.**