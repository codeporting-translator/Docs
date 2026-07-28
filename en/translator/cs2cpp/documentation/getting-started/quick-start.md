# Quick start #

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

For more complex scenarios, refer to the developer guide.
