A **lambda expression** is a **short, inline function** that can be used wherever a **delegate** or **Func/Action** is expected.

The `=>` operator is read as **“goes to”**.

---

## 🔹 Syntax

`(parameters) => expression`

Or with a statement block:

`(parameters) => { statements }`

---

## 🔹 Examples

### 1. Basic Lambda (single expression)

`Func<int, int> square = x => x * x;   Console.WriteLine(square(5)); // 25`

- `Func<int, int>` → delegate type (input: int, output: int)
    
- `x => x * x` → function that returns `x * x`
    

---

### 2. Multiple parameters

`Func<int, int, int> add = (a, b) => a + b;   Console.WriteLine(add(3, 4)); // 7`

---

### 3. Statement block

```
Action<string> greet = name => 
{
    string message = $"Hello, {name}!";
    Console.WriteLine(message);
};
greet("Alice"); // Hello, Alice!
```

- `Action<T>` → delegate type with **no return value**.
    

---

### 4. Using Lambda with LINQ

```
int[] numbers = {1, 2, 3, 4, 5};
var evenNumbers = numbers.Where(n => n % 2 == 0);  

foreach(var num in evenNumbers)
    Console.WriteLine(num); // 2, 4
```

- `n => n % 2 == 0` is the lambda passed to `Where()`.
    

---

## 🔹 Key Points

1. Lambda expressions are **anonymous functions** — no name needed.
    
2. They are **short-hand for delegate implementations**.
    
3. Can capture **variables from outer scope** (closures).
    
4. Often used in **LINQ, events, callbacks, or functional-style programming**.
    

---

✅ **Summary:**

- `=>` = “goes to”
    
- `(parameters) => expression` = inline function
    
- Works with **Func, Action, Predicate** or any delegate type