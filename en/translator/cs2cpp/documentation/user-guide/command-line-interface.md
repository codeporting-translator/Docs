# Command Line Interface Reference #

## Overview ##

CodePorting.Translator Cs2Cpp provides a command-line option to convert C# code into equivalent C++ code. The input for the CodePorting.Translator Cs2Cpp is a C# project and a configuration file that contains options governing different aspects of the conversion process. The output of CodePorting.Translator Cs2Cpp a set of .cpp and .h files and a set of Cmake configuration files that can be used to generate project files and makefiles and build the sources for one of the supported platforms/compilers.

Translator comes with a default configuration file, which contains default values for all options. Translator uses default configuration file if user does not specify an alternative configuration file. Usually default configuration file is a good option for the simplest projects that have no dependencies. However, for more complex projects and/or projects that depend on other projects, configuration file has to be created and passed to the Translator.

Translator executable *CodeTranslator.Cs2Cpp.Console.exe* file for command-line auto translating option is located in *bin/code_translator* directory in Translator installation directory.

> Usage: CodeTranslator.Cs2Cpp.Console.exe [project] [output_dir] [options]

**project** A path to the .csproj file of the C# project to be converted. This can be a full path or a path relative to the current directory. Single files (\*.cs) and entire solutions (\*.sln) are supported too. When 'project' option is not specified in command line, it can be specified in configuration file.

**output_dir** A path to the directory where Translator will put the generated files. This can be a full path or a path relative to the current directory. If the specified directory does not exist Translator will create it. When 'output_dir' option is not specified in command line, it can be specified in configuration file.

**options** Denotes a list of options that control different aspects of conversion process.

> Example: CodeTranslator.Cs2Cpp.Console.exe C:\SimpleConsoleApp\SimpleConsoleApp.csproj C:\output -c C:\SimpleConsoleApp\SimpleConsoleApp.translator.config

## Supported Options ##

**-h, --help, /h, /?** Prints short instruction of how to use Translator from command line and a list of command line options with short description.

**-c \<path\>** Specifies a configuration file containing the conversion options to be applied when converting the C# project. 'path' is a path to the configuration file; this can be a full path or a path relative to the current directory. If this option is not specified Translator will use default configuration file *translator.config* located in the Translator’s home directory.

**-ct** Traces config hierarchy being loaded to log.

**-H \<path\>** Sets a Translator home directory used by Translator when resolving paths. Normally this option should not be used.

**-g \<configuration\>** Specifies a name of the configuration (e.g. Debug, Release etc.) defined in input C# project to be used by Translator. In order to properly convert the project, Translator needs to know project settings specified under particular configuration. This option tells Translator which configuration to read the settings from. The input C# project must have at least one configuration defined, otherwise conversion fails. If this option is missing, a configuration with name “Debug” is used by default.

**-w0** Specifies that the Translator should always return 0 despite the number of errors or warnings that occurred during translating. This option is inconsistent with **-w1** and **-w2**.

**-w1** Specifies that the Translator should return 1 in case if an error (but not warning) occurs during translating. If no errors occurred during translating the return value will be 0. This behavior is default. This option is inconsistent with **-w0** and **-w2**.

**-w2** Specifies that the Translator should return 1 in case if an error or warning occurs during translating. If neither errors nor warnings occurred during translating, the return value will be 0. This option is inconsistent with **-w0** and **-w1**.

**-d0** Specifies that all pre-processor directives encountered in C# code should not be inserted in the output C++ code as comments. Also see option **-d1**. This option is inconsistent with **-d1**.

**-d1** Specifies that all pre-processor directives encountered in C# code should be inserted to the resulting C++ code as comments. This may be helpful when debugging the conversion. This behavior is default. This option is inconsistent with **-d0**.

**-m \<true|false|N\>** Specifies if Translator should use a single thread (when 'value' is `false`) or multiple threads (maximum available when 'value' `true` or specified `N` number) when translating a project. If this option is not specified, the default value `true` is used.

**-d \<name\>** Defines a variable with the specified name which can be referred to in \<if\> node in Translator configuration file. Refer to Translator configuration documentation for further details.

**-o \<name\>=\<value\>** Assigns a value 'value' to the Translator configuration option named 'name'. A value specified by this option takes precedence over the value specified in the input configuration file.

**-ot \<name\>** trace config option named 'name' value to log (allows wildcards).
> Example: -ot force_* will trace to log all options values whose names start from 'force_'

**-O** Instructs Translator to put the output files and directories directly to the directory specified by **<output_dir>** command line argument. By default if this option is omitted, Translator will create a sub-directory under **<output_dir>** named **<project_name>_Cpp** (where **<project_name>** is the name of the input C# project) and will put the output files and directories in it.

**-ts** Skips translation phase, only load configuration and log if needed.

## Obsolete Options ##

Options **-q**, **-a**, **-st** and **-lf** are obsolete and will be ignored by Translator.
