# Quick start #

To translate a single C# file, project or entire solution, you need to follow these steps:

1. Clone translator repository ([https:/git.uly.dynabic.com/dynabic/csporter/cpp/asposecpplib](https://git.uly.dynabic.com/dynabic/csporter/cpp/asposecpplib)) on your computer or update it to lastest version.
1. Make sure the **ASPOSE_ROOT** environment variable contains the path to this folder (for example, //"c:\aspose\csporter\cpp\asposecpplib"//).
1. Build the **"CodeTranslator.sln"** solution located in "**src\code_translator"** subfolder.
1. Use the executable file **"bin\code_translator\CodeTranslator.Cs2Cpp.Console.exe"** to translate the source code in C# to C++.

After the translation has completed successfully, the project should be made using CMAKE, after which the resulting solution can be built and run.
