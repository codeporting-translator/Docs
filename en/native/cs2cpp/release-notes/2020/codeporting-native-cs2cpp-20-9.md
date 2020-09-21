---
date: "2020-08-16"
author:
  display_name: "Muhammad Rizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.9"
linktitle: "CodePorting.Native Cs2Cpp 20.9"
menu:
  docs:
    parent: "2020"
    weight: "8"
lastmod: "2020-08-16"
weight: "1"
---

## Major Features ##

1. 'explicit_destructors' option was supported by porter making it possible to generate destructors for all classes (e. g. for debugging via breakpoints placement).
2. Wrapper classes were added making it possible using STL streams with ported API by wrapping STL streams into System::IO::Stream-like classes, as well as by wrapping System::IO::Stream with STL-like stream adapter classes.
3. 'prefer_short_type_names' option was supported by porter making it possible using short (not fully qualified) names where possible for some contexts.
4. A subsystem was added to the porter to decide between CppConstMethod and CppConstWrapper attributes if both are inherited or applied to a single entity by any other means. The choice is based on which attribute is applied more specifically.
## Minor fixes ##

1. Some minor behavior issues were fixed for System::Security::Cryptography namespace, Convert, and Environment classes.
2. Template type names are now suffixed with '<T>' string in output gv files.
3. The memory leak was fixed in the Skia gif submodule.
4. A bug was fixed in porter when creating exceptions and passing null as the only string argument.
5. The memory management issue was fixed in EnumValues class.
6. Porter no longer generates unnecessary boxing code for calls to Console::Write/WriteLine and String::Format.
7. Globalization data was updated for better compatibility with Windows 10 2004.
8. gtest fork used was upgraded so that char16_t data is now properly formatted when dumped to console.
9. Porter reports on documentation comment replacement were improved.
10. Range-based for loop was fixed for the SortedDictionary container.
11. Vertically aligned text rendering was fixed for some cases.
12. The generation of forwarding declarations for delegates was fixed in porter.
13. Rendering of some italic fonts on Linux was fixed.
15. Linking ported projects with GCC in case of using enum metadata was fixed.
16. Some missing includes for int32_t were fixed.
17. Call to System.Reflection.MethodBase.GetCurrentMethod().DeclaringType.FullName now provides fully qualified typename instead of short name.
18. ICUEncoder class now properly works with SMP characters.
19. System::StaticCast and System::DynamicCast functions were optimized for speed.
20. The 'Ignore' parameter of the TestCase attribute is now recognized by the porter.
21. A bug was fixed in porter causing truncated include paths in include_map.
22. XmlTextReader::ReadString() now properly handles whitespaces.
23. Conversion from utf-8 to System::String was switched to explicit as the operation may take a long time.
24. Some memory leaks were fixed in the Xml submodule.
25. A bug was fixed in porter not getting Object-typed fields into GetSharedMembers() data.
26. A bug was fixed in porter for per-class export mode.
27. The porter now generates RTTI macros for TestFixture classes. The old behavior can be restored using a newly introduced 'rtti_on_testfixture' option.
28. The behavior of IPAddress::TryParse() and XmlWriter::Create() methods was fixed.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-3548|Porting tests for the System.Security.Cryptography namespace.|Task
|SLIDESCPP-2501|Fix memory leak of GifMakeMapObject|Bug
|CSPORTCPP-3569|Analyzing and porting the following source files from the SystemLibraryTest project to MonoTests|Enhancement
|CSPORTCPP-3467|Instances of exception classes are created incorrectly when null is passed as the message parameter value|Bug
|WORDSCPP-999|Simplify ported code with System::Console::WriteLine calls|Enhancement
|CSPORTCPP-2953|Add option to generate destructors unconditionally|Task
|CSPORTCPP-1534|Create System::Stream-like wrappers around iostreams|Task
|SLIDESCPP-2515|Fix curvature direction for SketchLineTests|Bug
|WORDSCPP-963|SystemPal::OpenStreamFromHref doesn't support URI with the file with escaped characters|Bug
|CSPORTCPP-1809|Add char16_t serialization to gtest|Enhancement
|SLIDESCPP-2528|Porter: Improve CommentReplacementHelper's reports|Enhancement
|SLIDESCPP-2518|Enable foreach_as_range_based_for_loop porter option for Aspose.Slides for C++|Task
|SLIDESCPP-2406|Fix RegressionTests_v15_3.SLIDESNET_36181 test|Task
|WORDSCPP-1003|Use short type names for DynamicCast and enums|Enhancement
|PDFCPP-1304|Asposecpplib thread fixes|Bug
|CSPORTCPP-3623|Incorrect forward declaration generation|Bug
|SLIDESCPP-2535|Fix font with italic style in Linux|Bug
|EMAILCPP-242|Preparing Aspose.Email for C++ Release 20.7|Task
|EMAILCPP-243|Preparing Aspose.Email for C++ Release 20.8|Task
|WORDSCPP-962|Incorrect output from XmlTextWriter for SMP characters / TestSmpCodePointsEscaped|Bug
|TASKSCPP-1464|Fix StaticCast/DynamicCast to avoid the extra call of dymanic_cast|Enhancement
|WORDSCPP-915|Add support for TestCase@Ignore named parameter|New feature
|PDFCPP-1367|porter bug in generating include_maps|Bug
|WORDSCPP-823|Some Odt tests never end|Bug
|CSPORTCPP-3621|Verify that implicit conversion from utf8 to System::String|Improvement
|SLIDESCPP-2507|Fix memory leak of xmlNewDoc|Bug
|SLIDESCPP-2540|Fix memory leak of xmlNewNs|Bug
|SLIDESCPP-2541|Fix memory leak in XmlTextReader::Read|Bug
|SLIDESCPP-2543|Fix memory leak of Hyperlink|Bug
|PDFCPP-1385|Fix Cs2CppPorter - reflection & shared library|Bug
|CSPORTCPP-3557|Issues with 'override' subsystem|Bug
|PDFCPP-1399|RTTI for test fixtures|Enhancement

## Public API and Backward Incompatible Changes ##

1. System::Random is no longer ported as a value type.
2. FileWebRequest and FileWebResponse classes were supported in System::Net module.
3. Thread::Interrupt() method was supported.
