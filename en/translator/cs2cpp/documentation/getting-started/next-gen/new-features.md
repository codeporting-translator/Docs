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

```cpp
class Circle : public IShape, public System::Details::BoxableObjectBase
{
    typedef Circle ThisType;
    typedef IShape BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    ASPOSECPP_VALUE_TYPE_IMPLEMENTS_INTERFACES();
    
public:

    Circle();
    
    float GetArea();

};
```

The `ASPOSECPP_VALUE_TYPE_IMPLEMENTS_INTERFACES` macro adds a definition of the `operator->`  to the class, which allows such a structure to be used in generic methods like reference types.

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
```

The yield function is transformed into a state machine, a special enumerator is created that references this machine and yields values ​​one by one, calling the asynchronous function each time we call MoveNext().

```cpp
System::SharedPtr<System::Collections::Generic::IEnumerable<int32_t>> GetNumbers()
{
    return System::MakeYieldEnumerable<int32_t>([=](System::Details::YieldContext<int32_t>& __) mutable
    {
        switch (__.stage) {case 1: goto __1; case 2: goto __2; case 3: goto __3;}
        __.YieldReturn(1, 1); return; __1:;
        __.YieldReturn(2, 2); return; __2:;
        __.YieldReturn(3, 3); return; __3:;
    });
}
```

If the "honest" implementation seems redundant, and the enumeration result would be better precalculated, you can use the [CppPreMaterialize](../../user-guide/cpp-attributes/reference.md#cppprematerialize) attribute. In this case, a list will be created that will collect all the enumeration values ​​and return it via the `IEnumerable` interface.

```cpp
System::SharedPtr<System::Collections::Generic::IEnumerable<int32_t>> GetNumbers()
{
    auto __result = System::MakeObject<System::Collections::Generic::List<int32_t>>();
    __result->Add(1);
    __result->Add(2);
    __result->Add(3);
    return __result;
}
```

> ⚠️ Asynchronous enumerators are not implemented for now.

## C# 3.0 ##

### Anonymous types ###

```cs
var person = new { name = "Henry", age = 17, pet = new { name = "Joy", kind = "Puppy" } };
```

Anonymous types are translated into `System::Tuple` template specifications. This results in the loss of property names, and they are accessed through indexes: `auto name = person->get_Item<0/*name*/>()`. The comment is added to clarify index meaning.

```cpp
auto person = System::TupleFactory::Create(System::String(u"Henry"), 17, System::TupleFactory::Create(System::String(u"Joy"), System::String(u"Puppy")));
```

### Query Linq syntax ###

```cs
var query = from person in people
            where person.Age > 30
            orderby person.Name
            select new { Person = person, Age = person.Age };
```

Query-like syntax are converted to function-like syntax and then translated to C++ as ordinary code.

```cpp
auto query = people
    ->LINQ_Where(System::Func<System::SharedPtr<System::Tuple<System::String, int32_t, System::SharedPtr<System::Tuple<System::String, System::String>>>>, bool>(
        [=](const auto& person){ return person->template get_Item<1/*Age*/>() > 30; }))
    ->LINQ_OrderBy(System::Func<System::SharedPtr<System::Tuple<System::String, int32_t, System::SharedPtr<System::Tuple<System::String, System::String>>>>, System::String>(
        [=](const auto& person){ return person->template get_Item<0/*Name*/>(); }))
    ->LINQ_Select(System::Func<System::SharedPtr<System::Tuple<System::String, int32_t, System::SharedPtr<System::Tuple<System::String, System::String>>>>, System::SharedPtr<System::Tuple<System::SharedPtr<System::Tuple<System::String, int32_t, System::SharedPtr<System::Tuple<System::String, System::String>>>>, int32_t>>>(
        [=](const auto& person){ return System::TupleFactory::Create(person, person->template get_Item<1/*Age*/>()); }));
```

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

```cpp
System::RTaskPtr<System::String> DownloadContentAsync(System::String url)
{
    System::String __e1; System::SharedPtr<System::Net::Http::HttpClient> client;
    return System::MakeAsync<System::String>([=](System::Details::ResultAsyncContext<System::String>& __) mutable
    {
        if (__.stage == 1) goto __using0;
        {
            client = System::MakeObject<System::Net::Http::HttpClient>();
            __using0: System::Details::DisposeGuard<1> __guard0({client});
            try
            {
                if (__.stage == 1) goto __1;
                if (__.Await(client->GetStringAsync(url), __e1, 1)) {__guard0.Release(); return; __1: __.Continue();}
                return __.Return(__e1);
            }
            catch(...) {__guard0.SetCurrentException(std::current_exception());}
        }
    });
}
```

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

```cpp
System::Nullable<int32_t> length = System::SafeInvoke(myString, [&](auto&& expr) { return expr.get_Length(); });
```

### Strings interpolation ###

```cs
string name = "Alice";
int age = 30;
string message = $"Hello, my name is {name} and I am {age} years old.";
Console.WriteLine(message);
```

String interpolation is converted to the `System::String::Format` function with numbered placeholders in the appropriate places.

```cpp
System::String name = u"Alice";
int32_t age = 30;
System::String message = System::String::Format(u"Hello, my name is {0} and I am {1} years old.", name, age);
System::Console::WriteLine(message);
```

### Auto properties initialization ###

```cs
public string First { get; set; } = "Jane";
```

Related field will be initialized in the containing class constructor.

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

```cpp
auto numbers = [&]{ auto tmp_0 = System::MakeObject<System::Collections::Generic::Dictionary<int32_t, System::String>>();
    tmp_0->idx_set(7, u"seven"); tmp_0->idx_set(9, u"nine"); tmp_0->idx_set(13, u"thirteen"); return tmp_0; }();
```

### Operator 'nameof' ###

```cs
Console.WriteLine(nameof(System.String));
```

Expression should be resolved to appropriate constant string literal.

```cpp
System::Console::WriteLine(u"String");
```

### Contextual catches ###

```cs
try
{
    throw new Exception("Hello");
}
catch (Exception e) when (e.Message == "Good bye")
{
    return;
}
```

Catches with filters should be translated to a "ladder" of **catch-if-else-throw** constructions.

```cpp
try
{
    throw System::Exception(u"Hello");
}
catch (System::Exception& e) { if (e->get_Message() == u"Good bye")
{
    return;
}
else throw; }
```

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

```cpp
int32_t SomeMethod()
{
    auto SomeLocalMethod = []() -> int32_t { return 10; };
    return SomeLocalMethod() * SomeLocalMethod();
}
```

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

```cpp
System::ValueTuple<System::String, int32_t> GetPerson()
{
    return {System::String(u"Alice"), 25};
}

System::String name; int32_t age;
System::TieTuple(name, age) = GetPerson();
```

### Value tasks ###

```cs
public ValueTask<int> GetSomeInt()
{
    return new ValueTask<int>(10);
}
```

Value tasks are translated into the `System::Threading::Tasks::(Result)ValueTask` templates specifications.

```cpp
System::Threading::Tasks::ResultValueTask<int32_t> GetSomeInt()
{
    return System::Threading::Tasks::ResultValueTask<int32_t>(10);
}
```

> ⚠️ IValueTaskSource-based constructors are not implemented yet.

### Inline out variable declarations ###

```cs
if (TryGet(out var result))
{
    Do(result);
}
```

The translator adds variable declarations before the statement in which they are designated in C#.

```cpp
int32_t result;
if (TryGet(result))
{
    Do(result);
}
```

### Type pattern matching ###

```cs
if (enumerable is int[] array)
{
    return array.Length;
}
```

Overloaded function `System::Is` with pre-declared variable before **if** statement is used to translate this.

```cpp
System::ArrayPtr<int32_t> array;
if (System::Is<System::Array<int32_t>>(enumerable, array))
{
    return array->get_Length();
}
```

### Constant pattern matching ###

```cs
if (data is 1.0f)
{
    Console.WriteLine("Data contains floating one");
}
```

Overloaded function `System::Is` is used to translate this.

```cpp
if (System::Is(data, 1.0f))
{
    System::Console::WriteLine(u"Data contains floating one");
}
```

### Discarding operator "_" ###

```cs
var (x, _, z) = (1, 2, 3);
if (int.TryParse("123", out _)) {}
```

Special method template `System::Discard` is used to accept discarded vaules.

```cpp
int32_t x, z;
System::TieTuple(x, System::Discard<int32_t>(), z) = System::MakeTuple(1, 2, 3);

if (System::Int32::TryParse(u"123", System::Discard<int32_t>()))
{
}
```

### Ref methods ###

```cs
ref int SomeMethod()
{
    return ref m_someIntData;
}
```

C++ reference type (&) is used for such methods.

```cpp
int32_t& SomeMethod()
{
    return m_someIntData;
}
```

### Ref local variables ###

```cs
int a = 1, b = 2;

ref var r = ref a;
r = 10;

r = ref b;
r = 20;
```

Raw C++ pointer type (*) is used for such variables.

```cpp
int32_t a = 1, b = 2;

int32_t *r = &a;
*r = 10;

r = &b;
*r = 20;
```

### Ref properties and readonly ref properties ###

```cs
ref int SomeProperty => ref m_someIntData;
```

C++ reference type (&) and const refrerence type (const&) are used for such properies.

```cpp
int32_t& get_SomeProperty()
{
    return m_someIntData;
}
```

### In parameters ###

```cs
public double CalculateDistance(in Point p1, in Point p2)
```

C++ const refrerence type (const&) is used for such properies.

```cpp
double CalculateDistance(const Point& p1, const Point& p2)
```

### `Span<T>` and `ReadOnlySpan<T>`, stackalloc ###

```cs
Span<int> numbers = stackalloc[] { 1, 2, 3, 4, 5 };
```

`System.Span` and `System.ReadOnlySpan` are translated to C++ class templates `System::Span` and `System::ReadOnlySpan` respectively. `stackalloc` arrays are translated to the internal `System::Details::StackArray` class template.

```cpp
System::Details::StackArray<int32_t, 5> array_0 = {1, 2, 3, 4, 5};
System::Span<int32_t> numbers = array_0;
```

### `Memory<T>` and `ReadOnlyMemory<T>`, `MemoryManager<T>` ###

```cs
Memory<int> data = new[] { 1, 2, 3, 4, 5 };
```

`System.Memory` and `System.ReadOnlyMemory` are translated to C++ class templates `System::Memory` and `System::ReadOnlyMemory` relatively.

```cpp
System::Memory<int32_t> data = System::Memory<int32_t>::to_Memory(System::MakeArray<int32_t>({1, 2, 3, 4, 5}));
```

### Numeric literals delimeter "_" ###

```cs
var bigNumber = 100_000_000;
```

Translator ignores _ inside of numeric literals.

```cpp
int32_t bigNumber = 100000000;
```

## C# 8.0 ##

### Nullable reference types and null-forgiving operator "!" ###

```cs
string? nullableString = GetNullableString();
string nonNullableString = nullableString!;
```

The translator ignores these annotations on C++ side, although in the future, this may become the basis for some optimization (it will be possible to eliminate null checks when dereferencing a smart pointer).

```cpp
System::String nullableString = GetNullableString();
System::String nonNullableString = nullableString;
```

### Switch expressions ###

```cs
string result = input switch {1 => "one", 2 => "two", _ => "many"};
```

On the C++ side, the translator builds a "ladder" of ternary operators from this.

```cpp
System::String result = input == 1 ? System::String(u"one") :
    input == 2 ? System::String(u"two") :
    System::String(u"many");
```

### Property and positional patterns ###

```cs
string result = input switch {{Length: 0} => "Empty", _ => "Non-empty"};
string other = tuple switch {(0, 0) => "Zero tuple", _ => "Other tuple"};
```

Translator uses speical pattern objects to repesent such patterns or logical expressions in most simple cases.

```cpp
System::String result = System::Is(input, (&System::String::get_Length% (_== 0))) ? System::String(u"Empty") :
    System::String(u"Non-empty");
System::String other = System::Is(tuple, _(_== 0, _== 0)) ? System::String(u"Zero tuple") :
    System::String(u"Other tuple");
```

### Using declarations ###

```cs
using var d = new Disposable();
d.DoSome();
```

It should be translated as ordinary using block with braces to the end of scope.

```cpp
{
    auto d = System::MakeObject<RefLocalTests::Disposable>();
    // Clearing resources under 'using' statement
    System::Details::DisposeGuard<1> __dispose_guard_0({d});
    // ------------------------------------------
    
    try
    {
        d->DoSome();
    }
    catch(...)
    {
        __dispose_guard_0.SetCurrentException(std::current_exception());
    }
}
```

### Indexs and ranges ###

```cs
var element = array[^1];
var slice = array[1..^2];
```

It should be translated to free `System::Get` methods with instance of `System::Index` or `System::Range` object relatively as second argument.

```cpp
int32_t element = System::Get(array, System::Index(1, true));
auto slice = System::Get(array, System::Range(1, System::Index(2, true)));
```

## C# 9.0 ##

### Primary constructors ###

```cs
class Car(string Manufacturer, string Model)
{
    public override string ToString()
    {
        return Manufacturer + ":" + Model;
    }
}
```

Translator generates all necessary constructor, fields and properties needed.

```cpp
class Car : public System::Object
{
    typedef Car ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    Car(System::String Manufacturer, System::String Model);
    
    System::String ToString() const override;
    
private:

    System::String Manufacturer;
    System::String Model;
    
};
```

### Initialization property accessors ###

```cs
public class Person
{
    public string FirstName { get; init; }
};
```

Init-only accessors are translated like regular setters but with "init_" prefix. They are public, so on C++ side programmer can use them in any code point on his own risk.

```cpp
class Person : public System::Object
{
    typedef Person ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::String get_FirstName();
    void init_FirstName(System::String value);
    
private:

    System::String pr_FirstName;
    
};
```

### Records and 'with' keyword ###

```cs
public record Person(string FirstName, string LastName);

var husband = new Person("John", "Doe");
var wife = husband with { FirstName = "Jane"};
```

Translator generates C++ class and adds all necessary record methods automatically.

```cpp
class Person : public System::IEquatable<System::SharedPtr<RefLocalTests::Person>>
{
    typedef Person ThisType;
    typedef System::IEquatable<System::SharedPtr<RefLocalTests::Person>> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::String get_FirstName() const { return pr_FirstName; }
    void init_FirstName(System::String value) { pr_FirstName = value; }
    System::String get_LastName() const { return pr_LastName; }
    void init_LastName(System::String value) { pr_LastName = value; }
    Person(System::String FirstName, System::String LastName);
    
    bool operator==(const ThisType& other) const;
    bool operator!=(const ThisType& other) const;
    bool Equals(System::SharedPtr<ThisType> other) override;
    bool Equals(System::SharedPtr<System::Object> obj) override;
    int32_t GetHashCode() const override;
    System::String ToString() const override;
    void Deconstruct(System::String& FirstName_, System::String& LastName_);
    
protected:

    virtual void PrintMembers(System::Text::StringBuilder& builder);
    
    template<typename T, typename A> friend System::SharedPtr<T> System::With(const System::SharedPtr<T>&, const A&);
    
    virtual RefLocalTests::Person* _Clone_() const;
    
private:

    System::String pr_FirstName;
    System::String pr_LastName;
    
};

auto husband = System::MakeObject<RefLocalTests::Person>(u"John", u"Doe");
auto wife = System::With(husband, [&](auto& copy){ copy.init_FirstName(u"Jane"); });
```

### Function pointers ###

```cs
delegate*<int, int, int> pointer = &Add;
var summ = pointer(1, 2);
```

Function pointers are translated to C++ function pointer alias `System::FunctionPtr` or to `std::function` in some cases.

```cpp
System::FunctionPtr<int32_t, int32_t, int32_t> pointer = &Add;
int32_t summ = pointer(1, 2);
```

### Implicit 'new' expressions ###

```cs
Unit unit = new(10);
```

Translator deduces object type from creation semantics and uses explicit type specification on C++ side.

```cpp
Unit unit = Unit(10);
```

### Type, logical and relation patterns ###

```cs
if (obj is int and > 10)
{
    Console.WriteLine("Integer greater than 10");
}
```

Translator generates special pattern objects or simple logical expressions where suitable.

```cpp
if (System::ObjectExt::Is<int32_t>(obj) && System::Greater(obj, 10))
{
    System::Console::WriteLine(u"Integer greater than 10");
}
```

## C# 10.0 ##

### File-scoped namespaces ###

```cs
namespace UI.Widgets.Button;
```

Translator works with such namespaces like with a single classic namespace declaration. No tabs will be added (like with regular ones too).

### Record structs ###

```cs
record struct Vector(int X, int Y);
```

Should be translated like ordinary struct but with auto methods like with reference record.

```cpp
class Vector : public System::IEquatable<RefLocalTests::Vector>, public System::Details::BoxableObjectBase
{
    typedef Vector ThisType;
    typedef System::IEquatable<RefLocalTests::Vector> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    ASPOSECPP_VALUE_TYPE_IMPLEMENTS_INTERFACES();
    
public:

    int32_t get_X() const { return pr_X; }
    void set_X(int32_t value) { pr_X = value; }
    int32_t get_Y() const { return pr_Y; }
    void set_Y(int32_t value) { pr_Y = value; }
    Vector(int32_t X, int32_t Y);
    Vector();
    
    bool operator==(const ThisType& other) const;
    bool operator!=(const ThisType& other) const;
    bool Equals(ThisType other) override;
    bool Equals(System::SharedPtr<System::Object> obj) override;
    int32_t GetHashCode() const override;
    System::String ToString() const override;
    void Deconstruct(int32_t& X_, int32_t& Y_);
    
protected:

    virtual void PrintMembers(System::Text::StringBuilder& builder);
    
private:

    int32_t pr_X;
    int32_t pr_Y;
    
};
```

## C# 11.0 ##

### List patterns ###

```cs
if (array is [0, .. var mid, 10])
{
    return mid;
}
```

Translator generates special pattern objects only. No simple logical expressions suitable here.

```cpp
System::ArrayPtr<int32_t> mid;
if (System::Is(array, _._(_== 0, _._[_[mid]], _== 10)))
{
    return mid;
}
```

### UTF8 string literals ###

```cs
ReadOnlySpan<byte> utf8_str = "Привет!"u8;
```

Translator uses native C++11 UTF8 string literals and uses special constructor to initialize `ReadOnlySpan` with it.

```cpp
System::ReadOnlySpan<uint8_t> utf8_str = u8"Привет!";
```

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

There is no principal difference between records and other types on the C++ side, so methods, inheritance and fields are fully applicable to them.

```cpp
class Person : public virtual System::IEquatable<System::SharedPtr<RefLocalTests::Person>>
{
    typedef Person ThisType;
    typedef System::IEquatable<System::SharedPtr<RefLocalTests::Person>> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::String get_Name() const { return pr_Name; }
    void init_Name(System::String value) { pr_Name = value; }
    virtual System::String GetName();
    
    Person(System::String Name);
    
    bool operator==(const ThisType& other) const;
    bool operator!=(const ThisType& other) const;
    bool Equals(System::SharedPtr<ThisType> other) override;
    bool Equals(System::SharedPtr<System::Object> obj) override;
    int32_t GetHashCode() const override;
    System::String ToString() const override;
    void Deconstruct(System::String& Name_);
    
protected:

    virtual void PrintMembers(System::Text::StringBuilder& builder);
    
    template<typename T, typename A> friend System::SharedPtr<T> System::With(const System::SharedPtr<T>&, const A&);
    
    virtual RefLocalTests::Person* _Clone_() const;
    
private:

    System::String pr_Name;
    
};

class Employee : public RefLocalTests::Person, public System::IEquatable<System::SharedPtr<RefLocalTests::Employee>>
{
    typedef Employee ThisType;
    typedef RefLocalTests::Person BaseType;
    typedef System::IEquatable<System::SharedPtr<RefLocalTests::Employee>> BaseType1;
    
    typedef ::System::BaseTypesInfo<BaseType, BaseType1> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    int32_t get_EmployeeId() const { return pr_EmployeeId; }
    void init_EmployeeId(int32_t value) { pr_EmployeeId = value; }
    System::String GetName() override;
    
    Employee(System::String Name, int32_t EmployeeId);
    
    bool operator==(const ThisType& other) const;
    bool operator!=(const ThisType& other) const;
    bool Equals(System::SharedPtr<ThisType> other) override;
    bool Equals(System::SharedPtr<System::Object> obj) override;
    int32_t GetHashCode() const override;
    System::String ToString() const override;
    void Deconstruct(System::String& Name_, int32_t& EmployeeId_);
    
protected:

    void PrintMembers(System::Text::StringBuilder& builder) override;
    
    template<typename T, typename A> friend System::SharedPtr<T> System::With(const System::SharedPtr<T>&, const A&);
    
    RefLocalTests::Person* _Clone_() const override;
    
private:

    int32_t pr_EmployeeId;
    
};
```

### Collection expressions ###

```cs
int[] x = [1, 2, 3, .. otherCollection];
```

Translator uses object builder to construct such collections.

```cpp
System::ArrayPtr<int32_t> x = System::BuildArray<int32_t>().Add({1, 2, 3}).AddSpread(otherCollection).Get();
```
