---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Translating Simple NUnit Test"
linktitle: "Translating Simple NUnit Test"
menu:
  docs:
    parent: "Translating Simple C# Projects"
    weight: "3"
lastmod: "2019-05-28"
weight: "3"
---

&nbsp;
{{<note>}}
Note that this example is built upon several assumptions, namely:
<ul>
<li>
Translator is installed to C:\CodePorting.Translator_Cs2Cpp_19.4 directory
</li>
<li>
A sample C# project is located in C:\SimpleNUnitTest directory
</li>
<li>
The output directory for the project is C:\output
</li>
</ul>
{{</note>}}

## Translating a simple NUnit test project ##

This example demonstrates how to translate a C# NUnit test project. We’ll use pre-existing project from **SimpleNUnitTest** example located [here](https://github.com/codeporting-translator/codeporting-translator-cs2cpp).

SimpleNUnitTest is a library project that contains NUnit tests. Translator translates C# NUnit library projects into C++ executable projects. **SimpleNUnitTest** project consists of a single .cs source file ``SimpleNUnitTest.cs`` and a project file ``SimpleNUnitTest.csproj``. This project does not have any dependencies on other C# projects. **SimpleNUnetTest** project's configuration file is pre-created, its name is ``SimpleNUnitTest.translator.config`` and it is located in the project’s directory **``SimpleNUnitTest``**. Let us have a closer look at the configuration file.

{{< highlight xml >}}
SimpleNUnitTest.translator.config begins with an XML declaration, which specifies that the file contains an XML document    

Then goes the XML root element <translator> which is mandatory for Translator configuration XML document  

    <translator>  

Next, the default Translator configuration file is imported using <import> element. The default configuration will assign default values to all configuration options  

    <import config="translator.config"/>  

Next, we want Translator to add some commands to the output CMakeLists.txt. We do that by adding <cmake_commands> element to the configuration file containing raw Cmake commands  

    <cmake_commands>  
      <![CDATA[    

The first command sets the output directory for the executable binary by setting the corresponding property on the target ${PROJECT_NAME}_gtest  

      set_target_properties(${PROJECT_NAME}_gtest PROPERTIES RUNTIME_OUTPUT_DIRECTORY "${CMAKE_CURRENT_SOURCE_DIR}/../bin")  

Here ${PROJECT_NAME} is the name of the Cmake project and ${PROJECT_NAME}_gtest is the name of the main Cmake executable target.  

Then the <cmake_commands> element is closed  

      ]]>  
   </cmake_commands>  

Finally the XML document is finished with closing tag of the root element <translator>  

    </translator>
{{< /highlight >}}

With the C# project at hand and configuration file ready, we can convert the project.

In order to covert SimpleNUnitTest project we run CMD and navigate to the directory with translator binary:

```
  >cd C:\CodePorting.Translator_Cs2Cpp_19.4\bin\translator
```

And run Translator:

```
 >CodePorting.Translator.Cs2Cpp.exe -c C:\SimpleNUnitTest\SimpleNUnitTest.translator.config C:\SimpleNUnitTest\SimpleNUnitTest.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory ``C:\output`` will contain a directory named Simple``NUnitTest.Cpp`` containing the generated C++ source files and Cmake configuration files.

Now we want to use Cmake to generate makefile/project files. Let it be a Visual Studio 2017 x86 project file. In CMD we navigate to the ``C:\output\SimpleNUnitTest.Cpp`` directory

```
 >cd C:\output\SimpleNUnitTest.Cpp
```

And run Cmake in configuration mode:

```
  >Cmake --G "Visual Studio 15 2017"
```

And now we can build the sources using either Cmake or Visual Studio. Let us use Cmake:

```
>Cmake --build . --config Release
```

When build finishes, directory ``D:\output\bin\Release`` should contain two files: ``SimpleNUnitTest.Cpp_gtest.exe``, which has just been built from sources, and ``aspose_cpp_vc140.dll``, which was copied from Translator installation directory during a post-build step. When we run ``SimpleNUnitTest.Cpp_gtest.exe`` it executes tests and prints results to the console window. The tests ``SimpleNUnitTest.Cpp_gtest.exe`` executes are similar to those in original C# project.
