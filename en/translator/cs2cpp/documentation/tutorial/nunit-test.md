---
navTitle: "NUnit Test"
---

# Translating an NUnit Test #

This example demonstrates how to translate a C# NUnit test project. We’ll use a pre-existing project from the [SimpleNUnitTest example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleNUnitTest).

SimpleNUnitTest is a library project that contains NUnit tests. Translator converts C# NUnit library projects into C++ executable projects. The *SimpleNUnitTest* project consists of a single .cs source file, *SimpleNUnitTest.cs*, and a project file, *SimpleNUnitTest.csproj*. This project does not have any dependencies on other C# projects.

## Configuration file ##

The *SimpleNUnitTest* project's configuration file is pre-created. Its name is *SimpleNUnitTest.porter.config*, and it is located in the *SimpleNUnitTest* project directory. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
  
    <cmake_commands>
    <![CDATA[
      set_target_properties(${PROJECT_NAME}_gtest PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
    ]]>
  </cmake_commands>
</porter>
```

The command sets the output directory for the test executable by setting the corresponding property on the ${PROJECT_NAME}_gtest target.

```cmake
set_target_properties(${PROJECT_NAME}_gtest PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here ${PROJECT_NAME} is the name of the CMake project which is equal to the name of the main CMake executable target.

## Expected output ##

The translation and build algorithm is standard. When the build finishes, the *C:\output\bin\Release* directory should contain two files: *SimpleNUnitTest.Cpp_gtest.exe* and *codeporting.translator.cs2cpp.framework_vc14x64.dll*. When we run *SimpleNUnitTest.Cpp_gtest.exe*, it executes the tests and prints the results to the console window. The tests executed by *SimpleNUnitTest.Cpp_gtest.exe* are similar to those in the original C# project.
