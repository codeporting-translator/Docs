---
date: "2019-11-18"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.11"
linktitle: "CodePorting.Native Cs2Cpp 19.11"
menu:
  docs:
    parent: "2019"
    weight: "12"
lastmod: "2019-11-18"
weight: "12"
---

## Major Features ##

1. Support of Microsoft Visual Studio x86 configuration has been returned for asposecpplib.
1. CodePorting.Native.Cs2Cpp.API.targets have been added into VS packages for simpler use of packages without Nuget or cmake.
1. Support of containers that holds weak pointers has been enhanced to simplify their using.

## Minor fixes ##

1. Conversion from enum PaddingMode to string has been added.
1. Scaling for CustomLineCap has been improved.
1. The cloning issue for 1 byte-per-pixel bitmap has been fixed.
1. Angularity artifacts on drawing dashed lines have been fixed.
1. Boundary checks to several Systems::Collection::Specialized::StringCollection methods have been added.
1. Porter attribute "Obsolete" for documentation comments generation has been added. Attribute arguments will be transformed to the comments under "@deprecated" comment.
1. CppForceForwardDeclaration attribute has been enhanced to support template classes.
1. FileShare class support has been enhanced for the Linux platform.
1. StringComparer and Environment multithreading access issues have been fixed.
1. Gamma correction for linear gradient brush has been fixed.
1. For forwarded enums underlying types made to be resolved by the porter.
1. Namespace cutting in templated class properties has been fixed.
1. Include path for "*_api_defs.h" file has been fixed.
1. Force public include path has been fixed.
1. Porting of Equals invocation with the null literal argument has been fixed for the case when .Net code selects not an Object. Equals overload and argument is null literal.
1. CppForceInclude has been enhanced to ignore include cross-check.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|SLIDESCPP-1970|Improve Cryptography::CryptoStream class|Enhancement
|SLIDESCPP-1936|Improve CustomLineCap calculations|Enhancement
|WORDSCPP-817|StringCollection boundary check| Enhancement
|CSPORTCPP-2972|Add better syntax for DynamicSmartPtr for the user to use|Enhancement
|PDFCPP-1075|build aspose.pdf under gcc|Enhancement
|PDFCPP-1103|CSPorter attribute CppForceInclude modifications|Enhancement
|PDFCPP-1105|Move headers to Aspose.PDF.Cpp folder|Enhancement
|SLIDESCPP-1996|Porter: Implement [Obsolete] attribute processing|Task
|SLIDESCPP-1998|Implement ISO10126 and Zeros padding modes for Rijndael symmetric encryption algorithm|Task
|SLIDESCPP-1944|Fix Aspose::PptxPackage::PackageUnsupportedFormatException while running Aspose.Slides.FuncTests.Cpp|Bug
|SLIDESCPP-1950|Fix System::NullReferenceException while running RegressionTests_v19_8.SLIDESNET_41048 test|Bug
|SLIDESCPP-1955|Fix 1bpp bitmap cloning issue|Bug
|SLIDESCPP-1928|Fix angularity artifacts on drawing dashed lines|Bug
|SLIDESCPP-2019|Compilation error when BaseEffectiveData.h is included|Bug
|SLIDESCPP-2016|Analyze tests issues with file locking in Linux (v19.9)|Bug
|SLIDESCPP-2013|Fix System::Exception in the RegressionTests_v16_5.Render_SLIDESNET_37403 in Linux|Bug
|SLIDESCPP-2046|Fix gamma correction for linear gradient brush|Bug
|WORDSCPP-830|generate_includes_subdirectory porter option misses some headers|Bug
|PDFCPP-1064|Convert problem from PDF to Word|Bug
|PDFCPP-1092|Fixes for build Aspose.Foundation|Bug

## Public API and Backward Incompatible Changes ##

1. XmlReader::ReadToDescendant() methods were implemented.
1. BinaryWriter::Write(char16_t value) method was implemented.
1. ISO10126 and Zeros padding modes for Rijndael symmetric encryption algorithm have been supported. Cryptography::CryptoStream class was enhanced to support this change.
1. Bitmap::ComputeHash() was implemented.
1. Stub class StreamingContext was added to exported classes.
1. Several parameters of Marshal::Copy methods were changed to allow passing references.
1. Stub class FileWebResponse was updated to be compiled by gcc.
1. List::TrueForAll has been added.
1. Environment::IsWindowsSubsystemForLinux method has been added.
1. ASPOSE_COLLECTION_POINTER_MODE_CONTROL macro and SetTemplateWeakPtr have been added to support the simplified using of containers with weak pointers.
1. Some undefs were added to fix clashes issues with Cells.
1. Enhanced support of GraphicPath, ImageFormat and Pen classes by adding new methods implementations.
1. Enhanced support of XmlNode, XmlDocument, XmlDeclaration and XmlAttribute classes by adding new methods implementations.
1. Enhanced support of SortedDictionary by adding new methods implementations.
1. The random class made to be derived from Object class.
1. Stubs for classes from reflection, serialization, compression namespaces as well as for Console class were updated.
1. HttpUtility::HtmlDecode stub was added.
