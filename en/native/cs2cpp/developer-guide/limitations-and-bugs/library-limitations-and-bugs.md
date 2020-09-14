---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Library Limitations and Bugs"
linktitle: "Library Limitations and Bugs"
menu:
  docs:
    parent: "Limitations and Bugs"
    weight: "2"
lastmod: "2019-05-28"
weight: "2"
---

## Library Limitations and Bugs ##

This page contains Library Limitation and Bugs.


### Only strongly typed collection classes are implemented by the Library ###

Library does not provide implementation of collection classes and interfaces disigned to deal with heterogenuous collections of objects. Only generic classes and interfaces representing strongly typed collections defined in .NET library's System.Collections.Generic namespace are implemented and supported by Libray.

### Class System::Nullable<T> is not fully implemented ###

Class System::Nullable<T> does not have GetValueOrDefault() method overload that takes no arguments.

### Method Convert() of System::Text::Encoder class does not throw in some situations ###

The .NET version of method System::Text::Encoder::Convert() throws ArgumentException exception if the output buffer passed to the method is too small to contain any of the converted input. The Library's version of this method does not throw exceptions in this case.

### Return value of method ToString() of System::Type class may be not what is expected ###

Method Type::ToString() returns a string containig the name of the current type. For most types represented by class Type method Type::ToString() returns a value that is different from that returned by .NET version of this method. For example the following piece of C# code will print 'System.String[]' when executed

{{< highlight cpp >}}
string[] a = new string[3];
System.Console.WriteLine(a.GetType().ToString());

{{< /highlight >}}

while corresponding C++ code

{{< highlight cpp >}}
System::ArrayPtr<System::String> a = System::MakeArray<System::String>(3);
System::Console::WriteLine(System::ObjectExt::ToString(System::ObjectExt::GetType(a)));

{{< /highlight >}}

will print something like 'class System::Array<class System::String>'. Note that the actual output produced by C++ code may vary depneding on the compiler used to compile the code.

### Operations with instances of System::Object class imposes a performance penalty ###

Operations on entities of type System::Object (including type conversion operations) impose additional overhead which results in performance penalty. It is strongly advised to avoid using entities of type System::Object as much as possible and stick to using entities of most derived types instead.

### Behavior of classes from System::Globalization namespace may be not what is expected ###

Behavior of classes from System::Globalization namespace may be slightly different from that of same classes in .NET library.

### Support of reflection classes from System::Reflection namespace is not complete ###

Not all classes found in System.Reflection namespace in .NET library are provided by the library. Also, behavior of those provided may slightly deviate from that of corresponding classes in .NET library.

### EMF image format is not supported by the Library ###

Classes from System::Drawing namespace do not support EMF image format.

### Default font is not substituted for missing glyphs ###

In .NET, when rendering string containing rare (e. g. Japanese) characters which are not supported by the font used, default font is substituted. Our library doesn't do substitutions like this.

### Rendering does not work exactly like in .NET ###

After porting, rendering results may change.
