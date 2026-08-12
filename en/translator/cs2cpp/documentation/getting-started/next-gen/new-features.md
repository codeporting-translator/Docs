---
order: "1"
navTitle: "New features"
---

# New supported language features #

Here is a list of C# language features that the old translator could not translate, but the new one translates in whole or in part.

[TOC]

## C# 1.0 ##

### Structures implementing interfaces ###

```cs
public struct Circle : IShape
{
    float GetArea() {...}
}
```

When casting a value type to an interface it implements, pseudo-boxing (the value object is not wrapped in a `BoxedValue`, but is created as is) of the object occurs on the heap using the copy constructor.

> ⚠️ With regular boxing (casting a value type to an object), the wrapping occurs as before. This can cause some errors. For example, if a structure implements an interface, is boxed into an object, and then an attempt is made to cast that object to the interface, the cast will fail.

## C# 2.0 ##

### Operator yield ###

```cs
public static IEnumerable<int> GetNumbers()
{
    yield return 1;
    yield return 2;
    yield return 3;
}

foreach (var number in GetNumbers())
{
    Console.WriteLine(number);
}
```

The yield function is transformed into a state machine, a special enumerator is created that references this machine and yields values ​​one by one, calling the asynchronous function each time we call MoveNext(). If the "honest" implementation seems redundant, and the enumeration result would be better precalculated, you can use the CppPreMaterializeAttribute attribute. In this case, a list will be created that will collect all the enumeration values ​​and return it via the `IEnumerable` interface.

> ⚠️ Asynchronous enumerators are not implemented for now.

## C# 3.0 ##

### Anonymous types ###

```cs
var person = new { name = "Henry", age = 17, pet = new { name = "Joy", kind = "Puppy" } };
```

Anonymous types are translated into `System::Tuple` template specifications. This results in the loss of property names, and they are accessed through indexes: `auto name = person->get_Item<0/*name*/>()`. The comment is added to clarify index meaning.

### Query Linq syntax ###

```cs
var query = from person in people
            where person.Age > 30
            orderby person.Name
            select new { Person = person, Age = person.Age };
```

Query-like syntax are converted to function-like syntax and then translated to C++ as ordinary code.

## C# 5.0 ##

### Async-await ###

```cs
public async Task<string> DownloadContentAsync(string url)
{
    using (var client = new HttpClient())
    {
        return await client.GetStringAsync(url);
    }
}
```

The asynchronous functions are transformed into a state machines wrapped with `System::Threading::Tasks::Task` class (`System::Threading::Tasks::ResultTask` class template is used for non-void tasks). Task implementation isn't full, but covers all main principles: tasks schenuling, cancellation, exceptions processing, asynchronous IO, async tests, async lambdas, async `Main` method and other.

> ⚠️ Awaits in catch and finally blocks are not supported.

## C# 6.0 ##

### Expression bodies ###

```cs
public void Print() => Console.WriteLine("Hello, world!");
```

Non-void bodies will be converted to functions of the form `return [expression body]`. Regular functions, constructors, properties, local functions, lambdas, etc. are supported.

### Null-propagation operators "?.", "?[]" ###

```cs
var length = myString?.Length;
```

Converts to a call to the `System::SafeInvoke` function, where the first parameter is the expression to the left of the `?.` operator, and the second is the method call code and all the rest of the code that will be called if the first expression is not null.

### Strings interpolation ###

```cs
string name = "Alice";
int age = 30;
string message = $"Hello, my name is {name} and I am {age} years old.";
Console.WriteLine(message);
```

String interpolation is converted to the `System::String::Format` function with numbered placeholders in the appropriate places.

### Auto properties initialization ###

```cs
public string First { get; set; } = "Jane";
```

Related field will be initilized in containing class constructor.

### Indexers initialization ###

```cs
var numbers = new Dictionary<int, string>
{
    [7] = "seven",
    [9] = "nine",
    [13] = "thirteen"
};
```

Explicit method `idx_set()` should be called to replace indexer.

### Operator 'nameof' ###

```cs
WriteLine(nameof(person.Address.ZipCode));
```

Expression should be resolved to appropriate constant string literal.

### Contextual catches ###

```cs
try { … } catch (Exception e) when (Check(e)) { … }
```

Catches with filters should be translated to a "ladder" of "catch-if-else-throw" constructions.

## C# 7.0 - 7.2 ##

### Local functions ###

```cs
int SomeMethod()
{
    int SomeLocalMethod()
    {
         return 10;
    }
    return SomeLocalMethod() * SomeLocalMethod();
}
```

On the C++ side, local functions are converted to local instances of lambda functors.

> ⚠️ Generic recursive local methods require more complex invocation and declaration semantics and not implemented for now.

### Value tuples ###

```cs
public (string Name, int Age) GetPerson()
{
    return ("Alice", 25);
}

var (name, age) = GetPerson();
```

Value tuples are translated into the System::ValueTuple template specification. This leads to the loss of field names, and they are accessed through indexes.

### Value tasks ###

```cs
public virtual ValueTask<int> GetSomeInt()
{
    return new ValueTask(10);
}
```

Value tasks are translated into the System::Threading::Tasks::(Result)ValueTask templates specifications.

> ⚠️ IValueTaskSource-based constructors are not implemented yet.

### Inline out variable declarations ###

```cs
if (TryGet(out var result))
{
    Do(result);
}
```

The translator adds variable declarations before the statement in which they are designated in C#.

### Type pattern matching ###

```cs
if (baseObject is Derived derived)
{
    derived.MethodSpecificToDerived();
}
```

Internal method `System::ObjectExt::IsDeclaration with` pre-declared variable before 'if' statement is used to translate this.

### Constant pattern matching ###

```cs
if (data is 1.0f)
{
    Console.WriteLine("Data contains floating one");
}
```

Internal method `System::ObjectExt::IsConstant` is used to translate this.

### Discarding operator "_" ###

```cs
var (x, _, z) = (1, 2, 3);
if (int.TryParse("123", out _)) {}
if (obj is string _) {}
```

Special method template `System::Discard` is used to accept discarded vaules.

### Ref methods ###

```cs
ref int SomeMethod()
{
    return ref m_someIntData;
}
```

C++ reference type (&) is used for such methods.

### Ref local variables ###

```cs
var ref tempRef = ref m_someIntData;
```

Raw C++ pointer type (*) is used for such variables.

### Ref properties and readonly ref properties ###

```cs
ref int SomeProperty() => return ref m_someIntData;
```

C++ reference type (&) and const refrerence type (const&) are used for such properies.

### In parameters ###

```cs
public double CalculateDistance(in Point p1, in Point p2)
```

C++ const refrerence type (const&) is used for such properies.

### `Span<T>` and `ReadOnlySpan<T>`, stackalloc ###

```cs
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5 };
```

`System.Span` and `System.ReadOnlySpan` are translated to C++ class templates `System::Span` and `System::ReadOnlySpan` relatively. `stackallock` arrays are translate to internal `System::Details::StackArray` class template.

### `Memory<T>` and `ReadOnlyMemory<T>`, `MemoryManager<T>` ###

```cs
Memory<int> data = [] { 1, 2, 3, 4, 5 };
```

`System.Memory` and `System.ReadOnlyMemory` are translated to C++ class templates `System::Memory` and `System::ReadOnlyMemory` relatively.

### Numeric literals delimeter "_" ###

```cs
var bigNumber = 100_000_000;
```

Translator ignores _ inside of numeric literals.

## C# 8.0 ##

### Nullable reference types and null-forgiving operator "!" ###

```cs
string? nullableString = GetNullableString();
string nonNullableString = nullableString!;
```

The translator ignores these annotations on C++ side, although in the future, this may become the basis for some optimization (it will be possible to eliminate null checks when dereferencing a smart pointer).

### Switch expressions ###

```cs
string result = input switch {1 => "one", 2 => "two", _ => "many"};
```

On the C++ side, the translator builds a "ladder" of ternary operators from this.

### Property and positional patterns ###

```cs
string result = input switch {{Length: 0} => "Empty", _ => "Non-empty"};
string other = tuple switch {(0, 0) => "Zero tuple", _ => "Other tuple"};
```

Translator uses speical pattern objects to repesent such patterns or logical expressions in most simple cases.

### Using declarations ###

```cs
using var d = new Disposable();
```

It should be translated as ordinary using block with braces to the end of scope.

### Indexs and ranges ###

```cs
var element = array[^1];
var slice = array[1..^2];
```

It should be translated to free `System::Get` methods with instance of `System::Index` or `System::Range` object relatively as second argument.

## C# 9.0 ##

### Records and 'with' keyword ###

```cs
public record Person(string FirstName, string LastName);

var husband = new Person("John", "Doe");
var wife = person with {FirstName: "Jane"};
```

Translator adds all necessary record methods automaticly.

### Primary constructors ###

```cs
public class Car(string Manufacturer, string Model);
```

Translator generates all necessary constructor and properties needed.

### Initialization property accessors ###

```cs
public class Person
{
    public string FirstName { get; init; }
};
```

Init-only accessors are translated like regular setters but with "init_" prefix. They are public, so on C++ side programmer can use them in any code point on his own risk.

### Function pointers ###

```cs
delegate*<int, int, int> pointer = &Sum;
```

Function pointers are translated to C++ function pointer alias `System::FunctionPtr` or to `std::function` in some cases.

### Implicit 'new' expressions ###

```cs
Unit unit = new(10);
```

Translator deduces object type from creation semantics and uses explicit type specification on C++ side.

### Type, logical and relation patterns ###

```cs
if (obj is int and > 10) return "Integer grater than 10";
```

Translator generates special pattern objects or simple logical expressions where suitable.

## C# 10.0 ##

### File-scoped namespaces. ###

```cs
namespace UI.Widgets.Button;
```

Translator works with such namespaces like with sigle classic namespace declaration. No tabs will be added (like with regular ones too).

### Record structs. ###

```cs
public record struct Point(int X, int Y);
```

Should be translated like ordinary struct but with auto methods like with reference record.

## C# 11.0 ##

### List patterns ###

```cs
if (array is [0, .. mid, 10]) return mid;
```

Translator generates special pattern objects only. No simple logical expressions suitable here.

### UTF8 string literals ###

```cs
ReadOnlySpan<byte> utf8_str = "Привет!"u8;
```

Translator uses native C++11 UTF8 string literals and uses special constructor to initialize `ReadOnlySpan` with it.

## C# 12.0 ##

### Extended records (with inheritance and methods) ###

```cs
public record Person(string Name) 
{ 
   public virtual string GetName() => Name;
}

public record Employee(string Name, int EmployeeId) : Person(Name) 
{ 
   public override string GetName() => $"{Name} (ID: {EmployeeId})";
}
```

There is no principal difference between records and other types on C++ side, so method, inherintance and fields are fully applicable to them.

### Collection expressions ###

```cs
int[] x = [1, 2, 3, .. otherCollection];
```

Translator uses object builder to construct such collections.
