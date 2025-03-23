# Generics-in-C-

## **Generics in C# – Complete Guide with Explanations**

Generics in C# allow defining classes, methods, interfaces, and delegates with a placeholder for the data type. They improve **type safety**, **code reusability**, and **performance**. Here’s everything you need to know about generics.

---

## **1. Introduction to Generics**
### **Why Use Generics?**
- Generics **allow type safety** by ensuring that only specific types are used, preventing runtime errors.
- **Code reusability** enables defining algorithms or classes that work with any data type.
- **Performance improvement** occurs because generics avoid boxing/unboxing of value types.

For example, using **non-generic collections** like `ArrayList` requires **boxing/unboxing**, which reduces performance:

```csharp
ArrayList list = new ArrayList();
list.Add(10); // Boxing (int → object)
int value = (int)list[0]; // Unboxing (object → int)
```
With **generics**, boxing/unboxing is eliminated:

```csharp
List<int> list = new List<int>();
list.Add(10); // No boxing
int value = list[0]; // No unboxing
```

---

## **2. Generic Classes**
Generic classes allow defining **type parameters** to store and manipulate any data type.

### **Example of a Generic Class**
```csharp
public class GenericClass<T>
{
    private T value;
    
    public void SetValue(T val) => value = val;
    
    public T GetValue() => value;
}
```
### **How It Works**
- `<T>` represents a placeholder type that will be determined when the class is instantiated.
- `SetValue(T val)` and `GetValue()` operate on any data type.

### **Usage**
```csharp
GenericClass<int> intObj = new GenericClass<int>();
intObj.SetValue(10);
Console.WriteLine(intObj.GetValue()); // Output: 10
```
```csharp
GenericClass<string> strObj = new GenericClass<string>();
strObj.SetValue("Hello");
Console.WriteLine(strObj.GetValue()); // Output: Hello
```

### **Multiple Type Parameters**
A class can have multiple generic types:
```csharp
public class Pair<T1, T2>
{
    public T1 First { get; set; }
    public T2 Second { get; set; }
}
```
```csharp
Pair<string, int> pair = new Pair<string, int> { First = "Alice", Second = 25 };
Console.WriteLine($"{pair.First}, {pair.Second}"); // Output: Alice, 25
```

---

## **3. Generic Methods**
Generic methods allow **specific methods** within non-generic classes to use generic type parameters.

### **Example**
```csharp
public class Utility
{
    public static void Swap<T>(ref T a, ref T b)
    {
        T temp = a;
        a = b;
        b = temp;
    }
}
```

### **Usage**
```csharp
int x = 10, y = 20;
Utility.Swap(ref x, ref y);
Console.WriteLine($"{x}, {y}"); // Output: 20, 10
```

---

## **4. Generic Interfaces**
A generic interface defines operations on different data types while enforcing structure.

### **Example**
```csharp
public interface IRepository<T>
{
    void Add(T item);
    T Get(int id);
}
```
```csharp
public class Repository<T> : IRepository<T>
{
    private List<T> data = new();
    
    public void Add(T item) => data.Add(item);
    
    public T Get(int id) => data[id];
}
```

---

## **5. Generic Delegates**
Delegates can also be generic, allowing them to hold references to methods with different type parameters.

### **Example**
```csharp
public delegate T Transformer<T>(T input);
```
### **Usage**
```csharp
Transformer<int> square = x => x * x;
Console.WriteLine(square(5)); // Output: 25
```

---

## **6. Constraints in Generics**
Constraints **restrict** the types that can be used with generics.

| Constraint | Explanation |
|------------|------------|
| `where T : struct` | Only value types (`int`, `double`, etc.). |
| `where T : class` | Only reference types. |
| `where T : new()` | Must have a **parameterless constructor**. |
| `where T : SomeBaseClass` | Must inherit from `SomeBaseClass`. |
| `where T : ISomeInterface` | Must implement `ISomeInterface`. |

### **Example**
```csharp
public class GenericClass<T> where T : new()
{
    public T CreateInstance() => new T();
}
```

---

## **7. Default Values in Generics**
Use `default(T)` to get the default value:
```csharp
T value = default(T);
```
- `null` for reference types.
- `0` for numbers.
- `false` for `bool`.

---

## **8. Covariance and Contravariance**
Used in **interfaces and delegates**.

| Feature | Explanation |
|---------|------------|
| **Covariance (`out`)** | Enables **returning a more derived type**. |
| **Contravariance (`in`)** | Enables **accepting a less derived type** as input. |

### **Example**
```csharp
public interface ICovariant<out T> { }
public interface IContravariant<in T> { }
```
```csharp
ICovariant<object> obj = new ICovariant<string>(); // Valid
IContravariant<string> str = new IContravariant<object>(); // Valid
```

---

## **9. Generic Collections**
`System.Collections.Generic` provides **type-safe** collections:
- `List<T>` → Dynamic array
- `Dictionary<TKey, TValue>` → Key-value pairs
- `Queue<T>` → FIFO
- `Stack<T>` → LIFO
- `HashSet<T>` → Unique elements

Example:
```csharp
List<int> numbers = new List<int> { 1, 2, 3 };
Dictionary<string, int> ages = new Dictionary<string, int> { { "Alice", 30 } };
```

---

## **10. Generic Type Inference**
Compiler **infers** type parameters:
```csharp
void Print<T>(T value) => Console.WriteLine(value);
Print(100); // Compiler infers T as int
```

---

## **11. Performance Benefits of Generics**
Generics:
✔ Avoid **boxing/unboxing**.  
✔ Improve **memory efficiency**.  
✔ Allow **compile-time type checking**.

---

## **12. Real-World Use Cases**
1. **Repository Pattern**:
   ```csharp
   public class Repository<T> where T : class { }
   ```
2. **Service Layer Abstraction**
3. **Generic Event Handlers**
4. **Dependency Injection Containers**

---

## **13. Generic Extension Methods**
```csharp
public static class Extensions
{
    public static bool IsDefault<T>(this T value) => EqualityComparer<T>.Default.Equals(value, default);
}
```
```csharp
int x = 0;
Console.WriteLine(x.IsDefault()); // Output: True
```

---

### **Summary**
Generics in C#:
- Provide **type safety**.
- Improve **performance**.
- Enable **code reusability**.
- Work in **classes, methods, interfaces, and delegates**.
- Support **constraints, covariance, and contravariance**.

---

## **🔍 Additional Edge Cases and Details in Generics**

### **1. Nested Generics (Generics inside Generics)**
You can have **nested generics** inside a class or method.
```csharp
public class OuterGeneric<T>
{
    public class InnerGeneric<U>
    {
        public T OuterValue;
        public U InnerValue;
    }
}
```
✅ **Usage:**
```csharp
OuterGeneric<int>.InnerGeneric<string> obj = new OuterGeneric<int>.InnerGeneric<string>();
obj.OuterValue = 10;
obj.InnerValue = "Hello";
```

---

### **2. Using `typeof(T)` Inside Generic Classes**
- The `typeof(T)` operator can be used to check the type at runtime.
```csharp
public class GenericClass<T>
{
    public void DisplayType()
    {
        Console.WriteLine($"Type of T: {typeof(T)}");
    }
}
```
✅ **Usage:**
```csharp
GenericClass<int> obj = new GenericClass<int>();
obj.DisplayType(); // Output: Type of T: System.Int32
```

---

### **3. Generic Static Members and Shared State**
- Generic **static fields** are **separate per type parameter**.

```csharp
public class GenericClass<T>
{
    public static int Counter;
}
```
✅ **Usage:**
```csharp
GenericClass<int>.Counter = 10;
GenericClass<string>.Counter = 20;

Console.WriteLine(GenericClass<int>.Counter);   // Output: 10
Console.WriteLine(GenericClass<string>.Counter); // Output: 20
```
🛑 **Corner Case:** Static members are **not shared** across different generic instantiations.

---

### **4. Type Checking and Reflection with Generics**
- You can check if a type is a **generic type** at runtime:
```csharp
Type type = typeof(List<int>);
Console.WriteLine(type.IsGenericType); // Output: True
Console.WriteLine(type.GetGenericTypeDefinition()); // Output: System.Collections.Generic.List`1[T]
```
- Extract generic arguments:
```csharp
Type[] typeArgs = type.GetGenericArguments();
Console.WriteLine(typeArgs[0]); // Output: System.Int32
```

---

### **5. Generic Type Inheritance**
- A **generic class** can inherit from **another generic class**.

```csharp
public class Base<T> { }
public class Derived<T> : Base<T> { }
```

- A **generic class** can inherit from a **non-generic class**.

```csharp
public class Base { }
public class Derived<T> : Base { }
```

✅ **Usage:**
```csharp
Derived<int> obj = new Derived<int>();
```

🛑 **Corner Case:** **Constructors of base classes must be handled carefully**.

---

### **6. Special Case: `default(T)` and Nullable Types**
- `default(T)` **behaves differently for value types and reference types**.

```csharp
T GetDefaultValue<T>() => default;
```

✅ **Usage:**
```csharp
Console.WriteLine(GetDefaultValue<int>());  // Output: 0
Console.WriteLine(GetDefaultValue<string>()); // Output: null
Console.WriteLine(GetDefaultValue<bool>()); // Output: False
```

🛑 **Corner Case:** **For nullable value types (`T?`), `default(T)` is `null`**.

---

### **7. Using `Activator.CreateInstance<T>()` Instead of `new()`**
- If a type **doesn’t have a parameterless constructor**, `new()` constraint **won’t work**.
- Instead, `Activator.CreateInstance<T>()` can create instances dynamically.

```csharp
public static T CreateInstance<T>() where T : new()
{
    return Activator.CreateInstance<T>();
}
```

✅ **Usage:**
```csharp
var obj = CreateInstance<MyClass>();
```

🛑 **Corner Case:** **Does not work for abstract classes or interfaces**.

---

### **8. Avoiding Struct Constraints (`where T : struct`) with Nullable<T>**
- **`where T : struct` prevents using `null`**, but `Nullable<T>` allows it.

```csharp
public void PrintValue<T>(T? value) where T : struct
{
    Console.WriteLine(value?.ToString() ?? "null");
}
```

✅ **Usage:**
```csharp
PrintValue<int>(null); // Output: null
PrintValue(10);  // Output: 10
```

🛑 **Corner Case:** **Nullable<T> (`T?`) is only valid for value types**.

---

### **9. Covariance and Contravariance Edge Cases**
✅ **Covariance (Only for Output)**
```csharp
public interface ICovariant<out T>
{
    T Get();
}
ICovariant<string> str = null;
ICovariant<object> obj = str; // Allowed
```

🛑 **Contravariance (Only for Input)**
```csharp
public interface IContravariant<in T>
{
    void Set(T item);
}
IContravariant<object> obj = null;
IContravariant<string> str = obj; // Allowed
```

🛑 **Corner Case:** **Covariant types cannot be used as method parameters**.

---

### **10. Generic Type Parameter Names in Inheritance**
- Be **careful** when using generics in **inheritance chains**.

```csharp
public class Base<T> { }
public class Derived<T> : Base<T> { }
```
🛑 **Issue:** The `T` in `Derived<T>` **hides** the `T` in `Base<T>`. Always check which `T` you are using.

---

### **11. Avoiding Recursion in Generic Constraints**
You can create recursive type constraints, but be cautious.

```csharp
public class Recursive<T> where T : Recursive<T>
{
}
```
✅ **Usage:**
```csharp
public class Derived : Recursive<Derived> { }
```

🛑 **Corner Case:** **This can lead to infinite loops if not handled properly**.

---

## **🛠 Final Summary: Edge Cases to Watch**
1. **Static fields in generics** are **not shared** across different type instances.
2. **`typeof(T)` in generics** can be used for reflection-based logic.
3. **Covariance and contravariance rules** apply **only** to interfaces and delegates.
4. **Generic type inference** doesn’t always work if there’s **ambiguity**.
5. **Recursive constraints** can lead to **infinite loops**.
6. **Default values (`default(T)`) vary** for value types (`0`) and reference types (`null`).
7. **Avoid struct constraints (`where T : struct`) if nullability is required**.
8. **Using `Activator.CreateInstance<T>()` is useful** when `new()` cannot be used.



