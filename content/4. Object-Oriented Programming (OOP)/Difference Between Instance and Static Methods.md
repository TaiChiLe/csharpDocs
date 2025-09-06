Let’s go through **instance methods** and **static methods** in C# — two fundamental types of methods with important differences.

---

# 📌 1. **Instance Methods**

- Belong to a **specific object (instance) of a class**
    
- Can access **instance fields, properties, and other instance methods**
    
- Must be called on an **object** of the class
    

### **Example**

```csharp
public class Calculator
{
    public int Value;

    // Instance method
    public void Add(int number)
    {
        Value += number;
    }
}

Calculator calc = new Calculator();
calc.Add(5);  // Call instance method on object
Console.WriteLine(calc.Value); // 5
```

- `Add` is an **instance method**
    
- Requires `calc` object to call it
    

---

# 📌 2. **Static Methods**

- Belong to the **class itself**, not any specific instance
    
- Cannot access **instance fields or instance methods directly**
    
- Called using the **class name**, not an object
    

### **Example**

```csharp
public class MathHelper
{
    // Static method
    public static int Square(int x)
    {
        return x * x;
    }
}

int result = MathHelper.Square(5); // Call static method
Console.WriteLine(result); // 25
```

- `Square` is a **static method**
    
- Can’t use instance members like `Value` unless you create an object
    

---

# 📌 3. **Key Differences**

|Feature|Instance Method|Static Method|
|---|---|---|
|Belongs to|Object instance|Class itself|
|Access to instance|Yes|No (without object)|
|Called using|Object (e.g., `obj.Method()`)|Class name (e.g., `Class.Method()`)|
|Can be virtual/override|Yes|No|
|Use case|Behaviors dependent on object state|Utility or shared behavior|

---

# 📌 4. **Mixing Static and Instance**

```csharp
public class Circle
{
    public double Radius;

    public Circle(double radius)
    {
        Radius = radius;
    }

    // Instance method
    public double Area()
    {
        return Math.PI * Radius * Radius;
    }

    // Static method
    public static double CircleArea(double radius)
    {
        return Math.PI * radius * radius;
    }
}

Circle c = new Circle(3);
Console.WriteLine(c.Area());           // Instance method
Console.WriteLine(Circle.CircleArea(3)); // Static method
```

- Instance method depends on **object state (`Radius`)**
    
- Static method works **without creating an object**
    

---

# 📌 5. **Best Practices**

1. Use **instance methods** when behavior depends on object data
    
2. Use **static methods** for **utility/helper functions** that don’t depend on object state
    
3. Avoid excessive use of static methods for operations that should be object-specific
    

---

✅ **Tip:**

- Think: **“Do I need the object’s data to do this?”**
    
    - Yes → Instance method
        
    - No → Static method
        

---
