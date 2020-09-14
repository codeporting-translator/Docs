---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Porting Simple Library"
linktitle: "Porting Simple Library"
menu:
  docs:
    parent: "Porting Simple C# Projects"
    weight: "2"
lastmod: "2019-05-28"
weight: "2"
---

&nbsp;
{{<note>}}
Note that this example is built upon several assumptions, namely:
<ul>
<li>
Porter is installed to C:\CodePorting.Native_Cs2Cpp_19.4 directory
</li>
<li>
All C# projects are located in C:\SimpleLibrary directory
</li>
<li>
The output directory is C:\output
</li>
</ul>
{{</note>}}

## Porting a simple C# library project ##

This example demonstrates how to a simple C# library project. We’ll use pre-existing projects from **SimpleLibrary** example located [here](https://github.com/codeporting-native/codeporting-native-cs2cpp).

**SimpleLibrary** is a library project that consists of a single .cs source file ``SimpleLibrary.cs`` and a project file ``SimpleLibrary.csproj``. This project does not depend on any other C# projects. SimpleLibrary project's configuration file is pre-created, its name is ``SimpleLibrary.porter.config`` and it is located in the project’s directory **``SimpleLibrary``**. Let us have a closer look at the configuration file.

{{< highlight xml >}}
SimpleLibrary.porter.config file is quite simple. It begins with an XML declaration, which specifies that the file contains an XML document  

Then goes the XML root element <porter> which is mandatory for Porter configuration XML document  

    <porter>  

Next, the default Porter configuration file is imported using <import> element. The default configuration will assign default values to all configuration options  

    <import config="porter.config"/>  

Finally the XML document is finished with closing tag of the root element <porter>  

    </porter>
{{< /highlight >}}

This example assumes that C# SimpleLibrary project should be ported into C++ static library project, which is a default setting.

With the C# project at hand and configuration file ready, we can convert the project.

In order to covert **ComplexConsoleApp** project we run CMD and navigate to the directory with porter binary:

```
>cd C:\CodePorting.Native_Cs2Cpp_19.4\bin\porter
```

And run Porter:

```
 >CsToCppPorter.exe -c C:\SimpleLibrary\SimpleLibrary.porter.config C:\SimpleLibrary\SimpleLibrary.csproj C:\output
```

Porter will print some logs of the porting process to the console window and when it finishes porting, directory ``C:\output`` will contain a directory named ``SimpleLibrary.Cpp`` containing the generated C++ source files and Cmake configuration files.

Now we want to use Cmake to generate makefile/project files. Let it be a Visual Studio 2017 x86 project file. In CMD we navigate to the ``C:\output\SimpleLibrary.Cpp`` directory

```
 >cd C:\output\SimpleLibrary.Cpp
```

And run Cmake in configuration mode:

```
 >Cmake --G "Visual Studio 15 2017"  
```

And now we can build the sources using either Cmake or Visual Studio. Let us use Cmake:

```
 >Cmake --build . --config Release
```

**The library is built.**
