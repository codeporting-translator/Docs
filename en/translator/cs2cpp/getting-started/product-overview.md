---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Product Overview"
linktitle: "Product Overview"
menu:
  docs:
    parent: "Getting Started"
    weight: "01"
lastmod: "2019-05-28"
weight: "01"
---

## CodePorting.Translator Cs2Cpp ##

CodePorting.Translator Cs2Cpp is a translator application that translates C# project to C++. It is very easy to use without any major learning curve and saves number of hours and money of development for translating C# codebase to C++ .  It converts C# codebase to C++ in such a way that it can be used by C++ developers in the same way as if they were originally developed in this language. It can be used to:

* Set up automated translating of continuously updated C# code to C++ e. g. to release same versions of software for both languages.
* Deliver C# applications and libraries to platforms where .NET support is absent or problem.
* Keep API, code documentation and delivery format of original product for easy use.
* Resolve translating issues such as memory management model incompatibilities.
* Emulate .NET environment in pure C++ application.

## Key Features ##

Following are key features of CodePorting.Translator Cs2Cpp.


* Source code to source code
* API preservation
* Type system reproduction
* Memory management model
* Translating control
* Support library
* Typemap
* Translated code overriding
* Satisfying External Dependencies
* UnitTests Translating

### Source code to source code ###

To convert a project from C# to C++ using CodePorting.Translator Cs2Cpp one needs the source code rather than compiled assembly. This allows reproducing even internal classes and routines in the same way as they are implemented in original code. It is impossible to use CodePorting.Translator Cs2Cpp on IL code produced by C# compiler.

Once you have converted your project to C++, you can build it for any of supported target platforms.

### API preservation ###

Not only CodePorting.Translator Cs2Cpp converts C# projects to C++, __but it also preserves API when doing so. This means that language constructs used in C# are translated into best matched ones available in C++__. See the below table for several examples.

| Original C# construct| Translated C++ construct
---| ---|
| Class | Class
| Structure | Structure
| Enumeration | Enumeration
| Virtual method | Virtual method
| Non-virtual method | Non-virtual method
| Member variable | Member variable
| Static variable | Static variable
| ThreadStatic variable | thread_local variable
| Generic class | Template class
| Generic method | Template method
| Anonymous method | Lambda function

Such an approach allows not only executing translated code, but also using it from natural C++ perspective. This includes inheritance, type conversions, method overloads and so on.

### Type system reproduction ###

CodePorting.Translator Cs2Cpp reproduces original C# type system in translated C++ code. This means that it uses its own analogs of original .NET types rather than closest STL substitutes. For example, if in C# code one has 'System.Containers.Generic.List<System.String>' type, it will be translated into C++ type called 'System::Containers::Generic::List<System::String>' rather than into 'std::vector<std::string>'. This allows using this type in the same way it is used in original code by calling specific methods absent in std::vector and std::string and converting it to interfaces it implements (there are no interfaces implemented by std::vector). Similarly, if one has 'typeof(System.String)' construct in C# which is an expression of 'System.TypeInfo' type, in translated code it is replaced with 'System::String::Type()' custom function call which has return value of 'const System::TypeInfo&' type rather than with 'typeid(System::String)' for API compatibility reasons.

### Memory management model ###

In C#, allocated objects are collected by GC when there is no further need for them. In C++, __the code is unmanaged. To perform garbage collection in translated code, all objects are wrapped into (intrusive) smart pointers which count the number of active references and deallocate memory once the object has no active references to it left.__To fix loop references issues, CodePorting.Translator Cs2Cpp also provides means to mark specific pointers as weak ones. Currently, weak pointers labeling can only be done manually. To improve weak pointers lookup procedure, special debug modes are available.

### Translating control ###

The task of translating C# code to C++ is very complex. Many choices can be done on how to translate exact language constructs. To enable such control, CodePorting.Translator Cs2Cpp provides two mechanisms: [translator configuration](/translator/cs2cpp/developer-guide/codeporting-translator-cs2cpp-configuration-file/) and [translator attributes](/translator/cs2cpp/developer-guide/codeporting-translator-cs2cpp-attributes/). Attributes can be placed in C# code to mark the constructs that must be translated in a special way. Configuration files mostly define global project-wide options and rules. While attributes follow usual C# attribute notion, config file follows rich XML syntax. Inclusion and using single configuration file with multiple projects or using multiple configuration files with single projects (e. g. to enable several translating modes). There are also means to enable or disable parts of configuration file using translating application command lines or to pass variables via translating application command line.

So, some adjustments can be done to translating procedure using attributes and configuration files when required. While most of the process is automated there are some corner cases (e. g. weak pointer attributes placement) that can require manual adjustments.

### Support library ###

In most cases, code written in C# uses .NET library classes such as String or List. In C++ standard libraries some of these classes are absent and those present follow completely different rules and have incompatible API. This makes it impossible to map C#/.NET types into C++__/__STL ones directly. To bypass this limitation, CodePorting.Translator Cs2Cpp provides a special support library which comes in pre-compiled form for all supported platforms and exposes equivalents for .NET native types that can be used by both translated code and user code that works with it.

While translating application a. k. a. Translator runs once to convert the code, support library is distributed alongside with translated modules. It is impossible to use translated code without support library.We are constantly working to widen the coverage of .NET libraries available for translated code. The ultimate goal is to provide substitutions for all .NET types in translated code. Another function of support library is providing helper types and algorithms required (e. g. for memory management).

### Typemap ###

Some information should be shared between projects being translated. This includes information about types available in translated project, and their relation with types in original C# build. When translator translates the project, it maps original C# types to generated C++ ones. If there is a dependent project to be translated (e. g. application or library depending on code library), translator needs information on which types translate into which ones. Also, some additional information can be required. To satisfy this need, the translator generates so called typemap file which should be included from the configuration file which is then used when translating the dependent project.

### Translated code overriding ###

In some cases, translated code can be non-optimal. Also, it can call into modules you don't have sources for. Also, there are several constructs in C# that don't have immediate or close analogs in C++. __For example, there is no substitution for 'yeild' statement in C, or virtual functions can't be templates. We constantly work on improving our language support; however, we skip supporting some concepts that are completely foreign for C++__. For the full list of language constructs we don't support, see [Translator bugs and limitations](/translator/cs2cpp/developer-guide/limitations-and-bugs/translator-limitations-and-bugs/) page. You can as well have some other reasons for willing to replace translator-generated code with the code you prepare manually.

There are several ways to exclude some code from translating. One can exclude members, types or whole files. Also, there are ways to replace translator-generated members and types with manually written code.

### Satisfying External Dependencies ###

C# code can have dependencies from the libraries you don't have source for, or there can be legal situation preventing one from translating the code. In such cases, there is a way to substitute manually written code as a replacement for dependency code which is not available for translating. There are special rules for how to implement your API in such a way so it can be used with translated code. Except for API, there are no requirements on how to implement such parts of the code, so you can use third party library available on your target platform.

### UnitTests Translating ###

CodePorting.Translator Cs2Cpp is able to translate unit tests written using NUnit and XUnit frameworks to C++ and run tests on translated code. Translated tests can thereafter be run on translated code. So, if original code is covered with unit tests, translated code is also covered.
