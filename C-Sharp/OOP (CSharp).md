#CSharp #SoftDev 
**Constructor** is called on each newly created object.
**Destructor** is called as soon as there's no references to particular object.
**Encapsulation** is knowledge of how to manipulate objects included in object itself.
**Abstract Classes** are not instantiated.
**Abstract methods** declare method's signature, not implementation. Class with such method must also be abstract. While inheriting from abstract class, all abstract methods must be defined by derived class with same (or < restricted) visibility.
**Interface** states that object is able to perform some action, regardless of its origin interface. Used when objects are not inherited from one another but have similar behavior.
```csharp
class Person
{
    public string Name;
    public void Greet() => Console.WriteLine($"Hello, I’m {Name}!");
}
Person p = new Person();
p.Name = "Alice";
p.Greet();
```
**Encapsulation**
- Bundling data (fields) + methods that operate on the data.
- Use **access modifiers** to control visibility: `public`, `private`, `protected`, `internal`
```csharp
class Account
{
    private decimal balance;

    public void Deposit(decimal amount)
    {
        if (amount > 0)
            balance += amount;
    }

    public decimal GetBalance() => balance;
}
```
**Inheritance** - Class can inherit from another class. Promotes **code reuse**.
```csharp
class Animal
{
    public void Speak() => Console.WriteLine("Some sound");
}
class Dog : Animal
{
    public void Bark() => Console.WriteLine("Woof!");
}
```
**Polymorphism**: One interface - many implementations. Achieved by **Method overriding** & **interfaces**. Use `virtual`, `override`, & `abstract` keywords.
```csharp
class Animal
{
    public virtual void Speak() => Console.WriteLine("Animal sound");
}
class Cat : Animal
{
    public override void Speak() => Console.WriteLine("Meow");
}
```
**Abstraction** - hiding complex [[Logic]] & showing only essential features. Use **abstract classes** or **interfaces**.  
```csharp
abstract class Shape
{
    public abstract double GetArea();
}
class Circle : Shape
{
    public double Radius;
    public override double GetArea() => Math.PI * Radius * Radius;
}
```
**Constructors** - special methods that init objects. Same name as the class. Can be overloaded.
```csharp
class Book
{
    public string Title;
    public Book(string title) => Title = title;
}
```
`this` keyword - refers to the current instance of the class.
```csharp
class Student
{
    private string name;
    public Student(string name) => this.name = name;
}
```
**Properties** Controlled access to fields. Getters & setters with optional [[Logic]].
```csharp
class Car
{
    private int speed;
    public int Speed
    {
        get => speed;
        set
        {
            if (value >= 0) speed = value;
        }
    }
}
```
**Static Members** belong to the **class**, not instances. Use `static` keyword.
```csharp
class MathUtil
{
    public static double Pi = 3.1415;
    public static double Square(double x) => x * x;
}
```
**Interfaces** defines a **contract** without implementation. Classes implement interfaces using `:`
```csharp
interface IDrawable => void Draw();
class Rectangle : IDrawable
{
    public void Draw() => Console.WriteLine("Drawing Rectangle");
}
```

|Modifier|Access Scope|
|---|---|
|`public`|Everywhere|
|`private`|Within the same class|
|`protected`|Same class + derived classes|
|`internal`|Same assembly|
|`protected internal`|Derived classes + same assembly|
|`private protected`|Same class + derived in same assembly|
**Object Initializers** - concise way to init object properties without a constructor.
`var user = new User { Name = "Alex", Age = 25 };`
**Method Overloading**: Multiple methods with the **same name**, but different **params** (type, num, or order). Resolved at **compile time** (static polymorphism).
```csharp
class Calculator
{
    public int Add(int a, int b) => a + b;
    public double Add(double a, double b) => a + b;
    public int Add(int a, int b, int c) => a + b + c;
}
```
**Method Overriding**: **Child class** provides a different implementation for a **virtual** method from its parent. Resolved at **runtime** (dynamic polymorphism).
```csharp
class Animal { public virtual void Speak() => Console.WriteLine("Animal sound"); }
class Dog : Animal { public override void Speak() => Console.WriteLine("Woof"); }
```
**Sealed Classes & Methods**: `sealed` prevents inheritance or overriding. Use it to **lock down behavior**.   
```csharp
sealed class FinalClass {} // Can't inherit
class Base { public virtual void Run() {} }
class Child : Base
{
    public sealed override void Run() {} // Can't override further
}
```
**Partial Classes**: Split a class across **multiple files**. Useful for large projects or code generation (e.g. WinForms, [[Entity framework]]).  
```csharp
partial class Player { public string Name; } // File1.cs
partial class Player { public int Score; } // File2.cs
```
**Nested Classes** - a class **defined within another class**. Can access private members of the outer class.
```csharp
class Outer
{
    private int x = 42;
    public class Inner
    {
        public void Show() => Console.WriteLine("Inner class");
    }
}
```

| Feature              | Abstract Class | Interface                     |
| -------------------- | -------------- | ----------------------------- |
| Can contain code     | Yes            | No (until C# 8 default impl.) |
| Constructors         | Yes            | No                            |
| Multiple inheritance | No             | Yes                           |
| Fields               | Yes            | No                            |
**Getters & Setters (Auto-Implemented Properties)** - quick way to define properties.
```csharp
public string Name { get; set; }  // auto-property
```
 Add custom [[Logic]]:
```csharp
private int _age;
public int Age
{
    get => _age;
    set => _age = value > 0 ? value : 0;
}
```
**Static Constructors used to init **static data**. Runs **once** before the first object or static member is accessed.  
```csharp
class App
{
    static App()
    {
        Console.WriteLine("Static constructor called");
    }
}
```
**Destructors / Finalizers**: Runs when the object is **garbage collected**. Rarely used; for unmanaged resources.  
```csharp
class Resource
{
    ~Resource() => Console.WriteLine("Destructor called");
}
```
**Copy Constructor (Manually Defined)** - copies the values from one object into a new one.
```csharp
class Person
{
    public string Name;
    public Person(Person other) { Name = other.Name; }
}
```
`base` calls a **parent class constructor** or method.
```csharp
class Parent { public virtual void Greet() => Console.WriteLine("Hi from Parent"); }
class Child : Parent
{
    public override void Greet()
    {
        base.Greet(); // calls Parent’s method
        Console.WriteLine("Hi from Child");
    }
}
```
`new` Keyword (Hiding Members) - Use `new` to **hide** an inherited member instead of overriding.
```csharp
class A { public void Display() => Console.WriteLine("A"); }
class B : A { public new void Display() => Console.WriteLine("B"); }
```

| Keyword    | Value Set                 | Scope              | Mutable at runtime |
| ---------- | ------------------------- | ------------------ | ------------------ |
| `const`    | At compile time           | static only        | No                 |
| `readonly` | At runtime in Constructor | Instance or static | Yes                |
