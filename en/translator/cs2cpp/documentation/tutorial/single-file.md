---
navTitle: "Single File"
---

# Translating a Single C# File #

This example demonstrates how to translate a single C# file into a C++ project. We’ll use a pre-existing file from the [SingleFile example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SingleFile).

> Note that this and all other examples are based on several [assumptions](index.md#base-assumptions).

**SingleFile** is a simple "Hello, world!" application that consists of a single source file, *SingleFile.cs*.

## Running the translator ##

To translate the **SingleFile** source file, we open **CMD** and navigate to the directory containing the translator binary:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
```

Then we run Translator:

```cmd
>CodeTranslator.Cs2Cpp.Console.exe C:\SingleFile\SingleFile.cs C:\output
```

Translator will print translation logs to the console window. When translation finishes, the *C:\output* directory will contain a directory named *SingleFile.Cpp*, containing the generated C++ source files and CMake configuration files.

> Note that, by default, Translator converts even a single C# source file into an entire C++ project containing sources, headers, and makefiles, rather than just an `.h`/`.cpp` pair.

## Making and building of resulting C++ code ##

Now we use CMake to generate makefiles or project files. In this example, we generate a Visual Studio 2022 project file. In CMD, we navigate to the *C:\output\SingleFile.Cpp* directory.

```cmd
>cd C:\output\SingleFile.Cpp
```

Then we run CMake in configuration mode:

```cmd
>CMake -G "Visual Studio 17 2022" .
```

We can now build the sources using either CMake or Visual Studio. Let us use CMake:

```cmd
>CMake --build . --config Release
```

## Expected output ##

When the build finishes, the *C:\output\bin\Release* directory should contain two files: *SingleFile.Cpp.exe*, which has just been built from the C++ sources, and *codeporting.translator.cs2cpp.framework_vc14x64.dll*, which was copied from the Translator installation directory during a post-build step. When we run *SingleFile.Cpp.exe*, its output in the console window should be similar to the output of the original C# application we translated:

```txt
Hello, World!
```
