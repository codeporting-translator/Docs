# CodePorting.Translator Extension for Visual Studio(VSIX)
The VSIX extension for Visual Studio is referred to the main translator application provided as part of the CodePorting.Translator Cs2Java product.

The rest of the product's applications, also used to translate C# to Java code, are designed to provide complete user interfaces and provide, with minor modifications, the same functionality.

Launch Visual Studio 2019. Working with extension commands will be available from the menu `Extensions > CodePorting` (for Visual Studio 2019 IDE):

![Installed](../../Images/vsix-installed-vs2019-en.png)

## The full application menu contains the following functions.



|**Tab**|**Description**|
| - | - |
|Analyze|Run code analysis|
|Port|Run code translation|
|Cancel|Cancellation of a running but not yet completed process.|
|Option|Configuration options. Configuring the destination directory.|
|Progress|Display progress bar of code translation.|
|About|Show details on the current implementation of the program: version, license, brief description, etc.|

**Notes:**\
In Visual Studio 2017 and 2019, the extension control is placed in different menu items.\
VS 2017:`Menu > CodePorting`\
VS 2019:`Menu > Extensions > CodePorting`
