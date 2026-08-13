# Translating simple library #

Note that this example is built upon several assumptions, namely:

* Translator is installed to *C:\CodePorting.Translator_Cs2Cpp*.4 directory
* All C# projects are located in *C:\SimpleLibrary* directory
* The output directory is *C:\output*

This example demonstrates how to a simple C# library project. We’ll use pre-existing projects from [SimpleLibrary example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SimpleLibrary).

**SimpleLibrary** is a library project that consists of a single .cs source file *SimpleLibrary.cs* and a project file *SimpleLibrary.csproj*. This project does not depend on any other C# projects. SimpleLibrary project's configuration file is pre-created, its name is *SimpleLibrary.translator.config* and it is located in the project’s directory *SimpleLibrary*. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
</porter>
```

SimpleLibrary.translator.config file is quite simple. It begins with an XML declaration, which specifies that the file contains an XML document. Then goes the XML root element \<porter\> which is mandatory for Translator configuration XML document. Next, the default Translator configuration file is imported using \<import\> element. The default configuration will assign default values to all configuration options. Finally the XML document is finished with closing tag of the root element \</porter\>  

This example assumes that C# SimpleLibrary project should be translated into C++ static library project, which is a default setting.

With the C# project at hand and configuration file ready, we can convert the project.

In order to convert **SimpleLibrary** project we run CMD and navigate to the directory with translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\translator
```

And run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe -c C:\SimpleLibrary\SimpleLibrary.translator.config C:\SimpleLibrary\SimpleLibrary.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory *C:\output* will contain a directory named *SimpleLibrary.Cpp* containing the generated C++ source files and CMake configuration files.

Now we want to use CMake to generate makefile/project files. Let it be a Visual Studio 2022 x86 project file. In CMD we navigate to the *C:\output\SimpleLibrary.Cpp* directory

```cmd
>cd C:\output\SimpleLibrary.Cpp
```

And run CMake in configuration mode:

```cmd
>CMake --G "Visual Studio 17 2022"  
```

And now we can build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
>CMake --build . --config Release
```

The library is built.
