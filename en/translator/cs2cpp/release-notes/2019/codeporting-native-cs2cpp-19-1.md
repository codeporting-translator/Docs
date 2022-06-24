---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.1"
linktitle: "CodePorting.Native Cs2Cpp 19.1"
menu:
  docs:
    parent: "2019"
    weight: "1"
lastmod: "2019-10-11"
weight: "1"
---

## Major Features ##

1. Networking functionality is added into System::Net namespace.
1. aspose_cpp library is switched to explicit exports. Internal members are no longer exported.
1. Aliasing constructor is supported by SmartPtr class.
1. Equals method overloads ambiguity is fixed when methods with different parameter names are added.
1. Assertion faults now break whole test instead of current function only which caused inconsistency between C# and C++ behavior.
1. MinGW 7.1 compiler is no longer supported.

## Minor fixes ##

1. CppConstexpr attribute is fixed to work properly on generic class members.
1. Missing include fixed when generating code using System::MakeObject() function but not referencing System::Array type.
1. Issue with events declared in interfaces is fixed. Previously, duplicated code is being generated.
1. Issue tranlating implicit casts when assigning to properties is fixed.
1. Issue translating static events is fixed.
1. gtest supported version is upgraded to #9ab640c.
1. BitArray to null comparison translation is fixed.
1. Inheritance from IDictionary<TKey,TValue> interface is now translated properly.
1. 4-argument version of String.Concat() is supported.
1. String::EndsWith() behavior is fixed for zero length substrings lookup.
1. Queue and Stack containers now support CppWeakPtr(0) attribute to keep contained pointers weak.
1. StringBuilder class now works with null objects properly.
1. CppSkipDefinition attribute now works properly with destructors.
1. A error is fixed for partial class porting when another class is present in the same file.
1. Environment::GetFolderPath() is implemented on Windows.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category
---| ---|  ---|
|WORDSCPP-723|Incorrect work of CppConstexpr attribute for generic classes|Bug
|PDFCPP-802|Memory Leaks|Investigation
|EMAILCPP-178|Implementing System.Net.* functionality in asposcpplib|Task
|PDFCPP-849|Preparing to release|Task
|EMAILCPP-183|Merge 18.11 release changes into Asposecpplib (Net implementation)|Task
|CSPORTCPP-2387|Switch aspose_cpp.dll to explicit exports|Task
|WORDSCPP-731|Implement aliasing constructor for SmartPtr|Task
|PDFCPP-877|fix Asposecpplib StringBuilder|Task
|WORDSCPP-739|Implement the depth-first iterative traversal deconstructor|Task
|WORDSCPP-581|Include Html codec to porting|New feature
|PDFCPP-879|Environment::GetFolderPath() impl (Windows)|Task
|CSPORTCPP-1039|Make test assertion faults abort the test|Task

## Public API and Backward Incompatible Changes ##

1. Several networking-related classes are added under System::Net namespace.
1. Dependencies on non-public aspose_cpp library members will be breached. Use public API analogs instead.
1. MinGW compiler support is stopped.
1. WriteStartAttribute() and WriteEndAttribute() methods are implemented in System::Xml::XmlWriter and System::Xml::XmlTextWriter classes.
1. TypeInitializationException exception type is implemented.
1. DtdProcessing property is supported by XmlTextReader class.
1. Missing versions of List<T>::LastIndexOf() methods are implemented.
