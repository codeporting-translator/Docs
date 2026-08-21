---
order: "2"
navTitle: ".NET"
---

# .NET attributes #

This section lists .NET attributes recognized by the translation application.

## System.Diagnostics.Conditional ##

**Used on**: Methods

**Argument**: Mandatory string name of preprocessor definition

Wraps method body with `#if defined` directives with attribute argument as definition name. Multiple attributes join using 'logical or' coupling.

```cs
[System.Diagnostics.Conditional("TEST_DEF")]
public void func1(int val)
{
    func3(val);
}
[Conditional("NONEXISTENT"), ConditionalAttribute("TEST_DEF"), System.Diagnostics.Conditional("BOO")]
public void func2(int val)
{
    func3(val);
}
```

```cpp
void ClassName::func1(int32_t val)
{
#if defined(TEST_DEF)
    func3(val);
#endif
}
void ClassName::func2(int32_t val)
{
#if defined(NONEXISTENT) || defined(TEST_DEF) || defined(BOO)
    func3(val);
#endif
}
```

## System.ThreadStatic ##

**Used on**: static fields

**Arguments**: None

Puts static variable into thread scope. In C++, translates into **thread_local** scope specifier.

```cs
class MyThread
{
    public int value0;
    [ThreadStatic]
    public static int value1 = 0;
    [ThreadStatic]
    public int value2 = 0;
    public static int value3 = 0;
}
```

```cpp
class MyThread
{
    ...
    int32_t value0;
    static thread_local int32_t value1;
    int32_t value2;
    static int32_t value3;
};
thread_local int32_t MyThread::value1 = 0;
int32_t MyThread::value3 = 0;
```

## System.Obsolete ##

**Used on**: classes and members

**Arguments**: None

**Supported since version**: 19.11

Translates to `@deprecated` Doxygen annotation.
