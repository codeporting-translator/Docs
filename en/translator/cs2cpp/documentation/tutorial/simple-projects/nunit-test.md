# Translating nUnit test #

Note that this example is built upon several assumptions, namely:

* Translator is installed to *C:\CodePorting.Translator_Cs2Cpp* directory
* A sample C# project is located in *C:\SimpleNUnitTest* directory
* The output directory for the project is *C:\output*

This example demonstrates how to translate a C# NUnit test project. We’ll use pre-existing project from [SimpleNUnitTest example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleNUnitTest).

SimpleNUnitTest is a library project that contains NUnit tests. Translator translates C# NUnit library projects into C++ executable projects. *SimpleNUnitTest* project consists of a single .cs source file *SimpleNUnitTest.cs* and a project file *SimpleNUnitTest.csproj*. This project does not have any dependencies on other C# projects. *SimpleNUnetTest* project's configuration file is pre-created, its name is *SimpleNUnitTest.translator.config* and it is located in the project’s directory *SimpleNUnitTest*. Let us have a closer look at the configuration file.

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

SimpleConsoleApp.translator.config begins with an XML declaration, which specifies that the file contains an XML document. Then goes the XML root element \<porter\> which is mandatory for Translator configuration XML document.

Next, the default Translator configuration file is imported using \<import\> element. The default configuration will assign default values to all configuration options.  

Also we want Translator to add some commands to the output CMakeLists.txt. We do that by adding \<cmake_commands\> element to the configuration file containing raw CMake commands. The first command sets the output directory for the executable binary by setting the corresponding property on the target ${PROJECT_NAME}_gtest.

```cmake
set_target_properties(${PROJECT_NAME}_gtest PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here ${PROJECT_NAME} is the name of the CMake project which is equal to the name of the main CMake executable target.

Then the \</cmake_commands\> element is closed. Finally the XML document is finished with closing tag of the root element \</porter\>  

With the C# project at hand and configuration file ready, we can convert the project.

In order to convert SimpleNUnitTest project we run CMD and navigate to the directory with translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\translator
```

And run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe -c C:\SimpleNUnitTest\SimpleNUnitTest.translator.config C:\SimpleNUnitTest\SimpleNUnitTest.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory *C:\output* will contain a directory named *SimpleNUnitTest.Cpp* containing the generated C++ source files and CMake configuration files.

Now we want to use Cmake to generate makefile/project files. Let it be a Visual Studio 2022 x86 project file. In CMD we navigate to the *C:\output\SimpleNUnitTest.Cpp* directory

```cmd
>cd C:\output\SimpleNUnitTest.Cpp
```

And run CMake in configuration mode:

```cmd
>CMake --G "Visual Studio 17 2022"
```

And now we can build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
CMake --build . --config Release
```

When build finishes, directory *D:\output\bin\Release* should contain two files: *SimpleNUnitTest.Cpp_gtest.exe*, which has just been built from sources, and *aspose_cpp_vc140.dll*, which was copied from Translator installation directory during a post-build step. When we run *SimpleNUnitTest.Cpp_gtest.exe* it executes tests and prints results to the console window. The tests *SimpleNUnitTest.Cpp_gtest.exe* executes are similar to those in original C# project.
