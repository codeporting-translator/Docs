---
navTitle: "Console Application"
---

# Translating a Simple Console Application #

This example demonstrates how to translate a C# console application project into a C++ console application project. We’ll use a pre-existing project from the [SimpleConsoleApp example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleConsoleApp).

**SimpleConsoleApp** is a C# console application project that consists of a single .cs source file, *SimpleConsoleApp.cs*, and a project file, *SimpleConsoleApp.csproj*. This project does not depend on any other C# projects.

## Configuration file ##

The SimpleConsoleApp project's configuration file is pre-created. Its name is *SimpleConsoleApp.porter.config*, and it is located in the project’s directory. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
  
  <cmake_commands>
    <![CDATA[
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
    ]]>
  </cmake_commands>
  
</porter>
```

SimpleConsoleApp.porter.config begins with an XML declaration, which specifies that the file contains an XML document. The XML root element, \<porter\>, follows and is mandatory for a Translator configuration XML document.

Next, the default Translator configuration file is imported using the \<import\> element. The default configuration assigns default values to all configuration options.

We also want Translator to add some commands to the output CMakeLists.txt file. We do this by adding a \<cmake_commands\> element containing raw CMake commands to the configuration file. The first command sets the output directory for the executable binary by setting the corresponding property on the ${PROJECT_NAME} target.

```cmake
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here, ${PROJECT_NAME} is the name of the CMake project, which is equal to the name of the main CMake target.

Then the \</cmake_commands\> element is closed. Finally, the XML document is finished with the closing tag of the root element, \</porter\>.

## Converting the project ##

With the C# project and configuration file ready, we can convert the project. To convert the **SimpleConsoleApp** project, we navigate to the directory containing the translator binary and run Translator:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\SimpleConsoleApp\SimpleConsoleApp.porter.config C:\SimpleConsoleApp\SimpleConsoleApp.csproj C:\output
```

The *C:\output* directory will contain a directory named *SimpleConsoleApp.Cpp*, containing the generated C++ source files and CMake configuration files. Next, as in the [previous example](single-file.md#making-and-building-of-resulting-c-code), we build the resulting C++ project.

```cmd
>cd C:\output\SimpleConsoleApp.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

## Expected output ##

When the build finishes, the *C:\output\bin\Release* directory should contain two files: *SimpleConsoleApp.Cpp.exe*, which has just been built from the C++ sources, and *codeporting.translator.cs2cpp.framework_vc14x64.dll*, which was copied from the Translator installation directory during a post-build step. When we run *SimpleConsoleApp.Cpp.exe*, its output in the console window should be similar to the output of the original C# application we translated.
