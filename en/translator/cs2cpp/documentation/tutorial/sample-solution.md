---
navTitle: "Sample Solution"
---

# Translating a Sample Solution #

This example demonstrates how to translate an entire solution comprising three projects: a library, a console application, and a test project. The latter two depend on the library. We will use a pre-existing project from the [SampleSolution example](https://github.com/codeporting-translator/codeporting-translator-cs2cpp/tree/master/ExampleProjects/SampleSolution).

When translating the entire solution, we do not need any special configuration files for each individual project. The translator automatically determines the dependencies and generates a single CMake project that correctly builds the entire solution on the C++ side.

> Note. If you do create a configuration file and specify it during compilation, it will apply to every project in the solution, and all of those projects will be compiled using the same settings.

## Translating SampleSolution ##

The sample solution consists of a single file, *SampleSolution.sln*, which references three projects located in subdirectories. To build it, we do not need to know the internal structure of the solution; we simply translate the .sln file itself:

```cmd
>cd C:\CodePorting.Translator_Cs2Cpp\bin\code_translator
>CodeTranslator.Cs2Cpp.Console.exe C:\SampleSolution\SampleSolution.sln C:\output
>cd C:\output\SampleSolution.Cpp
>CMake -G "Visual Studio 17 2022" .
>CMake --build . --config Release
```

That’s it. All three projects are built and ready for use.

## Expected output ##

When the build finishes, the *C:\output\SampleSolution.Cpp* directory should contain three subfolders:

* *SimpleLibrary.Cpp*; its *Release* subfolder contains the *SimpleLibrary.Cpp_vc14x64.lib* static library.
* *TestProject.Cpp*; its *Release* subfolder contains the *codeporting.translator.cs2cpp.framework_vc14x64.dll* dynamic C++ framework library and the *TestProject.Cpp_gtest.exe* test executable with all tests translated.
* *ConsoleApp.Cpp*; its *Release* subfolder contains the *codeporting.translator.cs2cpp.framework_vc14x64.dll* dynamic C++ framework library and the *ConsoleApp.Cpp.exe* executable. When you run it, its output in the console window should be similar to the output of the original C# application project we translated.

> Note. If you want both binary files to be placed in a single directory, you need to add CMake commands similar to those used in previous lessons.
