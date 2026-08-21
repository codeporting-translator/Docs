---
order: "1"
navTitle: "Attributes"
---

# CodePorting.Translator Cs2Cpp Atrributes #

This section collects the documentation for the C# to C++ translator attributes.

## Attributes usage ##

There are a number of attributes recognized by CodePorting.Translator.Cs2Cpp. These include both widely known attributes introduced by [.NET](dotnet.md) or third-party test frameworks such as [NUnit](test.md#nunit-attributes) or [xUnit](test.md#xunit-framework-attributes) and special attributes introduced by [CodePorting.Translator.Cs2Cpp](reference.md) itself. The former attributes can be used in the same way they are normally used in C# applications. The latter can be placed in source code manually to prepare the code for translation; they are defined in the CodePorting.Translator.Cs2Cpp namespace. This section summarizes their effects.

To use an attribute, you must make it visible from your C# code. To do so, you should reference the CodePorting.Translator.Cs2Cpp.Control project from the one you are preparing to translate. Please note that the translator itself doesn't analyze attribute definitions; attributes are recognized by name, so inheriting attributes to simplify constructor arguments or for other reasons won't work.

All obsolete and unsupported attributes are listed in the [Obsolete](obsolete.md) section.
