---
navTitle: "Dependent Console Application"
---

# Translating a Dependent Console Application #

The following example demonstrates how to translate two C# projects: a console application and a library project on which the console application depends. We’ll use a pre-existing project from the [DependentConsoleApp example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/DependentConsoleApp).

This example consists of two C# projects: CommonLib and DependentConsoleApp.

## Translating CommonLib ##

CommonLib is a library project consisting of a single .cs source file, *SomeClass.cs*, and a project file, *CommonLib.csproj*. This project does not have any special dependencies on other projects or third-party assemblies. The CommonLib project directory also contains a pre-created configuration file, *CommonLib.translator.config*, which is quite simple and can be omitted.

This example assumes that the C# CommonLib project should be translated into a C++ static library, which is the default setting. With the C# project and configuration file ready, we can convert the project.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\DependentConsoleApp\CommonLib\CommonLib.translator.config C:\DependentConsoleApp\CommonLib\CommonLib.csproj C:\output
```

Translator will print translation logs to the console window. When translation finishes, the *C:\output* directory will contain a directory named *CommonLib.Cpp*, containing the generated C++ source files and CMake configuration files. We can then use CMake to generate makefiles or project files.

```cmd
>cd C:\output\CommonLib.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

The library is built.

## Translating DependentConsoleApp ##

DependentConsoleApp is a C# console application project that consists of a single .cs source file, *Program.cs*, and a project file, *DependentConsoleApp.csproj*. This project depends on the previously translated CommonLib project. This dependency must be reflected in the DependentConsoleApp project's configuration file. In our example, this configuration file is pre-created. Its name is *DependentConsoleApp.translator.config*, and it is located in the *DependentConsoleApp* project directory. Let us have a closer look at the configuration file.

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

We need to import an *include_map.config* file from the translated CommonLib project. This file maps public types exported by the CommonLib library to the generated C++ header files in which these types are declared. Translator generates an *include_map.config* file for each project it translates. Thus, before translating the DependentConsoleApp project, we must translate the CommonLib project so that Translator generates the *include_map.config* file. The file is included in *DependentConsoleApp.translator.config* as follows:

```xml
    <import config="../../output/CommonLib.Cpp/include_map.config" />   
```

Here, *../../output* is the output directory passed to Translator when the CommonLib project was translated.

Next, we want Translator to add some commands to the output CMakeLists.txt file. We do this by adding a \<cmake_commands\> element containing raw CMake commands to the configuration file.

```xml
    <cmake_commands>   
       <![CDATA[     
```

The first command sets the output directory for the executable binary by setting the corresponding property on the ${PROJECT_NAME} target.

```txt
    set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")   
```

Here, ${PROJECT_NAME} is the name of the CMake project, which is equal to the name of the main CMake target. Then the \<cmake_commands\> element is closed.

```xml
      ]]>   
    </cmake_commands>   
```

Then, we need to tell Translator that the DependentConsoleApp project depends on the CommonLib library. We do this using the \<lib\> element:

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

Here, \${PROJECT_NAME}_dependencies is the name of the CMake interface library target defined in the output *CMakeLists.txt* file and linked to the main target, \${PROJECT_NAME}. Thus, libraries linked to \${PROJECT_NAME}_dependencies are automatically linked to the \${PROJECT_NAME} target.

With the C# project at hand and configuration file ready, we can convert the project.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\DependentConsoleApp\DependentConsoleApp\DependentConsoleApp.translator.config C:\DependentConsoleApp\DependentConsoleApp\DependentConsoleApp.csproj C:\output
```

We can then use CMake to generate makefiles or project files.

```cmd
>cd C:\output\DependentConsoleApp.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

## Expected output ##

When the build finishes, the *C:\output\bin\Release* directory should contain two files: *DependentConsoleApp.Cpp.exe*, which has just been built from the C++ sources, and *codeporting.translator.cs2cpp.framework_vc14x64.dll*, which was copied from the Translator installation directory during a post-build step. When we run *DependentConsoleApp.Cpp.exe*, its output in the console window should be similar to the output of the original C# application we translated.
