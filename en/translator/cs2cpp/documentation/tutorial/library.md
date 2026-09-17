---
navTitle: "Simple Library"
---

# Translating a Simple Library #

This example demonstrates how to translate a simple C# library project. We’ll use a pre-existing project from the [SimpleLibrary example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleLibrary).

**SimpleLibrary** is a library project that consists of a single .cs source file, *SimpleLibrary.cs*, and a project file, *SimpleLibrary.csproj*. This project does not depend on any other C# projects. The SimpleLibrary project's configuration file is pre-created. Its name is *SimpleLibrary.porter.config*, and it is located in the *SimpleLibrary* project directory. Its content is very simple:

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
</porter>
```

> Note that a configuration file with such simple content can be omitted, and the project can be built without explicitly specifying a configuration file.

This example assumes that the C# SimpleLibrary project should be translated into a C++ static library, which is the default setting. The project must be [converted and built](console-application.md#converting-the-project) in the same way as in the previous lesson.

When the build finishes, the *C:\output\SimpleLibrary.Cpp\Release* directory should contain one file: *SimpleLibrary.Cpp_vc14x64.lib* static library. It can be statically linked to any project and used via the headers located in the *C:\output\SimpleLibrary.Cpp\include* folder.

> The library will also require the C++ framework dynamic library, *codeporting.translator.cs2cpp.framework_vc14x64.dll*. The project that compiles the final binary (dynamic library or executable) is responsible for providing it.
