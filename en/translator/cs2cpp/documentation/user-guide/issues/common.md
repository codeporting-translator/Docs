# Common Issues #

This section describes issues that arise during common translator processing.

[TOC]

## C01: Unsupported target extension {#c01} ##

This occurs when the target translation file is in an unsupported format. Currently, the Cs2Cpp translator supports individual C# files (\*.cs), Visual Studio C# projects (\*.csproj), and entire solutions (\*.sln).

**Default severity**: FATAL ERROR

**Example**: invalid target (C# shared project)

```txt
CodeTranslator.Cs2Cpp.Console.exe source.shproj
```

**Solution**: select one from supported target formats

```txt
CodeTranslator.Cs2Cpp.Console.exe source.csproj
```

## C10: Configuration error {#c10} ##

Reports an error while reading the .config file (whether explicitly provided or loaded by default). This may indicate the absence of the file itself, a XML-like file format error, or configuration parameters falling outside permissible limits.

**Default severity**: FATAL ERROR

**Example**: invalid configuration file format (typo in opening tag)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<proter>
    <import config="translator.config"/>
</proter>
```

**Solution**: correct the document format in accordance with the established [standard](../configuration-file/index.md)

```xml
<?xml version="1.0" encoding="utf-8" ?>
<porter>
    <import config="translator.config"/>
</porter>
```

## C11: Project generation error {#c11} ##

Reports an error in generating Makefiles for C++ projects upon completion of the source code translation process.

**Default severity**: FATAL ERROR

**Solution**: check the specific details of the problem at the end of the error message and fix it.

## C15: Internal warning {#c15} ##

Reports an internal translator issue that may not affect the translation result but is related to the occurrence of situations not anticipated by the developers.

**Default severity**: WARNING

**Solution**: ignore this message if the resulting code is correct; otherwise, rewrite the source code or contact the developers to have the issue resolved.

## C16: Internal error {#c16} ##

Reports an internal compiler error due to an unexpected situation that will almost certainly result in the generation of incorrect C++ code.

**Default severity**: ERROR

**Solution**: rewrite the source code or contact the developers to have the issue resolved.

## C20: 'ASPOSE_ROOT' environment variable is not set, please set it to 'asposecpplib' folder full path {#c20} ##

The `ASPOSE_ROOT` environment variable must be set for the translator to function correctly and for the translated code to build successfully.

**Default severity**: FATAL ERROR

**Solution**: set this environment variable to the folder where the aposecpplib library is installed.

## C21: The tools version is unrecognized {#c21} ##

The toolset required to build the C# project is not installed on your system.

**Default severity**: WARNING

**Solution**: ignore this issue if the translation completes successfully, install the necessary tools or correct the project settings.

## C30: Directory not exitsts {#c30} ##

The specified folder does not exist or cannot be accessed.

**Default severity**: ERROR

**Solution**: ensure that the folder exists and is accessible (not in use by another process), or modify the project configuration to eliminate the need for it.

## C31: File not exitsts {#c31} ##

The specified file does not exist or cannot be accessed.

**Default severity**: ERROR

**Solution**: ensure that the file exists and is accessible (not in use by another process), or modify the project configuration to eliminate the need for it.

## C32: Failed to copy file {#c32} ##

The specified file cannot be copied to specified location.

**Default severity**: ERROR

**Solution**: check the specific details of the problem at the end of the error message and fix it.

## C33: Failed to read file {#c33} ##

The specified file cannot be read.

**Default severity**: ERROR

**Solution**: check the specific details of the problem at the end of the error message and fix it.

## C40: Detected failure(s) while loading the workspace {#c40} ##

The translator loads projects in the same way Visual Studio does. If errors occur during the project loading process, this can lead to incorrect configuration of the translated code, missing files, and so on. You can choose to ignore this issue, but please keep this warning in mind should such defects arise.

**Default severity**: WARNING

**Solution**: Check the specified log file (*translatorSourceErrors.log* located in the folder where the translation output will be written) and resolve the indicated errors.

## C41: Detected error(s) during project(s) compilation {#c41} ##

The translator compiles the translated project just as Visual Studio does during a build. This message indicates that compilation errors occurred during the build process (warnings are not included). Typically, such errors prevent the derivation of correct semantics, leading to incorrect translation (or a failure to translate) parts of the source code. However, this may also indicate an incorrect project configuration, errors in conditional compilation branches, and so on. You are free to ignore these errors; however, should specific semantic errors (such as [T0150](translation.md#t0150)) arise during translation, their root cause will most likely lie here.

**Default severity**: WARNING

**Solution**: Check the specified log file (*translatorSourceErrors.log* located in the folder where the translation output will be written) and resolve the indicated errors.

## C45: Detected unsued and/or obsolete attribute(s) {#c45} ##

This message indicates that, during the translation of the project, attributes were detected that have absolutely no effect on the translation result. Such attributes are divided into two groups: unused and obsolete.

**Unused** attributes are those physically present in the code or configuration whose existence was never queried and which did not affect the compilation process. The most common scenario is when an attribute is applied via a .config file to a non-existent symbol (for example, a method that is missing from the specified class, etc.) Read more about this in the section on [configuration attributes](../configuration-file/attributes.md).

**Obsolete** attributes are attributes that were significant in earlier versions of the translator but are now obsolete and no longer supported (for example, because the functionality they provided is now handled automatically, without the need to include them). You can find a list of such attributes on the [deprecated attributes page](../cpp-attributes/obsolete.md).

**Default severity**: WARNING

**Solution**: Check the specified log file (*unusedAttributes.log* located in the folder where the translation output will be written) and remove these attributes or fix their location.

## C50: Unknown attribute is used {#c50} ##

Indicates that the code or configuration uses an attribute that resembles a compiler-specific attribute but lacks a definition or is not described within the compiler itself. Such an attribute will be ignored; however, this likely signals a typo in the configuration.

**Default severity**: WARNING

**Example**: typo in attribute name

```cs
[CodePorting.Translator.Cs2Cpp.RenameEntity("Foo")]
void Bar()
{
}
```

**Solution**: fix attribute name

```cs
[CodePorting.Translator.Cs2Cpp.CppRenameEntity("Foo")]
void Bar()
{
}
```

## C51: There is an attribute applied to method of type through \<attribute\> tag in config file, but this type actually doesn't contain such member {#c51} ##

It reports that the method to which the attribute is to be added via the configuration file does not exist. The error may lie not only in the method name but also in the parameters or the return type.

**Default severity**: WARNING

**Example**: invalid return type (interface method `ICollection.Add` dosen't return anything)

```xml
<attribute name="CppArgumentKind" method="bool Add(?)" interface="System.Collections.Generic.ICollection" parametername="*" parameterkind="ConstReference" condition="parameter"/>
```

**Solution**: fix return type

```xml
<attribute name="CppArgumentKind" method="void Add(?)" interface="System.Collections.Generic.ICollection" parametername="*" parameterkind="ConstReference" condition="parameter"/>
```

## C98: License error, the application will run in evaluation mode {#c98} ##

Indicates that the translator version requiring a license file cannot find the file or that the file contains errors (has been modified, is outdated, etc.) The application will launch in trial mode with significant limitations (please read the [license terms](../../getting-started/licensing.md)).

**Default severity**: WARNING

**Solution**: read the specific problem in the error message and fix it.

## C99: License limitations violation {#c99} ##

It reports that the limits imposed on the evaluation version of the application have been exceeded and the translation will be interrupted immediately.

**Default severity**: FATAL ERROR (cannot be overriden through severity rule)

**Solution**: obtain a license or bring the code back within the limits specified for the evaluation mode.
