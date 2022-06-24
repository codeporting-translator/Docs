---
date: "2020-07-18"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.7"
linktitle: "CodePorting.Native Cs2Cpp 20.7"
menu:
  docs:
    parent: "2020"
    weight: "6"
lastmod: "2020-07-18"
weight: "6"
---

## Major Features ##

1. C++-styled iteration was added over the library containers. begin/end/cbegin/cend/rbegin/rend methods are now present where possible.
1. Documentation translation was improved. Porter is now capable of expanding 'cref' references (which is controlled by a new 'try_expand_cref_types' option). Multiple tags are now supported. A new 'hide_forward_declarations' option was supported to make porter hide forward declarations from Doxygen processing. Documentation comments were fixed for the classes containing enums.
1. New 'CppAllowStackAllocation' attribute was supported by porter making it possible to unhide classes' destructor in ported code so it can be allocated on stack in C++.
1. Translation of System::String class to and from various STL string types were improved. Respective constructors and conversion operators were added (yet explicit, to avoid undesired invocations of suboptimal operations). Also, the String class is now working fine with STL streams.

## Minor fixes ##

1. Ownership issues were fixed for the Xml subsystem. XmlDocument can no longer be deleted while there are references to any of its nodes.
1. 3rd party licenses file was actualized.
1. GV files are now named after tests they stand for to allow running tests in parallel without overwriting files.
1. TestCaseSource attribute was fixed for the case of the test method with void return value and single argument.
1. Compiler warnings were fixes for the cases of using Aspose C++ code from existing or uncontrolled (e. g. Qt Creator) projects. Porter is now capable of suppressing C4100 warnings in headers it creates.
1. 'CppLambdaPassByValue' attribute was fixed to work with constructors.
1. Rendering issues related to drawing caps with compound array and text smoothing were fixed.
1. Porter is now capable of producing warnings if virtual methods, properties, or indexers are invoked from constructors or finalizers. This behavior is controlled by a new 'enable_warnings_for_virtual_func_calls_logging' option.
1. Use of IntPtr in unsafe C# code is now porter correctly.
1. The translation of null coalescing for nullable types was fixed.
1. Zip files signature was put in line with format requirements.
1. System::IO::Path::GetTempPath() now always returns string terminated by a directory separator, same as in .Net.

Please consult the respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|CSPORTCPP-2430|Refactor System::Xml using SmartPtr aliasing constructor|Improvement
|CSPORTCPP-1530|Add begin-end methods to containers|Improvement
|WORDSCPP-970|Implement missing System::Net::WebClient methods|Bug
|CSPORTCPP-3373|Support CodePorting.Native Cs2Cpp|Task
|SLIDESCPP-2422|Fix broken links in the generated documentation|Bug
|SLIDESCPP-2438|Porter: Implement an ability to replace documentation comments with more than one tag specified|Task
|SLIDESCPP-2446|Porter: Fix documentation comments for a class which contains an enum|Bug
|CSPORTCPP-2921|Add a new attribute for a porter - CppAllowOnStack|New feature
|CSPORTCPP-3326|Fix warnings when building Qt Creator project|Improvement
|EMAILCPP-236|Preparing Aspose.Email for C++ Release 20.5|Task
|SLIDESCPP-2275|Incorrect cap styles|Bug
|CSPORTCPP-1529|Improve strings conversion to and from C++ types|Task
|CSPORTCPP-1107|Add porter option to auto-detect virtual function calls from constructors|New feature
|SLIDESCPP-2408|Use Aspose.Slides for .NET 20.7 features|Task
|SLIDESCPP-2272|Incorrect text smoothing|Bug
|TASKSCPP-1428|Null coalescing enhancement for nullable types (Linux GCC issue)|Bug
|SLIDESCPP-2442|Porter: Wrap forward declarations with a mark that excludes them from processing by Doxygen|New feature

## Public API and Backward Incompatible Changes ##

1. System::Net::WebClient class partially implemented.
1. StringBuilder::Clear() method was supported.
1. System::Linq::Enumerable::Empty method was supported.
1. System::IO::Stream::CopyTo method was supported.
1. System::Drawing::ImageFormatConverter class was implemented.
1. get_CommandLine(), get_ExitCode() and set_ExitCode() methods were supported by System::Environment class.
