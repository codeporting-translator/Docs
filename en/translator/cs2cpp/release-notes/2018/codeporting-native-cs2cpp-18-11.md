---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 18.11"
linktitle: "CodePorting.Native Cs2Cpp 18.11"
menu:
  docs:
    parent: "2018"
    weight: "4"    
lastmod: "2019-05-28"
categories:
- "release_notes_2018"
weight: "4"
---

## Major Features ##

Following major improvements and fixes are applied in this regular monthly release:

1. Tests translation approach is improved. Now, virtual function called from methods marked with TestFixtureSetUp, TestFixtureTearDown, SetUp and TearDown attributes are fully supported. Also, TestFixture objects are now created once before running tests from current fixture, and deleted afterwards. 'cleanup_tests' option is now ignored by porter
1. Named mutex implementation is added
1. Compilation of library code using Visual Studio 2015 is explicitly disabled
1. Possible static initialization fiasco is now detected by porter and reported as warning
1. PreserveWhitespace mode is enabled for XmlDocument class
1. New 'obfuscate_cpp_headers' option is supported. It allows generating headers with some type and name information removed for non-public fields
1. New 'nunit_assert_class_aliases' option is supported. It allows treating calls into methods of some ported classes same way calls into NUnit's Assert class methods are treated
1. Graphics::FillRegion() method is implemented
1. Threading::Thread class'es behavior is improved to match .Net behavior better. Now the thread is detached after start, so dropping thread object reference doesn't result in interthreading errors
1. Several System::Diagnostics::Process methods are implemented for Linux platform
1. A way to detect multiimage gifs is added
1. Simple string raw pointer operations are supported inside 'fixed' statement

## Minor fixes ##

1. StringBuilder now calls proper version of ToString() method even if not marked with CppConstMethod attribute
1. NS deduction mechanism is fixed in System::Xml::Node::SetAttribute() method
1. Array::Sort() implementation is fixed for value types supporting CompareTo method
1. Types of enumerators of collections from System::Collections namespace being treated as value types is fixed
1. Porter crash when having ambiguity between two or more methods, one having 'params'-typed argument block, is fixed
1. Dot in subject string expression now properly matched against 'any symbol' dot by Regex class
1. 'aspose_cpp' library suffixes for Visual Studio 2017 are switched from 'vc150' to 'vc141' which is more common
1. System::String::Contains behavior is fixed for empty string used as argument
1. XmlTextReader::get_AttributeCount() is fixed when situations when reader is currently pointed to attribute node
1. Porter behavior is fixed for situations when method parameter name matches such of one of the class'es parameters
1. Graphics::MeasureString() behavior is fixed for some corner cases
1. Buffer::BlockCopy() method is fixed to work with rvalues when compiled with GCC
1. GraphicsPath::AddBeziers() overloads are implemented
1. PlatformNotSupportedException exception type is supported
1. Environment::GetFolderPath() is supported on Linux for SpecialFolder::Personal folder type
1. Visualizations of System::String and System::SmartPtr inside Visual Studio are fixed and improved
1. Accuracy of Graphics::MeasureString() method is improved. Special treatment of leading spaces is added. Behavior when no glyphs are present in the font used is fixed
1. 'exclude' parameter of 'cut_namespaces' option is fixed to work with delegates
1. Some errors within FileStream class are fixed
1. Header name resolving mechanism is fixed when porter-generated header name matches name of existing header file
1. Few memory issues within the library are fixed
1. Complex assignment operators on indexers are fixed. There is an expression duplication issue causing increment or other similar operations to be executed twice
1. List and Array sorting order is fixed for NaN values to match C# behavior
1. GraphicsPath::AddPolygon() behavior is fixed for some cases.
1. Tiff format with CMYK color model is now supported
1. XmlConvert::ToString() method now uses correct invariant culture with DateTime objects.
1. Quality of hashes provided by Object::GetHashCode() method is improved.
1. False positive 'Failed to parse constructor initializer' porter errors are downgraded to warnings. The aren't removed totally to avoid loosing true alerts.
1. String::Join() method is fixed for empty input array to return empty string instead of null string.
1. Missing include when using cascaded CppConstexpr attribute is fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##


| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-1805|Improve TestFixture translation approach|Enhancement
|WORDSCPP-651|Port classes from namespace Aspose.TestFx.GoldComparers.FailedTestCollector|Task
|PDFCPP-767|Fix CsToCppPorter - conversion to string|Bug
|CSPORTCPP-2187|Add VS2015 disablement macro to public headers|Task
|BARCODECPP-374|Implement SIOF detection|Task
|WORDSCPP-673|System::Array<System::String>::Sort does not work the same as in .NET|Bug
|CSPORTCPP-2127|Fix member class porting as value type|Bug
|WORDSCPP-672|bool vs String ambiguity detector doesn't work with params arguments|Bug
|WORDSCPP-610|XmlDocument.PreserveWhitespace throws NotImplementedException|Bug
|WORDSCPP-570|/TestReplace.TestReplaceAcrossParagraph/ Dot is not recognized as "any character" by Regex.|Bug
|PDFCPP-793|Fix issues with Text tests|Task
|PDFCPP-747|Get tests passed|Investigation
|BARCODECPP-379|Support Aspose.BarCode for C++|Task
|SLIDESCPP-1371|Fix Aspose.Slides for C++ build issues in Linux environment|Task
|SLIDESCPP-1377|Analyze missing slide elements rendering issue|Task
|SLIDESCPP-1379|Implement 'Personal' case of Environment::GetFolderPath for Linux environment|Task
|SLIDESCPP-1380|Improve accuracy of the Graphics::MeasureString method|Task
|SLIDESCPP-1386|Fix aborting execution when running RegressionTests_v18_8 tests|Task
|SLIDESCPP-1393|Porter: Option cut_namespaces:exclude is not applied for delegates|Task
|SLIDESCPP-1395|Implement StringFormatFlags.MeasureTrailingSpaces functionality for MeasureString|Task
|SLIDESCPP-1405|Implement System::Diagnostics::Process::GetCurrentProcess() for Linux environment|Task
|SLIDESCPP-1411|Fix text measuring when a font doesn't have required glyphs|Task
|SLIDESCPP-1412|The output pptx file is corrupted|Bug
|SLIDESCPP-1422|Porter: Headers conflict resolving mechanism is broken|Task
|SLIDESCPP-1424|Check Aspose C++ Library for memory access issues (v18.9)|Task
|SLIDESCPP-1429|Fix translation of complex assignment operations to indexers|Task
|SLIDESCPP-1435|Fix Array/List sorting when it contains NaN values|Task
|SLIDESCPP-1426|Fix RegressionTests_v17_2.SLIDESNET_35542 test|Task
|SLIDESCPP-1444|Modify Skia to support decoding of Tiff images with CMYK color model|Task
|SLIDESCPP-1459|Fix NotImplementedException while running Miscelaneous.Test_SLIDESNET_33637 test|Task
|SLIDESCPP-1449|Improve quality of hash values returned by Object::GetHashCode()|Task
|SLIDESCPP-1451|Force porter to fail the build on file errors|Task
|SLIDESCPP-1463|Fix RegressionTests_v16_4.SLIDESNET_36934 test|Task
|SLIDESCPP-1472|Implement identification of multi-images GIF|Task
|SLIDESCPP-1473|Porter: Missing include directive when CppConstexpr is applied|Task
|SLIDESCPP-1479|Porter: Add support of simple raw pointer operations in the fixed statement|Task

## Public API and Backward Incompatible Changes ##

1. The way tests were translated is changed. If you have tests in your solution, you must re-port full solution instead of individual project, as tests ported by previous porter versions won't be compatible with tests ported by actual porter.
1. In System::Threading::Mutex class, Mutex(bool, String) constructor and Remove(String) static method are added.
1. System::Details::Skia namespace is moved to System::Drawing::Details::Skia. No clients' code should be impacted.
1. IsMultiImage() method is added into Bitmap and Image classes.
