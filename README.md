# Master C# Object-Oriented Programming: Complete A to Z Guide

> **From Zero to Hero: Master OOP Concepts, Patterns, and Best Practices in C#**

---

## Table of Contents

1. [Introduction to Object-Oriented Programming](#1-introduction-to-object-oriented-programming)
2. [Classes and Objects](#2-classes-and-objects)
3. [Constructors and Destructors](#3-constructors-and-destructors)
4. [Properties and Indexers](#4-properties-and-indexers)
5. [Access Modifiers](#5-access-modifiers)
6. [Encapsulation](#6-encapsulation)
7. [Inheritance](#7-inheritance)
8. [Polymorphism](#8-polymorphism)
9. [Abstract Classes and Methods](#9-abstract-classes-and-methods)
10. [Interfaces](#10-interfaces)
11. [Sealed Classes and Methods](#11-sealed-classes-and-methods)
12. [Static Members and Classes](#12-static-members-and-classes)
13. [Partial Classes and Methods](#13-partial-classes-and-methods)
14. [Generics in OOP](#14-generics-in-oop)
15. [Operator Overloading](#15-operator-overloading)
16. [Design Patterns in OOP](#16-design-patterns-in-oop)
17. [SOLID Principles](#17-solid-principles)
18. [Best Practices and Common Pitfalls](#18-best-practices-and-common-pitfalls)

---

## 1. Introduction to Object-Oriented Programming

### What is Object-Oriented Programming?

Object-Oriented Programming (OOP) is a programming paradigm that organizes software design around objects rather than functions and logic. An object is a self-contained unit that combines data (attributes or properties) and behavior (methods or functions) that operate on that data. This approach mirrors how we perceive the real world, where objects like cars, animals, and buildings have both state (color, size, position) and behavior (move, eat, communicate). OOP provides a natural and intuitive way to model complex systems by representing real-world entities as objects in code.

The fundamental idea behind OOP is that programs are collections of objects that interact with each other through well-defined interfaces. Each object is responsible for a specific piece of functionality and maintains its own internal state, which other objects cannot directly access or modify. This separation of concerns leads to more modular, maintainable, and reusable code. When you need to change how something works, you typically only need to modify one object rather than making changes throughout the entire codebase.

OOP emerged as a solution to the growing complexity of software systems. As programs became larger and more complex, procedural programming (where the focus is on functions and procedures) became increasingly difficult to manage. Code became tangled with global variables and functions that depended on each other in unpredictable ways. OOP addresses these problems by encapsulating related data and behavior into objects, creating clear boundaries between different parts of a program, and enabling code reuse through inheritance and polymorphism.

### The Four Pillars of OOP

Object-Oriented Programming is built upon four fundamental principles, often called the "four pillars" of OOP. These principles guide how we design and structure our code to achieve the benefits of object orientation. Understanding these principles deeply is essential for writing good object-oriented code.

**Encapsulation** is the practice of bundling data and the methods that operate on that data within a single unit (a class), while hiding the internal details of how the data is stored and manipulated. Encapsulation protects an object's internal state from outside interference and misuse. It allows the object to control its own state and ensures that the state remains valid. In C#, encapsulation is achieved through access modifiers (public, private, protected, internal) that control the visibility of class members.

**Inheritance** is a mechanism that allows one class to inherit the properties and methods of another class. The class being inherited from is called the base class (or parent class), and the class that inherits is called the derived class (or child class). Inheritance enables code reuse and establishes a hierarchical relationship between classes. A derived class can add new members or override existing members to provide specialized behavior while inheriting the common functionality from its base class.

**Polymorphism** means "many forms" and refers to the ability of objects of different classes to be treated as objects of a common base class. Polymorphism allows you to write code that works with objects of different types through a common interface, with the specific behavior determined at runtime. This is achieved through method overriding (runtime polymorphism) and method overloading (compile-time polymorphism). Polymorphism enables flexible, extensible code that can work with new types without modification.

**Abstraction** is the practice of hiding complex implementation details and exposing only the essential features of an object. Abstraction allows you to focus on what an object does rather than how it does it. In C#, abstraction is achieved through abstract classes and interfaces, which define contracts that derived classes must implement. Abstraction simplifies complex systems by breaking them into smaller, more manageable pieces with clearly defined responsibilities.

### Why OOP Matters

Object-Oriented Programming offers several significant advantages that make it the dominant paradigm for building complex software systems. First, OOP promotes code reusability through inheritance and composition. When you create a class that encapsulates some functionality, you can reuse that class in multiple projects or extend it to add new features without modifying the original code. This reuse saves development time and reduces the potential for bugs.

Second, OOP improves code organization and maintainability. By organizing code into classes that represent real-world entities or abstract concepts, you create a structure that's easier to understand and navigate. Each class has a clear responsibility, making it easier to locate the code you need to modify and understand the impact of your changes. This organization becomes increasingly valuable as projects grow in size and complexity.

Third, OOP enhances code flexibility and extensibility through polymorphism and abstraction. When you program to interfaces rather than concrete implementations, you can easily swap out one implementation for another without affecting the rest of your code. You can add new types of objects without modifying existing code, making your system more resilient to change. This flexibility is essential in modern software development, where requirements frequently evolve.

---

## 2. Classes and Objects

### Understanding Classes

A class is a blueprint or template that defines the structure and behavior of objects. It specifies what data an object will hold (through fields and properties) and what operations can be performed on that data (through methods). Think of a class as a cookie cutter and objects as the cookies—the class defines the shape and characteristics, and each object created from the class has those same characteristics but with its own specific values.

In C#, classes are reference types, meaning that variables of a class type store a reference to an object on the heap rather than the object itself. When you create a new object using the `new` keyword, memory is allocated on the heap for the object, and a reference to that memory is returned. This has important implications for how objects are passed to methods and compared for equality.

```csharp
// Basic class definition
public class Person
{
    // Fields (data members)
    private string _name;
    private int _age;
    
    // Constructor
    public Person(string name, int age)
    {
        _name = name;
        _age = age;
    }
    
    // Method (behavior)
    public void Introduce()
    {
        Console.WriteLine($"Hi, I'm {_name} and I'm {_age} years old.");
    }
    
    // Property (controlled access to data)
    public string Name
    {
        get { return _name; }
        set { _name = value; }
    }
    
    public int Age
    {
        get { return _age; }
        set 
        { 
            if (value >= 0)
                _age = value; 
        }
    }
}
```

### Creating and Using Objects

An object is an instance of a class—a concrete entity that exists in memory with specific values for its data members. Creating an object is called instantiation, and it involves allocating memory for the object and initializing its state. Once an object is created, you can access its public members using the dot notation (`objectName.MemberName`).

```csharp
// Creating objects
Person person1 = new Person("Alice", 30);
Person person2 = new Person("Bob", 25);

// Using object members
person1.Introduce();  // Output: Hi, I'm Alice and I'm 30 years old.
person2.Introduce();  // Output: Hi, I'm Bob and I'm 25 years old.

// Accessing properties
Console.WriteLine(person1.Name);  // Output: Alice
person1.Age = 31;  // Update age using property

// Object initialization syntax
Person person3 = new Person("Charlie", 35)
{
    Age = 36  // Can set properties after construction
};

// Target-typed new (C# 9+)
Person person4 = new("Diana", 28);

// Object initializer with default constructor
public class Product
{
    public string Name { get; set; }
    public decimal Price { get; set; }
    public int Quantity { get; set; }
}

Product product = new Product
{
    Name = "Laptop",
    Price = 999.99m,
    Quantity = 10
};
```

### The this Keyword

The `this` keyword refers to the current instance of the class. It's used to distinguish between instance members and parameters with the same name, to pass the current object to other methods, and to chain constructor calls. Understanding `this` is essential for writing clean and unambiguous code.

```csharp
public class Employee
{
    private string _name;
    private decimal _salary;
    
    // Using 'this' to distinguish field from parameter
    public Employee(string name, decimal salary)
    {
        this._name = name;      // 'this.' is optional here but clarifies intent
        this._salary = salary;
    }
    
    // Using 'this' to pass current instance to another method
    public void PrintDetails()
    {
        PrintEmployee(this);
    }
    
    private void PrintEmployee(Employee emp)
    {
        Console.WriteLine($"Employee: {emp._name}, Salary: {emp._salary:C}");
    }
    
    // Using 'this' for indexer (covered later)
    public object this[int index]
    {
        get { return null; /* implementation */ }
    }
    
    // Method chaining with 'this'
    public Employee SetName(string name)
    {
        this._name = name;
        return this;  // Return current instance for chaining
    }
    
    public Employee SetSalary(decimal salary)
    {
        this._salary = salary;
        return this;
    }
}

// Fluent usage through method chaining
Employee emp = new Employee("John", 50000)
    .SetName("John Doe")
    .SetSalary(55000);
```

### Class Members Overview

A class can contain various types of members, each serving a different purpose. Understanding these member types is fundamental to OOP in C#.

```csharp
public class ClassMembers
{
    // Constants - compile-time constant values
    public const double PI = 3.14159;
    
    // Fields - variables that hold data
    private int _privateField;
    public int PublicField;
    
    // Properties - controlled access to data
    public int Property { get; set; }
    public int ReadOnlyProperty { get; private set; }
    
    // Indexers - array-like access to objects
    public int this[int index]
    {
        get { return index; }
        set { _privateField = value; }
    }
    
    // Constructors - initialization code
    public ClassMembers()
    {
        // Default constructor
    }
    
    // Static constructor - initializes static members
    static ClassMembers()
    {
        // Runs once before any instance is created
    }
    
    // Destructor - cleanup code (rarely used in C#)
    ~ClassMembers()
    {
        // Called by garbage collector
    }
    
    // Methods - behavior/functionality
    public void InstanceMethod() { }
    
    // Static methods - belong to the type, not instances
    public static void StaticMethod() { }
    
    // Events - notification mechanism
    public event EventHandler SomeEvent;
    
    // Delegates - type-safe function pointers
    public delegate void ProcessHandler(string message);
    
    // Nested types
    public class NestedClass { }
}
```

---

## 3. Constructors and Destructors

### Understanding Constructors

A constructor is a special method that is called when an instance of a class is created. Its primary purpose is to initialize the object's state—setting fields to initial values, allocating resources, and performing any other setup needed before the object can be used. Constructors have the same name as the class and no return type (not even void). Every class must have at least one constructor, though if you don't define one, the compiler provides a default parameterless constructor.

```csharp
public class BankAccount
{
    private string _accountNumber;
    private string _owner;
    private decimal _balance;
    
    // Default constructor
    public BankAccount()
    {
        _accountNumber = GenerateAccountNumber();
        _owner = "Unknown";
        _balance = 0;
    }
    
    // Parameterized constructor
    public BankAccount(string owner, decimal initialBalance)
    {
        _accountNumber = GenerateAccountNumber();
        _owner = owner;
        _balance = initialBalance;
    }
    
    // Full constructor with all parameters
    public BankAccount(string accountNumber, string owner, decimal balance)
    {
        _accountNumber = accountNumber;
        _owner = owner;
        _balance = balance;
    }
    
    private static string GenerateAccountNumber()
    {
        return Guid.NewGuid().ToString().Substring(0, 8).ToUpper();
    }
    
    public void DisplayInfo()
    {
        Console.WriteLine($"Account: {_accountNumber}, Owner: {_owner}, Balance: {_balance:C}");
    }
}

// Using different constructors
BankAccount account1 = new BankAccount();
BankAccount account2 = new BankAccount("Alice", 1000);
BankAccount account3 = new BankAccount("ACC12345", "Bob", 5000);
```

### Constructor Chaining

Constructor chaining allows one constructor to call another constructor in the same class, avoiding code duplication. This is done using the `this` keyword. The called constructor executes first, then the calling constructor continues with its own initialization logic.

```csharp
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; }
    public decimal Price { get; set; }
    public string Category { get; set; }
    
    // Most comprehensive constructor
    public Product(int id, string name, decimal price, string category)
    {
        Id = id;
        Name = name ?? throw new ArgumentNullException(nameof(name));
        Price = price >= 0 ? price : throw new ArgumentException("Price cannot be negative");
        Category = category ?? "General";
    }
    
    // Chain to full constructor with defaults
    public Product(int id, string name, decimal price)
        : this(id, name, price, "General")
    {
    }
    
    // Chain with more defaults
    public Product(int id, string name)
        : this(id, name, 0, "General")
    {
    }
    
    // Default constructor chains to the simplest parameterized one
    public Product()
        : this(0, "Unnamed Product")
    {
    }
}

// All these work correctly
Product p1 = new Product();                          // Id=0, Name="Unnamed Product", Price=0, Category="General"
Product p2 = new Product(1, "Laptop");               // Price=0, Category="General"
Product p3 = new Product(2, "Phone", 699.99m);       // Category="General"
Product p4 = new Product(3, "Tablet", 499.99m, "Electronics");
```

### Static Constructors

A static constructor is used to initialize static members of a class. It's called automatically before any static members are accessed or any instances are created. A class can have only one static constructor, and it cannot have parameters or access modifiers (it's implicitly private). Static constructors are useful for expensive one-time initialization operations.

```csharp
public class Configuration
{
    public static string ApplicationName { get; }
    public static string Version { get; }
    public static DateTime StartTime { get; }
    
    // Static constructor - runs once per AppDomain
    static Configuration()
    {
        ApplicationName = "My Application";
        Version = "1.0.0";
        StartTime = DateTime.Now;
        Console.WriteLine("Static constructor executed!");
    }
    
    public static void DisplayInfo()
    {
        Console.WriteLine($"{ApplicationName} v{Version}, Started: {StartTime}");
    }
}

// Static constructor runs before this line
Configuration.DisplayInfo();
// Output:
// Static constructor executed!
// My Application v1.0.0, Started: [current time]

// Second call - static constructor doesn't run again
Configuration.DisplayInfo();
// Output: My Application v1.0.0, Started: [same time as above]
```

### Destructors

A destructor (also called a finalizer) is called when an object is being reclaimed by the garbage collector. In C#, destructors are rarely needed because the garbage collector automatically manages memory. However, when working with unmanaged resources (file handles, database connections, etc.), you may need to implement proper cleanup. Modern C# code typically uses the `IDisposable` pattern instead of destructors.

```csharp
public class ResourceHolder
{
    private IntPtr _handle;  // Unmanaged resource
    private bool _disposed = false;
    
    public ResourceHolder()
    {
        _handle = /* allocate unmanaged resource */;
        Console.WriteLine("Resource acquired");
    }
    
    // Destructor (finalizer)
    ~ResourceHolder()
    {
        Console.WriteLine("Destructor called");
        Dispose(false);
    }
    
    // IDisposable implementation
    public void Dispose()
    {
        Dispose(true);
        GC.SuppressFinalize(this);  // Don't call destructor if Dispose was called
    }
    
    protected virtual void Dispose(bool disposing)
    {
        if (!_disposed)
        {
            if (disposing)
            {
                // Dispose managed resources
            }
            
            // Dispose unmanaged resources
            if (_handle != IntPtr.Zero)
            {
                // Free the handle
                _handle = IntPtr.Zero;
            }
            
            _disposed = true;
        }
    }
}

// Proper usage with using statement
using (var holder = new ResourceHolder())
{
    // Use the resource
}  // Dispose() is called automatically here
```

---

## 4. Properties and Indexers

### Understanding Properties

Properties provide a flexible way to read, write, or compute the value of a private field. They combine the syntax of fields with the safety and flexibility of methods. Properties enable encapsulation by allowing you to control access to your data, validate input, and execute code when values are read or written. In C#, properties are first-class citizens and are preferred over public fields.

```csharp
public class Temperature
{
    private double _celsius;
    
    // Basic property
    public double Celsius
    {
        get { return _celsius; }
        set 
        { 
            if (value < -273.15)
                throw new ArgumentOutOfRangeException(nameof(value), 
                    "Temperature cannot be below absolute zero");
            _celsius = value; 
        }
    }
    
    // Computed property (read-only, no backing field)
    public double Fahrenheit
    {
        get { return _celsius * 9 / 5 + 32; }
    }
    
    // Property with different access levels
    public double Kelvin
    {
        get { return _celsius + 273.15; }
        private set { _celsius = value - 273.15; }
    }
    
    // Auto-implemented property (compiler creates backing field)
    public string Description { get; set; } = "Normal";
    
    // Auto-implemented property with private setter
    public DateTime CreatedAt { get; }  // Read-only, set only in constructor
    
    // Auto-implemented property with initial value
    public string Unit { get; init; } = "C";  // Can only be set during initialization
    
    public Temperature(double celsius)
    {
        Celsius = celsius;
        CreatedAt = DateTime.Now;
    }
}

// Using properties
Temperature temp = new Temperature(25);
temp.Celsius = 30;  // Uses setter with validation
Console.WriteLine(temp.Fahrenheit);  // 86.0 (computed)
Console.WriteLine(temp.CreatedAt);   // Set in constructor

// Init-only property (C# 9+)
Temperature temp2 = new Temperature(20) { Unit = "C" };
// temp2.Unit = "F";  // Error! Can't change init-only property
```

### Auto-Implemented Properties

Auto-implemented properties provide a concise syntax when you don't need additional logic in the property accessors. The compiler automatically creates a private, anonymous backing field. This is the most common way to define properties in modern C#.

```csharp
public class Customer
{
    // Basic auto-implemented property
    public string Name { get; set; }
    
    // Auto-property with default value
    public int Age { get; set; } = 18;
    
    // Read-only auto-property (set in constructor only)
    public string Id { get; }
    
    // Init-only property (C# 9+)
    public string Email { get; init; }
    
    // Required property (C# 11+)
    public required string PhoneNumber { get; init; }
    
    // Private setter (common for encapsulation)
    public int LoyaltyPoints { get; private set; }
    
    public Customer(string id, string name)
    {
        Id = id;
        Name = name;
    }
    
    public void AddPoints(int points)
    {
        if (points > 0)
            LoyaltyPoints += points;
    }
}

// Object initializer with required properties
Customer customer = new Customer("C001", "Alice")
{
    Email = "alice@example.com",
    PhoneNumber = "+1234567890"  // Required!
};
```

### Indexers

Indexers allow objects to be indexed like arrays. They enable you to access an object's data using array syntax (`object[index]`). Indexers are particularly useful when a class represents a collection or has logical indices. Like properties, indexers can have get and set accessors with varying access levels.

```csharp
public class Team
{
    private string[] _members;
    
    public Team(int size)
    {
        _members = new string[size];
    }
    
    // Basic indexer
    public string this[int index]
    {
        get 
        { 
            if (index < 0 || index >= _members.Length)
                throw new IndexOutOfRangeException();
            return _members[index]; 
        }
        set 
        { 
            if (index < 0 || index >= _members.Length)
                throw new IndexOutOfRangeException();
            _members[index] = value; 
        }
    }
    
    // Indexer with string parameter
    public int this[string name]
    {
        get
        {
            for (int i = 0; i < _members.Length; i++)
            {
                if (_members[i] == name)
                    return i;
            }
            return -1;  // Not found
        }
    }
    
    // Read-only property for count
    public int Count => _members.Length;
}

// Using indexers
Team team = new Team(3);
team[0] = "Alice";
team[1] = "Bob";
team[2] = "Charlie";

Console.WriteLine(team[0]);       // Output: Alice
Console.WriteLine(team["Bob"]);   // Output: 1 (Bob's index)

// Multi-dimensional indexer
public class Matrix
{
    private int[,] _data;
    
    public Matrix(int rows, int cols)
    {
        _data = new int[rows, cols];
    }
    
    // Multi-dimensional indexer
    public int this[int row, int col]
    {
        get => _data[row, col];
        set => _data[row, col] = value;
    }
}

Matrix matrix = new Matrix(3, 3);
matrix[0, 0] = 1;
matrix[1, 1] = 2;
int value = matrix[0, 0];  // 1
```

---

## 5. Access Modifiers

### Understanding Access Modifiers

Access modifiers are keywords that control the visibility and accessibility of types and their members. They are fundamental to encapsulation, allowing you to hide implementation details and expose only the necessary parts of your classes. C# provides five access modifiers that define different levels of accessibility.

```csharp
// Access Modifier Summary:
// public    - Accessible from anywhere
// private   - Accessible only within the containing class
// protected - Accessible within containing class and derived classes
// internal  - Accessible within the same assembly
// protected internal - Accessible within same assembly OR derived classes
// private protected - Accessible within containing class OR derived classes in same assembly
```

### Detailed Access Modifier Examples

```csharp
// Assembly A (ClassLibrary.dll)
public class AccessModifiersDemo
{
    // Public - accessible everywhere
    public int PublicField;
    public void PublicMethod() { }
    public class PublicNestedClass { }
    
    // Private - accessible only in this class
    private int _privateField;
    private void PrivateMethod() { }
    
    // Protected - accessible in this class and derived classes
    protected int ProtectedField;
    protected void ProtectedMethod() { }
    
    // Internal - accessible within this assembly
    internal int InternalField;
    internal void InternalMethod() { }
    
    // Protected Internal - accessible within assembly OR in derived classes
    protected internal int ProtectedInternalField;
    protected internal void ProtectedInternalMethod() { }
    
    // Private Protected - accessible within this class OR derived classes in same assembly
    private protected int PrivateProtectedField;
    private protected void PrivateProtectedMethod() { }
    
    // Property with different accessor access levels
    public string Name { get; private set; }  // Public get, private set
    
    // Constructor accessibility
    public AccessModifiersDemo()
    {
        // Public constructor - anyone can create instances
    }
    
    private AccessModifiersDemo(string name)
    {
        // Private constructor - only this class can create instances this way
        Name = name;
    }
    
    internal AccessModifiersDemo(int id)
    {
        // Internal constructor - only within assembly
    }
}

// Derived class in same assembly
public class DerivedClass : AccessModifiersDemo
{
    public void AccessBaseMembers()
    {
        PublicMethod();              // OK
        ProtectedMethod();           // OK (protected accessible in derived)
        InternalMethod();            // OK (same assembly)
        ProtectedInternalMethod();   // OK (same assembly)
        PrivateProtectedMethod();    // OK (same assembly and derived)
        // PrivateMethod();         // Error! Private not accessible
    }
}

// Class in different assembly (would need to reference ClassLibrary.dll)
public class ExternalClass : AccessModifiersDemo
{
    public void AccessBaseMembers()
    {
        PublicMethod();              // OK
        ProtectedMethod();           // OK (protected accessible in derived)
        // InternalMethod();        // Error! Different assembly
        ProtectedInternalMethod();   // OK (derived class, different assembly)
        // PrivateProtectedMethod(); // Error! Different assembly
    }
}
```

### Best Practices for Access Modifiers

```csharp
public class BestPracticesExample
{
    // Good: Private backing field with public property
    private string _name;
    public string Name
    {
        get => _name;
        set => _name = value ?? throw new ArgumentNullException(nameof(value));
    }
    
    // Good: Auto-property with private setter
    public DateTime CreatedAt { get; private set; }
    
    // Good: Constants and static readonly can be public
    public const double TaxRate = 0.08;
    public static readonly string DefaultCategory = "General";
    
    // Good: Private methods for internal implementation
    private bool ValidateInput(string input)
    {
        return !string.IsNullOrWhiteSpace(input);
    }
    
    // Good: Protected methods for extensibility
    protected virtual void OnCreated()
    {
        // Derived classes can override for custom behavior
    }
    
    // Good: Internal for assembly-level utilities
    internal static string GenerateId()
    {
        return Guid.NewGuid().ToString();
    }
}
```

---

## 6. Encapsulation

### What is Encapsulation?

Encapsulation is one of the fundamental principles of object-oriented programming. It refers to bundling data (fields) and methods that operate on that data within a single unit (class), while restricting direct access to some of the object's components. Encapsulation protects the internal state of an object by preventing direct modification from outside the class, instead providing controlled access through properties and methods.

The primary benefits of encapsulation include: protecting data integrity by validating changes before they're applied, hiding implementation details so that changes don't affect external code, providing a clean interface for interacting with objects, and enabling you to change internal implementation without breaking existing code. In C#, encapsulation is achieved primarily through access modifiers (private fields, public properties) and methods that validate and control access to data.

### Implementing Encapsulation

```csharp
// Bad example - no encapsulation
public class BankAccountBad
{
    public string Owner;
    public decimal Balance;
    public string AccountNumber;
}

// Anyone can do this:
var badAccount = new BankAccountBad();
badAccount.Balance = -1000;  // Invalid state! Negative balance allowed.

// Good example - proper encapsulation
public class BankAccount
{
    // Private fields - hidden from outside
    private string _accountNumber;
    private string _owner;
    private decimal _balance;
    private List<Transaction> _transactions;
    
    // Public properties with controlled access
    public string AccountNumber => _accountNumber;
    public string Owner => _owner;
    public decimal Balance => _balance;
    public IReadOnlyList<Transaction> TransactionHistory => _transactions.AsReadOnly();
    
    public BankAccount(string owner, decimal initialBalance)
    {
        ValidateOwner(owner);
        ValidateAmount(initialBalance);
        
        _accountNumber = GenerateAccountNumber();
        _owner = owner;
        _balance = initialBalance;
        _transactions = new List<Transaction>
        {
            new Transaction("Initial Deposit", initialBalance, DateTime.Now)
        };
    }
    
    // Public methods provide controlled operations
    public void Deposit(decimal amount)
    {
        ValidateAmount(amount);
        _balance += amount;
        _transactions.Add(new Transaction("Deposit", amount, DateTime.Now));
    }
    
    public bool Withdraw(decimal amount)
    {
        ValidateAmount(amount);
        
        if (_balance < amount)
        {
            Console.WriteLine("Insufficient funds");
            return false;
        }
        
        _balance -= amount;
        _transactions.Add(new Transaction("Withdrawal", -amount, DateTime.Now));
        return true;
    }
    
    public bool TransferTo(BankAccount destination, decimal amount)
    {
        if (Withdraw(amount))
        {
            destination.Deposit(amount);
            _transactions.Add(new Transaction(
                $"Transfer to {destination.AccountNumber}", -amount, DateTime.Now));
            return true;
        }
        return false;
    }
    
    // Private helper methods - hidden implementation details
    private static void ValidateOwner(string owner)
    {
        if (string.IsNullOrWhiteSpace(owner))
            throw new ArgumentException("Owner name is required", nameof(owner));
    }
    
    private static void ValidateAmount(decimal amount)
    {
        if (amount <= 0)
            throw new ArgumentException("Amount must be positive", nameof(amount));
    }
    
    private static string GenerateAccountNumber()
    {
        return "ACC" + Guid.NewGuid().ToString().Substring(0, 8).ToUpper();
    }
}

// Transaction record (C# 9+)
public record Transaction(string Description, decimal Amount, DateTime Date);
```

### Encapsulation with Validation

```csharp
public class Person
{
    private string _firstName;
    private string _lastName;
    private int _age;
    private string _email;
    
    public string FirstName
    {
        get => _firstName;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("First name is required", nameof(value));
            if (value.Length > 50)
                throw new ArgumentException("First name too long", nameof(value));
            _firstName = value.Trim();
        }
    }
    
    public string LastName
    {
        get => _lastName;
        set
        {
            if (string.IsNullOrWhiteSpace(value))
                throw new ArgumentException("Last name is required", nameof(value));
            if (value.Length > 50)
                throw new ArgumentException("Last name too long", nameof(value));
            _lastName = value.Trim();
        }
    }
    
    public int Age
    {
        get => _age;
        set
        {
            if (value < 0 || value > 150)
                throw new ArgumentOutOfRangeException(nameof(value), 
                    "Age must be between 0 and 150");
            _age = value;
        }
    }
    
    public string Email
    {
        get => _email;
        set
        {
            if (!IsValidEmail(value))
                throw new ArgumentException("Invalid email format", nameof(value));
            _email = value.ToLowerInvariant();
        }
    }
    
    // Computed property (no backing field)
    public string FullName => $"{_firstName} {_lastName}";
    
    // Method with business logic
    public bool IsAdult(int adultAge = 18) => _age >= adultAge;
    
    // Private validation
    private static bool IsValidEmail(string email)
    {
        if (string.IsNullOrWhiteSpace(email))
            return false;
        try
        {
            var addr = new System.Net.Mail.MailAddress(email);
            return addr.Address == email;
        }
        catch
        {
            return false;
        }
    }
    
    public Person(string firstName, string lastName, int age, string email)
    {
        FirstName = firstName;  // Uses property setter for validation
        LastName = lastName;
        Age = age;
        Email = email;
    }
}
```

---

## 7. Inheritance

### Understanding Inheritance

Inheritance is a mechanism that allows a class to derive properties and characteristics from another class. The class being inherited from is called the base class (or parent/superclass), and the class that inherits is called the derived class (or child/subclass). Inheritance enables code reuse, establishes hierarchical relationships between classes, and supports polymorphism.

In C#, a class can only inherit from one base class (single inheritance), but a class can implement multiple interfaces. This design decision simplifies the language and avoids the complexity and ambiguity of multiple inheritance (such as the diamond problem). When you need functionality from multiple sources, you can use interfaces or composition instead.

```csharp
// Base class
public class Animal
{
    public string Name { get; set; }
    public int Age { get; set; }
    
    public Animal(string name, int age)
    {
        Name = name;
        Age = age;
    }
    
    public virtual void MakeSound()
    {
        Console.WriteLine($"{Name} makes a sound");
    }
    
    public void Eat()
    {
        Console.WriteLine($"{Name} is eating");
    }
    
    public void Sleep()
    {
        Console.WriteLine($"{Name} is sleeping");
    }
}

// Derived class
public class Dog : Animal
{
    public string Breed { get; set; }
    
    // Call base constructor
    public Dog(string name, int age, string breed) : base(name, age)
    {
        Breed = breed;
    }
    
    // Override base method
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} says: Woof!");
    }
    
    // New method specific to Dog
    public void Fetch()
    {
        Console.WriteLine($"{Name} is fetching the ball");
    }
}

// Another derived class
public class Cat : Animal
{
    public bool IsIndoor { get; set; }
    
    public Cat(string name, int age, bool isIndoor) : base(name, age)
    {
        IsIndoor = isIndoor;
    }
    
    public override void MakeSound()
    {
        Console.WriteLine($"{Name} says: Meow!");
    }
    
    public void Purr()
    {
        Console.WriteLine($"{Name} is purring");
    }
}

// Using inheritance
Dog dog = new Dog("Buddy", 3, "Golden Retriever");
Cat cat = new Cat("Whiskers", 5, true);

dog.MakeSound();  // Output: Buddy says: Woof!
cat.MakeSound();  // Output: Whiskers says: Meow!

dog.Eat();        // Inherited method: Buddy is eating
cat.Sleep();      // Inherited method: Whiskers is sleeping

dog.Fetch();      // Dog-specific method
cat.Purr();       // Cat-specific method
```

### The base Keyword

The `base` keyword is used to access members of the base class from within a derived class. It's commonly used to call the base class constructor or to access base class methods that have been overridden.

```csharp
public class Vehicle
{
    public string Make { get; }
    public string Model { get; }
    public int Year { get; }
    
    public Vehicle(string make, string model, int year)
    {
        Make = make;
        Model = model;
        Year = year;
    }
    
    public virtual void DisplayInfo()
    {
        Console.WriteLine($"{Year} {Make} {Model}");
    }
    
    public virtual void Start()
    {
        Console.WriteLine("Vehicle starting...");
    }
}

public class Car : Vehicle
{
    public int NumberOfDoors { get; }
    public string FuelType { get; }
    
    public Car(string make, string model, int year, int doors, string fuel)
        : base(make, model, year)  // Call base constructor
    {
        NumberOfDoors = doors;
        FuelType = fuel;
    }
    
    public override void DisplayInfo()
    {
        base.DisplayInfo();  // Call base method first
        Console.WriteLine($"  Doors: {NumberOfDoors}, Fuel: {FuelType}");
    }
    
    public override void Start()
    {
        base.Start();  // Call base method
        Console.WriteLine("Car engine running...");
    }
}

public class ElectricCar : Car
{
    public int BatteryCapacity { get; }
    
    public ElectricCar(string make, string model, int year, int doors, int batteryCapacity)
        : base(make, model, year, doors, "Electric")  // Chain to Car constructor
    {
        BatteryCapacity = batteryCapacity;
    }
    
    public override void DisplayInfo()
    {
        base.DisplayInfo();  // Call Car's DisplayInfo
        Console.WriteLine($"  Battery: {BatteryCapacity} kWh");
    }
    
    public override void Start()
    {
        Console.WriteLine("Electric motor activating...");
        // Note: Deliberately not calling base.Start() for different behavior
    }
    
    public void Charge()
    {
        Console.WriteLine("Charging battery...");
    }
}

// Usage
Car car = new Car("Toyota", "Camry", 2023, 4, "Gasoline");
car.DisplayInfo();
// Output:
// 2023 Toyota Camry
//   Doors: 4, Fuel: Gasoline

ElectricCar tesla = new ElectricCar("Tesla", "Model 3", 2023, 4, 75);
tesla.DisplayInfo();
// Output:
// 2023 Tesla Model 3
//   Doors: 4, Fuel: Electric
//   Battery: 75 kWh
```

### Method Overriding vs Hiding

Method overriding (using `virtual` and `override`) enables polymorphism, where the actual method called is determined at runtime based on the object's type. Method hiding (using `new`) simply hides the base method and doesn't support polymorphism.

```csharp
public class Shape
{
    public virtual void Draw()
    {
        Console.WriteLine("Drawing a shape");
    }
    
    public void Area()
    {
        Console.WriteLine("Calculating area of shape");
    }
}

public class Circle : Shape
{
    public override void Draw()
    {
        Console.WriteLine("Drawing a circle");
    }
    
    // Method hiding - NOT recommended unless necessary
    public new void Area()
    {
        Console.WriteLine("Calculating area of circle: πr²");
    }
}

// Demonstration of the difference
Shape shape = new Shape();
Shape circleAsShape = new Circle();
Circle circle = new Circle();

// Virtual method - polymorphic behavior
shape.Draw();          // Output: Drawing a shape
circleAsShape.Draw();  // Output: Drawing a circle (polymorphic!)
circle.Draw();         // Output: Drawing a circle

// Non-virtual method - no polymorphism
shape.Area();          // Output: Calculating area of shape
circleAsShape.Area();  // Output: Calculating area of shape (base method!)
circle.Area();         // Output: Calculating area of circle (derived method)
```

### Inheritance Levels and Limits

C# supports multi-level inheritance (a chain of inheritance), but not multiple inheritance (inheriting from multiple classes at once). Understanding these limits helps you design your class hierarchies effectively.

```csharp
// Multi-level inheritance (allowed)
public class A { }
public class B : A { }  // B inherits from A
public class C : B { }  // C inherits from B (which includes A)

// Multiple inheritance (NOT allowed)
// public class D : A, B { }  // Error! Cannot have multiple base classes

// Workaround: Use interfaces for multiple "inheritance"
public interface IAlpha { void AlphaMethod(); }
public interface IBeta { void BetaMethod(); }

public class Combined : A, IAlpha, IBeta
{
    public void AlphaMethod() { }
    public void BetaMethod() { }
}
```

---

## 8. Polymorphism

### Understanding Polymorphism

Polymorphism, derived from Greek meaning "many shapes," is the ability of objects of different classes to be treated as objects of a common base class. At runtime, the actual type of the object determines which method implementation is called. This powerful concept enables writing flexible, extensible code that can work with objects of many types through a common interface.

There are two types of polymorphism in C#: compile-time polymorphism (also called static polymorphism) and runtime polymorphism (also called dynamic polymorphism). Compile-time polymorphism is achieved through method overloading and operator overloading, where the compiler determines which method to call based on the method signature. Runtime polymorphism is achieved through method overriding, where the actual method to execute is determined at runtime based on the object's actual type.

### Runtime Polymorphism with Virtual and Override

```csharp
public abstract class Employee
{
    public string Name { get; }
    public decimal BaseSalary { get; }
    
    protected Employee(string name, decimal baseSalary)
    {
        Name = name;
        BaseSalary = baseSalary;
    }
    
    // Virtual method - can be overridden
    public virtual decimal CalculateSalary()
    {
        return BaseSalary;
    }
    
    // Abstract method - MUST be overridden
    public abstract string GetRole();
    
    public void DisplayInfo()
    {
        Console.WriteLine($"{Name} - {GetRole()}");
        Console.WriteLine($"Salary: {CalculateSalary():C}");
    }
}

public class FullTimeEmployee : Employee
{
    public decimal AnnualBonus { get; }
    
    public FullTimeEmployee(string name, decimal baseSalary, decimal annualBonus)
        : base(name, baseSalary)
    {
        AnnualBonus = annualBonus;
    }
    
    public override decimal CalculateSalary()
    {
        return BaseSalary + (AnnualBonus / 12);
    }
    
    public override string GetRole() => "Full-Time Employee";
}

public class PartTimeEmployee : Employee
{
    public int HoursWorked { get; set; }
    public decimal HourlyRate { get; }
    
    public PartTimeEmployee(string name, decimal hourlyRate)
        : base(name, 0)
    {
        HourlyRate = hourlyRate;
    }
    
    public override decimal CalculateSalary()
    {
        return HoursWorked * HourlyRate;
    }
    
    public override string GetRole() => "Part-Time Employee";
}

public class Manager : Employee
{
    public decimal TeamBonus { get; }
    public int TeamSize { get; }
    
    public Manager(string name, decimal baseSalary, decimal teamBonus, int teamSize)
        : base(name, baseSalary)
    {
        TeamBonus = teamBonus;
        TeamSize = teamSize;
    }
    
    public override decimal CalculateSalary()
    {
        return BaseSalary + TeamBonus + (TeamSize * 100);  // Bonus per team member
    }
    
    public override string GetRole() => "Manager";
}

// Polymorphism in action
List<Employee> employees = new List<Employee>
{
    new FullTimeEmployee("Alice", 5000, 12000),
    new PartTimeEmployee("Bob", 25) { HoursWorked = 80 },
    new Manager("Charlie", 8000, 1000, 5)
};

foreach (Employee emp in employees)
{
    emp.DisplayInfo();  // Calls appropriate implementations
    Console.WriteLine();
}

// Output:
// Alice - Full-Time Employee
// Salary: $6,000.00
//
// Bob - Part-Time Employee
// Salary: $2,000.00
//
// Charlie - Manager
// Salary: $9,500.00
```

### Compile-Time Polymorphism (Method Overloading)

```csharp
public class Calculator
{
    // Method overloading - same name, different parameters
    public int Add(int a, int b)
    {
        return a + b;
    }
    
    public int Add(int a, int b, int c)
    {
        return a + b + c;
    }
    
    public double Add(double a, double b)
    {
        return a + b;
    }
    
    public decimal Add(decimal a, decimal b)
    {
        return a + b;
    }
    
    // Overloading with different parameter types
    public int Add(params int[] numbers)
    {
        return numbers.Sum();
    }
    
    // Overloading with optional parameters
    public int Multiply(int a, int b, int c = 1)
    {
        return a * b * c;
    }
}

Calculator calc = new Calculator();
Console.WriteLine(calc.Add(1, 2));           // 3
Console.WriteLine(calc.Add(1, 2, 3));       // 6
Console.WriteLine(calc.Add(1.5, 2.5));      // 4.0
Console.WriteLine(calc.Add(1, 2, 3, 4, 5)); // 15
```

### Polymorphism with Interfaces

Interfaces provide another powerful way to achieve polymorphism. Multiple unrelated classes can implement the same interface, allowing them to be treated uniformly.

```csharp
public interface IDrawable
{
    void Draw();
}

public interface IResizable
{
    void Resize(double factor);
}

public class Square : IDrawable, IResizable
{
    public double Side { get; set; }
    
    public Square(double side) => Side = side;
    
    public void Draw() => Console.WriteLine($"Drawing a square with side {Side}");
    
    public void Resize(double factor) => Side *= factor;
}

public class Triangle : IDrawable
{
    public double Base { get; set; }
    public double Height { get; set; }
    
    public Triangle(double @base, double height)
    {
        Base = @base;
        Height = height;
    }
    
    public void Draw() => 
        Console.WriteLine($"Drawing a triangle with base {Base} and height {Height}");
}

public class Circle : IDrawable, IResizable
{
    public double Radius { get; set; }
    
    public Circle(double radius) => Radius = radius;
    
    public void Draw() => Console.WriteLine($"Drawing a circle with radius {Radius}");
    
    public void Resize(double factor) => Radius *= factor;
}

// Polymorphic behavior through interface
List<IDrawable> shapes = new List<IDrawable>
{
    new Square(5),
    new Triangle(3, 4),
    new Circle(2.5)
};

foreach (IDrawable shape in shapes)
{
    shape.Draw();
    
    if (shape is IResizable resizable)
    {
        resizable.Resize(2);
        Console.WriteLine("  Resized!");
    }
}

// Output:
// Drawing a square with side 5
//   Resized!
// Drawing a triangle with base 3 and height 4
// Drawing a circle with radius 2.5
//   Resized!
```

---

## 9. Abstract Classes and Methods

### Understanding Abstract Classes

An abstract class is a class that cannot be instantiated directly and is designed to be inherited by other classes. Abstract classes provide a partial implementation that derived classes can build upon. They can contain both abstract members (which must be implemented by derived classes) and concrete members (which provide implementation that derived classes inherit or can override).

Abstract classes are useful when you want to define common functionality that multiple related classes share, while also requiring derived classes to provide specific implementations. They serve as a middle ground between interfaces (which define contracts without implementation) and concrete classes (which provide complete implementations).

```csharp
// Abstract class - cannot be instantiated
public abstract class Shape
{
    // Properties
    public string Color { get; set; }
    public string Name { get; protected set; }
    
    // Constructor - called by derived classes
    protected Shape(string name, string color)
    {
        Name = name;
        Color = color;
    }
    
    // Abstract method - MUST be implemented by derived classes
    public abstract double CalculateArea();
    
    public abstract double CalculatePerimeter();
    
    // Virtual method - CAN be overridden by derived classes
    public virtual void DisplayInfo()
    {
        Console.WriteLine($"{Name} (Color: {Color})");
        Console.WriteLine($"  Area: {CalculateArea():F2}");
        Console.WriteLine($"  Perimeter: {CalculatePerimeter():F2}");
    }
    
    // Concrete method - inherited as-is by derived classes
    public void Paint(string newColor)
    {
        Color = newColor;
        Console.WriteLine($"{Name} painted {newColor}");
    }
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public Rectangle(double width, double height, string color)
        : base("Rectangle", color)
    {
        Width = width;
        Height = height;
    }
    
    public override double CalculateArea() => Width * Height;
    
    public override double CalculatePerimeter() => 2 * (Width + Height);
    
    public override void DisplayInfo()
    {
        base.DisplayInfo();
        Console.WriteLine($"  Dimensions: {Width} x {Height}");
    }
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public Circle(double radius, string color)
        : base("Circle", color)
    {
        Radius = radius;
    }
    
    public override double CalculateArea() => Math.PI * Radius * Radius;
    
    public override double CalculatePerimeter() => 2 * Math.PI * Radius;
}

// Cannot instantiate abstract class
// Shape shape = new Shape();  // Error!

// Must instantiate concrete derived classes
Rectangle rect = new Rectangle(5, 3, "Red");
Circle circle = new Circle(4, "Blue");

rect.DisplayInfo();
circle.DisplayInfo();
```

### Abstract Properties

```csharp
public abstract class DatabaseConnection
{
    // Abstract property - must be implemented
    public abstract string ConnectionString { get; set; }
    
    // Abstract read-only property
    public abstract bool IsConnected { get; }
    
    // Abstract methods
    public abstract void Connect();
    public abstract void Disconnect();
    public abstract int ExecuteNonQuery(string query);
    public abstract T? ExecuteScalar<T>(string query);
    
    // Template method pattern - uses abstract methods
    public T ExecuteWithRetry<T>(string query, int maxRetries = 3)
    {
        for (int i = 0; i < maxRetries; i++)
        {
            try
            {
                if (!IsConnected) Connect();
                return ExecuteScalar<T>(query);
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Attempt {i + 1} failed: {ex.Message}");
                if (i == maxRetries - 1) throw;
                Thread.Sleep(1000 * (i + 1));  // Exponential backoff
            }
        }
        throw new Exception("Max retries exceeded");
    }
}

public class SqlConnection : DatabaseConnection
{
    private bool _isConnected;
    
    public override string ConnectionString { get; set; }
    public override bool IsConnected => _isConnected;
    
    public override void Connect()
    {
        Console.WriteLine($"Connecting to SQL Server: {ConnectionString}");
        _isConnected = true;
    }
    
    public override void Disconnect()
    {
        Console.WriteLine("Disconnecting from SQL Server");
        _isConnected = false;
    }
    
    public override int ExecuteNonQuery(string query)
    {
        Console.WriteLine($"Executing: {query}");
        return 1;  // Rows affected
    }
    
    public override T? ExecuteScalar<T>(string query)
    {
        Console.WriteLine($"Executing scalar: {query}");
        return default;
    }
}
```

### Abstract Class vs Interface

Understanding when to use abstract classes versus interfaces is crucial for good OOP design:

| Feature | Abstract Class | Interface |
|---------|---------------|-----------|
| Can have implementation | Yes | Yes (C# 8+ default methods) |
| Can have fields | Yes | No (only properties) |
| Can have constructors | Yes | No |
| Multiple inheritance | No (single) | Yes (multiple interfaces) |
| Access modifiers | Any | Public (implicitly) |
| Purpose | Share common implementation | Define contract |
| Version safety | Can add members | Risky to add members |

```csharp
// Use abstract class when classes are related and share code
public abstract class Vehicle
{
    public string Make { get; }
    public string Model { get; }
    
    protected Vehicle(string make, string model)
    {
        Make = make;
        Model = model;
    }
    
    public abstract void Start();
    public abstract void Stop();
    
    // Shared implementation
    public void Honk() => Console.WriteLine("Beep beep!");
}

// Use interface for unrelated classes with common behavior
public interface IRecordable
{
    void Save();
    void Delete();
}

public interface ICloneable<T>
{
    T Clone();
}

// Class using both
public class Car : Vehicle, IRecordable, ICloneable<Car>
{
    public Car(string make, string model) : base(make, model) { }
    
    public override void Start() => Console.WriteLine("Car starting...");
    public override void Stop() => Console.WriteLine("Car stopping...");
    
    public void Save() => Console.WriteLine("Saving car to database");
    public void Delete() => Console.WriteLine("Deleting car from database");
    
    public Car Clone() => new Car(Make, Model);
}
```

---

## 10. Interfaces

### Understanding Interfaces

An interface defines a contract that classes can implement. It specifies a set of methods, properties, events, and indexers that the implementing class must provide. Interfaces are purely about defining what a class can do, not how it does it. Unlike abstract classes, interfaces cannot contain fields, constructors, or destructors, and until C# 8, they couldn't contain any implementation at all.

Interfaces enable polymorphism across unrelated classes—a class can implement multiple interfaces, and different classes can implement the same interface. This makes interfaces extremely powerful for defining capabilities that can be added to any class, regardless of its position in the inheritance hierarchy.

```csharp
// Interface definition
public interface IRepository<T>
{
    T GetById(int id);
    IEnumerable<T> GetAll();
    void Add(T entity);
    void Update(T entity);
    void Delete(int id);
    bool Exists(int id);
}

// Interface implementation
public class CustomerRepository : IRepository<Customer>
{
    private readonly Dictionary<int, Customer> _customers = new();
    
    public Customer GetById(int id)
    {
        _customers.TryGetValue(id, out var customer);
        return customer;
    }
    
    public IEnumerable<Customer> GetAll() => _customers.Values;
    
    public void Add(Customer entity)
    {
        _customers[entity.Id] = entity;
    }
    
    public void Update(Customer entity)
    {
        if (Exists(entity.Id))
            _customers[entity.Id] = entity;
    }
    
    public void Delete(int id)
    {
        _customers.Remove(id);
    }
    
    public bool Exists(int id) => _customers.ContainsKey(id);
}

public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
}
```

### Explicit Interface Implementation

When a class implements multiple interfaces that have members with the same signature, or when you want to hide an interface member from the class's public API, you can use explicit interface implementation. Explicit interface members can only be accessed through the interface type, not through the class type directly.

```csharp
public interface IWriter
{
    void Write(string content);
}

public interface IReader
{
    string Read();
}

public interface IFileHandler
{
    void Process(string path);
}

public class Document : IWriter, IReader, IFileHandler
{
    // Implicit implementation (accessible from class or interface)
    public void Write(string content)
    {
        Console.WriteLine($"Writing: {content}");
    }
    
    // Implicit implementation
    public string Read()
    {
        Console.WriteLine("Reading content...");
        return "Sample content";
    }
    
    // Explicit implementation (only accessible from interface)
    void IFileHandler.Process(string path)
    {
        Console.WriteLine($"Processing file: {path}");
    }
}

// Using the implementations
Document doc = new Document();
doc.Write("Hello");        // OK - implicit implementation
doc.Read();                // OK - implicit implementation
// doc.Process("file.txt"); // Error! Not accessible through class type

// Must use interface type for explicit implementation
IFileHandler handler = doc;
handler.Process("file.txt");  // OK - accessible through interface

// Another example: resolving naming conflicts
public interface ICounter
{
    void Increment();
    int Count { get; }
}

public interface IAdvancedCounter
{
    void Increment();      // Same method name
    void Reset();
    int Count { get; }     // Same property name
}

public class SmartCounter : ICounter, IAdvancedCounter
{
    private int _basicCount;
    private int _advancedCount;
    
    // Implicit implementation for ICounter
    public int Count => _basicCount;
    
    public void Increment()
    {
        _basicCount++;
        Console.WriteLine($"Basic count: {_basicCount}");
    }
    
    // Explicit implementation for IAdvancedCounter
    int IAdvancedCounter.Count => _advancedCount;
    
    void IAdvancedCounter.Increment()
    {
        _advancedCount += 10;
        Console.WriteLine($"Advanced count: {_advancedCount}");
    }
    
    public void Reset()
    {
        _basicCount = 0;
        _advancedCount = 0;
    }
}

SmartCounter counter = new SmartCounter();
counter.Increment();        // Calls ICounter.Increment: Basic count: 1

ICounter basicCounter = counter;
basicCounter.Increment();   // Same as above: Basic count: 2

IAdvancedCounter advancedCounter = counter;
advancedCounter.Increment(); // Calls IAdvancedCounter.Increment: Advanced count: 10
```

### Default Interface Methods (C# 8+)

Starting with C# 8, interfaces can provide default implementations for methods. This allows adding new members to interfaces without breaking existing implementations. Classes are not required to override default interface methods, but they can if they want different behavior.

```csharp
public interface ILogger
{
    void Log(string message);
    
    // Default implementation
    void LogError(string message) => Log($"[ERROR] {message}");
    
    void LogWarning(string message) => Log($"[WARNING] {message}");
    
    void LogInfo(string message) => Log($"[INFO] {message}");
}

public class ConsoleLogger : ILogger
{
    public void Log(string message) => Console.WriteLine(message);
    
    // Inherits default implementations for LogError, LogWarning, LogInfo
}

public class FileLogger : ILogger
{
    private readonly string _filePath;
    
    public FileLogger(string filePath) => _filePath = filePath;
    
    public void Log(string message) => File.AppendAllText(_filePath, message + "\n");
    
    // Override default implementation
    public void LogError(string message) => Log($"{DateTime.Now} [ERROR] {message}");
}

// Usage
ILogger logger = new ConsoleLogger();
logger.LogInfo("Application started");    // Uses default implementation
logger.LogError("Something went wrong");  // Uses default implementation
```

### Interface Inheritance

Interfaces can inherit from other interfaces, creating a hierarchy of contracts. A class implementing a derived interface must implement all members from the entire interface hierarchy.

```csharp
public interface IReadable
{
    string Read();
}

public interface IWritable
{
    void Write(string content);
}

// Interface inheritance
public interface IStream : IReadable, IWritable
{
    long Position { get; set; }
    long Length { get; }
    void Seek(long offset);
    void Flush();
    void Close();
}

public class MemoryStream : IStream
{
    private byte[] _buffer;
    private long _position;
    
    public MemoryStream(int capacity)
    {
        _buffer = new byte[capacity];
        _position = 0;
    }
    
    public long Position
    {
        get => _position;
        set => _position = value;
    }
    
    public long Length => _buffer.Length;
    
    public void Seek(long offset) => _position = offset;
    
    public void Flush() { /* No-op for memory */ }
    
    public void Close() { /* Release resources */ }
    
    // IReadable implementation
    public string Read()
    {
        // Simplified implementation
        return System.Text.Encoding.UTF8.GetString(_buffer, 0, (int)_position);
    }
    
    // IWritable implementation
    public void Write(string content)
    {
        var bytes = System.Text.Encoding.UTF8.GetBytes(content);
        bytes.CopyTo(_buffer, _position);
        _position += bytes.Length;
    }
}
```

---

## 11. Sealed Classes and Methods

### Sealed Classes

A sealed class cannot be inherited. This is useful when you want to prevent derivation, either for security reasons (preventing malicious overrides), design reasons (the class isn't designed for inheritance), or performance reasons (the runtime can optimize sealed classes). Many .NET framework classes are sealed, including `string`, `DateTime`, and various value types.

```csharp
// Sealed class - cannot be inherited
public sealed class CurrencyConverter
{
    private readonly Dictionary<string, decimal> _rates;
    
    public CurrencyConverter()
    {
        _rates = new Dictionary<string, decimal>
        {
            ["USD"] = 1.0m,
            ["EUR"] = 0.85m,
            ["GBP"] = 0.73m,
            ["JPY"] = 110.0m
        };
    }
    
    public decimal Convert(decimal amount, string from, string to)
    {
        if (!_rates.ContainsKey(from) || !_rates.ContainsKey(to))
            throw new ArgumentException("Unknown currency");
        
        // Convert to USD first, then to target
        decimal usdAmount = amount / _rates[from];
        return usdAmount * _rates[to];
    }
}

// Cannot inherit from sealed class
// public class MyConverter : CurrencyConverter { }  // Error!

// Sealed class with static factory
public sealed class Configuration
{
    public string ConnectionString { get; }
    public string ApplicationName { get; }
    public int Timeout { get; }
    
    private Configuration(string connectionString, string appName, int timeout)
    {
        ConnectionString = connectionString;
        ApplicationName = appName;
        Timeout = timeout;
    }
    
    // Factory method - only way to create instances
    public static Configuration Load(string filePath)
    {
        // Load from file...
        return new Configuration(
            "Server=localhost;Database=MyDb",
            "MyApp",
            30
        );
    }
    
    public static Configuration Default => new Configuration(
        "Server=localhost",
        "DefaultApp",
        15
    );
}
```

### Sealed Methods

In a derived class, you can seal a virtual method by marking it with `sealed` along with `override`. This prevents further overriding in classes that derive from your class. This is useful when you've provided a specific implementation that shouldn't be changed further down the inheritance chain.

```csharp
public class BaseClass
{
    public virtual void Method1() => Console.WriteLine("Base.Method1");
    public virtual void Method2() => Console.WriteLine("Base.Method2");
}

public class DerivedClass : BaseClass
{
    public override void Method1() => Console.WriteLine("Derived.Method1");
    
    // Sealed override - cannot be overridden further
    public sealed override void Method2() => Console.WriteLine("Derived.Method2 (sealed)");
}

public class FurtherDerived : DerivedClass
{
    public override void Method1() => Console.WriteLine("FurtherDerived.Method1");  // OK
    
    // Cannot override sealed method
    // public override void Method2() { }  // Error!
}

// Real-world example
public abstract class ControllerBase
{
    public virtual void Initialize() { /* Base initialization */ }
    public virtual void Execute() { /* Base execution logic */ }
    public virtual void Cleanup() { /* Base cleanup */ }
}

public class ApiController : ControllerBase
{
    public override void Initialize()
    {
        base.Initialize();
        Console.WriteLine("API controller initialized");
    }
    
    // Seal the Execute method - this is the final implementation
    public sealed override void Execute()
    {
        Console.WriteLine("API execution - this cannot be changed");
    }
}

// Trying to override Execute in a derived class would be an error
public class SecureApiController : ApiController
{
    public override void Initialize()
    {
        base.Initialize();
        Console.WriteLine("Secure initialization");
    }
    
    // Cannot override Execute - it's sealed in ApiController
}
```

---

## 12. Static Members and Classes

### Static Members

Static members belong to the class itself rather than to any specific instance. They are shared among all instances of the class and can be accessed without creating an instance. Static members are useful for utility methods, factory methods, and data that should be shared across all instances.

```csharp
public class MathUtility
{
    // Static field - shared across all instances
    private static int _calculationCount;
    
    // Static property
    public static int CalculationCount => _calculationCount;
    
    // Static method - no instance needed
    public static double Square(double x)
    {
        _calculationCount++;
        return x * x;
    }
    
    public static double Cube(double x)
    {
        _calculationCount++;
        return x * x * x;
    }
    
    // Static method with overloads
    public static int Max(int a, int b) => a > b ? a : b;
    public static int Max(int a, int b, int c) => Max(Max(a, b), c);
    
    // Instance members can access static members
    public void PrintCalculationCount()
    {
        Console.WriteLine($"Total calculations: {_calculationCount}");
    }
}

// Using static members without creating instance
double result = MathUtility.Square(5);  // 25
Console.WriteLine(MathUtility.CalculationCount);  // 1

// Can still create instance for instance methods
MathUtility utility = new MathUtility();
utility.PrintCalculationCount();
```

### Static Classes

A static class cannot be instantiated and cannot be inherited. All members of a static class must be static. Static classes are ideal for utility classes, extension method containers, and organizing related methods that don't need instance state.

```csharp
public static class StringExtensions
{
    // Extension methods must be in a static class
    public static string Reverse(this string input)
    {
        if (string.IsNullOrEmpty(input)) return input;
        char[] chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
    
    public static string Truncate(this string input, int maxLength, string suffix = "...")
    {
        if (string.IsNullOrEmpty(input) || input.Length <= maxLength)
            return input;
        return input.Substring(0, maxLength - suffix.Length) + suffix;
    }
    
    public static bool IsNullOrEmpty(this string input)
    {
        return string.IsNullOrEmpty(input);
    }
    
    public static string ToTitleCase(this string input)
    {
        if (string.IsNullOrEmpty(input)) return input;
        return System.Globalization.CultureInfo.CurrentCulture.TextInfo.ToTitleCase(input.ToLower());
    }
}

// Using extension methods
string text = "hello world";
Console.WriteLine(text.Reverse());      // "dlrow olleh"
Console.WriteLine(text.ToTitleCase());  // "Hello World"
Console.WriteLine(text.Truncate(7));    // "he..."

// Static utility class
public static class Guard
{
    public static void AgainstNull(object argument, string paramName)
    {
        if (argument == null)
            throw new ArgumentNullException(paramName);
    }
    
    public static void AgainstNullOrEmpty(string argument, string paramName)
    {
        if (string.IsNullOrEmpty(argument))
            throw new ArgumentException("Value cannot be null or empty", paramName);
    }
    
    public static void AgainstNegative(int value, string paramName)
    {
        if (value < 0)
            throw new ArgumentOutOfRangeException(paramName, "Value cannot be negative");
    }
    
    public static void AgainstOutOfRange(int value, int min, int max, string paramName)
    {
        if (value < min || value > max)
            throw new ArgumentOutOfRangeException(paramName, 
                $"Value must be between {min} and {max}");
    }
}

// Using Guard
public void ProcessOrder(Order order)
{
    Guard.AgainstNull(order, nameof(order));
    Guard.AgainstNullOrEmpty(order.CustomerName, nameof(order.CustomerName));
    Guard.AgainstNegative(order.Quantity, nameof(order.Quantity));
    // Process order...
}
```

### Static Constructors and Initialization

```csharp
public class DatabaseSettings
{
    // Static fields with initialization
    public static readonly string DefaultConnectionString;
    public static readonly Dictionary<string, string> ConnectionStrings;
    
    // Static constructor - runs once before any static member access
    static DatabaseSettings()
    {
        Console.WriteLine("Static constructor running...");
        
        // Load configuration
        DefaultConnectionString = "Server=localhost;Database=DefaultDb";
        ConnectionStrings = new Dictionary<string, string>
        {
            ["Production"] = "Server=prod-server;Database=ProdDb",
            ["Development"] = "Server=localhost;Database=DevDb",
            ["Testing"] = "Server=test-server;Database=TestDb"
        };
    }
    
    // Static property with lazy initialization
    private static Lazy<Random> _random = new Lazy<Random>(() => new Random());
    public static Random Random => _random.Value;
    
    // Cannot create instance
    private DatabaseSettings() { }
}

// Accessing static members triggers static constructor
Console.WriteLine(DatabaseSettings.DefaultConnectionString);
```

---

## 13. Partial Classes and Methods

### Partial Classes

The `partial` keyword allows you to split the definition of a class, struct, interface, or method across multiple files. All parts are combined at compile time into a single type. Partial classes are particularly useful when working with auto-generated code (like Entity Framework models or Windows Forms designer), allowing developers to add custom code in a separate file without modifying the generated code.

```csharp
// File: Customer.generated.cs (auto-generated)
public partial class Customer
{
    public int Id { get; set; }
    public string Name { get; set; }
    public string Email { get; set; }
}

// File: Customer.cs (developer-written)
public partial class Customer
{
    // Additional properties
    public string DisplayName => Name ?? Email?.Split('@')[0] ?? "Unknown";
    
    // Methods
    public bool IsValidEmail()
    {
        try
        {
            var addr = new System.Net.Mail.MailAddress(Email);
            return addr.Address == Email;
        }
        catch
        {
            return false;
        }
    }
    
    public override string ToString() => $"Customer: {DisplayName} ({Id})";
}

// Another partial file: Customer.Business.cs
public partial class Customer
{
    public decimal CalculateDiscount(decimal orderTotal)
    {
        // Business logic
        if (orderTotal > 1000) return orderTotal * 0.10m;
        if (orderTotal > 500) return orderTotal * 0.05m;
        return 0;
    }
}

// At compile time, all parts are combined into one Customer class
Customer customer = new Customer { Id = 1, Name = "Alice", Email = "alice@example.com" };
Console.WriteLine(customer.DisplayName);     // Alice
Console.WriteLine(customer.IsValidEmail());  // True
```

### Partial Methods

Partial methods allow one part of a partial class to define a method signature, and another part to optionally provide an implementation. If no implementation is provided, the method and all calls to it are removed by the compiler. This is useful for generated code that may need optional extensibility points.

```csharp
// File: Entity.generated.cs
public partial class Entity
{
    public int Id { get; set; }
    
    // Partial method declaration - no implementation
    partial void OnSaving();
    partial void OnSaved();
    partial void OnDeleting();
    
    public void Save()
    {
        OnSaving();  // Call the partial method (may or may not exist)
        
        Console.WriteLine($"Saving entity {Id} to database...");
        
        OnSaved();  // Call after save
    }
    
    public void Delete()
    {
        OnDeleting();  // Call before delete
        
        Console.WriteLine($"Deleting entity {Id}...");
    }
}

// File: Entity.cs (developer customization)
public partial class Entity
{
    // Optional implementation of partial method
    partial void OnSaving()
    {
        Console.WriteLine("Validating before save...");
        // Add validation logic
    }
    
    // Another optional implementation
    partial void OnSaved()
    {
        Console.WriteLine("Entity saved successfully!");
        // Could log, update cache, etc.
    }
    
    // OnDeleting is NOT implemented - calls to it are removed by compiler
}

// Usage
Entity entity = new Entity { Id = 1 };
entity.Save();
// Output:
// Validating before save...
// Saving entity 1 to database...
// Entity saved successfully!
```

---

## 14. Generics in OOP

### Understanding Generics

Generics allow you to define classes, interfaces, and methods with type parameters that are specified when the generic type is used. This provides type safety, code reuse, and performance benefits by avoiding boxing/unboxing and type casting. Generics are fundamental to many C# collections and patterns.

```csharp
// Generic class
public class Repository<T> where T : class, new()
{
    private readonly List<T> _items = new();
    
    public void Add(T item)
    {
        ArgumentNullException.ThrowIfNull(item);
        _items.Add(item);
    }
    
    public void Remove(T item) => _items.Remove(item);
    
    public T? Find(Func<T, bool> predicate) => _items.FirstOrDefault(predicate);
    
    public IEnumerable<T> GetAll() => _items;
    
    public int Count => _items.Count;
    
    public void Clear() => _items.Clear();
}

// Generic interface
public interface IValidator<T>
{
    bool Validate(T item);
    IEnumerable<string> GetErrors(T item);
}

// Generic interface with multiple type parameters
public interface IConverter<TInput, TOutput>
{
    TOutput Convert(TInput input);
}

// Using generic classes
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public decimal Price { get; set; }
}

var productRepo = new Repository<Product>();
productRepo.Add(new Product { Id = 1, Name = "Laptop", Price = 999.99m });

Product? laptop = productRepo.Find(p => p.Name == "Laptop");
```

### Generic Constraints

Constraints specify what types can be used as type arguments, enabling you to use members of the constrained type within the generic class.

```csharp
// Type constraint: T must be a reference type
public class ReferenceRepository<T> where T : class
{
    public T? FindById(int id) { /* implementation */ return default; }
}

// Type constraint: T must be a value type
public class ValueContainer<T> where T : struct
{
    public T Value { get; set; }
    public bool HasValue { get; set; }
}

// Constructor constraint: T must have a parameterless constructor
public class Factory<T> where T : new()
{
    public T Create() => new T();
}

// Base class constraint: T must inherit from specified class
public class AnimalRepository<T> where T : Animal
{
    public void MakeSound(T animal) => animal.MakeSound();
}

// Interface constraint: T must implement interface
public class Validator<T> where T : IValidatable
{
    public bool IsValid(T item) => item.Validate();
}

// Multiple constraints
public class AdvancedRepository<T> where T : class, IEntity, ICloneable, new()
{
    public T CreateCopy(T original)
    {
        return (T)original.Clone();
    }
}

// Generic method with constraints
public class Processor
{
    public T Process<T>(T item) where T : IProcessable
    {
        item.Process();
        return item;
    }
    
    // Generic method with multiple type parameters
    public TResult Convert<TInput, TResult>(TInput input, IConverter<TInput, TResult> converter)
    {
        return converter.Convert(input);
    }
}
```

### Generic Inheritance

Generic classes can inherit from other generic classes and implement generic interfaces.

```csharp
// Base generic class
public abstract class RepositoryBase<T> where T : class, IEntity
{
    protected readonly List<T> _items = new();
    
    public virtual void Add(T item) => _items.Add(item);
    public virtual void Remove(T item) => _items.Remove(item);
    public T? GetById(int id) => _items.FirstOrDefault(i => i.Id == id);
    public IEnumerable<T> GetAll() => _items;
}

// Derived generic class
public class CachedRepository<T> : RepositoryBase<T> where T : class, IEntity
{
    private readonly Dictionary<int, T> _cache = new();
    
    public override void Add(T item)
    {
        base.Add(item);
        _cache[item.Id] = item;
    }
    
    public new T? GetById(int id)
    {
        if (_cache.TryGetValue(id, out var cached))
            return cached;
        
        var item = base.GetById(id);
        if (item != null)
            _cache[id] = item;
        
        return item;
    }
    
    public void ClearCache() => _cache.Clear();
}

// Non-generic derived class
public class ProductRepository : RepositoryBase<Product>
{
    public IEnumerable<Product> GetByCategory(string category)
    {
        return _items.Where(p => p.Category == category);
    }
    
    public IEnumerable<Product> GetExpensive(decimal threshold)
    {
        return _items.Where(p => p.Price > threshold);
    }
}
```

---

## 15. Operator Overloading

### Understanding Operator Overloading

Operator overloading allows you to define how operators (like +, -, ==, <, etc.) behave with your custom types. This makes your types feel like built-in types and can make code more readable and intuitive. Not all operators can be overloaded, and some have specific rules for overloading.

```csharp
public struct Point
{
    public double X { get; }
    public double Y { get; }
    
    public Point(double x, double y)
    {
        X = x;
        Y = y;
    }
    
    // Overload unary + operator
    public static Point operator +(Point p) => p;
    
    // Overload unary - operator
    public static Point operator -(Point p) => new Point(-p.X, -p.Y);
    
    // Overload binary + operator
    public static Point operator +(Point a, Point b) => 
        new Point(a.X + b.X, a.Y + b.Y);
    
    // Overload binary - operator
    public static Point operator -(Point a, Point b) => 
        new Point(a.X - b.X, a.Y - b.Y);
    
    // Overload * operator (scalar multiplication)
    public static Point operator *(Point p, double scalar) => 
        new Point(p.X * scalar, p.Y * scalar);
    
    public static Point operator *(double scalar, Point p) => p * scalar;
    
    // Overload / operator (scalar division)
    public static Point operator /(Point p, double scalar) => 
        new Point(p.X / scalar, p.Y / scalar);
    
    // Overload == and != operators (must override Equals and GetHashCode)
    public static bool operator ==(Point a, Point b) => 
        Math.Abs(a.X - b.X) < double.Epsilon && Math.Abs(a.Y - b.Y) < double.Epsilon;
    
    public static bool operator !=(Point a, Point b) => !(a == b);
    
    // Overload comparison operators
    public static bool operator <(Point a, Point b) => 
        a.DistanceFromOrigin() < b.DistanceFromOrigin();
    
    public static bool operator >(Point a, Point b) => 
        a.DistanceFromOrigin() > b.DistanceFromOrigin();
    
    public static bool operator <=(Point a, Point b) => 
        a.DistanceFromOrigin() <= b.DistanceFromOrigin();
    
    public static bool operator >=(Point a, Point b) => 
        a.DistanceFromOrigin() >= b.DistanceFromOrigin();
    
    // Helper method
    private double DistanceFromOrigin() => Math.Sqrt(X * X + Y * Y);
    
    // Must override these when overloading == and !=
    public override bool Equals(object? obj) => 
        obj is Point p && this == p;
    
    public override int GetHashCode() => HashCode.Combine(X, Y);
    
    public override string ToString() => $"({X}, {Y})";
}

// Using overloaded operators
Point p1 = new Point(3, 4);
Point p2 = new Point(1, 2);

Point sum = p1 + p2;       // (4, 6)
Point diff = p1 - p2;      // (2, 2)
Point scaled = p1 * 2;     // (6, 8)
Point negated = -p1;       // (-3, -4)

bool areEqual = p1 == new Point(3, 4);  // true
bool isLess = p2 < p1;                  // true (distance from origin)
```

### Overloading True/False Operators

```csharp
public struct TruthValue
{
    public bool? Value { get; }
    
    public TruthValue(bool? value) => Value = value;
    
    // Overload true operator
    public static bool operator true(TruthValue tv) => tv.Value == true;
    
    // Overload false operator
    public static bool operator false(TruthValue tv) => tv.Value == false;
    
    // Overload logical operators
    public static TruthValue operator !(TruthValue tv)
    {
        return tv.Value switch
        {
            true => new TruthValue(false),
            false => new TruthValue(true),
            null => new TruthValue(null)
        };
    }
    
    public static TruthValue operator &(TruthValue a, TruthValue b)
    {
        if (a.Value == false || b.Value == false)
            return new TruthValue(false);
        if (a.Value == true && b.Value == true)
            return new TruthValue(true);
        return new TruthValue(null);
    }
    
    public static TruthValue operator |(TruthValue a, TruthValue b)
    {
        if (a.Value == true || b.Value == true)
            return new TruthValue(true);
        if (a.Value == false && b.Value == false)
            return new TruthValue(false);
        return new TruthValue(null);
    }
    
    public override string ToString() => Value?.ToString() ?? "Unknown";
}

// Using in conditional
TruthValue tv = new TruthValue(true);
if (tv)  // Uses overloaded true operator
{
    Console.WriteLine("Value is true");
}
```

---

## 16. Design Patterns in OOP

### Creational Patterns

**Singleton Pattern** ensures a class has only one instance and provides a global point of access to it.

```csharp
// Thread-safe singleton
public sealed class Singleton
{
    private static readonly Lazy<Singleton> _instance = 
        new Lazy<Singleton>(() => new Singleton());
    
    public static Singleton Instance => _instance.Value;
    
    private Singleton()
    {
        // Private constructor prevents instantiation
    }
    
    public void DoSomething()
    {
        Console.WriteLine("Singleton doing something...");
    }
}

// Usage
Singleton.Instance.DoSomething();

// Alternative: Dependency Injection (preferred in modern C#)
// Register as singleton in DI container
// services.AddSingleton<ISingletonService, SingletonService>();
```

**Factory Pattern** creates objects without specifying their exact classes.

```csharp
// Product interface
public interface IPaymentProcessor
{
    void ProcessPayment(decimal amount);
}

// Concrete products
public class CreditCardProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount) => 
        Console.WriteLine($"Processing credit card payment: ${amount}");
}

public class PayPalProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount) => 
        Console.WriteLine($"Processing PayPal payment: ${amount}");
}

public class BankTransferProcessor : IPaymentProcessor
{
    public void ProcessPayment(decimal amount) => 
        Console.WriteLine($"Processing bank transfer: ${amount}");
}

// Factory
public static class PaymentProcessorFactory
{
    public static IPaymentProcessor Create(string paymentMethod)
    {
        return paymentMethod.ToLower() switch
        {
            "creditcard" => new CreditCardProcessor(),
            "paypal" => new PayPalProcessor(),
            "bank" => new BankTransferProcessor(),
            _ => throw new ArgumentException($"Unknown payment method: {paymentMethod}")
        };
    }
}

// Usage
IPaymentProcessor processor = PaymentProcessorFactory.Create("creditcard");
processor.ProcessPayment(99.99m);
```

**Builder Pattern** separates the construction of complex objects from their representation.

```csharp
public class Email
{
    public string To { get; set; } = "";
    public string From { get; set; } = "";
    public string Subject { get; set; } = "";
    public string Body { get; set; } = "";
    public List<string> Cc { get; set; } = new();
    public List<string> Bcc { get; set; } = new();
    public List<Attachment> Attachments { get; set; } = new();
    public bool IsHtml { get; set; }
    public Priority Priority { get; set; } = Priority.Normal;
    
    public void Send() => Console.WriteLine($"Sending email to: {To}");
}

public enum Priority { Low, Normal, High }

public class EmailBuilder
{
    private readonly Email _email = new();
    
    public EmailBuilder To(string to)
    {
        _email.To = to;
        return this;
    }
    
    public EmailBuilder From(string from)
    {
        _email.From = from;
        return this;
    }
    
    public EmailBuilder Subject(string subject)
    {
        _email.Subject = subject;
        return this;
    }
    
    public EmailBuilder Body(string body)
    {
        _email.Body = body;
        return this;
    }
    
    public EmailBuilder AsHtml()
    {
        _email.IsHtml = true;
        return this;
    }
    
    public EmailBuilder AddCc(string cc)
    {
        _email.Cc.Add(cc);
        return this;
    }
    
    public EmailBuilder WithPriority(Priority priority)
    {
        _email.Priority = priority;
        return this;
    }
    
    public Email Build() => _email;
}

// Fluent builder usage
Email email = new EmailBuilder()
    .To("recipient@example.com")
    .From("sender@example.com")
    .Subject("Hello")
    .Body("<h1>Hello World</h1>")
    .AsHtml()
    .AddCc("manager@example.com")
    .WithPriority(Priority.High)
    .Build();

email.Send();
```

### Structural Patterns

**Adapter Pattern** allows incompatible interfaces to work together.

```csharp
// Target interface
public interface ITarget
{
    string GetRequest();
}

// Adaptee (existing class with incompatible interface)
public class Adaptee
{
    public string GetSpecificRequest() => "Specific request from Adaptee";
}

// Adapter
public class Adapter : ITarget
{
    private readonly Adaptee _adaptee;
    
    public Adapter(Adaptee adaptee) => _adaptee = adaptee;
    
    public string GetRequest() => 
        $"Adapted: {_adaptee.GetSpecificRequest()}";
}

// Usage
Adaptee adaptee = new Adaptee();
ITarget target = new Adapter(adaptee);
Console.WriteLine(target.GetRequest());
// Output: Adapted: Specific request from Adaptee
```

**Decorator Pattern** adds behavior to objects dynamically.

```csharp
// Component interface
public interface INotifier
{
    void Send(string message);
}

// Concrete component
public class Notifier : INotifier
{
    public void Send(string message) => Console.WriteLine($"Sending: {message}");
}

// Base decorator
public abstract class NotifierDecorator : INotifier
{
    protected readonly INotifier _wrapped;
    
    protected NotifierDecorator(INotifier wrapped) => _wrapped = wrapped;
    
    public virtual void Send(string message) => _wrapped.Send(message);
}

// Concrete decorators
public class EmailNotifier : NotifierDecorator
{
    public EmailNotifier(INotifier wrapped) : base(wrapped) { }
    
    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"Also sending email: {message}");
    }
}

public class SmsNotifier : NotifierDecorator
{
    public SmsNotifier(INotifier wrapped) : base(wrapped) { }
    
    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"Also sending SMS: {message}");
    }
}

public class SlackNotifier : NotifierDecorator
{
    public SlackNotifier(INotifier wrapped) : base(wrapped) { }
    
    public override void Send(string message)
    {
        base.Send(message);
        Console.WriteLine($"Also sending Slack notification: {message}");
    }
}

// Usage - stack decorators
INotifier notifier = new Notifier();
notifier = new EmailNotifier(notifier);
notifier = new SmsNotifier(notifier);
notifier = new SlackNotifier(notifier);

notifier.Send("Hello World!");
```

### Behavioral Patterns

**Strategy Pattern** defines a family of algorithms and makes them interchangeable.

```csharp
// Strategy interface
public interface IDiscountStrategy
{
    decimal ApplyDiscount(decimal amount);
}

// Concrete strategies
public class NoDiscount : IDiscountStrategy
{
    public decimal ApplyDiscount(decimal amount) => amount;
}

public class PercentageDiscount : IDiscountStrategy
{
    private readonly decimal _percentage;
    
    public PercentageDiscount(decimal percentage) => _percentage = percentage;
    
    public decimal ApplyDiscount(decimal amount) => 
        amount * (1 - _percentage / 100);
}

public class FixedDiscount : IDiscountStrategy
{
    private readonly decimal _fixedAmount;
    
    public FixedDiscount(decimal fixedAmount) => _fixedAmount = fixedAmount;
    
    public decimal ApplyDiscount(decimal amount) => 
        Math.Max(0, amount - _fixedAmount);
}

// Context
public class ShoppingCart
{
    private readonly List<decimal> _items = new();
    private IDiscountStrategy _discountStrategy = new NoDiscount();
    
    public void AddItem(decimal price) => _items.Add(price);
    
    public void SetDiscountStrategy(IDiscountStrategy strategy) => 
        _discountStrategy = strategy;
    
    public decimal CalculateTotal()
    {
        decimal subtotal = _items.Sum();
        return _discountStrategy.ApplyDiscount(subtotal);
    }
}

// Usage
var cart = new ShoppingCart();
cart.AddItem(100);
cart.AddItem(50);
cart.AddItem(25);

cart.SetDiscountStrategy(new PercentageDiscount(10));  // 10% off
Console.WriteLine($"Total with 10% discount: {cart.CalculateTotal()}");  // 157.5

cart.SetDiscountStrategy(new FixedDiscount(20));  // $20 off
Console.WriteLine($"Total with $20 discount: {cart.CalculateTotal()}");  // 155
```

**Observer Pattern** defines a subscription mechanism to notify multiple objects about events.

```csharp
// Observer interface
public interface IObserver
{
    void Update(string message);
}

// Subject interface
public interface ISubject
{
    void Attach(IObserver observer);
    void Detach(IObserver observer);
    void Notify(string message);
}

// Concrete subject
public class NewsAgency : ISubject
{
    private readonly List<IObserver> _observers = new();
    
    public void Attach(IObserver observer) => _observers.Add(observer);
    
    public void Detach(IObserver observer) => _observers.Remove(observer);
    
    public void Notify(string message)
    {
        foreach (var observer in _observers)
        {
            observer.Update(message);
        }
    }
    
    public void PublishNews(string news)
    {
        Console.WriteLine($"NewsAgency: Publishing news: {news}");
        Notify(news);
    }
}

// Concrete observers
public class NewsChannel : IObserver
{
    private readonly string _name;
    
    public NewsChannel(string name) => _name = name;
    
    public void Update(string message) => 
        Console.WriteLine($"  {_name} received news: {message}");
}

public class NewsWebsite : IObserver
{
    public void Update(string message) => 
        Console.WriteLine($"  Website updated with: {message}");
}

// Usage
NewsAgency agency = new NewsAgency();

agency.Attach(new NewsChannel("CNN"));
agency.Attach(new NewsChannel("BBC"));
agency.Attach(new NewsWebsite());

agency.PublishNews("Breaking: Major technology breakthrough!");
```

---

## 17. SOLID Principles

### Single Responsibility Principle (SRP)

A class should have only one reason to change, meaning it should have only one responsibility.

```csharp
// Bad: Multiple responsibilities
public class BadUserManager
{
    public void CreateUser(User user) { /* create user */ }
    public void SendEmail(User user, string message) { /* send email */ }
    public void GenerateReport(User user) { /* generate report */ }
    public void LogActivity(User user, string activity) { /* log */ }
}

// Good: Single responsibility
public class UserService
{
    private readonly IUserRepository _repository;
    
    public UserService(IUserRepository repository) => _repository = repository;
    
    public void CreateUser(User user) => _repository.Add(user);
    public User? GetUser(int id) => _repository.GetById(id);
    public void DeleteUser(int id) => _repository.Delete(id);
}

public class EmailService
{
    public void SendEmail(string to, string subject, string body)
    {
        // Email logic
    }
}

public class UserReportGenerator
{
    public string GenerateUserReport(User user) => $"Report for {user.Name}";
}

public class ActivityLogger
{
    public void Log(int userId, string activity)
    {
        // Logging logic
    }
}
```

### Open/Closed Principle (OCP)

Software entities should be open for extension but closed for modification.

```csharp
// Bad: Must modify class to add new shapes
public class BadAreaCalculator
{
    public double CalculateArea(object shape)
    {
        if (shape is Rectangle r) return r.Width * r.Height;
        if (shape is Circle c) return Math.PI * c.Radius * c.Radius;
        // Must add new if statement for each new shape!
        throw new ArgumentException("Unknown shape");
    }
}

// Good: Open for extension, closed for modification
public abstract class Shape
{
    public abstract double CalculateArea();
}

public class Rectangle : Shape
{
    public double Width { get; set; }
    public double Height { get; set; }
    
    public override double CalculateArea() => Width * Height;
}

public class Circle : Shape
{
    public double Radius { get; set; }
    
    public override double CalculateArea() => Math.PI * Radius * Radius;
}

// New shape added without modifying existing code
public class Triangle : Shape
{
    public double Base { get; set; }
    public double Height { get; set; }
    
    public override double CalculateArea() => 0.5 * Base * Height;
}

// Calculator works with any shape
public class AreaCalculator
{
    public double TotalArea(IEnumerable<Shape> shapes) => 
        shapes.Sum(s => s.CalculateArea());
}
```

### Liskov Substitution Principle (LSP)

Objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

```csharp
// Bad: Violates LSP
public class Bird
{
    public virtual void Fly() => Console.WriteLine("Flying");
}

public class Sparrow : Bird { }

public class Penguin : Bird
{
    public override void Fly() => throw new NotSupportedException("Penguins can't fly!");
}

// Good: Proper inheritance hierarchy
public abstract class Bird
{
    public abstract void Move();
}

public abstract class FlyingBird : Bird
{
    public override void Move() => Fly();
    public virtual void Fly() => Console.WriteLine("Flying");
}

public abstract class SwimmingBird : Bird
{
    public override void Move() => Swim();
    public virtual void Swim() => Console.WriteLine("Swimming");
}

public class Sparrow : FlyingBird { }

public class Penguin : SwimmingBird
{
    public override void Swim() => Console.WriteLine("Swimming like a penguin");
}
```

### Interface Segregation Principle (ISP)

Clients should not be forced to depend on interfaces they don't use.

```csharp
// Bad: Fat interface
public interface IWorker
{
    void Work();
    void Eat();
    void Sleep();
}

// Good: Segregated interfaces
public interface IWorkable
{
    void Work();
}

public interface IFeedable
{
    void Eat();
}

public interface ISleepable
{
    void Sleep();
}

public class HumanWorker : IWorkable, IFeedable, ISleepable
{
    public void Work() => Console.WriteLine("Working");
    public void Eat() => Console.WriteLine("Eating");
    public void Sleep() => Console.WriteLine("Sleeping");
}

public class RobotWorker : IWorkable
{
    public void Work() => Console.WriteLine("Working");
    // Robot doesn't need Eat() or Sleep()
}
```

### Dependency Inversion Principle (DIP)

High-level modules should not depend on low-level modules. Both should depend on abstractions.

```csharp
// Bad: High-level depends on low-level
public class BadOrderService
{
    private readonly SqlOrderRepository _repository = new();
    private readonly SmtpEmailSender _emailSender = new();
    
    public void ProcessOrder(Order order)
    {
        _repository.Save(order);
        _emailSender.SendEmail(order.CustomerEmail, "Order confirmed");
    }
}

// Good: Depend on abstractions
public interface IOrderRepository
{
    void Save(Order order);
    Order? GetById(int id);
}

public interface IEmailSender
{
    void SendEmail(string to, string subject, string body);
}

public class OrderService
{
    private readonly IOrderRepository _repository;
    private readonly IEmailSender _emailSender;
    
    // Dependencies injected through constructor
    public OrderService(IOrderRepository repository, IEmailSender emailSender)
    {
        _repository = repository;
        _emailSender = emailSender;
    }
    
    public void ProcessOrder(Order order)
    {
        _repository.Save(order);
        _emailSender.SendEmail(order.CustomerEmail, "Order Confirmed", 
            $"Your order #{order.Id} has been confirmed.");
    }
}

// Concrete implementations
public class SqlOrderRepository : IOrderRepository
{
    public void Save(Order order) { /* SQL implementation */ }
    public Order? GetById(int id) { /* SQL implementation */ return null; }
}

public class SmtpEmailSender : IEmailSender
{
    public void SendEmail(string to, string subject, string body) { /* SMTP implementation */ }
}

// Can easily swap implementations for testing
public class MockOrderRepository : IOrderRepository
{
    public void Save(Order order) => Console.WriteLine($"Mock save: {order.Id}");
    public Order? GetById(int id) => new Order { Id = id };
}
```

---

## 18. Best Practices and Common Pitfalls

### Best Practices

**1. Favor Composition Over Inheritance**

```csharp
// Inheritance can lead to rigid hierarchies
public class BadEmployee : Person
{
    // Tied to Person class forever
}

// Composition is more flexible
public class Employee
{
    private readonly Person _person;
    public string EmployeeId { get; }
    
    public Employee(Person person, string employeeId)
    {
        _person = person;
        EmployeeId = employeeId;
    }
    
    // Delegate to person
    public string Name => _person.Name;
}
```

**2. Program to Interfaces, Not Implementations**

```csharp
// Bad: Tied to specific implementation
public List<Customer> GetCustomers() { /* ... */ }

// Good: Return interface
public IEnumerable<Customer> GetCustomers() { /* ... */ }
// or
public IReadOnlyList<Customer> GetCustomers() { /* ... */ }
```

**3. Keep Classes Small and Focused**

```csharp
// Each class should do one thing well
public class CustomerValidator { }
public class CustomerRepository { }
public class CustomerService { }

// Instead of one large class doing everything
public class CustomerManager { /* validation, data access, business logic */ }
```

### Common Pitfalls

**1. God Objects**

Avoid creating classes that know too much or do too much. A "God Object" is an anti-pattern where one class handles many unrelated responsibilities.

**2. Deep Inheritance Hierarchies**

```csharp
// Bad: Deep hierarchy
public class Animal { }
public class Mammal : Animal { }
public class Dog : Mammal { }
public class GermanShepherd : Dog { }
public class TrainedGermanShepherd : GermanShepherd { }

// Good: Flatter hierarchy with composition
public class Animal { }
public class Dog : Animal 
{ 
    public Breed Breed { get; set; }  // Composition
    public TrainingLevel Training { get; set; }  // Composition
}
```

**3. Violating Encapsulation**

```csharp
// Bad: Public fields expose implementation
public class BadBankAccount
{
    public decimal Balance;
}

// Good: Private field with validation
public class BankAccount
{
    private decimal _balance;
    public decimal Balance => _balance;
    
    public void Deposit(decimal amount)
    {
        if (amount <= 0) throw new ArgumentException("Invalid amount");
        _balance += amount;
    }
}
```

**4. Improper Use of Static**

```csharp
// Bad: Static state causes issues in testing and concurrency
public static class GlobalState
{
    public static User? CurrentUser;  // Shared across all code
}

// Good: Instance-based, injectable
public class UserContext
{
    public User? CurrentUser { get; private set; }
    
    public void SetUser(User user) => CurrentUser = user;
}
```

---

## Summary

This comprehensive guide has covered Object-Oriented Programming in C# from fundamentals to advanced concepts:

- **Classes and Objects**: The building blocks of OOP, defining structure and behavior
- **Constructors and Destructors**: Object lifecycle management
- **Properties and Indexers**: Controlled access to data
- **Access Modifiers**: Encapsulation through visibility control
- **Inheritance**: Code reuse and hierarchical relationships
- **Polymorphism**: Many forms through virtual/override and interfaces
- **Abstract Classes and Interfaces**: Contracts and partial implementations
- **Generics**: Type-safe, reusable code
- **Design Patterns**: Proven solutions to common problems
- **SOLID Principles**: Guidelines for maintainable, extensible code

Mastering OOP enables you to design software that is modular, maintainable, and extensible. The principles and patterns covered here provide a foundation for building complex applications that can evolve over time. Practice these concepts by designing small systems, then gradually work on larger projects to reinforce your understanding.

---

*Created for educational purposes. Last updated: 2024*
