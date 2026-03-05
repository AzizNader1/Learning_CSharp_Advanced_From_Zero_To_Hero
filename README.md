# Master Advanced C# Programming: Complete A to Z Guide

> **From Zero to Hero: Master Advanced C# Features, Patterns, and Performance Optimization**

---

## Table of Contents

1. [Delegates](#1-delegates)
2. [Events](#2-events)
3. [Lambda Expressions](#3-lambda-expressions)
4. [LINQ (Language Integrated Query)](#4-linq-language-integrated-query)
5. [Extension Methods](#5-extension-methods)
6. [Async/Await and Asynchronous Programming](#6-asyncawait-and-asynchronous-programming)
7. [Task Parallel Library (TPL)](#7-task-parallel-library-tpl)
8. [Parallel LINQ (PLINQ)](#8-parallel-linq-plinq)
9. [Concurrent Collections](#9-concurrent-collections)
10. [Memory Management and Garbage Collection](#10-memory-management-and-garbage-collection)
11. [Span and Memory Types](#11-span-and-memory-types)
12. [Nullable Reference Types](#12-nullable-reference-types)
13. [Records and Record Structs](#13-records-and-record-structs)
14. [Pattern Matching](#14-pattern-matching)
15. [Tuples and Discards](#15-tuples-and-discards)
16. [Advanced Generics (Covariance and Contravariance)](#16-advanced-generics-covariance-and-contravariance)
17. [Reflection](#17-reflection)
18. [Attributes](#18-attributes)
19. [Expression Trees](#19-expression-trees)
20. [Dynamic Programming](#20-dynamic-programming)
21. [Unsafe Code and Pointers](#21-unsafe-code-and-pointers)
22. [Best Practices and Performance Optimization](#22-best-practices-and-performance-optimization)

---

## 1. Delegates

### Understanding Delegates

A delegate is a type-safe function pointer that holds references to methods with a specific signature. Delegates provide a way to pass methods as parameters, enabling callback patterns, event handling, and functional programming techniques. Think of a delegate as a variable that can store a reference to a method rather than data. When you invoke a delegate, it calls the method(s) it references.

Delegates are the foundation for many advanced C# features including events, lambda expressions, and LINQ. Understanding delegates deeply is essential for mastering advanced C# programming. The .NET framework defines several built-in delegate types (Action, Func, Predicate) that cover most common scenarios, but you can also define custom delegates when needed.

### Declaring and Using Delegates

```csharp
// Custom delegate declaration
public delegate void NotifyHandler(string message);
public delegate int CalculateHandler(int a, int b);
public delegate bool PredicateHandler<T>(T item);

public class DelegateExamples
{
    // Method that matches NotifyHandler signature
    public static void SendEmail(string message)
    {
        Console.WriteLine($"Email sent: {message}");
    }
    
    public static void SendSms(string message)
    {
        Console.WriteLine($"SMS sent: {message}");
    }
    
    // Methods matching CalculateHandler signature
    public static int Add(int a, int b) => a + b;
    public static int Subtract(int a, int b) => a - b;
    public static int Multiply(int a, int b) => a * b;
    
    public static void Demo()
    {
        // Create delegate instance
        NotifyHandler handler = SendEmail;
        
        // Invoke delegate
        handler("Hello World");  // Output: Email sent: Hello World
        
        // Assign different method
        handler = SendSms;
        handler("Hello World");  // Output: SMS sent: Hello World
        
        // Using CalculateHandler
        CalculateHandler calculator = Add;
        Console.WriteLine(calculator(5, 3));  // 8
        
        calculator = Multiply;
        Console.WriteLine(calculator(5, 3));  // 15
    }
}
```

### Built-in Delegate Types

C# provides generic delegate types that eliminate the need for most custom delegate declarations. These are the most commonly used delegates in modern C# code:

```csharp
// Action - returns void, can have up to 16 parameters
Action greet = () => Console.WriteLine("Hello!");
Action<string> greetByName = name => Console.WriteLine($"Hello, {name}!");
Action<string, int> greetWithNameAndAge = (name, age) => 
    Console.WriteLine($"{name} is {age} years old");

greet();                    // Hello!
greetByName("Alice");       // Hello, Alice!
greetWithNameAndAge("Bob", 30);  // Bob is 30 years old

// Func - returns a value, last type parameter is return type
Func<int, int> square = x => x * x;
Func<int, int, int> add = (a, b) => a + b;
Func<string, int> parse = s => int.Parse(s);
Func<int, bool> isEven = n => n % 2 == 0;
Func<int, string, bool> compareLength = (n, s) => s.Length == n;

Console.WriteLine(square(5));      // 25
Console.WriteLine(add(3, 4));       // 7
Console.WriteLine(parse("42"));     // 42
Console.WriteLine(isEven(4));       // true

// Predicate - returns bool, special case of Func<T, bool>
Predicate<int> isPositive = n => n > 0;
Predicate<string> isEmpty = s => string.IsNullOrEmpty(s);
Predicate<int> isPrime = n =>
{
    if (n < 2) return false;
    for (int i = 2; i <= Math.Sqrt(n); i++)
        if (n % i == 0) return false;
    return true;
};

Console.WriteLine(isPositive(5));   // true
Console.WriteLine(isEmpty(""));     // true
Console.WriteLine(isPrime(17));     // true
```

### Multicast Delegates

Delegates can reference multiple methods simultaneously. When invoked, each method is called in the order it was added. This is called multicast delegation and is the foundation for C# events.

```csharp
public class MulticastExample
{
    public static void Method1(string message) => Console.WriteLine($"Method1: {message}");
    public static void Method2(string message) => Console.WriteLine($"Method2: {message}");
    public static void Method3(string message) => Console.WriteLine($"Method3: {message}");
    
    public static void Demo()
    {
        Action<string> multicast = Method1;
        multicast += Method2;
        multicast += Method3;
        
        multicast("Hello");
        // Output:
        // Method1: Hello
        // Method2: Hello
        // Method3: Hello
        
        // Remove a method
        multicast -= Method2;
        multicast("World");
        // Output:
        // Method1: World
        // Method3: World
        
        // Get all invocations
        Delegate[] invocations = multicast.GetInvocationList();
        Console.WriteLine($"Methods in delegate: {invocations.Length}");
    }
}

// Practical example: Notification system
public class NotificationService
{
    private Action<string> _notifications;
    
    public void Subscribe(Action<string> handler)
    {
        _notifications += handler;
    }
    
    public void Unsubscribe(Action<string> handler)
    {
        _notifications -= handler;
    }
    
    public void Notify(string message)
    {
        _notifications?.Invoke(message);
    }
}
```

### Passing Delegates as Parameters

Delegates enable powerful patterns like callback methods, strategy patterns, and functional programming:

```csharp
public class DelegateAsParameter
{
    // Method that accepts a delegate parameter
    public static void ProcessNumbers(int[] numbers, Func<int, int> transformation)
    {
        foreach (int num in numbers)
        {
            int result = transformation(num);
            Console.WriteLine($"Transformed: {num} -> {result}");
        }
    }
    
    // Method that accepts a predicate
    public static List<T> Filter<T>(IEnumerable<T> items, Predicate<T> predicate)
    {
        var result = new List<T>();
        foreach (T item in items)
        {
            if (predicate(item))
                result.Add(item);
        }
        return result;
    }
    
    // Higher-order function returning a delegate
    public static Func<int, int> CreateMultiplier(int factor)
    {
        return x => x * factor;
    }
    
    public static void Demo()
    {
        int[] numbers = { 1, 2, 3, 4, 5 };
        
        // Pass different transformations
        ProcessNumbers(numbers, x => x * 2);
        Console.WriteLine("---");
        ProcessNumbers(numbers, x => x * x);
        Console.WriteLine("---");
        ProcessNumbers(numbers, x => x + 10);
        
        // Filter using predicate
        List<int> evenNumbers = Filter(numbers, n => n % 2 == 0);
        Console.WriteLine($"Even: {string.Join(", ", evenNumbers)}");
        
        // Factory method creating delegates
        var triple = CreateMultiplier(3);
        Console.WriteLine(triple(5));  // 15
        Console.WriteLine(triple(10)); // 30
    }
}
```

---

## 2. Events

### Understanding Events

Events in C# provide a publish-subscribe mechanism that allows objects to notify other objects when something happens. An event is essentially a wrapper around a delegate that restricts subscribers from directly invoking or resetting the delegate. Events are fundamental to building loosely coupled systems and are used extensively in GUI applications, game development, and message-driven architectures.

The key difference between a delegate field and an event is encapsulation: with a plain delegate field, external code can invoke it, replace it, or clear all subscribers. An event restricts external code to only subscribing (`+=`) or unsubscribing (`-=`), preventing unauthorized invocation or manipulation.

### Declaring and Using Events

```csharp
// Publisher class
public class Stock
{
    private decimal _price;
    
    // Event declaration using EventHandler<T>
    public event EventHandler<PriceChangedEventArgs> PriceChanged;
    
    // Alternative: Custom delegate type
    public event Action<decimal, decimal>? PriceUpdated;
    
    public string Symbol { get; }
    
    public decimal Price
    {
        get => _price;
        set
        {
            if (_price != value)
            {
                var oldPrice = _price;
                _price = value;
                OnPriceChanged(oldPrice, value);
            }
        }
    }
    
    public Stock(string symbol, decimal price)
    {
        Symbol = symbol;
        _price = price;
    }
    
    // Protected virtual method to raise event
    protected virtual void OnPriceChanged(decimal oldPrice, decimal newPrice)
    {
        PriceChanged?.Invoke(this, new PriceChangedEventArgs(oldPrice, newPrice));
        PriceUpdated?.Invoke(oldPrice, newPrice);
    }
}

// Custom EventArgs
public class PriceChangedEventArgs : EventArgs
{
    public decimal OldPrice { get; }
    public decimal NewPrice { get; }
    public decimal ChangePercent => 
        OldPrice != 0 ? ((NewPrice - OldPrice) / OldPrice) * 100 : 0;
    
    public PriceChangedEventArgs(decimal oldPrice, decimal newPrice)
    {
        OldPrice = oldPrice;
        NewPrice = newPrice;
    }
}

// Subscriber class
public class StockMonitor
{
    public void Subscribe(Stock stock)
    {
        stock.PriceChanged += OnPriceChanged;
    }
    
    public void Unsubscribe(Stock stock)
    {
        stock.PriceChanged -= OnPriceChanged;
    }
    
    private void OnPriceChanged(object? sender, PriceChangedEventArgs e)
    {
        Console.WriteLine($"Price changed from {e.OldPrice:C} to {e.NewPrice:C} " +
                          $"({e.ChangePercent:F2}%)");
    }
}

// Usage
Stock stock = new Stock("AAPL", 150.00m);
StockMonitor monitor = new StockMonitor();

monitor.Subscribe(stock);

stock.Price = 155.00m;  // Triggers event
stock.Price = 160.00m;  // Triggers event

monitor.Unsubscribe(stock);
```

### Event Accessors

Like properties, events can have custom add and remove accessors. This is useful when you need to control how subscribers are stored or when implementing custom serialization:

```csharp
public class ObservableProperty<T>
{
    private T? _value;
    private readonly List<EventHandler<T?>> _handlers = new();
    private readonly object _lock = new();
    
    // Custom event accessors
    public event EventHandler<T?>? ValueChanged
    {
        add
        {
            lock (_lock)
            {
                _handlers.Add(value!);
            }
        }
        remove
        {
            lock (_lock)
            {
                _handlers.Remove(value!);
            }
        }
    }
    
    public T? Value
    {
        get => _value;
        set
        {
            if (!EqualityComparer<T>.Default.Equals(_value, value))
            {
                _value = value;
                OnValueChanged();
            }
        }
    }
    
    private void OnValueChanged()
    {
        EventHandler<T?>[] handlersCopy;
        lock (_lock)
        {
            handlersCopy = _handlers.ToArray();
        }
        
        foreach (var handler in handlersCopy)
        {
            handler(this, _value);
        }
    }
}

// Weak event pattern (prevent memory leaks)
public class WeakEventManager
{
    private readonly List<WeakReference> _handlers = new();
    
    public void Subscribe(EventHandler handler)
    {
        _handlers.Add(new WeakReference(handler));
    }
    
    public void Raise(object sender, EventArgs e)
    {
        for (int i = _handlers.Count - 1; i >= 0; i--)
        {
            if (_handlers[i].IsAlive && _handlers[i].Target is EventHandler handler)
            {
                handler(sender, e);
            }
            else
            {
                _handlers.RemoveAt(i);
            }
        }
    }
}
```

### Standard Event Pattern

```csharp
// Standard .NET event pattern
public class ProcessCompletedEventArgs : EventArgs
{
    public bool Success { get; }
    public string Message { get; }
    public TimeSpan Duration { get; }
    public Exception? Error { get; }
    
    public ProcessCompletedEventArgs(bool success, string message, 
        TimeSpan duration, Exception? error = null)
    {
        Success = success;
        Message = message;
        Duration = duration;
        Error = error;
    }
}

public class DataProcessor
{
    // Standard event pattern
    public event EventHandler<ProcessCompletedEventArgs>? ProcessCompleted;
    
    // For simple events without data
    public event EventHandler? Started;
    
    public event EventHandler? ProgressChanged;
    
    public async Task ProcessAsync(string data)
    {
        Started?.Invoke(this, EventArgs.Empty);
        
        var stopwatch = Stopwatch.StartNew();
        
        try
        {
            // Simulate processing
            for (int i = 0; i <= 100; i += 10)
            {
                await Task.Delay(100);
                ProgressChanged?.Invoke(this, EventArgs.Empty);
            }
            
            stopwatch.Stop();
            OnProcessCompleted(true, "Processing completed successfully", stopwatch.Elapsed);
        }
        catch (Exception ex)
        {
            stopwatch.Stop();
            OnProcessCompleted(false, ex.Message, stopwatch.Elapsed, ex);
        }
    }
    
    protected virtual void OnProcessCompleted(bool success, string message, 
        TimeSpan duration, Exception? error = null)
    {
        ProcessCompleted?.Invoke(this, 
            new ProcessCompletedEventArgs(success, message, duration, error));
    }
}
```

---

## 3. Lambda Expressions

### Understanding Lambda Expressions

Lambda expressions are anonymous functions that you can use to create delegates or expression trees. They provide a concise syntax for writing code that can be passed as arguments or stored as variables. Lambda expressions are fundamental to LINQ and functional programming in C#. The lambda operator `=>` separates the input parameters from the expression body.

```csharp
// Evolution from method to lambda
// 1. Named method
bool IsEven(int n) => n % 2 == 0;

// 2. Anonymous method (C# 2.0)
Func<int, bool> isEvenAnon = delegate(int n) { return n % 2 == 0; };

// 3. Lambda expression (C# 3.0+)
Func<int, bool> isEvenLambda = n => n % 2 == 0;

// Using each approach
var numbers = new[] { 1, 2, 3, 4, 5 };
var evens1 = numbers.Where(IsEven);
var evens2 = numbers.Where(isEvenAnon);
var evens3 = numbers.Where(isEvenLambda);
var evens4 = numbers.Where(n => n % 2 == 0);  // Inline lambda
```

### Lambda Expression Syntax

```csharp
// Expression lambda - single expression
Func<int, int> square = x => x * x;
Func<int, int, int> add = (x, y) => x + y;
Func<int, bool> isPositive = x => x > 0;

// Statement lambda - multiple statements in braces
Func<int, string> describeNumber = x =>
{
    if (x < 0) return "negative";
    if (x == 0) return "zero";
    return "positive";
};

// No parameters
Action sayHello = () => Console.WriteLine("Hello!");
Func<DateTime> getCurrentTime = () => DateTime.Now;

// Single parameter - parentheses optional
Func<int, int> increment = x => x + 1;           // Preferred
Func<int, int> incrementParens = (x) => x + 1;   // Also valid

// Multiple parameters - parentheses required
Func<int, int, int> multiply = (x, y) => x * y;
Func<int, int, int, int> sum3 = (x, y, z) => x + y + z;

// Explicit types (rare, for disambiguation)
Func<int, int, int> divide = (int x, int y) => x / y;

// Discard parameters (C# 9+)
Func<int, int, int> secondParam = (_, y) => y;
Action<int, int> ignoreBoth = (_, _) => Console.WriteLine("Ignored");

// Ref, out, and in parameters
Func<int, int, int> tryAdd = (ref int x, int y) => x += y;  // Must use delegate
delegate int RefFunc(ref int x, int y);
RefFunc addRef = (ref int x, int y) => x += y;
```

### Capturing Variables (Closures)

Lambda expressions can capture variables from their enclosing scope, creating a closure. The captured variable's lifetime extends to match the lambda's lifetime:

```csharp
public class ClosureExamples
{
    public static Action CreateCounter()
    {
        int count = 0;  // This variable is captured
        
        return () =>
        {
            count++;
            Console.WriteLine($"Count: {count}");
        };
    }
    
    public static void Demo()
    {
        var counter = CreateCounter();
        counter();  // Count: 1
        counter();  // Count: 2
        counter();  // Count: 3
        
        // Each closure captures its own variable
        var counter2 = CreateCounter();
        counter2();  // Count: 1 (new closure, new variable)
    }
    
    // Common pitfall: capturing loop variable
    public static void LoopPitfall()
    {
        var actions = new List<Action>();
        
        for (int i = 0; i < 5; i++)
        {
            // All lambdas capture the SAME i variable!
            actions.Add(() => Console.WriteLine(i));
        }
        
        foreach (var action in actions)
        {
            action();  // Output: 5, 5, 5, 5, 5 (not 0, 1, 2, 3, 4)
        }
    }
    
    // Correct approach: capture a copy
    public static void LoopCorrect()
    {
        var actions = new List<Action>();
        
        for (int i = 0; i < 5; i++)
        {
            int copy = i;  // Create a new variable each iteration
            actions.Add(() => Console.WriteLine(copy));
        }
        
        // Or in modern C# (C# 5+), foreach already creates a copy
        foreach (var action in actions)
        {
            action();  // Output: 0, 1, 2, 3, 4
        }
    }
    
    // Capturing 'this'
    public class CaptureThis
    {
        private int _value = 10;
        
        public Func<int> GetValueCapture()
        {
            // Captures 'this', so changes to _value will be reflected
            return () => _value;
        }
        
        public void SetValue(int value) => _value = value;
    }
}
```

---

## 4. LINQ (Language Integrated Query)

### Understanding LINQ

LINQ provides a unified query syntax for working with different data sources—objects, databases (Entity Framework), XML, and more. LINQ combines the expressiveness of SQL-like queries with the type safety and IntelliSense support of C#. It offers two syntaxes: query syntax (SQL-like) and method syntax (using extension methods).

```csharp
// Sample data
public class Product
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string Category { get; set; } = "";
    public decimal Price { get; set; }
    public int Stock { get; set; }
}

public class Customer
{
    public int Id { get; set; }
    public string Name { get; set; } = "";
    public string City { get; set; } = "";
    public List<Order> Orders { get; set; } = new();
}

public class Order
{
    public int Id { get; set; }
    public int CustomerId { get; set; }
    public DateTime Date { get; set; }
    public decimal Total { get; set; }
}

var products = new List<Product>
{
    new Product { Id = 1, Name = "Laptop", Category = "Electronics", Price = 999.99m, Stock = 10 },
    new Product { Id = 2, Name = "Mouse", Category = "Electronics", Price = 29.99m, Stock = 50 },
    new Product { Id = 3, Name = "Keyboard", Category = "Electronics", Price = 79.99m, Stock = 30 },
    new Product { Id = 4, Name = "Desk", Category = "Furniture", Price = 299.99m, Stock = 5 },
    new Product { Id = 5, Name = "Chair", Category = "Furniture", Price = 199.99m, Stock = 8 },
    new Product { Id = 6, Name = "Monitor", Category = "Electronics", Price = 399.99m, Stock = 15 },
    new Product { Id = 7, Name = "Book", Category = "Books", Price = 24.99m, Stock = 100 },
};
```

### Query Syntax vs Method Syntax

```csharp
// Query syntax (SQL-like)
var expensiveElectronicsQuery =
    from p in products
    where p.Category == "Electronics" && p.Price > 50
    orderby p.Price descending
    select p;

// Method syntax (extension methods)
var expensiveElectronicsMethod = products
    .Where(p => p.Category == "Electronics" && p.Price > 50)
    .OrderByDescending(p => p.Price);

// Both produce the same result
// Query syntax is compiled into method syntax by the compiler
```

### Filtering and Projection

```csharp
// Where - filtering
var electronics = products.Where(p => p.Category == "Electronics");
var affordable = products.Where(p => p.Price < 100);
var inStock = products.Where(p => p.Stock > 0);

// Select - projection (transform)
var productNames = products.Select(p => p.Name);
var productSummaries = products.Select(p => new { p.Name, p.Price });
var pricedProducts = products.Select(p => $"{p.Name}: {p.Price:C}");

// SelectMany - flatten nested collections
var customers = new List<Customer>
{
    new Customer 
    { 
        Id = 1, Name = "Alice", 
        Orders = new List<Order>
        {
            new Order { Id = 101, Date = DateTime.Now.AddDays(-5), Total = 150 },
            new Order { Id = 102, Date = DateTime.Now.AddDays(-2), Total = 75 }
        }
    },
    new Customer 
    { 
        Id = 2, Name = "Bob", 
        Orders = new List<Order>
        {
            new Order { Id = 201, Date = DateTime.Now.AddDays(-3), Total = 200 }
        }
    }
};

// Get all orders from all customers
var allOrders = customers.SelectMany(c => c.Orders);

// Get order with customer name
var ordersWithCustomer = customers
    .SelectMany(c => c.Orders.Select(o => new { CustomerName = c.Name, o.Id, o.Total }));

// Distinct
var categories = products.Select(p => p.Category).Distinct();

// OfType - filter by type
var mixedList = new List<object> { 1, "hello", 2.5, "world", 3 };
var strings = mixedList.OfType<string>();  // "hello", "world"
```

### Ordering and Grouping

```csharp
// OrderBy, OrderByDescending
var sortedByPrice = products.OrderBy(p => p.Price);
var sortedByPriceDesc = products.OrderByDescending(p => p.Price);

// ThenBy, ThenByDescending (secondary sort)
var sortedByCategoryThenPrice = products
    .OrderBy(p => p.Category)
    .ThenByDescending(p => p.Price);

// Reverse
var reversed = products.Reverse();

// GroupBy
var groupedByCategory = products.GroupBy(p => p.Category);
foreach (var group in groupedByCategory)
{
    Console.WriteLine($"Category: {group.Key}");
    foreach (var product in group)
    {
        Console.WriteLine($"  - {product.Name}: {product.Price:C}");
    }
}

// GroupBy with aggregation
var categoryStats = products
    .GroupBy(p => p.Category)
    .Select(g => new
    {
        Category = g.Key,
        Count = g.Count(),
        TotalValue = g.Sum(p => p.Price * p.Stock),
        AveragePrice = g.Average(p => p.Price),
        MinPrice = g.Min(p => p.Price),
        MaxPrice = g.Max(p => p.Price)
    });

// GroupBy with multiple keys
var groupedByCategoryAndPrice = products
    .GroupBy(p => new { p.Category, IsExpensive = p.Price > 100 });
```

### Aggregation and Quantifiers

```csharp
// Count, Sum, Average, Min, Max
int totalProducts = products.Count();
int electronicsCount = products.Count(p => p.Category == "Electronics");
decimal totalValue = products.Sum(p => p.Price * p.Stock);
decimal avgPrice = products.Average(p => p.Price);
decimal minPrice = products.Min(p => p.Price);
decimal maxPrice = products.Max(p => p.Price);
Product? cheapest = products.MinBy(p => p.Price);
Product? mostExpensive = products.MaxBy(p => p.Price);

// Aggregate - custom aggregation
decimal runningTotal = products.Aggregate(0m, (sum, p) => sum + p.Price);
string allNames = products.Aggregate("", (names, p) => names + p.Name + ", ");
decimal largestOrderTotal = products
    .Aggregate(0m, (max, p) => Math.Max(max, p.Price * p.Stock));

// Any - returns true if any element satisfies condition
bool hasElectronics = products.Any(p => p.Category == "Electronics");
bool hasOutOfStock = products.Any(p => p.Stock == 0);

// All - returns true if all elements satisfy condition
bool allInStock = products.All(p => p.Stock > 0);
bool allPriced = products.All(p => p.Price > 0);

// Contains
var laptop = products.First(p => p.Name == "Laptop");
bool containsLaptop = products.Contains(laptop);

// SequenceEqual
var list1 = new[] { 1, 2, 3 };
var list2 = new[] { 1, 2, 3 };
bool areEqual = list1.SequenceEqual(list2);  // true
```

### Element Operations

```csharp
// First, FirstOrDefault
Product firstElectronics = products.First(p => p.Category == "Electronics");
Product? firstExpensive = products.FirstOrDefault(p => p.Price > 1000);

// Last, LastOrDefault
Product lastProduct = products.Last();
Product? lastElectronics = products.LastOrDefault(p => p.Category == "Electronics");

// Single, SingleOrDefault (throws if more than one)
Product? singleBook = products.SingleOrDefault(p => p.Category == "Books");

// ElementAt, ElementAtOrDefault
Product? thirdProduct = products.ElementAtOrDefault(2);

// DefaultIfEmpty
var emptyList = new List<Product>();
var withDefault = emptyList.DefaultIfEmpty(new Product { Name = "Default" });
```

### Set Operations and Joining

```csharp
// Union, Intersect, Except
var list1 = new[] { 1, 2, 3, 4 };
var list2 = new[] { 3, 4, 5, 6 };

var union = list1.Union(list2);        // 1, 2, 3, 4, 5, 6
var intersect = list1.Intersect(list2); // 3, 4
var except = list1.Except(list2);       // 1, 2

// Join
var orders = new List<Order>
{
    new Order { Id = 1, CustomerId = 1, Date = DateTime.Now, Total = 100 },
    new Order { Id = 2, CustomerId = 2, Date = DateTime.Now, Total = 200 },
    new Order { Id = 3, CustomerId = 1, Date = DateTime.Now, Total = 150 },
};

var customerOrders = customers.Join(
    orders,
    c => c.Id,
    o => o.CustomerId,
    (c, o) => new { CustomerName = c.Name, o.Id, o.Total });

// GroupJoin (left outer join)
var customersWithOrders = customers.GroupJoin(
    orders,
    c => c.Id,
    o => o.CustomerId,
    (c, orderList) => new
    {
        Customer = c.Name,
        Orders = orderList.ToList(),
        OrderCount = orderList.Count()
    });

// Zip (C# 4+)
var names = new[] { "Alice", "Bob", "Charlie" };
var ages = new[] { 25, 30, 35 };
var cities = new[] { "NYC", "LA", "Chicago" };

var people = names.Zip(ages, (name, age) => new { Name = name, Age = age });
var peopleWithCities = names.Zip(ages, cities, (name, age, city) => 
    new { Name = name, Age = age, City = city });
```

### Partitioning and Conversion

```csharp
// Take, Skip
var firstThree = products.Take(3);
var skipTwo = products.Skip(2);

// TakeWhile, SkipWhile
var takeWhileExpensive = products.TakeWhile(p => p.Price > 50);
var skipWhileInStock = products.SkipWhile(p => p.Stock > 10);

// Chunk (C# 6+)
var chunks = products.Chunk(3);  // Groups of 3 products

// ToList, ToArray, ToDictionary
var productList = products.ToList();
var productArray = products.ToArray();
var productDict = products.ToDictionary(p => p.Id, p => p.Name);

// ToLookup (multiple values per key)
var lookup = products.ToLookup(p => p.Category);
var electronics = lookup["Electronics"];  // Returns all electronics

// ToHashSet
var uniquePrices = products.Select(p => p.Price).ToHashSet();
```

### Deferred vs Immediate Execution

```csharp
// Deferred execution - query not executed until enumerated
var expensiveProducts = products.Where(p => p.Price > 100);
// Query hasn't run yet!

// Executed when enumerated
foreach (var p in expensiveProducts)  // Query runs here
{
    Console.WriteLine(p.Name);
}

// Also executed by ToList, ToArray, Count, etc.
var expensiveList = expensiveProducts.ToList();  // Query runs here

// Important: deferred execution means results may change
products.Add(new Product { Name = "Expensive Item", Price = 500 });
var newList = expensiveProducts.ToList();  // Now includes the new item

// Immediate execution operators
int count = products.Count();  // Executes immediately
decimal sum = products.Sum(p => p.Price);  // Executes immediately
Product first = products.First();  // Executes immediately

// Forcing immediate execution
var cachedResults = products.Where(p => p.Price > 100).ToList();
// Now cachedResults won't change even if products does

// Re-evaluating each time
var query = products.Where(p => p.Price > 100);
var count1 = query.Count();  // Evaluates once
var list1 = query.ToList();  // Evaluates again!
```

---

## 5. Extension Methods

### Understanding Extension Methods

Extension methods allow you to add methods to existing types without creating a new derived type or modifying the original type. They are static methods that appear to be instance methods due to the `this` keyword on the first parameter. Extension methods are essential for LINQ and are a powerful tool for extending sealed classes or interfaces.

```csharp
public static class StringExtensions
{
    // Basic extension method
    public static string Reverse(this string input)
    {
        if (string.IsNullOrEmpty(input)) return input;
        var chars = input.ToCharArray();
        Array.Reverse(chars);
        return new string(chars);
    }
    
    // Extension method with parameters
    public static string Truncate(this string input, int maxLength, string suffix = "...")
    {
        if (string.IsNullOrEmpty(input) || input.Length <= maxLength)
            return input;
        return input.Substring(0, maxLength - suffix.Length) + suffix;
    }
    
    // Extension method with validation
    public static bool IsValidEmail(this string input)
    {
        if (string.IsNullOrEmpty(input)) return false;
        try
        {
            var addr = new System.Net.Mail.MailAddress(input);
            return addr.Address == input;
        }
        catch
        {
            return false;
        }
    }
    
    // Extension method returning IEnumerable
    public static IEnumerable<string> SplitLines(this string input)
    {
        if (string.IsNullOrEmpty(input)) yield break;
        using var reader = new StringReader(input);
        while (reader.ReadLine() is { } line)
        {
            yield return line;
        }
    }
    
    // Extension method for fluent API
    public static string SurroundWith(this string input, string before, string after)
    {
        return $"{before}{input}{after}";
    }
}

// Usage
string text = "Hello World";
Console.WriteLine(text.Reverse());                    // "dlroW olleH"
Console.WriteLine(text.Truncate(8));                  // "Hello..."
Console.WriteLine("test@email.com".IsValidEmail());   // true
Console.WriteLine(text.SurroundWith("[", "]"));       // "[Hello World]"

foreach (var line in "Line1\nLine2\nLine3".SplitLines())
{
    Console.WriteLine(line);
}
```

### Extension Methods for Collections

```csharp
public static class CollectionExtensions
{
    // ForEach for IEnumerable (similar to List<T>.ForEach)
    public static void ForEach<T>(this IEnumerable<T> source, Action<T> action)
    {
        foreach (var item in source)
        {
            action(item);
        }
    }
    
    // Chunk without using built-in Chunk
    public static IEnumerable<List<T>> Partition<T>(this IEnumerable<T> source, int size)
    {
        var partition = new List<T>(size);
        foreach (var item in source)
        {
            partition.Add(item);
            if (partition.Count == size)
            {
                yield return partition;
                partition = new List<T>(size);
            }
        }
        if (partition.Count > 0)
        {
            yield return partition;
        }
    }
    
    // Shuffle
    public static IEnumerable<T> Shuffle<T>(this IEnumerable<T> source)
    {
        var random = new Random();
        return source.OrderBy(_ => random.Next());
    }
    
    // Distinct by property
    public static IEnumerable<T> DistinctBy<T, TKey>(
        this IEnumerable<T> source, 
        Func<T, TKey> keySelector)
    {
        var seenKeys = new HashSet<TKey>();
        foreach (var element in source)
        {
            if (seenKeys.Add(keySelector(element)))
            {
                yield return element;
            }
        }
    }
    
    // Safe element at
    public static T? ElementAtOrDefault<T>(this IEnumerable<T> source, int index, T? defaultValue)
    {
        if (index < 0) return defaultValue;
        
        if (source is IList<T> list)
        {
            return index < list.Count ? list[index] : defaultValue;
        }
        
        using var enumerator = source.GetEnumerator();
        int currentIndex = 0;
        while (enumerator.MoveNext())
        {
            if (currentIndex++ == index)
                return enumerator.Current;
        }
        return defaultValue;
    }
    
    // Join strings
    public static string JoinToString<T>(this IEnumerable<T> source, string separator)
    {
        return string.Join(separator, source);
    }
}

// Usage
var numbers = Enumerable.Range(1, 10);

numbers.ForEach(n => Console.WriteLine(n));

var chunks = numbers.Partition(3);
// [[1,2,3], [4,5,6], [7,8,9], [10]]

var shuffled = numbers.Shuffle().ToList();

var products = new List<Product>
{
    new Product { Category = "Electronics", Name = "Laptop" },
    new Product { Category = "Electronics", Name = "Phone" },
    new Product { Category = "Books", Name = "C# Guide" }
};

var uniqueCategories = products.DistinctBy(p => p.Category);
```

### Extension Methods for Interfaces

```csharp
public static class EnumerableExtensions
{
    // MinBy and MaxBy (pre-C# 10)
    public static T? MinBy<T, TKey>(this IEnumerable<T> source, Func<T, TKey> keySelector)
        where TKey : IComparable<TKey>
    {
        using var enumerator = source.GetEnumerator();
        if (!enumerator.MoveNext()) return default;
        
        T min = enumerator.Current;
        TKey minKey = keySelector(min);
        
        while (enumerator.MoveNext())
        {
            var currentKey = keySelector(enumerator.Current);
            if (currentKey.CompareTo(minKey) < 0)
            {
                min = enumerator.Current;
                minKey = currentKey;
            }
        }
        return min;
    }
    
    // Batch processing
    public static async Task ForEachAsync<T>(
        this IEnumerable<T> source,
        Func<T, Task> action,
        int maxDegreeOfParallelism = 4)
    {
        using var semaphore = new SemaphoreSlim(maxDegreeOfParallelism);
        var tasks = source.Select(async item =>
        {
            await semaphore.WaitAsync();
            try
            {
                await action(item);
            }
            finally
            {
                semaphore.Release();
            }
        });
        await Task.WhenAll(tasks);
    }
}
```

---

## 6. Async/Await and Asynchronous Programming

### Understanding Asynchronous Programming

Asynchronous programming allows your application to perform I/O-bound operations without blocking the calling thread. When an async method encounters an await, it yields control back to the caller until the awaited operation completes. This is essential for building responsive applications and scalable server applications. The async/await pattern makes asynchronous code as easy to write as synchronous code.

```csharp
public class AsyncBasics
{
    // Async method
    public async Task<string> DownloadContentAsync(string url)
    {
        using var client = new HttpClient();
        
        // await suspends the method until the task completes
        // Control returns to the caller during the wait
        string content = await client.GetStringAsync(url);
        
        return content;
    }
    
    // Async method with configuration
    public async Task<string> DownloadWithTimeoutAsync(string url, TimeSpan timeout)
    {
        using var client = new HttpClient();
        using var cts = new CancellationTokenSource(timeout);
        
        try
        {
            return await client.GetStringAsync(url, cts.Token);
        }
        catch (OperationCanceledException)
        {
            return "Request timed out";
        }
    }
    
    // Async void - only for event handlers!
    public async void ButtonClick(object sender, EventArgs e)
    {
        // This is okay for event handlers
        await SomeAsyncOperation();
    }
}
```

### Task-Based Asynchronous Pattern (TAP)

```csharp
public class FileService
{
    // Async file reading
    public async Task<string> ReadFileAsync(string path)
    {
        using var reader = new StreamReader(path);
        return await reader.ReadToEndAsync();
    }
    
    // Async file writing
    public async Task WriteFileAsync(string path, string content)
    {
        await File.WriteAllTextAsync(path, content);
    }
    
    // Progress reporting
    public async Task ProcessFileAsync(
        string path, 
        IProgress<int>? progress = null,
        CancellationToken cancellationToken = default)
    {
        var lines = await File.ReadAllLinesAsync(path, cancellationToken);
        
        for (int i = 0; i < lines.Length; i++)
        {
            cancellationToken.ThrowIfCancellationRequested();
            
            // Process line
            await Task.Delay(10, cancellationToken);  // Simulate work
            
            progress?.Report((i + 1) * 100 / lines.Length);
        }
    }
}

// Usage with progress
var progress = new Progress<int>(percent => 
{
    Console.WriteLine($"Progress: {percent}%");
});

var service = new FileService();
await service.ProcessFileAsync("data.txt", progress);
```

### Handling Multiple Tasks

```csharp
public class MultipleTasksExample
{
    // Wait for all tasks to complete
    public async Task<string[]> DownloadAllAsync(string[] urls)
    {
        var tasks = urls.Select(url => DownloadAsync(url)).ToArray();
        return await Task.WhenAll(tasks);
    }
    
    // Wait for any task to complete
    public async Task<string> DownloadFirstSuccessfulAsync(string[] urls)
    {
        var tasks = urls.Select(url => DownloadAsync(url)).ToArray();
        var completedTask = await Task.WhenAny(tasks);
        return await completedTask;
    }
    
    // With timeout
    public async Task<string?> DownloadWithTimeoutAsync(string url, TimeSpan timeout)
    {
        using var cts = new CancellationTokenSource(timeout);
        
        try
        {
            return await DownloadAsync(url, cts.Token);
        }
        catch (OperationCanceledException)
        {
            return null;  // Timed out
        }
    }
    
    // Process tasks as they complete
    public async Task ProcessAsCompletedAsync(IEnumerable<string> urls)
    {
        var tasks = urls.Select(url => DownloadAsync(url)).ToList();
        
        while (tasks.Count > 0)
        {
            var completedTask = await Task.WhenAny(tasks);
            tasks.Remove(completedTask);
            
            try
            {
                var result = await completedTask;
                Console.WriteLine($"Downloaded: {result.Length} chars");
            }
            catch (Exception ex)
            {
                Console.WriteLine($"Error: {ex.Message}");
            }
        }
    }
    
    private async Task<string> DownloadAsync(string url, CancellationToken ct = default)
    {
        using var client = new HttpClient();
        return await client.GetStringAsync(url, ct);
    }
}
```

### Cancellation Support

```csharp
public class CancellableOperations
{
    public async Task ProcessDataAsync(
        IEnumerable<int> data,
        CancellationToken cancellationToken = default)
    {
        foreach (var item in data)
        {
            // Check for cancellation
            cancellationToken.ThrowIfCancellationRequested();
            
            // Or cooperative cancellation
            if (cancellationToken.IsCancellationRequested)
            {
                Console.WriteLine("Cancellation requested, cleaning up...");
                // Clean up resources
                cancellationToken.ThrowIfCancellationRequested();
            }
            
            await ProcessItemAsync(item, cancellationToken);
        }
    }
    
    // Combining cancellation tokens
    public async Task WithCombinedCancellationAsync(
        CancellationToken externalToken,
        TimeSpan timeout)
    {
        using var timeoutCts = new CancellationTokenSource(timeout);
        using var linkedCts = CancellationTokenSource.CreateLinkedTokenSource(
            externalToken, timeoutCts.Token);
        
        try
        {
            await LongRunningOperationAsync(linkedCts.Token);
        }
        catch (OperationCanceledException) when (timeoutCts.IsCancellationRequested)
        {
            Console.WriteLine("Operation timed out");
        }
        catch (OperationCanceledException) when (externalToken.IsCancellationRequested)
        {
            Console.WriteLine("Operation cancelled by user");
        }
    }
    
    // Registering cancellation callback
    public async Task WithCleanupAsync(CancellationToken cancellationToken)
    {
        cancellationToken.Register(() =>
        {
            Console.WriteLine("Cancellation requested!");
        });
        
        await Task.Delay(5000, cancellationToken);
    }
    
    private Task ProcessItemAsync(int item, CancellationToken ct)
    {
        ct.ThrowIfCancellationRequested();
        return Task.CompletedTask;
    }
    
    private Task LongRunningOperationAsync(CancellationToken ct)
    {
        return Task.Delay(10000, ct);
    }
}
```

### Async Best Practices

```csharp
public class AsyncBestPractices
{
    // GOOD: Return Task, not void (except for event handlers)
    public async Task DoWorkAsync()
    {
        await Task.Delay(100);
    }
    
    // GOOD: Use ConfigureAwait(false) in library code
    public async Task<string> LibraryMethodAsync()
    {
        await Task.Delay(100).ConfigureAwait(false);
        return "Done";
    }
    
    // GOOD: Avoid async void except for event handlers
    public async void EventHandler(object sender, EventArgs e)
    {
        try
        {
            await DoWorkAsync();
        }
        catch (Exception ex)
        {
            // Must handle exceptions in async void!
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
    
    // AVOID: async over sync
    public Task<int> BadAsyncWrapper()
    {
        // Don't do this - it's not truly async
        return Task.FromResult(ComputeSync());
    }
    
    // GOOD: Expose sync and async versions
    public int ComputeSync() => 42;
    public Task<int> ComputeAsync() => Task.Run(ComputeSync);
    
    // GOOD: Prefer async all the way
    public async Task GoodAsync()
    {
        // Don't use .Result or .Wait()
        // var result = SomeAsync().Result;  // BAD!
        
        // Use await instead
        var result = await SomeAsync();  // GOOD!
    }
    
    // AVOID: Unnecessary async/await
    public async Task<string> UnnecessaryAsync()
    {
        return await Task.FromResult("Hello");  // Unnecessary
    }
    
    // GOOD: Return the task directly
    public Task<string> BetterAsync()
    {
        return Task.FromResult("Hello");
    }
    
    private async Task<string> SomeAsync()
    {
        await Task.Delay(10);
        return "result";
    }
    
    private int ComputeSync() => 42;
}
```

---

## 7. Task Parallel Library (TPL)

### Understanding TPL

The Task Parallel Library provides a higher-level abstraction for parallel and asynchronous programming. While async/await is ideal for I/O-bound operations, TPL's `Parallel` class and `Task` class are designed for CPU-bound parallel operations. TPL handles work stealing, load balancing, and efficient thread pool usage automatically.

```csharp
public class ParallelExamples
{
    // Parallel.For - parallel loop
    public void ParallelFor(int from, int to)
    {
        Parallel.For(from, to, i =>
        {
            Console.WriteLine($"Processing {i} on thread {Thread.CurrentThread.ManagedThreadId}");
            Thread.Sleep(100);  // Simulate work
        });
    }
    
    // Parallel.For with state
    public long ParallelSum(int[] numbers)
    {
        long total = 0;
        object lockObj = new();
        
        Parallel.For(0, numbers.Length, () => 0L, (i, state, localSum) =>
        {
            localSum += numbers[i];
            return localSum;
        },
        localSum =>
        {
            lock (lockObj)
            {
                total += localSum;
            }
        });
        
        return total;
    }
    
    // Parallel.ForEach
    public void ParallelForEach<T>(IEnumerable<T> items, Action<T> action)
    {
        Parallel.ForEach(items, item =>
        {
            action(item);
        });
    }
    
    // Parallel.ForEach with options
    public void ParallelWithOptions<T>(IEnumerable<T> items)
    {
        var options = new ParallelOptions
        {
            MaxDegreeOfParallelism = Environment.ProcessorCount,
            CancellationToken = CancellationToken.None
        };
        
        Parallel.ForEach(items, options, item =>
        {
            ProcessItem(item);
        });
    }
    
    // Breaking out of parallel loop
    public void ParallelWithBreak(int[] numbers)
    {
        Parallel.For(0, numbers.Length, (i, state) =>
        {
            if (numbers[i] < 0)
            {
                Console.WriteLine($"Found negative at {i}, stopping");
                state.Break();  // Stop new iterations, complete running ones
                // state.Stop(); // Stop immediately
            }
            else
            {
                ProcessNumber(numbers[i]);
            }
        });
    }
    
    private void ProcessItem<T>(T item) { }
    private void ProcessNumber(int n) { }
}
```

### Task Factory and Continuations

```csharp
public class TaskFactoryExamples
{
    // Creating tasks
    public void CreateTasks()
    {
        // Task.Run - preferred way
        var task1 = Task.Run(() => Compute(42));
        
        // Task.Factory.StartNew - more options
        var task2 = Task.Factory.StartNew(
            () => Compute(100),
            CancellationToken.None,
            TaskCreationOptions.LongRunning,
            TaskScheduler.Default);
        
        // Task with result
        var task3 = Task.Run(() =>
        {
            Thread.Sleep(100);
            return "Result";
        });
    }
    
    // Continuations
    public async Task ContinuationExample()
    {
        var task = Task.Run(() => Compute(42));
        
        // Continue with another task
        var continuedTask = task.ContinueWith(t =>
        {
            Console.WriteLine($"Result: {t.Result}");
            return t.Result * 2;
        });
        
        // Continue on specific conditions
        var successfulTask = task.ContinueWith(
            t => Console.WriteLine($"Success: {t.Result}"),
            TaskContinuationOptions.OnlyOnRanToCompletion);
        
        var failedTask = task.ContinueWith(
            t => Console.WriteLine($"Error: {t.Exception?.Message}"),
            TaskContinuationOptions.OnlyOnFaulted);
        
        // Multiple continuations
        var allContinuations = Task.WhenAll(successfulTask, failedTask);
    }
    
    // Parent-child tasks
    public void ParentChildTasks()
    {
        var parent = Task.Factory.StartNew(() =>
        {
            Console.WriteLine("Parent starting");
            
            var child1 = Task.Factory.StartNew(() =>
            {
                Console.WriteLine("Child 1");
                Thread.Sleep(100);
            }, TaskCreationOptions.AttachedToParent);
            
            var child2 = Task.Factory.StartNew(() =>
            {
                Console.WriteLine("Child 2");
                Thread.Sleep(150);
            }, TaskCreationOptions.AttachedToParent);
            
            Console.WriteLine("Parent done");
        });
        
        parent.Wait();  // Waits for parent and attached children
    }
    
    private int Compute(int value)
    {
        Thread.Sleep(100);
        return value * 2;
    }
}
```

### Dataflow (System.Threading.Tasks.Dataflow)

```csharp
// Requires System.Threading.Tasks.Dataflow NuGet package
public class DataflowExamples
{
    // ActionBlock - processes data
    public async Task ActionBlockExample()
    {
        var actionBlock = new ActionBlock<int>(n =>
        {
            Console.WriteLine($"Processing {n}");
            Thread.Sleep(100);
        }, new ExecutionDataflowBlockOptions
        {
            MaxDegreeOfParallelism = 4
        });
        
        for (int i = 0; i < 20; i++)
        {
            actionBlock.Post(i);
        }
        
        actionBlock.Complete();
        await actionBlock.Completion;
    }
    
    // TransformBlock - transforms data
    public async Task TransformBlockExample()
    {
        var transformBlock = new TransformBlock<string, int>(
            s => s.Length,
            new ExecutionDataflowBlockOptions
            {
                MaxDegreeOfParallelism = 4
            });
        
        var actionBlock = new ActionBlock<int>(
            length => Console.WriteLine($"Length: {length}"));
        
        // Link blocks together
        transformBlock.LinkTo(actionBlock, new DataflowLinkOptions
        {
            PropagateCompletion = true
        });
        
        transformBlock.Post("Hello");
        transformBlock.Post("World");
        transformBlock.Post("Dataflow!");
        
        transformBlock.Complete();
        await actionBlock.Completion;
    }
    
    // Pipeline with multiple blocks
    public async Task PipelineExample()
    {
        var downloadBlock = new TransformBlock<string, string>(
            url => DownloadAsync(url).Result);
        
        var parseBlock = new TransformBlock<string, Document>(
            content => ParseDocument(content));
        
        var saveBlock = new ActionBlock<Document>(
            doc => SaveDocument(doc));
        
        // Build pipeline
        var linkOptions = new DataflowLinkOptions { PropagateCompletion = true };
        downloadBlock.LinkTo(parseBlock, linkOptions);
        parseBlock.LinkTo(saveBlock, linkOptions);
        
        // Feed data
        var urls = new[] { "url1", "url2", "url3" };
        foreach (var url in urls)
        {
            downloadBlock.Post(url);
        }
        
        downloadBlock.Complete();
        await saveBlock.Completion;
    }
    
    private Task<string> DownloadAsync(string url) => Task.FromResult($"Content of {url}");
    private Document ParseDocument(string content) => new Document { Content = content };
    private void SaveDocument(Document doc) => Console.WriteLine($"Saved: {doc.Content.Length} chars");
}

public class Document
{
    public string Content { get; set; } = "";
}
```

---

## 8. Parallel LINQ (PLINQ)

### Understanding PLINQ

PLINQ is a parallel implementation of LINQ that can significantly speed up queries on multi-core machines. Not all queries benefit from parallelization—small collections, simple operations, or operations with side effects may be slower with PLINQ. Always measure performance before and after parallelizing.

```csharp
public class PlinqExamples
{
    // Basic PLINQ
    public void BasicPlinq()
    {
        var numbers = Enumerable.Range(1, 10_000_000);
        
        // Sequential LINQ
        var sequentialResult = numbers
            .Where(n => n % 2 == 0)
            .Select(n => n * n)
            .Sum();
        
        // Parallel LINQ - just add AsParallel()
        var parallelResult = numbers
            .AsParallel()
            .Where(n => n % 2 == 0)
            .Select(n => n * n)
            .Sum();
    }
    
    // Controlling degree of parallelism
    public void DegreeOfParallelism()
    {
        var numbers = Enumerable.Range(1, 1000);
        
        var result = numbers
            .AsParallel()
            .WithDegreeOfParallelism(4)  // Use 4 cores
            .Select(n => n * n)
            .ToList();
    }
    
    // Cancellation
    public async Task WithCancellation()
    {
        var cts = new CancellationTokenSource();
        var numbers = Enumerable.Range(1, 10_000_000);
        
        var task = Task.Run(() =>
        {
            var result = numbers
                .AsParallel()
                .WithCancellation(cts.Token)
                .Select(n =>
                {
                    Thread.Sleep(1);
                    return n * n;
                })
                .ToList();
        });
        
        await Task.Delay(100);
        cts.Cancel();  // Cancel after 100ms
        
        try
        {
            await task;
        }
        catch (OperationCanceledException)
        {
            Console.WriteLine("Query was cancelled");
        }
    }
    
    // Order preservation
    public void OrderPreservation()
    {
        var numbers = Enumerable.Range(1, 100);
        
        // Order not preserved
        var unordered = numbers
            .AsParallel()
            .Select(n => n * n)
            .ToList();
        
        // Order preserved
        var ordered = numbers
            .AsParallel()
            .AsOrdered()
            .Select(n => n * n)
            .ToList();
    }
    
    // ForAll - execute without merging
    public void ForAllExample()
    {
        var numbers = Enumerable.Range(1, 1000);
        var results = new ConcurrentBag<int>();
        
        numbers
            .AsParallel()
            .Where(n => n % 2 == 0)
            .ForAll(n => results.Add(n * n));
        
        // ForAll is more efficient than ToList() when you don't need ordering
    }
    
    // Merge options
    public void MergeOptions()
    {
        var numbers = Enumerable.Range(1, 10_000);
        
        // FullyBuffered - all results collected before any returned
        var fullyBuffered = numbers
            .AsParallel()
            .WithMergeOptions(ParallelMergeOptions.FullyBuffered)
            .Select(n => n * n);
        
        // NotBuffered - results returned as soon as available
        var notBuffered = numbers
            .AsParallel()
            .WithMergeOptions(ParallelMergeOptions.NotBuffered)
            .Select(n => n * n);
        
        // AutoBuffered - balance between speed and memory
        var autoBuffered = numbers
            .AsParallel()
            .WithMergeOptions(ParallelMergeOptions.AutoBuffered)
            .Select(n => n * n);
    }
}
```

---

## 9. Concurrent Collections

### Thread-Safe Collections

The `System.Collections.Concurrent` namespace provides thread-safe collections designed for concurrent access. These collections use lock-free algorithms and fine-grained locking for better performance than wrapping regular collections with locks.

```csharp
public class ConcurrentCollectionExamples
{
    // ConcurrentDictionary
    public void ConcurrentDictionaryExample()
    {
        var dict = new ConcurrentDictionary<string, int>();
        
        // Add or update
        dict.TryAdd("one", 1);
        dict.AddOrUpdate("two", 2, (key, oldValue) => oldValue + 1);
        
        // Get or add
        var value = dict.GetOrAdd("three", 3);
        var value2 = dict.GetOrAdd("three", key => CalculateValue(key));
        
        // Update if exists
        dict.TryUpdate("one", 10, 1);  // Only updates if current value is 1
        
        // Try get
        if (dict.TryGetValue("one", out var val))
        {
            Console.WriteLine(val);
        }
    }
    
    // ConcurrentQueue
    public void ConcurrentQueueExample()
    {
        var queue = new ConcurrentQueue<int>();
        
        // Enqueue
        queue.Enqueue(1);
        queue.Enqueue(2);
        
        // TryDequeue
        while (queue.TryDequeue(out var item))
        {
            Console.WriteLine(item);
        }
        
        // TryPeek (doesn't remove)
        if (queue.TryPeek(out var first))
        {
            Console.WriteLine($"First: {first}");
        }
    }
    
    // ConcurrentStack
    public void ConcurrentStackExample()
    {
        var stack = new ConcurrentStack<int>();
        
        // Push
        stack.Push(1);
        stack.PushRange(new[] { 2, 3, 4 });
        
        // TryPop
        while (stack.TryPop(out var item))
        {
            Console.WriteLine(item);
        }
        
        // TryPopRange
        var buffer = new int[10];
        int count = stack.TryPopRange(buffer);
    }
    
    // ConcurrentBag
    public void ConcurrentBagExample()
    {
        var bag = new ConcurrentBag<int>();
        
        // Add
        bag.Add(1);
        bag.Add(2);
        
        // TryTake (removes arbitrary element)
        while (bag.TryTake(out var item))
        {
            Console.WriteLine(item);
        }
    }
    
    // BlockingCollection
    public void BlockingCollectionExample()
    {
        var bc = new BlockingCollection<int>(boundedCapacity: 100);
        
        // Producer
        var producer = Task.Run(() =>
        {
            for (int i = 0; i < 1000; i++)
            {
                bc.Add(i);  // Blocks if full
            }
            bc.CompleteAdding();  // Signal completion
        });
        
        // Consumer
        var consumer = Task.Run(() =>
        {
            foreach (var item in bc.GetConsumingEnumerable())  // Blocks if empty
            {
                Console.WriteLine(item);
            }
        });
        
        Task.WaitAll(producer, consumer);
    }
    
    // Producer-Consumer pattern
    public async Task ProducerConsumerExample()
    {
        var dataQueue = new BlockingCollection<DataItem>();
        
        // Multiple producers
        var producers = Enumerable.Range(0, 3)
            .Select(producerId => Task.Run(() =>
            {
                for (int i = 0; i < 10; i++)
                {
                    var item = new DataItem { Id = producerId * 100 + i };
                    dataQueue.Add(item);
                    Console.WriteLine($"Producer {producerId} added item {item.Id}");
                }
            }))
            .ToArray();
        
        // Signal completion when all producers done
        _ = Task.Run(async () =>
        {
            await Task.WhenAll(producers);
            dataQueue.CompleteAdding();
        });
        
        // Single consumer
        foreach (var item in dataQueue.GetConsumingEnumerable())
        {
            Console.WriteLine($"Consumer processing item {item.Id}");
            await Task.Delay(10);
        }
    }
    
    private int CalculateValue(string key) => key.Length;
}

public class DataItem
{
    public int Id { get; set; }
}
```

---

## 10. Memory Management and Garbage Collection

### Understanding Garbage Collection

C# uses automatic memory management through garbage collection (GC). The GC automatically frees memory occupied by objects that are no longer reachable from the application. Understanding how GC works helps you write more efficient code and debug memory issues.

```csharp
public class MemoryManagementExamples
{
    // Object lifecycle
    public void ObjectLifecycle()
    {
        var obj = new LargeObject();
        
        // Use the object
        obj.DoWork();
        
        // When method ends, obj becomes eligible for GC
        // No explicit deletion needed
        
        // Force GC (usually not recommended)
        GC.Collect();
        GC.WaitForPendingFinalizers();
        GC.Collect();
    }
    
    // IDisposable pattern
    public class ResourceHolder : IDisposable
    {
        private IntPtr _handle;
        private bool _disposed = false;
        
        public ResourceHolder()
        {
            _handle = AllocateResource();
        }
        
        public void DoWork()
        {
            if (_disposed)
                throw new ObjectDisposedException(nameof(ResourceHolder));
            
            // Use resource
        }
        
        public void Dispose()
        {
            Dispose(true);
            GC.SuppressFinalize(this);
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
                FreeResource(_handle);
                _handle = IntPtr.Zero;
                
                _disposed = true;
            }
        }
        
        ~ResourceHolder()
        {
            Dispose(false);
        }
        
        private IntPtr AllocateResource() => IntPtr.Zero;
        private void FreeResource(IntPtr handle) { }
    }
    
    // Using declarations (C# 8+)
    public void UsingDeclarations()
    {
        using var holder = new ResourceHolder();
        using var stream = new FileStream("file.txt", FileMode.Open);
        using var reader = new StreamReader(stream);
        
        // Resources disposed at end of scope
        var content = reader.ReadToEnd();
        Console.WriteLine(content);
    }
    
    // Large Object Heap
    public void LargeObjectHeap()
    {
        // Objects > 85,000 bytes go to LOH
        // LOH is not compacted (in older .NET versions)
        
        var largeArray = new byte[100_000];  // Goes to LOH
        
        // In .NET 4.5.1+, can compact LOH
        // GCSettings.LargeObjectHeapCompactionMode = GCLargeObjectHeapCompactionMode.CompactOnce;
        // GC.Collect();
    }
    
    // GC generation info
    public void GenerationInfo()
    {
        var obj = new object();
        
        Console.WriteLine($"Generation: {GC.GetGeneration(obj)}");
        Console.WriteLine($"Total memory: {GC.GetTotalMemory(forceFullCollection: false):N0} bytes");
        
        // Generation 0: Short-lived objects
        // Generation 1: Medium-lived objects
        // Generation 2: Long-lived objects
    }
}

public class LargeObject
{
    public void DoWork() { }
}
```

### Weak References

```csharp
public class WeakReferenceExamples
{
    // Weak reference - doesn't prevent GC
    public void BasicWeakReference()
    {
        var weakRef = new WeakReference(new LargeObject());
        
        // Try to get the object
        if (weakRef.Target is LargeObject obj)
        {
            obj.DoWork();
        }
        else
        {
            Console.WriteLine("Object was collected");
        }
    }
    
    // Weak reference cache
    public class WeakCache<TKey, TValue> where TValue : class
    {
        private readonly Dictionary<TKey, WeakReference<TValue>> _cache = new();
        
        public bool TryGetValue(TKey key, out TValue? value)
        {
            if (_cache.TryGetValue(key, out var weakRef))
            {
                return weakRef.TryGetTarget(out value);
            }
            value = null;
            return false;
        }
        
        public void Add(TKey key, TValue value)
        {
            _cache[key] = new WeakReference<TValue>(value);
        }
        
        public void CleanDeadReferences()
        {
            var deadKeys = _cache
                .Where(kvp => !kvp.Value.TryGetTarget(out _))
                .Select(kvp => kvp.Key)
                .ToList();
            
            foreach (var key in deadKeys)
            {
                _cache.Remove(key);
            }
        }
    }
    
    // Conditional weak table - associates data with objects without extending their lifetime
    public void ConditionalWeakTable()
    {
        var table = new ConditionalWeakTable<string, List<string>>();
        
        var key = "hello";
        table.Add(key, new List<string> { "one", "two" });
        
        if (table.TryGetValue(key, out var list))
        {
            Console.WriteLine(string.Join(", ", list));
        }
    }
}
```

---

## 11. Span and Memory Types

### Understanding Span<T>

`Span<T>` is a ref struct that provides a type-safe view over contiguous memory without allocating new memory. It's stack-only and can point to arrays, stackalloc memory, or native memory. Span is essential for high-performance scenarios where you want to avoid allocations.

```csharp
public class SpanExamples
{
    // Basic span usage
    public void BasicSpan()
    {
        // From array
        int[] array = { 1, 2, 3, 4, 5 };
        Span<int> span = array;
        
        // Slice
        Span<int> slice = span.Slice(1, 3);  // 2, 3, 4
        
        // Modify through span
        span[0] = 10;
        Console.WriteLine(array[0]);  // 10
        
        // Iterate
        foreach (var item in span)
        {
            Console.WriteLine(item);
        }
    }
    
    // Span from stackalloc
    public void StackAllocSpan()
    {
        // Allocate on stack
        Span<int> buffer = stackalloc int[100];
        
        // Use without heap allocation
        for (int i = 0; i < buffer.Length; i++)
        {
            buffer[i] = i;
        }
        
        // Process buffer...
    }
    
    // String manipulation without allocation
    public void StringWithSpan()
    {
        string text = "Hello, World!";
        
        // ReadOnlySpan<char> from string
        ReadOnlySpan<char> span = text.AsSpan();
        
        // Slicing doesn't create new string
        ReadOnlySpan<char> hello = span.Slice(0, 5);  // "Hello"
        
        // Parsing without allocation
        string number = "12345";
        if (int.TryParse(number.AsSpan(), out int result))
        {
            Console.WriteLine(result);
        }
    }
    
    // High-performance parsing
    public void HighPerformanceParsing()
    {
        var input = "Name:John;Age:30;City:NYC"u8;  // UTF8 string literal
        
        var reader = new SpanReader(input);
        
        while (reader.TryReadTo(out ReadOnlySpan<byte> field, (byte)';'))
        {
            var colonIndex = field.IndexOf((byte)':');
            var name = field[..colonIndex];
            var value = field[(colonIndex + 1)..];
            
            Console.WriteLine($"{Encoding.UTF8.GetString(name)}: {Encoding.UTF8.GetString(value)}");
        }
    }
    
    // Span-based string operations
    public static class SpanStringUtils
    {
        public static bool EqualsIgnoreCase(ReadOnlySpan<char> a, ReadOnlySpan<char> b)
        {
            if (a.Length != b.Length) return false;
            
            for (int i = 0; i < a.Length; i++)
            {
                if (char.ToLowerInvariant(a[i]) != char.ToLowerInvariant(b[i]))
                    return false;
            }
            return true;
        }
        
        public static int IndexOf(ReadOnlySpan<char> span, ReadOnlySpan<char> value)
        {
            for (int i = 0; i <= span.Length - value.Length; i++)
            {
                if (span.Slice(i, value.Length).SequenceEqual(value))
                    return i;
            }
            return -1;
        }
        
        public static void ToUpper(ReadOnlySpan<char> source, Span<char> destination)
        {
            for (int i = 0; i < source.Length; i++)
            {
                destination[i] = char.ToUpperInvariant(source[i]);
            }
        }
    }
}

// Simple span reader helper
public ref struct SpanReader
{
    private ReadOnlySpan<byte> _span;
    
    public SpanReader(ReadOnlySpan<byte> span) => _span = span;
    
    public bool TryReadTo(out ReadOnlySpan<byte> result, byte delimiter)
    {
        int index = _span.IndexOf(delimiter);
        if (index >= 0)
        {
            result = _span[..index];
            _span = _span[(index + 1)..];
            return true;
        }
        
        if (_span.Length > 0)
        {
            result = _span;
            _span = ReadOnlySpan<byte>.Empty;
            return true;
        }
        
        result = default;
        return false;
    }
}
```

### Memory<T> for Async Scenarios

```csharp
public class MemoryExamples
{
    // Memory<T> can be used with async (Span<T> cannot)
    public async Task ProcessDataAsync(Memory<byte> buffer)
    {
        // Can await with Memory
        await Task.Delay(100);
        
        // Convert to Span for synchronous work
        ProcessBuffer(buffer.Span);
    }
    
    private void ProcessBuffer(Span<byte> buffer)
    {
        for (int i = 0; i < buffer.Length; i++)
        {
            buffer[i] = (byte)(buffer[i] * 2);
        }
    }
    
    // IMemoryOwner<T> for memory pooling
    public void MemoryOwner()
    {
        using var owner = MemoryPool<byte>.Shared.Rent(1024);
        Memory<byte> memory = owner.Memory;
        
        // Use memory...
        
        // Memory returned to pool when disposed
    }
    
    // ArrayPool for large arrays
    public void ArrayPoolExample()
    {
        var pool = ArrayPool<int>.Shared;
        int[] array = pool.Rent(1000);
        
        try
        {
            // Use array (actual size may be larger than requested)
            for (int i = 0; i < 1000; i++)
            {
                array[i] = i;
            }
        }
        finally
        {
            pool.Return(array, clearArray: true);  // Clear before returning
        }
    }
}
```

---

## 12. Nullable Reference Types

### Understanding Nullable Reference Types

Introduced in C# 8.0, nullable reference types help prevent null reference exceptions by making reference type nullability explicit. The compiler warns when you might dereference a null value, helping catch potential bugs at compile time.

```csharp
#nullable enable

public class NullableReferenceTypes
{
    // Non-nullable reference type (default)
    public string Name { get; set; } = "";
    
    // Nullable reference type
    public string? MiddleName { get; set; }
    
    // Warning: possible null assignment
    // public string NotNullable = null;  // Warning!
    
    public void Process(string required, string? optional)
    {
        // No warning - required is non-nullable
        Console.WriteLine(required.Length);
        
        // Warning - optional might be null
        // Console.WriteLine(optional.Length);  // Warning!
        
        // Safe access patterns
        if (optional is not null)
        {
            Console.WriteLine(optional.Length);  // No warning
        }
        
        // Null-conditional operator
        Console.WriteLine(optional?.Length ?? 0);
        
        // Null-coalescing with throw
        string definitelyNotNull = optional ?? throw new ArgumentNullException(nameof(optional));
    }
    
    // Null state attributes
    public bool TryGetValue([NotNullWhen(true)] out string? value)
    {
        value = "something";
        return true;
    }
    
    [return: NotNullIfNotNull("input")]
    public string? ProcessInput(string? input)
    {
        return input?.ToUpper();
    }
    
    // Member attributes
    [MemberNotNull(nameof(Name))]
    public void EnsureInitialized()
    {
        Name = "Default";
    }
}
```

### Nullable Attributes

```csharp
#nullable enable

public class NullableAttributes
{
    // NotNull - parameter will not be null after call
    public void EnsureNotNull([NotNull] ref string? value)
    {
        value ??= "default";
    }
    
    // DoesNotReturn - method never returns
    [DoesNotReturn]
    public void FailFast(string message)
    {
        throw new InvalidOperationException(message);
    }
    
    // DoesNotReturnIf - doesn't return if parameter is true/false
    public void Process(string? value)
    {
        ArgumentNullException.ThrowIfNull(value);  // Uses [DoesNotReturnIf(true)]
        
        // No warning - compiler knows value is not null here
        Console.WriteLine(value.Length);
    }
    
    // MaybeNull / MaybeNullWhen - might return null
    public bool TryGet([MaybeNullWhen(false)] out string value)
    {
        value = default!;  // Or actual value
        return false;
    }
    
    // NotNullWhen - definitely not null when result is true/false
    public bool IsNullOrEmpty([NotNullWhen(false)] string? value)
    {
        return string.IsNullOrEmpty(value);
    }
}
```

---

## 13. Records and Record Structs

### Understanding Records

Records (introduced in C# 9) are reference types that provide value-based equality semantics. They're designed for immutable data models and come with built-in equality, GetHashCode, and ToString implementations. Record structs (C# 10) provide similar functionality for value types.

```csharp
// Record (reference type)
public record Person(string FirstName, string LastName, int Age);

// Record with additional members
public record Student(string FirstName, string LastName, int Age, string School)
    : Person(FirstName, LastName, Age)
{
    public string StudentId { get; init; } = Guid.NewGuid().ToString();
    
    public string FullName => $"{FirstName} {LastName}";
    
    // Can add methods
    public bool IsAdult() => Age >= 18;
}

// Record struct (value type)
public record struct Point(double X, double Y)
{
    public double Distance() => Math.Sqrt(X * X + Y * Y);
}

// Positional record with mutable property (not recommended)
public record MutableRecord(string Name)
{
    public string Description { get; set; }  // Mutable
}

public class RecordExamples
{
    public void Demo()
    {
        var person = new Person("John", "Doe", 30);
        
        // Value-based equality
        var person2 = new Person("John", "Doe", 30);
        Console.WriteLine(person == person2);  // true
        
        // Built-in ToString
        Console.WriteLine(person);  // Person { FirstName = John, LastName = Doe, Age = 30 }
        
        // Non-destructive mutation (with-expression)
        var olderPerson = person with { Age = 31 };
        Console.WriteLine(olderPerson.Age);  // 31
        Console.WriteLine(person.Age);       // 30 (unchanged)
        
        // Deconstruction
        var (firstName, lastName, age) = person;
        Console.WriteLine($"{firstName} {lastName}, {age}");
        
        // Pattern matching
        string Describe(Person p) => p switch
        {
            { Age: < 18 } => "Minor",
            { Age: >= 18 and < 65 } => "Adult",
            { Age: >= 65 } => "Senior",
            _ => "Unknown"
        };
    }
    
    // Record inheritance
    public void RecordInheritance()
    {
        var student = new Student("Jane", "Smith", 20, "MIT");
        Person person = student;  // Polymorphism works
        
        Console.WriteLine(student.FullName);
        Console.WriteLine(student.IsAdult());
    }
    
    // Record struct
    public void RecordStruct()
    {
        var point = new Point(3, 4);
        var moved = point with { X = 10 };
        
        Console.WriteLine(point.Distance());    // 5.0
        Console.WriteLine(moved.Distance());    // 10.77...
    }
}
```

### Advanced Record Features

```csharp
// Record with validation
public record PositiveNumber(int Value)
{
    public int Value { get; init; } = Value > 0 
        ? Value 
        : throw new ArgumentException("Value must be positive", nameof(Value));
}

// Record with custom equality
public record CustomEquality(string Name, int Id)
{
    public virtual bool Equals(CustomEquality? other)
    {
        if (other is null) return false;
        return Id == other.Id;  // Compare only by Id
    }
    
    public override int GetHashCode() => Id.GetHashCode();
}

// Record as DTO
public record OrderDto(
    int Id,
    string CustomerName,
    decimal Total,
    DateTime OrderDate,
    List<OrderItemDto> Items);

public record OrderItemDto(int ProductId, string ProductName, int Quantity, decimal Price);

// Record with entity semantics
public abstract record Entity(int Id);

public record Customer(int Id, string Name, string Email) : Entity(Id);

public record Product(int Id, string Name, decimal Price) : Entity(Id);

// JSON serialization works well with records
public class RecordSerialization
{
    public void Serialize()
    {
        var person = new Person("John", "Doe", 30);
        string json = System.Text.Json.JsonSerializer.Serialize(person);
        // {"FirstName":"John","LastName":"Doe","Age":30}
        
        var deserialized = System.Text.Json.JsonSerializer.Deserialize<Person>(json);
    }
}
```

---

## 14. Pattern Matching

### Pattern Matching Evolution

C# has evolved pattern matching significantly since C# 7. Pattern matching allows you to test expressions for certain characteristics and extract information when patterns match. Modern C# supports complex nested patterns, list patterns, and more.

```csharp
public class PatternMatchingExamples
{
    // Basic type patterns
    public string Describe(object obj) => obj switch
    {
        null => "It's null",
        int i => $"Integer: {i}",
        double d => $"Double: {d}",
        string s => $"String: {s}",
        bool b => $"Boolean: {b}",
        var v => $"Unknown type: {v.GetType().Name}"
    };
    
    // Property patterns
    public decimal CalculateDiscount(Order order) => order switch
    {
        { Total: > 1000 } => 0.10m,
        { Total: > 500 } => 0.05m,
        { Total: > 100 } => 0.02m,
        { Customer.IsPremium: true } => 0.07m,
        _ => 0m
    };
    
    // Nested property patterns
    public string GetLocation(Person person) => person switch
    {
        { Address: { City: "New York", Country: "USA" } } => "NYC",
        { Address: { City: var city } } => city,
        { Address: null } => "No address",
        _ => "Unknown"
    };
    
    // Tuple patterns
    public string RockPaperScissors(string player1, string player2)
        => (player1, player2) switch
        {
            ("rock", "paper") => "Paper wins",
            ("rock", "scissors") => "Rock wins",
            ("paper", "scissors") => "Scissors wins",
            ("paper", "rock") => "Paper wins",
            ("scissors", "rock") => "Rock wins",
            ("scissors", "paper") => "Scissors wins",
            (_, _) when player1 == player2 => "Tie",
            _ => "Invalid input"
        };
    
    // Relational patterns (C# 9)
    public string ClassifyAge(int age) => age switch
    {
        < 0 => "Invalid",
        < 13 => "Child",
        < 20 => "Teenager",
        < 65 => "Adult",
        >= 65 => "Senior"
    };
    
    // Combined patterns (and, or, not)
    public bool IsValidInput(string? input) => input switch
    {
        null or "" or " " => false,
        not null and { Length: > 10 } => true,
        _ => false
    };
    
    // List patterns (C# 11)
    public string DescribeArray(int[] numbers) => numbers switch
    {
        [] => "Empty",
        [var single] => $"Single: {single}",
        [var first, var second] => $"Two: {first}, {second}",
        [var first, .., var last] => $"First: {first}, Last: {last}",
        [_, .., var secondLast, var last] => $"Last two: {secondLast}, {last}",
        [>= 0, ..] => "Starts with non-negative",
        _ => "Other"
    };
    
    // List pattern with slice
    public bool IsValidQueue(int[] queue) => queue switch
    {
        [1, 2, 3, ..] => true,  // Starts with 1, 2, 3
        [.., 99, 100] => true,  // Ends with 99, 100
        _ => false
    };
    
    // When clauses
    public decimal CalculateShipping(Order order) => order switch
    {
        { Items.Count: 0 } => 0m,
        { Items.Count: 1, Total: var t } when t > 100 => 0m,  // Free shipping
        { Items.Count: var count, Total: var t } when count > 5 && t > 200 => 0m,
        { Weight: > 10 } => 15m,
        _ => 5m
    };
    
    // Pattern with explicit type check
    public void ProcessShape(Shape shape)
    {
        double area = shape switch
        {
            Circle { Radius: var r } => Math.PI * r * r,
            Rectangle { Width: var w, Height: var h } => w * h,
            Triangle { Base: var b, Height: var h } => 0.5 * b * h,
            Shape s when s.IsRegular => s.CalculateRegularArea(),
            _ => throw new ArgumentException("Unknown shape")
        };
    }
}

// Supporting types
public record Address(string City, string Country);
public record Person(string Name, Address? Address);
public record Order(decimal Total, Customer Customer, List<Item> Items, double Weight);
public record Customer(string Name, bool IsPremium);
public record Item(string Name, decimal Price);

public abstract class Shape
{
    public bool IsRegular { get; init; }
    public abstract double CalculateRegularArea();
}

public record Circle(double Radius) : Shape { public override double CalculateRegularArea() => Math.PI * Radius * Radius; }
public record Rectangle(double Width, double Height) : Shape { public override double CalculateRegularArea() => Width * Height; }
public record Triangle(double Base, double Height) : Shape { public override double CalculateRegularArea() => 0.5 * Base * Height; }
```

---

## 15. Tuples and Discards

### Understanding Tuples

Tuples provide a lightweight way to group multiple values without defining a separate type. C# 7+ introduced value tuples with named elements, making them much more usable than the old `Tuple` generic classes.

```csharp
public class TupleExamples
{
    // Basic tuple creation
    public void BasicTuples()
    {
        // Tuple literal
        var point = (10, 20);
        Console.WriteLine(point.Item1);  // 10
        Console.WriteLine(point.Item2);  // 20
        
        // Named tuple elements
        var namedPoint = (X: 10, Y: 20);
        Console.WriteLine(namedPoint.X);  // 10
        Console.WriteLine(namedPoint.Y);  // 20
        
        // Tuple with mixed naming
        var mixed = (Name: "Alice", 30, City: "NYC");
        Console.WriteLine(mixed.Name);    // Alice
        Console.WriteLine(mixed.Item2);   // 30
        Console.WriteLine(mixed.City);    // NYC
        
        // Inferred names (C# 7.1+)
        string firstName = "John";
        string lastName = "Doe";
        var inferred = (firstName, lastName);
        Console.WriteLine(inferred.firstName);  // John
    }
    
    // Tuple return types
    public (int Min, int Max) FindMinMax(int[] numbers)
    {
        if (numbers.Length == 0)
            return (0, 0);
        
        int min = numbers[0];
        int max = numbers[0];
        
        foreach (var n in numbers)
        {
            if (n < min) min = n;
            if (n > max) max = n;
        }
        
        return (min, max);
    }
    
    // Tuple deconstruction
    public void Deconstruction()
    {
        var (min, max) = FindMinMax(new[] { 3, 1, 4, 1, 5, 9, 2, 6 });
        Console.WriteLine($"Min: {min}, Max: {max}");
        
        // With discards
        var (_, maximum) = FindMinMax(new[] { 1, 2, 3 });
        Console.WriteLine($"Only max: {maximum}");
        
        // Deconstruct with existing variables
        int minimum, maximumValue;
        (minimum, maximumValue) = FindMinMax(new[] { 5, 4, 3, 2, 1 });
    }
    
    // Custom deconstruction
    public class Rectangle
    {
        public double Width { get; }
        public double Height { get; }
        
        public Rectangle(double width, double height)
        {
            Width = width;
            Height = height;
        }
        
        // Deconstruct method
        public void Deconstruct(out double width, out double height)
        {
            width = Width;
            height = Height;
        }
        
        public void Deconstruct(out double width, out double height, out double area)
        {
            width = Width;
            height = Height;
            area = Width * Height;
        }
    }
    
    public void CustomDeconstruction()
    {
        var rect = new Rectangle(10, 20);
        
        var (w, h) = rect;
        Console.WriteLine($"{w}x{h}");
        
        var (width, height, area) = rect;
        Console.WriteLine($"Area: {area}");
    }
    
    // Tuple as method parameter
    public void ProcessCoordinates(IEnumerable<(int X, int Y)> points)
    {
        foreach (var (x, y) in points)
        {
            Console.WriteLine($"({x}, {y})");
        }
    }
    
    // Tuple with LINQ
    public void TupleWithLinq()
    {
        var numbers = Enumerable.Range(1, 10);
        
        var results = numbers
            .Select(n => (Number: n, Square: n * n, Cube: n * n * n))
            .Where(t => t.Square > 25)
            .Select(t => (t.Number, t.Square));
    }
    
    // Tuple equality
    public void TupleEquality()
    {
        var t1 = (X: 1, Y: 2);
        var t2 = (X: 1, Y: 2);
        var t3 = (A: 1, B: 2);  // Different names, same values
        
        Console.WriteLine(t1 == t2);  // true
        Console.WriteLine(t1 == t3);  // true (names don't matter for equality)
    }
}
```

### Discards

```csharp
public class DiscardExamples
{
    // Discard in deconstruction
    public void DiscardInDeconstruction()
    {
        var tuple = (1, 2, 3, 4, 5);
        var (first, _, third, _, fifth) = tuple;
        
        Console.WriteLine($"{first}, {third}, {fifth}");  // 1, 3, 5
    }
    
    // Discard in out parameter
    public void DiscardInOut()
    {
        string input = "123";
        
        // Don't care about the actual parsed value
        if (int.TryParse(input, out _))
        {
            Console.WriteLine("Valid integer");
        }
        
        // Common with dictionary
        var dict = new Dictionary<string, int>();
        if (dict.TryGetValue("key", out _))
        {
            Console.WriteLine("Key exists");
        }
    }
    
    // Discard in pattern matching
    public void DiscardInPatternMatching()
    {
        object obj = "Hello";
        
        if (obj is string _)
        {
            Console.WriteLine("It's a string");
        }
        
        // Switch with discards
        string result = obj switch
        {
            string _ => "String",
            int _ => "Integer",
            _ => "Unknown"
        };
    }
    
    // Discard in lambda
    public void DiscardInLambda()
    {
        // Single discard
        Func<int, int, int> add = (_, y) => y;
        
        // Multiple discards
        Action<int, int, int> logThird = (_, _, z) => Console.WriteLine(z);
    }
    
    // Discard in standalone expression (C# 9+)
    public void StandaloneDiscard()
    {
        _ = ComputeExpensiveValue();  // Discard result explicitly
        
        // Useful for validation
        _ = ComputeExpensiveValue() ?? throw new InvalidOperationException("Value is null");
    }
    
    private string? ComputeExpensiveValue() => "value";
}
```

---

## 16. Advanced Generics (Covariance and Contravariance)

### Understanding Variance

Variance describes how generic type parameters relate to each other in inheritance hierarchies. Covariance allows a more derived type to be used where a base type is expected. Contravariance allows a less derived type to be used where a more derived type is expected. Invariance means no substitution is allowed.

```csharp
// Covariance (out) - can only be used as output
public interface ICovariant<out T>
{
    T Get();
}

public class CovariantImplementation<T> : ICovariant<T>
{
    private T _value;
    public CovariantImplementation(T value) => _value = value;
    public T Get() => _value;
}

// Contravariance (in) - can only be used as input
public interface IContravariant<in T>
{
    void Process(T item);
}

public class ContravariantImplementation<T> : IContravariant<T>
{
    public void Process(T item) => Console.WriteLine(item);
}

// Combined
public interface IVariant<in TInput, out TOutput>
{
    TOutput Convert(TInput input);
}

public class VariantExamples
{
    public void CovarianceDemo()
    {
        // Covariance: can assign IEnumerable<Derived> to IEnumerable<Base>
        IEnumerable<string> strings = new List<string> { "a", "b" };
        IEnumerable<object> objects = strings;  // Works because IEnumerable is covariant
        
        // Our covariant interface
        ICovariant<string> stringProvider = new CovariantImplementation<string>("Hello");
        ICovariant<object> objectProvider = stringProvider;  // Works!
        
        object value = objectProvider.Get();  // Returns "Hello" as object
    }
    
    public void ContravarianceDemo()
    {
        // Contravariance: can assign Action<Base> to Action<Derived>
        Action<object> objectAction = o => Console.WriteLine(o);
        Action<string> stringAction = objectAction;  // Works because Action is contravariant
        
        // Our contravariant interface
        IContravariant<object> objectProcessor = new ContravariantImplementation<object>();
        IContravariant<string> stringProcessor = objectProcessor;  // Works!
        
        stringProcessor.Process("Hello");  // Processes string as object
    }
    
    public void VarianceRules()
    {
        // Covariant type parameter can only appear in output positions
        // public void Set(T value);  // Error! T is used as input
        
        // Contravariant type parameter can only appear in input positions
        // public T Get();  // Error! T is used as output
        
        // Invariant (no in/out) can appear anywhere
        // but cannot be assigned with variance
    }
}

// Practical example: Generic repository
public interface IRepository<out TRead, in TWrite> where TRead : class where TWrite : class
{
    TRead? GetById(int id);         // TRead in output position
    IEnumerable<TRead> GetAll();    // TRead in output position
    
    void Add(TWrite entity);        // TWrite in input position
    void Update(TWrite entity);     // TWrite in input position
}

// Generic constraints with variance
public interface IComparable<in T>
{
    int CompareTo(T? other);
}
```

### Generic Constraints Deep Dive

```csharp
// Multiple constraints
public class AdvancedGenericExamples
{
    // Class constraint with new()
    public T CreateInstance<T>() where T : class, new()
    {
        return new T();
    }
    
    // Struct constraint
    public T? GetDefault<T>() where T : struct
    {
        return default;
    }
    
    // Base class constraint
    public void ProcessEntity<T>(T entity) where T : EntityBase
    {
        Console.WriteLine(entity.Id);
    }
    
    // Interface constraints
    public T Max<T>(T a, T b) where T : IComparable<T>
    {
        return a.CompareTo(b) > 0 ? a : b;
    }
    
    // Multiple interface constraints
    public void Process<T>(T item) 
        where T : IComparable<T>, IEnumerable<T>, new()
    {
    }
    
    // Constraint with enum
    public Dictionary<T, string> GetEnumDescriptions<T>() where T : struct, Enum
    {
        return Enum.GetValues<T>()
            .ToDictionary(e => e, e => e.ToString());
    }
    
    // Delegate constraint
    public TInvoker CreateInvoker<TDelegate, TInvoker>(TDelegate @delegate)
        where TDelegate : Delegate
        where TInvoker : IInvoker<TDelegate>, new()
    {
        return new TInvoker { Delegate = @delegate };
    }
    
    // Generic type constraint
    public void Process<T, TCollection>(T item)
        where TCollection : IList<T>, new()
    {
        var collection = new TCollection();
        collection.Add(item);
    }
    
    // Notnull constraint (C# 8+)
    public void ProcessNotNull<T>(T item) where T : notnull
    {
        // T cannot be a nullable type
    }
    
    // Unmanaged constraint
    public unsafe void CopyToUnmanaged<T>(T[] source, T* destination, int count)
        where T : unmanaged
    {
        for (int i = 0; i < count; i++)
        {
            destination[i] = source[i];
        }
    }
}

// Supporting types
public abstract class EntityBase
{
    public int Id { get; set; }
}

public interface IInvoker<TDelegate> where TDelegate : Delegate
{
    TDelegate Delegate { get; set; }
}
```

---

## 17. Reflection

### Understanding Reflection

Reflection allows you to examine and manipulate types, methods, properties, and other code elements at runtime. It's essential for building frameworks, serializers, dependency injection containers, and other meta-programming tools. However, reflection has performance costs and can bypass compile-time type safety.

```csharp
public class ReflectionExamples
{
    // Getting type information
    public void TypeInformation()
    {
        // Various ways to get Type
        Type stringType1 = typeof(string);
        Type stringType2 = "hello".GetType();
        Type? stringType3 = Type.GetType("System.String");
        
        // Type properties
        Console.WriteLine($"Name: {stringType1.Name}");
        Console.WriteLine($"FullName: {stringType1.FullName}");
        Console.WriteLine($"Namespace: {stringType1.Namespace}");
        Console.WriteLine($"IsClass: {stringType1.IsClass}");
        Console.WriteLine($"IsValueType: {stringType1.IsValueType}");
        Console.WriteLine($"BaseType: {stringType1.BaseType?.Name}");
        
        // Get assembly
        Assembly assembly = stringType1.Assembly;
        Console.WriteLine($"Assembly: {assembly.FullName}");
    }
    
    // Examining members
    public void ExamineMembers()
    {
        Type type = typeof(DateTime);
        
        // Get all public instance methods
        MethodInfo[] methods = type.GetMethods(BindingFlags.Public | BindingFlags.Instance);
        
        foreach (var method in methods.Where(m => m.Name.StartsWith("To")))
        {
            Console.WriteLine($"{method.Name} returns {method.ReturnType.Name}");
        }
        
        // Get properties
        PropertyInfo[] properties = type.GetProperties();
        foreach (var prop in properties)
        {
            Console.WriteLine($"Property: {prop.PropertyType.Name} {prop.Name}");
        }
        
        // Get fields
        FieldInfo[] fields = type.GetFields(BindingFlags.Public | BindingFlags.Static);
        
        // Get constructors
        ConstructorInfo[] constructors = type.GetConstructors();
        
        // Get events
        EventInfo[] events = type.GetEvents();
        
        // Get attributes
        object[] attributes = type.GetCustomAttributes(true);
    }
    
    // Creating instances dynamically
    public object? CreateInstance(string typeName)
    {
        Type? type = Type.GetType(typeName);
        if (type == null) return null;
        
        // Using Activator
        object? instance = Activator.CreateInstance(type);
        
        // Using constructor info
        ConstructorInfo? constructor = type.GetConstructor(Type.EmptyTypes);
        object? instance2 = constructor?.Invoke(null);
        
        // With parameters
        ConstructorInfo? stringCtor = type.GetConstructor(new[] { typeof(string) });
        object? instance3 = stringCtor?.Invoke(new object[] { "Hello" });
        
        return instance;
    }
    
    // Invoking methods dynamically
    public void InvokeMethods()
    {
        object instance = "Hello World";
        Type type = instance.GetType();
        
        // Get and invoke method
        MethodInfo? method = type.GetMethod("Substring", new[] { typeof(int), typeof(int) });
        object? result = method?.Invoke(instance, new object[] { 0, 5 });
        Console.WriteLine(result);  // "Hello"
        
        // Get and set property
        PropertyInfo? lengthProp = type.GetProperty("Length");
        object? length = lengthProp?.GetValue(instance);
        Console.WriteLine(length);  // 11
        
        // Static method
        MethodInfo? parseMethod = typeof(int).GetMethod("Parse", new[] { typeof(string) });
        object? parsed = parseMethod?.Invoke(null, new object[] { "123" });
        Console.WriteLine(parsed);  // 123
    }
    
    // Generic type reflection
    public void GenericReflection()
    {
        Type listType = typeof(List<int>);
        
        // Check if generic
        Console.WriteLine($"IsGenericType: {listType.IsGenericType}");
        
        // Get generic type definition
        Type? genericDef = listType.GetGenericTypeDefinition();  // List<>
        Console.WriteLine($"Generic definition: {genericDef?.Name}");
        
        // Get type arguments
        Type[] typeArgs = listType.GetGenericArguments();
        Console.WriteLine($"Type argument: {typeArgs[0].Name}");  // Int32
        
        // Make generic type
        Type listStringType = genericDef.MakeGenericType(typeof(string));
        object? listInstance = Activator.CreateInstance(listStringType);
    }
}
```

### Dynamic Object Creation and Method Invocation

```csharp
public class DynamicFactory
{
    private readonly Dictionary<string, Type> _typeCache = new();
    
    public void RegisterType(string name, Type type)
    {
        _typeCache[name] = type;
    }
    
    public object? Create(string typeName, params object[] args)
    {
        if (!_typeCache.TryGetValue(typeName, out var type))
        {
            type = Type.GetType(typeName);
            if (type != null)
                _typeCache[typeName] = type;
        }
        
        if (type == null) return null;
        
        var argTypes = args.Select(a => a.GetType()).ToArray();
        var constructor = type.GetConstructor(argTypes);
        
        return constructor?.Invoke(args);
    }
    
    public T? Create<T>(params object[] args) where T : class
    {
        return Create(typeof(T).AssemblyQualifiedName!, args) as T;
    }
    
    public object? InvokeMethod(object instance, string methodName, params object[] args)
    {
        var type = instance.GetType();
        var argTypes = args.Select(a => a.GetType()).ToArray();
        var method = type.GetMethod(methodName, argTypes);
        return method?.Invoke(instance, args);
    }
    
    public void SetProperty(object instance, string propertyName, object value)
    {
        var type = instance.GetType();
        var property = type.GetProperty(propertyName);
        property?.SetValue(instance, value);
    }
    
    public object? GetProperty(object instance, string propertyName)
    {
        var type = instance.GetType();
        var property = type.GetProperty(propertyName);
        return property?.GetValue(instance);
    }
}
```

---

## 18. Attributes

### Understanding Attributes

Attributes provide a way to add metadata to code elements (classes, methods, properties, etc.). This metadata can be queried at runtime using reflection. Attributes are used extensively in .NET for serialization, validation, web API routing, testing, and more.

```csharp
// Custom attribute
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true)]
public class AuthorAttribute : Attribute
{
    public string Name { get; }
    public string? Email { get; set; }
    public int Version { get; set; } = 1;
    
    public AuthorAttribute(string name)
    {
        Name = name;
    }
}

// Validation attribute
[AttributeUsage(AttributeTargets.Property)]
public class ValidateAttribute : Attribute
{
    public int MinLength { get; set; }
    public int MaxLength { get; set; } = int.MaxValue;
    public bool Required { get; set; }
    public string? Pattern { get; set; }
}

// Usage
[Author("John Doe", Email = "john@example.com", Version = 2)]
[Author("Jane Smith")]
public class UserService
{
    [Validate(MinLength = 3, MaxLength = 50, Required = true)]
    public string UserName { get; set; } = "";
    
    [Validate(MinLength = 8, Pattern = @"^[A-Za-z0-9]+$")]
    public string Password { get; set; } = "";
    
    [Obsolete("Use ProcessAsync instead")]
    public void Process()
    {
        // Old implementation
    }
    
    [Author("Bob Developer")]
    public async Task ProcessAsync()
    {
        // New implementation
    }
}

// Reading attributes at runtime
public class AttributeReader
{
    public static IEnumerable<string> GetAuthors(Type type)
    {
        var attributes = type.GetCustomAttributes<AuthorAttribute>();
        return attributes.Select(a => $"{a.Name} (v{a.Version})");
    }
    
    public static IEnumerable<string> GetMethodAuthors(MethodInfo method)
    {
        var attributes = method.GetCustomAttributes<AuthorAttribute>();
        return attributes.Select(a => a.Name);
    }
    
    public static void ValidateObject(object obj)
    {
        var type = obj.GetType();
        
        foreach (var property in type.GetProperties())
        {
            var attr = property.GetCustomAttribute<ValidateAttribute>();
            if (attr == null) continue;
            
            var value = property.GetValue(obj)?.ToString() ?? "";
            
            if (attr.Required && string.IsNullOrEmpty(value))
                throw new ValidationException($"{property.Name} is required");
            
            if (value.Length < attr.MinLength)
                throw new ValidationException($"{property.Name} must be at least {attr.MinLength} characters");
            
            if (value.Length > attr.MaxLength)
                throw new ValidationException($"{property.Name} must be at most {attr.MaxLength} characters");
            
            if (attr.Pattern != null && !Regex.IsMatch(value, attr.Pattern))
                throw new ValidationException($"{property.Name} doesn't match required pattern");
        }
    }
}

public class ValidationException : Exception
{
    public ValidationException(string message) : base(message) { }
}
```

### Practical Attribute Examples

```csharp
// Caching attribute
[AttributeUsage(AttributeTargets.Method)]
public class CacheAttribute : Attribute
{
    public int DurationSeconds { get; }
    public string? KeyPrefix { get; set; }
    
    public CacheAttribute(int durationSeconds)
    {
        DurationSeconds = durationSeconds;
    }
}

// Audit logging attribute
[AttributeUsage(AttributeTargets.Method)]
public class AuditAttribute : Attribute
{
    public string Action { get; }
    public string Category { get; set; } = "General";
    
    public AuditAttribute(string action)
    {
        Action = action;
    }
}

// API documentation attribute
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method)]
public class ApiDocAttribute : Attribute
{
    public string Description { get; }
    public string[] Tags { get; }
    
    public ApiDocAttribute(string description, params string[] tags)
    {
        Description = description;
        Tags = tags;
    }
}

// Example usage
[ApiDoc("User management service", "Users", "Authentication")]
public class UserApiController
{
    [Cache(60, KeyPrefix = "user")]
    [Audit("GetUser", Category = "Read")]
    [ApiDoc("Retrieves a user by ID", "Query")]
    public User GetUser(int id)
    {
        return new User { Id = id, Name = "Test" };
    }
    
    [Audit("CreateUser", Category = "Write")]
    [ApiDoc("Creates a new user", "Command")]
    public User CreateUser(User user)
    {
        return user;
    }
}

public record User(int Id, string Name);
```

---

## 19. Expression Trees

### Understanding Expression Trees

Expression trees represent code as a tree structure where each node is an expression. Unlike delegates that compile to executable code, expression trees can be analyzed, modified, and compiled at runtime. They're used extensively in Entity Framework (LINQ to SQL), dynamic LINQ, and mocking frameworks.

```csharp
using System.Linq.Expressions;

public class ExpressionTreeExamples
{
    // Simple expression
    public void SimpleExpression()
    {
        // Lambda as delegate
        Func<int, bool> isEvenDelegate = n => n % 2 == 0;
        
        // Lambda as expression tree
        Expression<Func<int, bool>> isEvenExpression = n => n % 2 == 0;
        
        // Compile expression to delegate
        Func<int, bool> compiled = isEvenExpression.Compile();
        Console.WriteLine(compiled(4));  // true
    }
    
    // Analyzing expression trees
    public void AnalyzeExpression(Expression<Func<int, int, int>> expression)
    {
        // Body of the expression
        Console.WriteLine($"Body: {expression.Body}");
        
        // Parameters
        foreach (var param in expression.Parameters)
        {
            Console.WriteLine($"Parameter: {param.Name} ({param.Type.Name})");
        }
        
        // Analyze binary expression
        if (expression.Body is BinaryExpression binary)
        {
            Console.WriteLine($"Operation: {binary.NodeType}");
            Console.WriteLine($"Left: {binary.Left}");
            Console.WriteLine($"Right: {binary.Right}");
        }
    }
    
    // Building expressions manually
    public void BuildExpressionManually()
    {
        // Build: n => n > 10
        
        // Parameter: n
        ParameterExpression param = Expression.Parameter(typeof(int), "n");
        
        // Constant: 10
        ConstantExpression constant = Expression.Constant(10);
        
        // Comparison: n > 10
        BinaryExpression comparison = Expression.GreaterThan(param, constant);
        
        // Lambda: n => n > 10
        Expression<Func<int, bool>> lambda = 
            Expression.Lambda<Func<int, bool>>(comparison, param);
        
        // Compile and use
        Func<int, bool> isGreaterThan10 = lambda.Compile();
        Console.WriteLine(isGreaterThan10(15));  // true
    }
    
    // Complex expression building
    public Expression<Func<T, bool>> BuildPredicate<T>(
        string propertyName, 
        object value, 
        ExpressionType comparison)
    {
        // Parameter: x
        ParameterExpression param = Expression.Parameter(typeof(T), "x");
        
        // Property access: x.PropertyName
        PropertyInfo? property = typeof(T).GetProperty(propertyName);
        if (property == null)
            throw new ArgumentException($"Property {propertyName} not found");
        
        MemberExpression propertyAccess = Expression.Property(param, property);
        
        // Constant: value
        ConstantExpression constant = Expression.Constant(value, property.PropertyType);
        
        // Comparison
        BinaryExpression comparisonExpr = comparison switch
        {
            ExpressionType.Equal => Expression.Equal(propertyAccess, constant),
            ExpressionType.NotEqual => Expression.NotEqual(propertyAccess, constant),
            ExpressionType.GreaterThan => Expression.GreaterThan(propertyAccess, constant),
            ExpressionType.LessThan => Expression.LessThan(propertyAccess, constant),
            ExpressionType.GreaterThanOrEqual => Expression.GreaterThanOrEqual(propertyAccess, constant),
            ExpressionType.LessThanOrEqual => Expression.LessThanOrEqual(propertyAccess, constant),
            _ => throw new ArgumentException($"Unsupported comparison: {comparison}")
        };
        
        return Expression.Lambda<Func<T, bool>>(comparisonExpr, param);
    }
    
    // Usage of dynamic predicate builder
    public void DynamicQuery()
    {
        var products = new List<Product>
        {
            new Product { Id = 1, Name = "Laptop", Price = 999.99m },
            new Product { Id = 2, Name = "Mouse", Price = 29.99m },
            new Product { Id = 3, Name = "Keyboard", Price = 79.99m },
        };
        
        var predicate = BuildPredicate<Product>("Price", 50m, ExpressionType.GreaterThan);
        var filtered = products.AsQueryable().Where(predicate);
        
        foreach (var product in filtered)
        {
            Console.WriteLine($"{product.Name}: {product.Price}");
        }
        // Laptop: 999.99, Keyboard: 79.99
    }
}

// Expression visitor for modifying expressions
public class ParameterReplacer : ExpressionVisitor
{
    private readonly ParameterExpression _oldParameter;
    private readonly ParameterExpression _newParameter;
    
    public ParameterReplacer(ParameterExpression oldParameter, ParameterExpression newParameter)
    {
        _oldParameter = oldParameter;
        _newParameter = newParameter;
    }
    
    protected override Expression VisitParameter(ParameterExpression node)
    {
        return node == _oldParameter ? _newParameter : base.VisitParameter(node);
    }
    
    public static Expression Replace(
        Expression expression, 
        ParameterExpression oldParameter, 
        ParameterExpression newParameter)
    {
        return new ParameterReplacer(oldParameter, newParameter).Visit(expression);
    }
}
```

---

## 20. Dynamic Programming

### The dynamic Type

The `dynamic` type bypasses compile-time type checking. Operations on dynamic objects are resolved at runtime. This is useful for interop with dynamic languages, COM objects, and scenarios where the type isn't known at compile time.

```csharp
public class DynamicExamples
{
    // Basic dynamic usage
    public void BasicDynamic()
    {
        dynamic d = 10;
        Console.WriteLine(d.GetType());  // System.Int32
        
        d = "Hello";
        Console.WriteLine(d.GetType());  // System.String
        
        d = new List<int> { 1, 2, 3 };
        Console.WriteLine(d.Count);  // 3
        
        // No compile-time checking
        // d.FakeMethod();  // Compiles, but throws at runtime
    }
    
    // Dynamic with reflection alternative
    public void DynamicVsReflection()
    {
        // Using reflection
        object obj = "Hello";
        var length1 = obj.GetType().GetProperty("Length")?.GetValue(obj);
        
        // Using dynamic
        dynamic dyn = "Hello";
        var length2 = dyn.Length;  // Cleaner syntax
    }
    
    // Custom dynamic object
    public class DynamicDictionary : DynamicObject
    {
        private readonly Dictionary<string, object?> _dictionary = new();
        
        public override bool TryGetMember(GetMemberBinder binder, out object? result)
        {
            return _dictionary.TryGetValue(binder.Name, out result);
        }
        
        public override bool TrySetMember(SetMemberBinder binder, object? value)
        {
            _dictionary[binder.Name] = value;
            return true;
        }
        
        public override bool TryInvokeMember(
            InvokeMemberBinder binder, 
            object?[]? args, 
            out object? result)
        {
            if (_dictionary.TryGetValue(binder.Name, out var method) && method is Delegate del)
            {
                result = del.DynamicInvoke(args);
                return true;
            }
            result = null;
            return false;
        }
        
        public override IEnumerable<string> GetDynamicMemberNames()
        {
            return _dictionary.Keys;
        }
    }
    
    public void CustomDynamicObject()
    {
        dynamic person = new DynamicDictionary();
        person.Name = "John";
        person.Age = 30;
        person.Greet = new Func<string, string>(name => $"Hello, {name}!");
        
        Console.WriteLine(person.Name);  // John
        Console.WriteLine(person.Age);   // 30
        Console.WriteLine(person.Greet("World"));  // Hello, World!
    }
    
    // ExpandoObject
    public void ExpandoObjectExample()
    {
        dynamic person = new ExpandoObject();
        person.Name = "Jane";
        person.Age = 25;
        
        // Add method
        person.Introduce = new Action(() => 
            Console.WriteLine($"Hi, I'm {person.Name}, {person.Age} years old"));
        
        person.Introduce();
        
        // Can also be treated as dictionary
        var dict = (IDictionary<string, object?>)person;
        foreach (var kvp in dict)
        {
            Console.WriteLine($"{kvp.Key}: {kvp.Value}");
        }
        
        // Remove member
        dict.Remove("Age");
    }
}

// JSON to dynamic
public class JsonDynamicExample
{
    public void ParseJsonDynamic()
    {
        string json = """
            {
                "name": "John",
                "age": 30,
                "address": {
                    "city": "NYC",
                    "country": "USA"
                }
            }
            """;
        
        // Using System.Text.Json (returns JsonElement, not dynamic)
        using var doc = JsonDocument.Parse(json);
        var name = doc.RootElement.GetProperty("name").GetString();
        
        // Alternative: parse to ExpandoObject
        dynamic data = JsonConvert.DeserializeObject<ExpandoObject>(json);
        Console.WriteLine(data.name);  // John
        Console.WriteLine(data.address.city);  // NYC
    }
}

// Placeholder for Newtonsoft.Json
public static class JsonConvert
{
    public static dynamic DeserializeObject<ExpandoObject>(string json)
    {
        // In real code, use Newtonsoft.Json or System.Text.Json
        return new ExpandoObject();
    }
}
```

---

## 21. Unsafe Code and Pointers

### Understanding Unsafe Code

Unsafe code allows you to use pointers and manipulate memory directly, similar to C/C++. This is necessary for certain performance-critical scenarios, interoperability with native APIs, and implementing low-level data structures. Unsafe code bypasses memory safety guarantees and should be used carefully.

```csharp
public class UnsafeExamples
{
    // Basic pointer usage
    public unsafe void PointerBasics()
    {
        int number = 42;
        
        // Get pointer to variable
        int* ptr = &number;
        
        // Dereference pointer
        Console.WriteLine(*ptr);  // 42
        
        // Modify through pointer
        *ptr = 100;
        Console.WriteLine(number);  // 100
        
        // Pointer arithmetic
        int[] array = { 1, 2, 3, 4, 5 };
        fixed (int* pArray = array)
        {
            int* p = pArray;
            for (int i = 0; i < array.Length; i++)
            {
                Console.WriteLine(*p);
                p++;  // Move to next element
            }
        }
    }
    
    // Fixed statement
    public unsafe void FixedStatement()
    {
        string text = "Hello";
        
        // Pin the string in memory
        fixed (char* pText = text)
        {
            for (int i = 0; i < text.Length; i++)
            {
                Console.WriteLine(pText[i]);
            }
        }
        
        // Multiple fixed
        int[] arr1 = { 1, 2, 3 };
        int[] arr2 = { 4, 5, 6 };
        
        fixed (int* p1 = arr1, p2 = arr2)
        {
            // Work with both arrays
        }
    }
    
    // Stack allocation
    public unsafe void StackAllocation()
    {
        // Allocate on stack (no GC pressure)
        int* buffer = stackalloc int[100];
        
        for (int i = 0; i < 100; i++)
        {
            buffer[i] = i;
        }
        
        // Can also use with Span (safer)
        Span<int> spanBuffer = stackalloc int[100];
    }
    
    // Sizeof operator
    public unsafe void SizeOfExample()
    {
        Console.WriteLine(sizeof(int));      // 4
        Console.WriteLine(sizeof(double));   // 8
        Console.WriteLine(sizeof(DateTime)); // Compile error for managed types
        
        // Use Marshal.SizeOf for managed types
        int dateTimeSize = System.Runtime.InteropServices.Marshal.SizeOf<DateTime>();
    }
    
    // High-performance array copy
    public unsafe void FastCopy(byte[] source, byte[] destination)
    {
        if (source.Length != destination.Length)
            throw new ArgumentException("Arrays must have same length");
        
        fixed (byte* pSource = source, pDest = destination)
        {
            byte* src = pSource;
            byte* dst = pDest;
            
            // Copy in 8-byte chunks (long pointer)
            int count = source.Length / sizeof(long);
            for (int i = 0; i < count; i++)
            {
                *((long*)dst) = *((long*)src);
                src += sizeof(long);
                dst += sizeof(long);
            }
            
            // Copy remaining bytes
            int remaining = source.Length % sizeof(long);
            for (int i = 0; i < remaining; i++)
            {
                *dst = *src;
                src++;
                dst++;
            }
        }
    }
}

// Unmanaged type constraint
public class UnmanagedExamples
    where T : unmanaged
{
    public unsafe void Process(T[] array)
    {
        fixed (T* ptr = array)
        {
            // Can safely work with pointer to T
            T* current = ptr;
            for (int i = 0; i < array.Length; i++)
            {
                // Process *current
                current++;
            }
        }
    }
    
    public unsafe T[] CopyArray(T[] source)
    {
        T[] destination = new T[source.Length];
        
        fixed (T* pSource = source, pDest = destination)
        {
            Buffer.MemoryCopy(pSource, pDest, 
                source.Length * sizeof(T), 
                source.Length * sizeof(T));
        }
        
        return destination;
    }
}
```

---

## 22. Best Practices and Performance Optimization

### Performance Best Practices

```csharp
public class PerformanceBestPractices
{
    // 1. Avoid unnecessary allocations
    public void AvoidAllocations()
    {
        // Bad: Creates new string each time
        string bad = "";
        for (int i = 0; i < 1000; i++)
        {
            bad += i.ToString();
        }
        
        // Good: Use StringBuilder
        var sb = new StringBuilder();
        for (int i = 0; i < 1000; i++)
        {
            sb.Append(i);
        }
        string good = sb.ToString();
        
        // Better: Use Span for temporary strings
        Span<char> buffer = stackalloc char[100];
        "Hello".AsSpan().CopyTo(buffer);
    }
    
    // 2. Use appropriate collection types
    public void ChooseCorrectCollection()
    {
        // Frequent lookups by key
        var dictionary = new Dictionary<string, int>();
        
        // Unique values, fast membership test
        var hashSet = new HashSet<int>();
        
        // Ordered data, indexed access
        var list = new List<int>();
        
        // First-in-first-out
        var queue = new Queue<int>();
        
        // Last-in-first-out
        var stack = new Stack<int>();
        
        // Concurrent access
        var concurrentDict = new ConcurrentDictionary<string, int>();
    }
    
    // 3. Use struct for small, short-lived values
    public struct Point
    {
        public double X { get; }
        public double Y { get; }
        
        public Point(double x, double y) => (X, Y) = (x, y);
        
        public double Distance() => Math.Sqrt(X * X + Y * Y);
    }
    
    // 4. Pool large objects
    public void UseObjectPools()
    {
        // Array pool
        var pool = ArrayPool<byte>.Shared;
        byte[] buffer = pool.Rent(1024);
        try
        {
            // Use buffer
        }
        finally
        {
            pool.Return(buffer);
        }
        
        // Memory pool
        using var memoryOwner = MemoryPool<byte>.Shared.Rent(1024);
        Memory<byte> memory = memoryOwner.Memory;
    }
    
    // 5. Avoid boxing
    public void AvoidBoxing()
    {
        var list = new ArrayList();  // Non-generic - boxes value types
        list.Add(42);  // Boxes int to object
        
        var genericList = new List<int>();  // Generic - no boxing
        genericList.Add(42);  // No boxing
    }
    
    // 6. Use value tasks for async
    public async ValueTask<int> ValueTaskExample()
    {
        // ValueTask avoids allocation when result is cached
        if (_cachedResult.HasValue)
            return _cachedResult.Value;
        
        _cachedResult = await ComputeExpensiveAsync();
        return _cachedResult.Value;
    }
    
    private int? _cachedResult;
    private Task<int> ComputeExpensiveAsync() => Task.FromResult(42);
    
    // 7. Configure await in libraries
    public async Task LibraryMethodAsync()
    {
        // In library code, don't capture context
        await Task.Delay(100).ConfigureAwait(false);
    }
    
    // 8. Use benchmarks
    // Install BenchmarkDotNet and create benchmarks:
    // [Benchmark]
    // public void MyMethod() { }
}
```

### Memory Optimization

```csharp
public class MemoryOptimization
{
    // Use ArrayPool for large temporary arrays
    public byte[] ProcessLargeData(int size)
    {
        var pool = ArrayPool<byte>.Shared;
        byte[] rentedArray = pool.Rent(size);
        
        try
        {
            // Process data
            ProcessData(rentedArray.AsSpan(0, size));
            
            // Return result
            return rentedArray.AsSpan(0, size).ToArray();
        }
        finally
        {
            pool.Return(rentedArray);
        }
    }
    
    private void ProcessData(Span<byte> data)
    {
        // Process without allocation
    }
    
    // Use Memory for async operations
    public async Task ProcessAsync(Memory<byte> buffer)
    {
        await Task.Run(() =>
        {
            // Convert to Span for synchronous processing
            ProcessData(buffer.Span);
        });
    }
    
    // Use ref struct for maximum performance
    public ref struct RefStringBuilder
    {
        private Span<char> _buffer;
        private int _position;
        
        public RefStringBuilder(Span<char> buffer)
        {
            _buffer = buffer;
            _position = 0;
        }
        
        public void Append(ReadOnlySpan<char> text)
        {
            text.CopyTo(_buffer.Slice(_position));
            _position += text.Length;
        }
        
        public ReadOnlySpan<char> Build() => _buffer.Slice(0, _position);
    }
    
    // Avoid closures in hot paths
    public int SumWithoutClosure(int[] numbers)
    {
        // Closure-free: no allocation
        int sum = 0;
        for (int i = 0; i < numbers.Length; i++)
        {
            sum += numbers[i];
        }
        return sum;
    }
    
    // If you need LINQ without allocation
    public int SumWithLinq(int[] numbers)
    {
        // This creates iterator objects
        return numbers.Sum();
    }
}
```

### Code Quality Guidelines

```csharp
public class CodeQualityGuidelines
{
    // 1. Use meaningful names
    public void GoodNaming()
    {
        // Bad
        var d = DateTime.Now;
        var list = new List<int>();
        
        // Good
        var currentDate = DateTime.Now;
        var activeUserIds = new List<int>();
    }
    
    // 2. Keep methods small
    public decimal CalculateOrderTotal(Order order)
    {
        var subtotal = CalculateSubtotal(order);
        var discount = CalculateDiscount(order, subtotal);
        var tax = CalculateTax(subtotal - discount, order.TaxRate);
        
        return subtotal - discount + tax;
    }
    
    // 3. Use pattern matching for complex conditions
    public string DescribeShape(object shape) => shape switch
    {
        Circle { Radius: var r } when r > 10 => "Large circle",
        Circle => "Circle",
        Rectangle { Width: var w, Height: var h } when w == h => "Square",
        Rectangle => "Rectangle",
        _ => "Unknown shape"
    };
    
    // 4. Prefer immutable types
    public record ImmutableOrder(
        int Id,
        string Customer,
        IReadOnlyList<OrderLine> Lines,
        decimal Total);
    
    // 5. Use nullable reference types
#nullable enable
    public string? FindCustomerName(int id)
    {
        // Explicit about possible null return
        return _customers.TryGetValue(id, out var name) ? name : null;
    }
    
    private readonly Dictionary<int, string> _customers = new();
    
    // 6. Avoid magic numbers
    private const int MaxRetryAttempts = 3;
    private const int RetryDelayMilliseconds = 1000;
    
    public async Task<T> WithRetry<T>(Func<Task<T>> operation)
    {
        for (int attempt = 0; attempt < MaxRetryAttempts; attempt++)
        {
            try
            {
                return await operation();
            }
            catch when (attempt < MaxRetryAttempts - 1)
            {
                await Task.Delay(RetryDelayMilliseconds * (attempt + 1));
            }
        }
        throw new InvalidOperationException("Max retries exceeded");
    }
}
```

---

## Summary

This comprehensive guide has covered Advanced C# programming from fundamentals to expert-level techniques:

- **Delegates and Events**: Type-safe function pointers and publish-subscribe patterns
- **Lambda Expressions**: Anonymous functions, closures, and functional programming
- **LINQ**: Query syntax, method syntax, deferred execution, and custom operators
- **Extension Methods**: Adding methods to existing types
- **Async/Await**: Asynchronous programming patterns and best practices
- **Task Parallel Library**: Parallel loops, task factories, and dataflow
- **PLINQ**: Parallel query execution
- **Concurrent Collections**: Thread-safe data structures
- **Memory Management**: GC, IDisposable, weak references
- **Span and Memory**: High-performance, allocation-free code
- **Nullable Reference Types**: Compile-time null safety
- **Records**: Immutable data types with value semantics
- **Pattern Matching**: Type, property, tuple, and list patterns
- **Tuples and Discards**: Lightweight data grouping
- **Generics**: Variance, constraints, and advanced scenarios
- **Reflection**: Runtime type inspection and manipulation
- **Attributes**: Metadata and aspect-oriented programming
- **Expression Trees**: Code as data for meta-programming
- **Dynamic Programming**: Runtime binding and ExpandoObject
- **Unsafe Code**: Pointers and direct memory manipulation
- **Performance Optimization**: Best practices for high-performance C#

Mastering these advanced features enables you to build high-performance, scalable, and maintainable applications. Continue practicing by building real-world projects that leverage these techniques, and always profile and benchmark to verify performance improvements.

---

*Created for educational purposes. Last updated: 2024*
