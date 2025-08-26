## Getting started with command line application (Windows)

### Download release archive
[Download latest release here](https://products.codeporting.com/translator/csharp-to-java/release)

### Dependencies
The translator requires the Visual Studio Build Tools to function properly. You can install either Visual Studio (2022, 2019, or 2017) or the Build Tools separately.  
To download Visual Studio, please visit: [Download Visual Studio](https://visualstudio.microsoft.com/ru/downloads/)

### Extracting archive
Release archive contains two folders : `bin/console` and `bin/gui`.
For this tutorial only `bin/console` folder is required.
1. Extract that folder somewhere (`C:/translator_tutorial`, for example). It must be `C:/translator_tutorial/bin/console` after extraction.
2. Copy your license file to `C:/translator_tutorial` so that it is `C:/translator_tutorial/CodePorting.Translator.Cs2Java.lic`.
3. Open your command line shell in the `C:/translator_tutorial` directory.

### Downloading example project with GIT (optional)
Run command
```
git clone https://github.com/codeporting-translator/codeporting-translator-cs2java
```
After that, directory `C:/translator_tutorial/codeporting-translator-cs2java` must appear.
### Running example project translation
To translate C# project, you must specify such arguments, as **workflow** (-wf), **source path** (-s) and **destination path** (-d) :
```
bin/console/CodePorting.Applications.Console.exe -wf <workflow-id> -s <source-path> -d <destination-path>
```
In order to translate example project, run following commands :
```
mkdir result_java
bin/console/CodePorting.Applications.Console.exe -wf "{634C1942-3E89-4312-9891-8A0671B4D7F1}" -s "codeporting-translator-cs2java/ExampleProjects/TranslatorTestProject/TranslatorTestProject.sln" -d "result_java"
```
When translation is over, result java files will be located at the path `C:/translator_tutorial/result_java`

The translated Java code requires the Aspose.JCL library to function correctly. For instructions on how to add the CodePorting.Translator JCL to your Maven project, please refer to the following guide: [Adding CodePorting.Translator JCL to the Maven project](../jcl/adding_jcl_to_maven_project.md)