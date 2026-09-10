# Translation Issues #

This section describes issues that arise during C# to C++ code translation phase. In the vast majority of cases, such issues occur when perfectly valid C# code cannot be translated into C++ due to either the limitations of the language itself or imperfections in the translator.

[TOC]

## T01: Initialization order fiasco possible while initializing static field {#t01} ##

In C++, the initialization order of static members is undefined if the objects reside in different translation units. Therefore, if one static variable is initialized using another defined in a different file, it can lead to undefined behavior. This message warns of the risk of such a situation occurring.

**Default severity**: WARNING

**Example**: dependent static variables

```cs
// a.cs
public class A
{
    public static int X = 10;
}

// b.cs
public class B
{
    public static int Y = A.X;
}
```

**Solution**: use singletons to ensure on-first-demand initialization of `A.X` field

```cs
// a.cs
public class A
{
    [CodePorting.Translator.Cs2Cpp.CppPortAsSingletonAttribute]
    public static int X = 10;
}

// b.cs
public class B
{
    public static int Y = A.X;
}
```

## T02: Possible virtual function call in constructor or destructor {#t02} ##

In C++, a class's virtual method table always corresponds to the specific class where the currently executing constructor or destructor is defined; in other words, they are not polymorphic in C++. Consequently, calls to virtual members within constructors can result in behavior different from what one would expect in C#. This message warns of the risk of such a situation.

> Use [enable_warnings_for_virtual_function_calls](../configuration-file/options.md#enable_warnings_for_virtual_function_calls) option to turn such diagnostics on or off.

**Default severity**: WARNING

**Example**: abstract method call in constructor (leads to "pure virtual method call" on C++ side)

```cs
abstract class A
{
    public A()
    {
        Go();
    }

    protected abstract void Go();
}
```

**Solution**: refactor C# code to avoid such situations, suppres or ignore this issue for false-positive checks.

## T03: Test method uses class as a value source, possibly causing undefined behavior unless made a singleton {#t03} ##

Due to the nature of C++ tests, values ​​from the test source class may be requested before they are initialized. This message warns of the risk of such a situation.

**Default severity**: WARNING

**Solution**: use [CppDeferredInitAttribute](../cpp-attributes/reference.md#cppdeferredinit) to entire value source class.

## T10: Type declaration is non-generic overload for generic type, which is prohibited in C++, renamed {#t10} ##

In C++, a type cannot be both a template and a non-template; that is, a type cannot be overloaded based on whether or not it is a template. Nor can a template be overloaded based on the number of template arguments. It can only be specialized, though this does not eliminate the need to write angle brackets after specifying the type, even if they are empty. Therefore, the translator automatically renames the non-generic overload by appending the suffix "Common" to the type name.

**Default severity**: INFO

**Example**: overloaded generic and non-generic type

```cs
public class A
{
}

public class A<T>
{
    T t;
}
```

**Solution**: rename on of overloads overload manually or just do nothing and let translator add "Common" to non-generic class name.

```cs
public class A
{
}

public class GenericA<T>
{
    T t;
}
```

## T11: Type declaration contains member which is reserved on C++ side, renamed {#t11} ##

The translator is required to add various auxiliary methods and data members to C++ classes that are present implicitly in C#. This can lead to name conflicts if the user adds a field or method with the same name to the class. This message indicates that such a conflict was detected and the user-defined field has been automatically renamed.

**Default severity**: INFO

**Example**: method `Type()` is reserved on C++ side

```cs
public class A
{
    public string Type;
}
```

**Solution**: rename conflicting member using attribute or just do nothing and let translator rename it on its own.

```cs
public class A
{
    [CodePorting.Translator.Cs2Cpp.CppRenameEntity("ClassType")]
    public string Type;
}
```

## T12: The non-generic interface 'System.Collections.IEnumerator' is not supported in C++, and its explicit implementations are ignored {#t12} ##

The translator has poor support for legacy non-generic interfaces, specifically `System.Collections.IEnumerator`. Typically, this interface is used alongside its generic counterpart, resulting in two `get_Current()` methods with different return types on the C++ side — a scenario that is not permitted. Consequently, the translator simply ignores this interface wherever possible, leaving the user to work solely with the generic implementation.

**Default severity**: INFO

**Solution**: ignore this message or refactor C# code if the way the translator eliminates this interface breaks the program's logic.

## T13: Possible mutable access to symbol using constant 'this', 'const_cast' added {#t13} ##

The translator detected an attempt to modify a member or call a non-const method within a method that is treated as const on the C++ side. To avoid a compilation error, the translator explicitly adds a `const_cast`; while this is obviously not good practice, it is permissible in some cases.

> This functionality works only with [detect_const_methods](../configuration-file/options.md#detect_const_methods) option turned on.

**Default severity**: INFO

**Example**: call of non-const method from inside on const one and change instance field value

```cs
public class A
{
    private int? _field;
    private int CalculateField() => 10;

    [CodePorting.Translator.Cs2Cpp.CppConstMethod]
    public int GetField() => _field ??= CalculateField();
}
```

**Solution**: leave code as is or add necessary attributes to other members.

```cs
public class A
{
    [CodePorting.Translator.Cs2Cpp.CppMutable]
    private int? _field;
    [CodePorting.Translator.Cs2Cpp.CppConstMethod]
    private int CalculateField() => 10;

    [CodePorting.Translator.Cs2Cpp.CppConstMethod]
    public int GetField() => _field ??= CalculateField();
}
```

## T30: The enum should have metadata for such operation {#t30} ##

Certain operations on enumerations require the registration of metadata containing a mapping table between numeric and string values. The translator detects calls to such methods and issues an error message if it finds that no metadata exists for the specified enum.

**Default severity**: ERROR

**Example**: conversion of non-registered enum to string

```cs
enum Gender
{
    Male,
    Female
}
var gender = Gender.Male;
var str = gender.ToString();
```

**Solution**: add specific [CodePorting.Translator.Cs2Cpp.CppEnumEnableMetadata](../cpp-attributes/reference.md#cppenumenablemetadata) attribute to enable metadata only to this enum or set configuration option [cpp_enum_enable_metadata](../configuration-file/options.md#cpp_enum_enable_metadata) to enable metadata to all enums.

```cs
[CodePorting.Translator.Cs2Cpp.CppEnumEnableMetadata]
enum Gender
{
    Male,
    Female
}
var gender = Gender.Male;
var str = gender.ToString();
```

## T50: Semantics for node is not available {#t50} ##

For a translator to function correctly, it requires the semantics of almost every symbol it processes; syntactic information alone is insufficient. In other words, the translator needs to know whether a given element is a method, property, field, variable, or constant, as well as its type, and so on. For valid C# code, Roslyn can always provide the semantics for any syntactic node without issue. However, if the code contains errors (such as references to undefined symbols) retrieving the semantics becomes impossible. In such cases, the translator issues a corresponding message but still attempts to proceed with the translation by assuming the most likely interpretation of the syntactic node.

Such issues are usually preceded by a [compiler message](common.md#c41) indicating errors in the C# source code; therefore, if the cause of the message is unclear, check the compilation error log and look for it there. Although this issue is classified as a warning by default, it often indicates that the code being translated contains an error, meaning the resulting output will fail to compile.

**Default severity**: WARNING

**Example**: access to disabled symbol

```cs
int Foo()
{
#if __cplusplus
    return 10;
#else
    var i = 20;
#endif
    return i;  // this line will be unreachable in the C++ code, but it will still be translate
}
```

**Solution**: ensure that the project compiles correctly in the specific configuration being built — including special conditional symbols, the framework version, etc.

```cs
int Foo()
{
#if __cplusplus
    return 10;
#else
    var i = 20;
    return i;
#endif
}
```

## T51: Node semantics is invalid {#t51} ##

This indicates that semantics are present but erroneous, preventing full analysis for the generation of correct C++ code. Often signals erroneous or undefined type symbol. In terms of causes and remediation, this message is similar to the previous one, [T50](#t50).

**Default severity**: WARNING

## T60: Method implementation not found {#t60} ##

It reports that the class contains a method with no definition, meaning there is no way to generate its body. Even if only the declaration remains, a C++ linking error will occur.

**Default severity**: ERROR

**Example**: empty partial method

```cs
partial class A
{
    partial void Foo();
}
```

**Solution**: add empty implementation

```cs
partial class A
{
    partial void Foo() {}
}
```

## T61: Extern method [DllImport] attribute not found {#t61} ##

Reports that a method without an implementation, marked as `extern`, lacks the `[System.Runtime.InteropServices.DllImport]` attribute, and the compiler has no other way to provide an implementation for the external method.

**Default severity**: ERROR

**Example**: extern method without DllImport atribute

```cs
private static extern int GetSystemDefaultLCID();
```

**Solution**: add attribute with method location

```cs
[System.Runtime.InteropServices.DllImport("kernel32.dll")]
private static extern int GetSystemDefaultLCID();
```

## T62: Unable to deduce object type to create {#t62} ##

The translator cannot infer the type of the object being created from the provided syntax. This may be caused by errors in the source C# code or by certain exotic syntactic constructs that are not yet supported by the translator.

**Default severity**: ERROR

**Solution**: fix source errors if any or rewrite object creation code to more common way.

## T70: Unsupported code {#t70} ##

This message indicates that the translator has encountered C# code that it cannot currently translate (or is fundamentally incapable of translating) into C++. The message always specifies the reason for this failure: it could be a virtual generic method, a dynamic type, or something less radical but not yet supported.

**Default severity**: ERROR

**Example**: generic interface member

```cs
interface I
{
    void DoSome<T>(T t);
}
```

**Example**: dynamic type

```cs
void Do(dynamic d)
{
}
```

**Example**: awaits inside of `catch` and `finally` clauses

```cs
async Task DoAsync()
{
    try
    {
        await DoSomeOtherAsync();
    }
    catch (Exception e)
    {
        await SendError(e.Message);
    }
}
```

**Solutions**. There is no universal and sufficiently inexpensive algorithm for bypassing such problems. Different approaches must be used in each specific case, depending on exactly how the unsupported language feature is employed. Here are a few of the most obvious options.

1. Rewrite the original C# code to eliminate such constructs. It is often better to replace the `dynamic` type with generics; an `await` inside a `catch` block can be handled by using `goto` to exit the block; and in some cases, generic virtual methods can be replaced with non-virtual methods or dispatching based on type identifiers.
1. Use custom C++ implementations for methods that fundamentally cannot be rewritten to translate correctly. Refer to the [documentation section](../configuration-file/nodes.md#implementation) on the configuration file.
1. Use the [CppFragment](../cpp-attributes/reference.md#cppfragment) attribute to selectively translate individual statements and expressions. This approach is preferable to completely replacing the method body, as it responds more effectively to changes in the original C# code.
1. Some specific issues can well be fixed within the translator itself. To do this, you can contact the developers and submit a bug report.

## T80: Type declaration inherits enclosing type, which is prohibited in C++ {#t80} ##

In C++, a class cannot inherit from the class in which it is nested, because inheritance requires complete types, and the enclosing type is not yet complete at the time the nested type is compiled.

**Default severity**: ERROR

**Example**:

```cs
class A
{
    class B : A
    {
    }
}
```

**Solution**: rewrite C# source code and move derived class oustide of base one

```cs
class A
{
}

class B : A
{
}
```

## T81: Method does not override in C#, however, it will override it in C++ {#t81} ##

In C++, it is impossible to declare a method in a derived class without it overriding the corresponding method in the base class. The `new` modifier is not supported in C++ either. This leads to situations where code translated into C++ may behave differently than code written in C#.

**Default severity**: ERROR (or WARNING if [unexpected_override_as_warning](../configuration-file/options.md#unexpected_override_as_warning) option is turned on)

**Example**:

```cs
class A
{
    public virtual void Foo()
    {
    }
}

class B : A
{
    public new void Foo()
    {
    }
}
```

**Solution**: rename derived method to avoid overriding on C++ side

```cs
class A
{
    public virtual void Foo()
    {
    }
}

class B : A
{
    [CodePorting.Translator.Cs2Cpp.CppRenameEntity("NewFoo")]
    public new void Foo()
    {
    }
}
```

## T90: Attributes cannot be applied to same node {#t90} ##

Some attributes cannot be added to the same node because it makes no sense to do so. This message informs you of such a situation.

**Default severity**: ERROR

**Example**: mutual placement attributes usage

```cs
class A
{
    void Foo() {}
    void Bar() {}
    [CodePorting.Translator.Cs2Cpp.CppPlaceAfter("Foo")]
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Bar")]
    void Gaz() {}
}
```

**Solution**: remove the unnecessary attribute

```cs
class A
{
    void Foo() {}
    void Bar() {}
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Bar")]
    void Gaz() {}
}
```

## T91: Attribute parameter has invalid value {#t91} ##

The message indicates that one of the parameters of the specified attribute has a value that makes no sense in the context of its application.

**Default severity**: ERROR

**Example**: bad placement anchor name

```cs
class A
{
    void Bar() {}
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Baz")]
    void Gaz() {}
}
```

**Solution**: fix parameter value

```cs
class A
{
    void Bar() {}
    [CodePorting.Translator.Cs2Cpp.CppPlaceBefore("Bar")]
    void Gaz() {}
}
```

## T92: Fragment doesn't match any suitable node in definition {#t92} ##

The first argument of the [CppFragment](../cpp-attributes/reference.md#cppfragment) attribute does not correspond to any node of a suitable type within the method to which the attribute is applied. The attribute will be ignored.

The permissible types of nodes to which the fragments can be applied are:

* **Statements** (with nested statements and semicolons).
* **Expressions** (almost all kinds).
* **Variable declarations** (including ones inside of `for` loops and some other cases).

If the fragment corresponds to a node of a different type (for example, substituting different patterns is not currently supported), we will also see this issue.

**Default severity**: WARNING

**Example**: missing ';' in fragment pattern string

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("Bar()", "Gaz()")]
void Foo()
{
    Bar();
}
```

**Solution**: fix parameter values

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("Bar();", "Gaz();")]
void Foo()
{
    Bar();
}
```

## T93: Replacement code fragment not found in translated code {#t93} ##

The [CppFragment](../cpp-attributes/reference.md#cppfragment) attribute may replace not the entire node, but only a portion of its translated code. In this case, if a replacement template is not found, a message to that effect will be displayed, and the attribute will be ignored.

**Default severity**: WARNING

**Example**: typo in replacement pattern

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("B...();", "Baz-->Gaz")]
void Foo()
{
    Bar();
}
```

**Solution**: fix replacement pattern

```cs
[CodePorting.Translator.Cs2Cpp.CppFragment("B...();", "Bar-->Gaz")]
void Foo()
{
    Bar();
}
```

## T94: Ambigous friend name for type {#t94} ##

A friend type added via the [CppDeclareFriendClass](../cpp-attributes/reference.md#cppdeclarefriendclass) attribute using a simple name is ambiguous and may resolve to different actual types. This can result in the wrong type being declared as a friend, leading to a C++ compilation error.

**Default severity**: WARNING

**Solution**: classify the type more unambiguously.

## T95: Unsupported test attribute {#t95} ##

The translator and the C++ framework do not fully support all .NET test frameworks. Some attributes are unsupported and simply ignored. This can cause the translated test to behave quite differently than it does in C#. This message warns of the risk of such a situation.

**Default severity**: WARNING

**Solution**: remove this attribute, redesign or skip test or just ignore warning.
