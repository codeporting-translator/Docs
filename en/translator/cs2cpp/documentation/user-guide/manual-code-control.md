# Manual code control #

A modern translator is developed with the approach that correctly written C# code should translate seamlessly into adequate C++ code without any additional configuration or subsequent edits. However, in reality, it's often necessary to resort to [translation settings](./configuration-file/main.md) and attribute assignments. But even these approaches don't always work, as options and attributes can't fully address all the limitations and bugs encountered in the translator. In this case, the "last resort" — manual code control — comes to the rescue.

## C# code control ##

In some cases, manual control of the translation process is not required; it's enough to simply specify which code fragment should be used when the code is executed as C# code, and which fragment should be used only when translating to C++. A special symbol, `__cplusplus`, is used for this purpose.

```cs
public static bool IsTranslatedCode()
{
#if __cplusplus
    return true;
#else
    return false;
#endif
}
```

This mechanism is a convenient way to remove definitions and code sections from C# code that are fundamentally untranslatable to C++, replacing them with translatable equivalents or inserting stubs. However, this tool should be used with caution to avoid unexpected pitfalls:

1. By inserting such a condition, you create code that will never be tested except when translated to C++. This often leads to situations where the C++ section becomes outdated or even inconsistent with the rest of the code. Errors arise that aren't visible in the IDE, but only in the translator's compilation error log (*translatorSourceErrors.log*), which is rarely noticed.
1. Quite often, adequate alternatives that would allow replacing non-translatable code with translatable code simply don't exist, or they're not entirely equivalent. This forces developers to resort to various hacks and tricks that result in difficult-to-diagnose failures. For example, attempting to "cut out" the `vritual` keyword from a generic method (virtual methods cannot be templates in C++) will sooner or later result in code that compiles but doesn't run correctly.
1. This approach is fundamentally C#-intrusive, and not all team policies allow polluting the source code of the translated project with such directives.

There's also a special symbol, `__roslyn`, which indicates that the project is being translated using the modern, Roslyn-based translator, rather than the older NRefactory-based one. This option will be useful for teams gradually migrating from the old translator to the new one, to avoid breaking backward compatibility.

## C++ code injection ##

Sometimes there's a need to inject some C++ code into your translated C# code. If translating your code just once, you can do this after the translation completes. However, if you are setting up a pipeline for translating some code continuously (e. g. to translate each version of your product), it is easier to let translator do this job.

There are several features providing this behavior.

### Definition replacement ###

Placing [CppSkipDefinition](cpp-attributes/reference.md#cppskipdefinition) at some method will remove this method's definition (but not declaration) from translated code. After doing this, you can either provide an alternative definition using [\<implementation\>](configuration-file/nodes.md#implementation), or simply include a *.cpp file containing one directly into your project - when translation is done, this file will be copied into output project unchanged.

### Code line injection ###

Alternatively, you may use a specially formatted comments in your C# code to add small portions of C++ code into your translated code. The example is below.

```cs
void Increment(ref int i, ref int j, ref int k)
{
    ++i;
    //CPPCODE: ++j;
    //++k;
}
```

In translated code, both `++i` and `++j` expressions will be present. However, `++k` is just a comment and it will remain this way after translating.

> ⚠️ This is a simple but outdated approach that is highly discouraged from being used in new code. It's unreliable, as the translator generally ignores comments and may simply "forget" to process such a construct if it occurs in an unexpected place. Secondly, line breaks and spaces may be "lost", leading to formatting errors or even uncompiled code. Thirdly, such tricks often break the semantics on the C# side, which creates problems for correct translation.

A modern compiler provides a better alternative - the [CppFragment](cpp-attributes/reference.md#cppfragment) attribute, which allows for a safer and more predictable replacement of some of the syntax nodes from the implementation of C# methods to the specified fragments of C++ code.
