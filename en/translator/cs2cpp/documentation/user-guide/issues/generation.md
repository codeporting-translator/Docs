# Generation Issues #

This section describes issues that arise during C++ code generation phase.

[TOC]

## G10: Undefined type, no include generated {#g10} ##

This message warns that a type was used on the C# side whose C++ definition path was not found in the include table. In other words, either the type is missing from the C++ framework, or it was omitted from the `<includes>` section of the configuration file. For this reason, the translator cannot add the include it deems necessary, and this may (or may not) lead to a compilation error in the translated code.

Since the translator for types defined within the target project itself builds the inclusion table independently, such warnings most likely pertain to system types that are simply not implemented in the framework. However, this could also indicate issues with dependency configuration across a group of projects being translated — specifically, cases where you fail to add the base project's type table to the configuration of the dependent project.

**Default severity**: WARNING

**Solution**. If the translated code works, you can ignore the warning, as the translator sometimes plays it safe and adds includes even when they aren't strictly necessary. However, if errors occur, it likely indicates that you are using APIs not implemented in the C++ framework. You will probably need to rewrite the original C# code to avoid using those types, or implement them yourself and add them to the project as a separate dependency.

## G11: Cross include found, forwarding if possible {#g11} ##

This message indicates a situation where two header files need to include each other. In some cases, this can cause C++ compilation errors, so the translator replaces such an inclusion with a forward declaration, hoping that this will suffice.

**Default severity**: WARNING

**Example**: mutual files dependency

```cs
// a.cs
struct ASmall
{
}

class ABig
{
    BSmall b;
}

// b.cs
struct BSmall
{
}

class BBig
{
    ASmall a;
}
```

Structs are value types; defining a class that contains one requires including the definition of the struct itself. Consequently, file `a.h` must include `b.h`, and vice versa. While include guards prevent infinite inclusion, `b.h` cannot include `a.h` when it is itself being included from within `a.h`; as a result, `b.h` is compiled without the definition of the `ASmall` struct, leading to a compilation error for class `BBig`.

**Solution**: The best solution is to place the definition of each type into its own separate file. As it happens, the translator places type definitions in the same files where they are located in C#. This is an obvious but not always ideal approach: given that the compilation models of the two languages ​​differ radically and what works for C# is often completely unacceptable in C++. Therefore, it is worth "helping" the translator by distributing the classes in a way that suits its requirements. If, for some reason, this is not possible, the only remaining option is to replace the struct with a class; this ensures that defining the owning classes does not require the full inclusion of the owned structs.

## G12: Can't include as the former contains classes inherited from the ones {#g12} ##

An error virtually identical to the previous one, with similar solutions.
