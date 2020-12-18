---
date: "2020-06-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.5"
linktitle: "CodePorting.Native Cs2Cpp 20.5"
menu:
  docs:
    parent: "2020"
    weight: "6"
lastmod: "2020-06-11"
weight: "8"
---

## Major Features ##

1. System::Xml namespace implementation was improved and extended. This includes implementing previously unsupported types and methods and establishing the basis for document signature support.
1. Issues with Aspose products' embedded resources were fixed.

## Minor fixes ##

1. Doxygen used to build project documentation was upgraded to 1.8.18 version. Some Doxygen warnings were fixed in header files.
1. Natives' visualizers supplied for our proprietary types were improved.
1. 'Nullptr to Nullable<T>' cast was fixed.
1. Translation of Assert.Throws() was fixed. False positives are no longer generated if no exception is being thrown.
1. XslCompiledTransform::Transform(const SharedPtr<XmlReader>& input, const SharedPtr<XmlWriter>& results) was implemented.
1. 'Where' LINQ extension method was supported by the porter.
1. GetType() was fixed for boxed enums.
1. File::ReadAllText() now detects UTF-8 encoding automatically.
1. A bug was fixed in porter placing output files above the project root if corresponding input files are placed above project root.
1. 'ASPOSE_CONST' legacy qualifier was removed by replacing it with a normal 'const' qualifier. Porter is no longer capable of generating it.
1. [CppConstWrapper](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstWrapper) attribute no longer requires [CppNoConstMethod ](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstMethod-1)one to be placed to the same method, as the same behavior is now supported implicitly.
1. Failures when loading inconsistent XML via XmlDocument::Load was fixed.
1. Empty comments skipping was fixed in porter.
1. The emplaced resource is no longer visible to other builds when the emplace_assembly_details option is enabled.
1. String::Substring() method now performs correct boundary checks the same way .Net does.
1. Assert.AreEqual() translation was extended to match the NUnit behavior when comparing streams (streams' contents get compared instead of pointers).
1. 64-bit long file sizes are now extracted properly.
1. Invariant cutlure is now supported by String::LastIndexOf().
1. DateTime::Parse() now throws same exceptions .Net does.
1. Relative paths are now generated for embedded resources instead of absolute ones.
1. Reflection support was improved.
1. GCC compilation issues with has_operator_equal template were fixed.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|PDFCPP-1248|Change for_each_member to generate weak links|Task
|EMAILCPP-218|Fix nullptr cast to Nullable|Bug
|SLIDESCPP-2349|Porter: Improve Assert.Throws() method translation|Improvement
|PDFCPP-1264|Fix XslCompiledTransform to support XmlReader.|Task
|PDFCPP-1269|implement Linq Where|Task
|PDFCPP-1271|fix Asposecpplib - GetType() for boxed enums works incorrectly|Bug
|PDFCPP-1272|Fix Asposecpplib - IO.File.ReadAllText() does not detect utf8 encoding automatically|Bug
|PDFCPP-1273|Fix CsToCppPorter - *.cs files started with "\.." are being written to incorrect folder|Improvement
|EMAILCPP-220|Prepearing Aspose.Email for C++ Release 20.4|Task
|TASKSCPP-1392|Implement dependent classes for reading XML content as DateTime|Task
|TASKSCPP-1355|Implement missing methods in XmlReader/XmlTextReader required for PWA porting.|Task
|SLIDESCPP-2265|Implement XmlReaderSettings' MaxCharactersFromEntities and MaxCharactersInDocument properties|Task
|SLIDESCPP-2129|Add support for document digital signing|Task
|SLIDESCPP-2340|Fix FodpIO.TestCorruptedFodp test|Bug
|SLIDESCPP-2345|Prepare C++ code examples (v20.4)|Task
|SLIDESCPP-2355|Fix visibility of embedded resources when emplace_assembly_details option is enabled|Bug
|PDFCPP-1283|Fix support embedded resources in CsToCppPorter|Improvement

## Public API and Backward Incompatible Changes ##

1. ZipEntry::GetCompressedStream() method was supported.
1. Signature of MemoryStream::ToArray() method was fixed.
1. The [CppConstMethod ](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppConstMethod)attribute no longer supports boolean arguments.
1. Some previously unimplemented methods and types are now supported in System::Xml namespace.
1. Reflection-related class support was improved.
