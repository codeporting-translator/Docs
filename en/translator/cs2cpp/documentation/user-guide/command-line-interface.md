# Command Line Interface Reference #

CodePorting.Translator Cs2Cpp provides a command-line option for converting C# code into equivalent C++ code. The input to the translator is a C# project and a configuration file that contains options governing different aspects of the conversion process. The output is a set of .cpp and .h files, together with CMake configuration files that can be used to generate project files and makefiles and build the sources for one of the supported platforms or compilers.

The translator comes with a default configuration file that contains default values for all options. It uses this file when the user does not specify an alternative configuration file. In most cases, the default configuration file is a good choice for simple projects that have no dependencies. For more complex projects, or for projects that depend on other projects, a custom configuration file should be created and passed to the translator.

## Usage ##

The translator executable file, CodeTranslator.Cs2Cpp.Console.exe, is located in the bin/code_translator directory of the translator installation folder.It should be run using format:

```ps
CodeTranslator.Cs2Cpp.Console.exe [project] [output_dir] [options]
```

**project** A path to the .csproj file of the C# project to be converted. This can be either a full path or a path relative to the current directory. Single files (\*.cs) and entire solutions (\*.sln) are also supported. If the project option is not specified on the command line, it can be provided in the configuration file.

**output_dir** A path to the directory where the translator will place the generated files. This can be either a full path or a path relative to the current directory. If the specified directory does not exist, the translator will create it. If the output directory is not specified on the command line, it can be provided in the configuration file.

**options** A list of options that control different aspects of the conversion process.

> Example: CodeTranslator.Cs2Cpp.Console.exe C:\SimpleConsoleApp\SimpleConsoleApp.csproj C:\output -c C:\SimpleConsoleApp\SimpleConsoleApp.translator.config

## Supported Options ##

**-h, --help, /h, /?** Prints a short usage message for the command-line interface and a list of available options with brief descriptions.

**-c \<path\>** Specifies a configuration file containing the conversion options to apply. The \<path\> can be either absolute or relative to the current directory. If this option is not specified, the translator uses the default configuration file, translator.config, located in the translator home directory.

**-ct** Traces the configuration hierarchy being loaded to the log.

**-H \<path\>** Sets the translator home directory used when resolving paths. Normally, this option should not be used.

**-g \<configuration\>** Specifies the name of the configuration (for example, Debug or Release) defined in the input C# project to be used by the translator. To convert the project correctly, the translator must read the project settings defined for that configuration. The input C# project must have at least one configuration defined; otherwise, conversion fails. If this option is omitted, the configuration named "Debug" is used by default.

**-w0** Specifies that the translator should always return 0 regardless of the number of errors or warnings that occur during translation. This option is incompatible with **-w1** and **-w2**.

**-w1** Specifies that the translator should return 1 if an error occurs, but not if only a warning occurs. If no errors occur, the return value is 0. This is the default behavior. This option is incompatible with **-w0** and **-w2**.

**-w2** Specifies that the translator should return 1 if an error or warning occurs. If neither errors nor warnings occur, the return value is 0. This option is incompatible with **-w0** and **-w1**.

**-d0** Specifies that preprocessor directives encountered in the C# code should not be inserted into the output C++ code as comments. See also **-d1**. This option is incompatible with **-d1**.

**-d1** Specifies that preprocessor directives encountered in the C# code should be inserted into the resulting C++ code as comments. This may be helpful when debugging the conversion. This is the default behavior. This option is incompatible with **-d0**.

**-m <true|false|N>** Specifies whether the translator should use a single thread (when the \<value\> is `false`) or multiple threads (when the \<value\> is `true` or when a specific `N` \<value\> is provided) when translating a project. If this option is not specified, the default value is `true`.

**-d \<name\>** Defines a variable with the specified name that can be referenced from an \<if\> node in the translator configuration file. Refer to the translator configuration documentation for more details.

**-o \<name\>=\<value\>** Assigns the specified \<value\> to the translator configuration option named \<name\>. A value specified by this option takes precedence over the value specified in the input configuration file.

**-ot \<name\>** Traces the value of the configuration option named \<name\> to the log. Wildcards are supported.

> Example: -ot force_* traces all option values whose names start with "force_".

**-O** Instructs the translator to place the output files and directories directly in the directory specified by the **<output_dir>** command-line argument. By default, if this option is omitted, the translator creates a subdirectory named **<project_name>_Cpp** under **<output_dir>** and places the output files and directories there.

**-ts** Skips the translation phase and only loads the configuration and logs, if needed.

## Obsolete Options ##

The options **-q**, **-a**, **-st**, and **-lf** are obsolete and will be ignored by the translator.
