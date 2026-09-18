---
navTitle: "Complex Console Application"
---

# Translating a Complex Console Application #

This example demonstrates how to translate five C# projects: one console application and four interdependent library projects on which the console application depends. We’ll use a pre-existing project from the [ComplexConsoleApp example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/ComplexConsoleApp).

This example consists of five C# projects: BaseLibrary, CommonLibrary, LibraryA, LibraryB, and ComplexConsoleApp. We’ll translate the projects one by one, starting with the least dependent one.

## Translating BaseLibrary ##

BaseLibrary is a library project consisting of a single .cs source file, *IBaseInterface.cs*, and a project file, *BaseLibrary.csproj*. This project does not have any special dependencies on other projects or third-party assemblies. The BaseLibrary project directory also contains a pre-created configuration file, *BaseLibrary.translator.config*, which is quite simple.

This example assumes that the C# BaseLibrary project should be translated into a C++ static library, which is the default setting.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\ComplexConsoleApp\BaseLibrary\BaseLibrary.translator.config C:\ComplexConsoleApp\BaseLibrary\BaseLibrary.csproj C:\output
>cd C:\output\BaseLibrary.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

## Translating CommonLibrary ##

The second project in this example is located in *CommonLibrary* directory. CommonLibrary is a library project consisting of a single .cs source file *BaseInterfaceSimplImpl.cs* and a project file *CommonLibrary.csproj*. This project has a dependency on BaseLibrary project. This dependency has to be reflected in CommonLibrary project's configuration file. In our example this configuration file is pre-created, its name is *CommonLibrary.translator.config* and it is located in the project’s directory *CommonLibrary*. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
  <import config="../../output/BaseLibrary.Cpp/include_map.config" />

  <opt name="make_shared_lib" value="true" export_per_member="true"/>
  
  <cmake_commands>
    <![CDATA[
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
      set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
    ]]>
  </cmake_commands>
  
  <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
    <cmake_link_template>
      <![CDATA[
        find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
        target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
      ]]>
    </cmake_link_template>
  </lib>
</porter>
```

This example assumes that the C# CommonLibrary project should be translated into a C++ shared or dynamic library. Therefore, we assign the value `true` to the `make_shared_lib` option:

```xml
    <opt name="make_shared_lib" value="true" export_per_member="true"/>
```

Next, we want Translator to add some commands to the output CMakeLists.txt file. We do this by adding a \<cmake_commands\> element containing raw CMake commands to the configuration file.

```xml
    <cmake_commands>
       <![CDATA[
```

The following commands set the output directory for the library’s binary by setting the corresponding properties on the target ${PROJECT_NAME}:

```txt
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY   "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
      set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here, \${PROJECT_NAME} is the name of the CMake project, which is equal to the name of the main CMake target.

Because on DLL platforms (i.e., Windows), CMake considers a DLL shared library to be an executable entity, while on non-DLL platforms (i.e., Linux), CMake considers a shared object to be a library, we set both the RUNTIME_OUTPUT_DIRECTORY and LIBRARY_OUTPUT_DIRECTORY properties.

Then the <cmake_commands> element is closed:

```xml
      ]]>    
    </cmake_commands>
```

Last but not least, we need to tell Translator that the CommonLibrary project depends on the BaseLibrary library. We do this using the \<lib\> element:

```xml
    <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
      <cmake_link_template>
        <![CDATA[
          find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
          target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
        ]]>
       </cmake_link_template>
    </lib>
```

Here, \${PROJECT_NAME}_dependencies is the name of the CMake interface library target defined in the output CMakeLists.txt file and linked to the main target, \${PROJECT_NAME}. Thus, libraries linked to \${PROJECT_NAME}_dependencies are automatically linked to the \${PROJECT_NAME} target.

With the C# project and configuration file ready, we can convert the project.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\ComplexConsoleApp\CommonLibrary\CommonLibrary.translator.config C:\ComplexConsoleApp\CommonLibrary\CommonLibrary.csproj C:\output
>cd C:\output\CommonLibrary.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

When the build finishes, the *C:\output\bin\Release* directory should contain the *CommonLibrary.Cpp.dll* file, which has just been built from the C++ sources.

## Translating LibraryA ##

The third project in this example is located in the *LibraryA* directory. LibraryA is a library project consisting of a single .cs source file, *ClassAImpl.cs*, and a project file, *LibraryA.csproj*. This project depends on two previously translated projects, BaseLibrary and CommonLibrary. These dependencies have to be reflected in the LibraryA project's configuration file. In our example, this configuration file is pre-created, its name is *LibraryA.translator.config*, and it is located in the project’s *LibraryA* directory. Let’s have a closer look at this file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
  
  <import config="../../output/BaseLibrary.Cpp/include_map.config" />
  <import config="../../output/CommonLibrary.Cpp/include_map.config" />

  <lib name="CommonLibrary.Cpp" csname="CommonLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(CommonLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
  <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
</porter>
```

Because LibraryA has dependencies on the BaseLibrary and CommonLibrary projects, we need to import the include_map.config files from both translated projects. The files are included in LibraryA.translator.config as follows:

```xml
    <import config="../../output/BaseLibrary.Cpp/include_map.config" />
    <import config="../../output/CommonLibrary.Cpp/include_map.config" />
```

Here *../../output* is a directory that was passed as an output directory to Translator when BaseLibrary and CommonLibrary projects were translated.

Again, we need to tell Translator that the LibraryA project depends on the BaseLibrary and CommonLibrary libraries. We do this using the \<lib\> element:

```xml
    <lib name="CommonLibrary.Cpp" csname="CommonLibrary">
       <cmake_link_template>
         <![CDATA[
           find_package(CommonLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibrary.Cpp" NO_DEFAULT_PATH)
           target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLibrary.Cpp)
         ]]>
      </cmake_link_template>
    </lib>
    <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
       <cmake_link_template>
         <![CDATA[
           find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
           target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
         ]]>
      </cmake_link_template>
    </lib>
```

With the C# project and configuration file ready, we can convert the project.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\ComplexConsoleApp\LibraryA\LibraryA.translator.config C:\ComplexConsoleApp\LibraryA\LibraryA.csproj C:\output
>cd C:\output\LibraryA.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

## Translating LibraryB ##

The fourth project in this example is located in the *LibraryB* directory. LibraryB is a library project consisting of a single .cs source file, *ClassBImpl.cs*, and a project file, *LibraryB.csproj*. This project depends on two previously translated projects, BaseLibrary and CommonLibrary. These dependencies have to be reflected in the LibraryB project's configuration file. In our example, this configuration file is pre-created, its name is *LibraryB.translator.config*, and it is located in the project’s *LibraryB* directory. Let us have a closer look at the LibraryB configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>
  <import config="../../output/BaseLibrary.Cpp/include_map.config" />
  <import config="../../output/CommonLibrary.Cpp/include_map.config" />
  
  <opt name="make_shared_lib" value="true" export_per_member="true"/>
  
  <cmake_commands>
    <![CDATA[
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
      set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
    ]]>
  </cmake_commands>
  
  <lib name="CommonLibrary.Cpp" csname="CommonLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(CommonLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
  <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
</porter>
```

LibraryB’s configuration file is much the same as the LibraryA configuration file described above. One difference is that, because this example assumes that LibraryB should be built as a shared dynamic library, this must be explicitly stated in the configuration file:

```xml
    <opt name="make_shared_lib" value="true" export_per_member="true"/>
```

Another difference is that we want to set the output directory for the library binary file, just as we did in the CommonLibrary configuration file described above:

```xml
    <cmake_commands>
      <![CDATA[
        set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
        set_target_properties(${PROJECT_NAME} PROPERTIES LIBRARY_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
      ]]>
    </cmake_commands>
```

With the C# project and configuration file ready, we can convert the project.

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\ComplexConsoleApp\LibraryB\LibraryB.translator.config C:\ComplexConsoleApp\LibraryB\LibraryB.csproj C:\output
>cd C:\output\LibraryB.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

When the build finishes, the *C:\output\bin\Release* directory should contain the newly built *LibraryB.Cpp.dll* file, along with the previously built *CommonLibrary.Cpp.dll* file.

## Translating ComplexConsoleApp ##

The last project in this example is located in *ComplexConsoleApp* directory. ComplexConsoleApp is an executable project that consists of a single .cs source file *Program.cs* and a project file *ComplexConsoleApp.csproj*. This project has a dependency on all four previously translated projects – BaseLibrary, CommonLibrary, LibraryA and LibraryB. These dependencies have to be reflected in the ComplexConsoleApp project's configuration file. In our example this configuration file is pre-created, its name is *ComplexConsoleApp.translator.config* and it is located in the project’s directory *ComplexConsoleApp*. Let us have a closer look at the configuration file.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
  <import config="translator.config"/>

  <import config="../../output/LibraryA.Cpp/include_map.config" />
  <import config="../../output/LibraryB.Cpp/include_map.config" />
  <import config="../../output/BaseLibrary.Cpp/include_map.config" />
  <import config="../../output/CommonLibrary.Cpp/include_map.config" />

  <cmake_commands>
      <![CDATA[
      set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
      ]]>
  </cmake_commands>
  
  <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
  <lib name="CommonLibrary.Cpp" csname="CommonLibrary">
    <cmake_link_template>
            <![CDATA[
            find_package(CommonLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibrary.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLibrary.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
  <lib name="LibraryA.Cpp" csname="LibraryA">
    <cmake_link_template>
            <![CDATA[
            find_package(LibraryA.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../LibraryA.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE LibraryA.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
  <lib name="LibraryB.Cpp" csname="LibraryB">
    <cmake_link_template>
            <![CDATA[
            find_package(LibraryB.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../LibraryB.Cpp" NO_DEFAULT_PATH)
            target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE LibraryB.Cpp)
            ]]>
    </cmake_link_template>
  </lib>
</porter>
```

We import the include_map.config files from all four previously translated library projects:

```xml
    <import config="../../output/LibraryA.Cpp/include_map.config" />
    <import config="../../output/LibraryB.Cpp/include_map.config" />
    <import config="../../output/BaseLibrary.Cpp/include_map.config" />
    <import config="../../output/CommonLibrary.Cpp/include_map.config" />
```

Next, we want Translator to add some commands to the output CMakeLists.txt. We do this by adding a \<cmake_commands\> element containing raw CMake commands to the configuration file.

```xml
     <cmake_commands>
      <![CDATA[   
```

The first command sets the output directory for the executable binary by setting the corresponding property on the target \${PROJECT_NAME}

```xml
    set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")
```

Here, \${PROJECT_NAME} is the name of the CMake project, which is equal to the name of the main CMake executable target.

Then the \<cmake_commands> element is closed:

```xml
      ]]>
     </cmake_commands>
```

Then, we need to tell Translator that the ComplexConsoleApp project depends on four libraries. We do this using the \<lib\> element:

```xml
    <lib name="BaseLibrary.Cpp" csname="BaseLibrary">
      <cmake_link_template>
        <![CDATA[
          find_package(BaseLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../BaseLibrary.Cpp" NO_DEFAULT_PATH)
          target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE BaseLibrary.Cpp)
        ]]>
     </cmake_link_template>
    </lib>

    <lib name="CommonLibrary.Cpp" csname="CommonLibrary">
       <cmake_link_template>
         <![CDATA[
           find_package(CommonLibrary.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../CommonLibrary.Cpp" NO_DEFAULT_PATH)
           target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE CommonLibrary.Cpp)
         ]]>
      </cmake_link_template>
    </lib>

    <lib name="LibraryA.Cpp" csname="LibraryA">
      <cmake_link_template>
        <![CDATA[
          find_package(LibraryA.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../LibraryA.Cpp" NO_DEFAULT_PATH)
          target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE LibraryA.Cpp)           
        ]]>
      </cmake_link_template>
    </lib>

    <lib name="LibraryB.Cpp" csname="LibraryB">
       <cmake_link_template>
        <![CDATA[
          find_package(LibraryB.Cpp REQUIRED CONFIG PATHS "${CMAKE_CURRENT_SOURCE_DIR}/../LibraryB.Cpp" NO_DEFAULT_PATH)
          target_link_libraries(${PROJECT_NAME}_dependencies INTERFACE LibraryB.Cpp)
        ]]>
      </cmake_link_template>
    </lib>
```

Here, \${PROJECT_NAME}_dependencies is the name of the CMake interface library target defined in the output CMakeLists.txt file and linked to the main executable target, \${PROJECT_NAME}. Thus, libraries linked to \${PROJECT_NAME}_dependencies are automatically linked to the \${PROJECT_NAME} target.

With the C# project and configuration file ready, we can convert the project. In order to convert the ComplexConsoleApp project, we open CMD and navigate to the directory containing the translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe -c C:\ComplexConsoleApp\ComplexConsoleApp\ComplexConsoleApp.translator.config C:\ComplexConsoleApp\ComplexConsoleApp\ComplexConsoleApp.csproj C:\output
>cd C:\output\ComplexConsoleApp.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

## Expected output ##

When the build finishes, the *C:\output\bin\Release* directory should contain four files: *CommonLibrary.Cpp.dll*, *LibraryB.Cpp.dll*, *ComplexConsoleApp.Cpp.exe*, which has just been built from the C++ sources, and *codeporting.translator.cs2cpp.framework_vc14x64.dll*, which was copied from the Translator installation directory during a post-build step. When we run *ComplexConsoleApp.Cpp.exe*, its output in the console window should be similar to the output of the original C# application project we translated.
