# Translating simple console application #

Note that this example is built upon several assumptions, namely:

* Translator is installed to *C:\CodePorting.Translator_Cs2Cpp* directory
* The sample C# project is located in *C:\SimpleConsoleApp* directory
* The output directory is *C:\output*

This example demonstrates how to translate a console application C# project to a C++ console application project. We’ll use pre-existing project from [SimpleConsoleApp example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleConsoleApp).

**SimpleConsoleApp** is a console application C# project that consists of a single .cs source file *SimpleConsoleApp.cs* and a project file *SimpleConsoleApp.csproj*. This project does not depend on any other C# projects. SimpleConsoleApp project's configuration file is pre-created, its name is *SimpleConsoleApp.translator.config* and it is located in the project’s directory. Let us have a closer look at the configuration file.

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

SimpleConsoleApp.translator.config begins with an XML declaration, which specifies that the file contains an XML document. Then goes the XML root element \<porter\> which is mandatory for Translator configuration XML document.

Next, the default Translator configuration file is imported using \<import\> element. The default configuration will assign default values to all configuration options.  

Also we want Translator to add some commands to the output CMakeLists.txt. We do that by adding \<cmake_commands\> element to the configuration file containing raw CMake commands. The first command sets the output directory for the executable binary by setting the corresponding property on the target ${PROJECT_NAME}.

```cmake
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here ${PROJECT_NAME} is the name of the CMake project which is equal to the name of the main CMake executable target.

Then the \</cmake_commands\> element is closed. Finally the XML document is finished with closing tag of the root element \</porter\>  

With the C# project at hand and configuration file ready, we can convert the project.

In order to convert **SimpleConsoleApp** project we run **CMD** and navigate to the directory with translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
```

And run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe -c C:\SimpleConsoleApp\SimpleConsoleApp.translator.config C:\SimpleConsoleApp\SimpleConsoleApp.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory *C:\output* will contain a directory named *SimpleConsoleApp.Cpp* containing the generated C++ source files and CMake configuration files.

Now we want to use CMake to generate makefile/project files. Let it be a Visual Studio 2022 x86 project file. In CMD we navigate to the *C:\output\SimpleConsoleApp.Cpp* directory

```cmd
>cd C:\output\SimpleConsoleApp.Cpp
```

And run CMake in configuration mode:

```cmd
>CMake --G "Visual Studio 17 2022"
```

And now we can build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
>CMake --build . --config Release
```

When build finishes, directory *C:\output\bin\Release* should contain two files: *SimpleConsoleApp.Cpp.exe*, which has just been built from sources, and *aspose_cpp_vc140.dll*, which was copied from Translator installation directory during a post-build step. When we run *SimpleConsoleApp.Cpp.exe* its output to the Console window should be similar to the output of the original C# application project we translated.
