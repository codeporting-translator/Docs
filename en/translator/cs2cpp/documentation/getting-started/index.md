---
order: "1"
navTitle: "Getting started"
---

# Quick start #

> Note! If you already have experience working with the old translator and want to learn about the new features and differences of the current Roslyn-based translator, you can immediately follow the link to the [Next Gen](next-gen/index.md) section.

To translate a single C# file, a project, or an entire solution, follow these steps:

1. [Install](installation.md) CodePorting Translator Cs2Cpp on your computer.
1. Open the installation folder and navigate to the "bin\code_translator" subfolder.
1. Run the translator from the console:
   "CodeTranslator.Cs2Cpp.Console.exe <full_path_to_your_cs_project.cs> <full_path_to_translated_cpp_project>"
1. Review the console output. Errors are shown in red and warnings in yellow. Make sure there are no errors and no unexpected warnings. If any appear, fix the underlying issue and run the translation again.
1. Go to the translated project folder and make it, for example, with "cmake -B ./".
1. Open the generated solution file and build the project. Fix any build errors and rerun the translator if necessary.
1. Run the built application. Fix any runtime errors.
1. Enjoy your translated C# to C++ application.

Conversion is performed on a per-project basis, thus in order to convert a multi-project C# solution each project has to be converted separately. If the C# project to be converted has a dependency on another C# project, the dependent-upon project has to be converted first. Then, when converting the dependent project, the information about its dependencies has to be specified in the configuration file that is passed to the Translator along with the project. The general rule is that projects should be converted in the order from the least dependent to the most dependent.

For more complex scenarios, refer to the [user guide](../user-guide/index.md).
