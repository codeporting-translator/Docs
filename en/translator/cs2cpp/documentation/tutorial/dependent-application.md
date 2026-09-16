---
order: "3"
navTitle: "4. Dependent Console Application"
---

# Lesson 4. Translating dependent console application #

Note that this example is built upon several assumptions, namely:

* Translator is installed to *C:\CodePorting.Translator_Cs2Cpp* directory
* The sample C# project is located in *C:\DependentConsoleApp* directory
* The output directory is *C:\output*

The following example demonstrates how to translate two C# projects one being a console application C# project and another – a library project on which the console application C# project depends. We’ll use pre-existing project from [DependentConsoleApp example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/DependentConsoleApp).

This example consists of two C# projects – CommonLib and DependentConsoleApp.

## Translating CommonLib ##

CommonLib is a library project consisting of a single .cs source file SomeClass.cs and a project file CommonLib.csproj. This project does not have any special dependencies on other projects or 3rd party assemblies.

Also CommonLib project directory contains precreated configuration file *CommonLib.translator.config*. Let us have a closer look at the configuration file. CommonLib.translator.config is quite simple.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config" />
</porter>
```

It begins with an XML declaration, which specifies that the file contains an XML document. Then goes the XML root element \<porter\> which is mandatory for Translator configuration XML document. Next, the default Translator configuration file is imported using \<import\> element. The default configuration assigns default values to all configuration options. And the XML document is finished with closing tag of the root element \<porter\>

This example assumes that C# CommonLib library project should be translated into a C++ static library project, which is a default setting. With C# project and configuration file ready, we can convert the project. In order to convert  CommonLib project we run CMD and navigate to the directory with translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
```

And run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe -c C:\DependentConsoleApp\CommonLib\CommonLib.translator.config C:\DependentConsoleApp\CommonLib\CommonLib.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory *C:\output* will contain a directory named *CommonLib.Cpp* containing the generated C++ source files and CMake configuration files.

Now we want to use CMake to generate makefile/project files. Let it be a Visual Studio 2022 project file. In CMD we navigate to the *C:\output\CommonLib.Cpp* directory

```cmd
>cd C:\output\CommonLib.Cpp
```

And run CMake in configuration mode:

```cmd
>CMake -G "Visual Studio 17 2022" .
```

And now we can build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
>CMake --build . --config Release
```

**The library is built.**

## Translating DependentConsoleApp ##

DependentConsoleApp is a console application C# project that consists of a single .cs source file *Program.cs* and a project file *DependentConsoleApp.csproj*. This project has a dependency on previously translated project CommonLib. This dependency has to be reflected in the DependentConsoleApp project's configuration file. In our example this configuration file is pre-created, its name is *DependentConsoleApp.translator.config* and it is located in the project’s directory *DependentConsoleApp*. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>

  <import config="../../output/CommonLib.Cpp/include_map.config" />

  <cmake_commands>
    <![CDATA[
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
    ]]>
  </cmake_commands>
  
  <lib name="CommonLib.Cpp" csname="CommonLib">
    <cmake_link_template>
            <![CDATA[
              find_package(CommonLib.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLib.Cpp" NO_DEFAULT_PATH)
              target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLib.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
</porter>
```

DependentConsoleApp.translator.config begins with an XML declaration, which specifies that the file contains an XML document. Then goes the XML root element \<porter\> which is mandatory for Translator configuration XML document. Next, the default Translator configuration file is imported using \<import\> element. The default configuration will assign default values to all configuration options.

Also we need to import a configuration file include_map.config from translated CommonLib project that maps public types exported by CommonLib library to generated C++ header files in which these types are declared. include_map.config is generated by Translator for each project it translates. Thus, before translating DependentConsoleApp project, CommonLib project has to be translated first so that Translator generates include_map.config. This is how include_map.config is included in DependentConsoleApp.translator.config  

```xml
    <import config="../../output/CommonLib.Cpp/include_map.config" />   
```

Here ../../output is a directory that was passed as an output directory to Translator when CommonLib project was translated.  

Next, we want Translator to add some commands to the output CMakeLists.txt. We do that by adding \<cmake_commands\> element to the configuration file containing raw CMake commands

```xml
    <cmake_commands>   
       <![CDATA[     
```

The first command sets the output directory for the executable binary by setting the corresponding property on the target ${PROJECT_NAME}

```txt
    set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")   
```

Here ${PROJECT_NAME} is the name of the CMake project that is equal to the name of the main CMake executable target. Then the \<cmake_commands> element is closed

```xml
      ]]>   
    </cmake_commands>   
```

Then, we need to tell Translator that DependentConsoleApp project depends on CommonLib library. We do it using \<lib\> element:

```xml
    <lib name="CommonLib.Cpp" csname="CommonLib">   
      <cmake_link_template>   
        <![CDATA[
          find_package(CommonLib.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLib.Cpp" NO_DEFAULT_PATH)   
          target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLib.Cpp)   
        ]]>   
     </cmake_link_template>   
    </lib>   
```

Here \${PROJECT_NAME}_dependencies is the name of the CMake Interface Library target that is defined in the output *CMakeLists.txt* file and is linked to main executable target \${PROJECT_NAME}. Thus libraries linked to \${PROJECT_NAME}_dependencies get automatically linked to \${PROJECT_NAME} target.

Finally the XML document is finished with closing tag of the root element \<porter\>.

With the C# project at hand and configuration file ready, we can convert the project. In order to convert DependentConsoleApp project we run CMD and navigate to the directory with translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
```

And run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe -c C:\DependentConsoleApp\DependentConsoleApp\DependentConsoleApp.translator.config C:\DependentConsoleApp\DependentConsoleApp\DependentConsoleApp.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory *C:\output* will contain a directory named *DependentConsoleApp.Cpp* containing the generated C++ source files and CMake configuration files.

Now we want to use CMake to generate makefile/project files. Let it be a Visual Studio 2022 project file. In CMD we navigate to the *C:\output\DependentConsoleApp.Cpp* directory

```cmd
>cd C:\output\DependentConsoleApp.Cpp
```

And run CMake in configuration mode:

```cmd
>CMake -G "Visual Studio 17 2022" .
```

And now we can build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
>CMake --build . --config Release
```

When the build finishes, directory *C:\output\bin\Release* should contain two files: *DependentConsoleApp.Cpp.exe*, which has just been built from C++ sources, and *aspose_cpp_vc140.dll*, which was copied from Translator installation directory during a post-build step. When we run *DependentConsoleApp.Cpp.exe* its output to the Console window should be similar to the output of the original C# application project we translated.
