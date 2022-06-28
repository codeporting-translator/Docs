---
date: "2019-12-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.12"
linktitle: "CodePorting.Native Cs2Cpp 19.12"
menu:
  docs:
    parent: "2019"
    weight: "13"
lastmod: "2019-12-11"
weight: "13"
---

## Major Features ##

1. The Botan library used by the project was upgraded to 2.11.0.
1. ['rename_files' configuration node](https://wiki.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-nodes/#Hrename_files) was supported by the porter.
1. FileStream class was refactored with its behavior fixed closer to such in .Net. Related classes were also improved. Files locking now works better on Linux.
1. The Skia library used was upgraded and patched.
1. Support was added for more Aspose C++ products to be used in single customer applications.

## Minor fixes ##

1. Same XML loading options used for XmlFile, XmlStream and XmlTextReader.
1. Issues generating Doxygen comments for static initializers were fixed.
1. XmlTextReader::get_Name() method was fixed for some special cases ('#' prefix).
1. Includes generation was fixed for the cases when 'generate_includes_subdirectory' option is enabled.
1. FileStream constructor was fixed for specific combination of parameters (FileMode::OpenOrCreate and FileAccess::Write).
1. XmlTextWriter::get_BaseStream() returning nullptr was fixed if specific constructor was used.
1. Thread.Interrupt() was implemented for sleeping threads only.
1. Includes for cross-referencing headers were fixed for specific cases.
1. Porting of increments and decrements used on protected properties was fixed.
1. Missing include for NotImplementedException class was fixed in porter when generating stubs.
1. MeasureCharacterRanges() method was fixed for special characters.
1. An emSize/6 pixels shift used by .Net when rendering non-generic typographic text is reproduced by Graphics:: DrawString().
1. LineCap drawing was fixed for some specific cases.
1. SortedList implementation is now capable of storing structures.
1. Porter is now capable of tracking changes in documentation comments by processing the 'HashCode' tag. Warnings are generated for unexpected changes.
1. Exceptions thrown by Bitmap::LockBits in some cases were fixed.
1. Some performance issues within System::Drawing namespace was fixed.
1. GraphicsPath::get_PathTypes was fixed for some cases.
1. Error handling by XmlTextWriter::WriteProcessingInstruction() method was fixed.
1. Fixing ZlibBaseStream no longer closes on a closed inner stream.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|PDFCPP-1117|Cs2Cpp enhancement - rename files.|Feature
|PDFCPP-1128|Try to implement Thread.Interrupt() for thread sleep mode|Feature
|SLIDESCPP-2044|Fix RegressionTests_v15_3.ImageMagick_Render_SLIDESNET_36181 test|Enhancement
|SLIDESCPP-2072|Porter: Implement tracking of XML comments changes in C# code|Enhancement
|SLIDESCPP-2104|Fix "System::NotImplementedException" exception while running ported tests|Enhancement
|SLIDESCPP-2116|Resolve performance degradation occurred in Jenkins builds (Linux)|Enhancement
|SLIDESCPP-2065|Improve the performance of saving the presentation in PDF format|Enhancement
|SLIDESCPP-1822|Use Aspose.Slides for .NET 19.12 features|Task
|SLIDESCPP-2140|Add support for the CodePorting.Native.Cs2Cpp.API package|Task
|WORDSCPP-822|Update Botan library|Task
|TASKSCPP-1295|Fix RegressionTests_5_3_0/TASKSNET_33310VP.Test/2 test|Bug
|TASKSCPP-1334|Fix static initializer's comments generation.|Bug
|TASKSCPP-1297|Fix TestHtml.TestCanSaveHtml and TestHeader tests|Bug
|WORDSCPP-903|System::IO::FileStream deletes file content when FileShare::ReadWrite is used|Bug
|WORDSCPP-914|XmlTextWriter::get_BaseStream() returns nullptr|Bug
|PDFCPP-1131|Fix Cs2Cpp porter - lost includes on cross-references|Bug
|TASKSCPP-1348|Fix derived property increments/decrements (porter issue)|Bug
|SLIDESCPP-2059|Porter: Fix missing include directive for cases when a code body is replaced with a stub|Bug
|SLIDESCPP-2042|Fix RegressionTests_v19_10.SLIDESNET_41164 test|Bug
|SLIDESCPP-2048|Resolve an issue with incorrect pixels on the image|Bug
|SLIDESCPP-2100|Fix "System::ArgumentException: key" exception while running ported tests|Bug
|PDFCPP-1137|fix GraphicsPath::get_PathTypes for single point counters|Bug
|WORDSCPP-903|System::IO::FileStream deletes file content when FileShare::ReadWrite is used|Bug
|PDFCPP-1143|Fix XmlTextWriter::WriteProcessingInstruction|Bug

## Public API and Backward Incompatible Changes ##

1. BadImageFormatException class was implemented.
1. Implementation of System.Collections.IEnumerator.Current property is no longer skipped by porter by default.
1. Debug::WriteIf() and Debug::WriteLineIf() methods were implemented.
