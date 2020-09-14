---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "How to Use Command line to port and build Projects"
linktitle: "How to Use Command line to port and build Projects"
menu:
  docs:
    parent: "How to use CodePorting.Native Cs2Cpp"
    weight: "1"    
lastmod: "2019-06-27"
weight: "1"
---

## Overview ##

CodePorting.Native Cs2Cpp provides a command-line option to convert C# code into equivalent C++ code. The input for the CodePorting.Native Cs2Cpp is a C# project and a configuration file that contains options governing different aspects of the conversion process. The output of CodePorting.Native Cs2Cpp a set of .cpp and .h files and a set of Cmake configuration files that can be used to generate project files and makefiles and build the sources for one of the supported platforms/compilers.

Conversion is performed on a per-project basis, thus in order to convert a multi-project C# solution each project has to be converted separately. If the C# project to be converted has a dependency on another C# project, the dependent-upon project has to be converted first. Then, when converting the dependent project, the information about its dependencies has to be specified in the configuration file that is passed to the Porter along with the project. The general rule is that projects should be converted in the order from the least dependent to the most dependent.

Porter comes with a default configuration file, which contains default values for all options. Porter uses default configuration file if user does not specify an alternative configuration file. Usually default configuration file is a good option for the simplest projects that have no dependencies. However, for more complex projects and/or projects that depend on other projects, configuration file has to be created and passed to the Porter. 

Porter executable //CsToCppPorter.exe// file for command-line auto porting option is located in //bin/porter// directory in Porter installation directory.

Usage: CsToCppPorter.exe <options> <project> <output_dir>

**<options>** Denotes a list of options that control different aspects of conversion process.

**<project>** A path to the .csproj file of the C# project to be converted. This can be a full path or a path relative to the current directory.

**<output_dir>** A path to the directory where Porter will put the generated files. This can be a full path or a path relative to the current directory. If the specified directory does not exist Porter will create it.

```
    >CsToCppPorter.exe -c C:\SimpleConsoleApp\SimpleConsoleApp.porter.config C:\SimpleConsoleApp\SimpleConsoleApp.csproj C:\output
```

## Supported Options ##

**-h, ~-~-h, /h, /?** Prints short instruction of how to use Porter from command line and a list of command line options with short description.

**-q** Prevents output of percent values indicating the conversion progress to the console window.

**-c <path>** Specifies a configuration file containing the conversion options to be applied when converting the C# project. **<path>** is a path to the configuration file; this can be a full path or a path relative to the current directory. If this option is not specified Porter will use default configuration file //porter.config// located in the Porter’s home directory.

**-H <path>** Sets a Porter home directory used by Porter when resolving paths. Normally this option should not be used.

**-a** Instructs Porter to run in Analysis mode, which means that Porter processes the input project and configuration file, generates warning and error messages (if any) just as in normal mode, but does not generate the output files. This may save some time during debugging of the C# project to be converted or a configuration file.

**-g <configuration>** Specifies a name of the configuration (e.g. Debug, Release etc.) defined in input C# project to be used by Porter. In order to properly convert the project, Porter needs to know project settings specified under particular configuration. This option tells Porter which configuration to read the settings from. The input C# project must have at least one configuration defined, otherwise conversion fails. If this option is missing, a configuration with name “Debug” is used by default.

**-w0** Specifies that the Porter should always return 0 despite the number of errors or warnings that occurred during porting. This behavior is default. This option is inconsistent with **-w1** and **-w2**.

**-w1** Specifies that the Porter should return 1 in case if an error (but not warning) occurs during porting. If no errors occurred during porting the return value will be 0. This option is inconsistent with **-w0 **and **-w2**.

**-w2** Specifies that the Porter should return 1 in case if an error or warning occurs during porting. If neither errors nor warnings occurred during porting, the return value will be 0. This option is inconsistent with **-w0** and **-w1**.

**-d0** Specifies that all pre-processor directives encountered in C# code should not be inserted in the output C++ code as comments. Also see option **-d1**. This option is inconsistent with **-d1**.

**-d1** Specifies that all pre-processor directives encountered in C# code should be inserted to the resulting C++ code as comments. This may be helpful when debugging the conversion. This behavior is default. This option is inconsistent with **-d0**.

**-m <value>** Specifies if Porter should use a single thread (when <value> # false) or multiple threads (when <value> # true) when porting a project. If this option is not specified, the default value ‘true’ is used. When the option to use multiple threads is specified, Porter will use up to as many threads as the number of CPUs.

**-d <name>** Defines a variable with the specified name which can be referred to in <if> node in Porter configuration file. Refer to Porter configuration documentation for further details.

**-st** Instructs Porter to generate only Cmake configuration files as its output without C++ sources.

**-o <name>#<value>** Assigns a value <value> to the Porter configuration option named <name>. A value specified by this option takes precedence over the value specified in the input configuration file.

**-O** Instructs Porter to put the output files and directories directly to the directory specified by **<output_dir>** command line argument. By default if this option is omitted, Porter will create a sub-directory under **<output_dir>** named **<project_name>_Cpp** (where **<project_name>** is the name of the input C# project) and will put the output files and directories in it.
