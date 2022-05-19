---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Translating Simple Library"
linktitle: "Translating Simple Library"
menu:
  docs:
    parent: "Translating Simple C# Projects"
    weight: "2"
lastmod: "2019-05-28"
weight: "2"
---

&nbsp;
{{<note>}}
Note that this example is built upon several assumptions, namely:
<ul>
<li>
Translator is installed to C:\CodePorting.Translator_Cs2Cpp_19.4 directory
</li>
<li>
All C# projects are located in C:\SimpleLibrary directory
</li>
<li>
The output directory is C:\output
</li>
</ul>
{{</note>}}

## Translating a simple C# library project ##

This example demonstrates how to a simple C# library project. We’ll use pre-existing projects from **SimpleLibrary** example located [here](https://github.com/codeporting-translator/codeporting-translator-cs2cpp).

**SimpleLibrary** is a library project that consists of a single .cs source file ``SimpleLibrary.cs`` and a project file ``SimpleLibrary.csproj``. This project does not depend on any other C# projects. SimpleLibrary project's configuration file is pre-created, its name is ``SimpleLibrary.translator.config`` and it is located in the project’s directory **``SimpleLibrary``**. Let us have a closer look at the configuration file.

{{< highlight xml >}}
SimpleLibrary.translator.config file is quite simple. It begins with an XML declaration, which specifies that the file contains an XML document  

Then goes the XML root element <translator> which is mandatory for Translator configuration XML document  

    <translator>  

Next, the default Translator configuration file is imported using <import> element. The default configuration will assign default values to all configuration options  

    <import config="translator.config"/>  

Finally the XML document is finished with closing tag of the root element <translator>  

    </translator>
{{< /highlight >}}

This example assumes that C# SimpleLibrary project should be translated into C++ static library project, which is a default setting.

With the C# project at hand and configuration file ready, we can convert the project.

In order to covert **ComplexConsoleApp** project we run CMD and navigate to the directory with translator binary:

```
>cd C:\CodePorting.Translator_Cs2Cpp_19.4\bin\translator
```

And run Translator:

```
 >CodePorting.Translator.Cs2Cpp.exe -c C:\SimpleLibrary\SimpleLibrary.translator.config C:\SimpleLibrary\SimpleLibrary.csproj C:\output
```

Translator will print some logs of the translating process to the console window and when it finishes translating, directory ``C:\output`` will contain a directory named ``SimpleLibrary.Cpp`` containing the generated C++ source files and Cmake configuration files.

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
