---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 19.4"
linktitle: "CodePorting.Native Cs2Cpp 19.4"
menu:
  docs:
    parent: "2019"
    weight: "4"
lastmod: "2019-10-11"
weight: "4"
---

## Major Features ##

1. Product name was changed from 'csPorter for C++' to 'CodePorting.Native Cs2Cpp'. Old product name was used for releases from 18.9 to 19.3. The new one is being used from 19.4 release onwards.
1. Support for CppSkipTest attribute was added allowing it to skip tests in gtest by placing attribute.
1. Autoproperties initialization in constructor was fixed for the case when no setter is provided.
1. Regex implementation was switched from boost::regex to PCRE2 library causing performance increase. Some previously unsupported syntaxes were supported.
1. boost library used was upgraded to 1.69.
1. Size of Skia library being used was optimized.

## Minor fixes ##

1. Include directive for enums used in class implementation was moved from .h files to .cpp files.
1. Types conversion genration was fixed for '##' and '!#' operators when they are defined for translated class.
1. Implicit type conversion operators were supported by porter.
1. gtest version used was updated to actual master branch.
1. 'CppSkipEntity' and 'Ignore' attributes are now taken into account when adding baseclass'es tests into current class.
1. NUnit.Framework.Assert.Warn() method translation was supported.
1. C4715 warnings were suppressed for translation of try-finally statement with finally_statement_as_lambda option enabled.
1. Collections comparison in tests translation was improved.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##


| Key | Summary | Category
---| ---|  ---|
|WORDSCPP-758|Add support for skipping tests with GTEST_SKIP()|New Feature
|CSPORTCPP-2626|Fix project name in the packages|Enhancement
|CSPORTCPP-2584|Change product name in license checking|Enhancement
|WORDSCPP-767|force_include_enum#true shouldn't include private enums|Enhancement
|PDFCPP-932|Fix CsToCppPorter - types convertion in equality/inequality operators|Enhancement
|PDFCPP-933|CsToCppPorter - implicit conversion to base type|Enhancement
|PDFCPP-939|Port Aspose.Font.Tests project: FormatterConverter|Enhancement
|PDFCPP-940|CsToCppPorter - fix AddBaseClassTests|Enhancement
|PDFCPP-937|Port Aspose.Font.Tests project|Enhancement
|CSPORTCPP-2020|Fix Regex issues|Enhancement
|EMAILCPP-188|Prepearing Aspose.Email for C++ release 19.01|Enhancement
|BARCODECPP-395|Support Aspose.BarCode for C++|Enhancement
|CSPORTCPP-2606|Switch asposecpplib to compact Skia build with is_official_build flag|Enhancement
|WORDSCPP-766|Disable C4715 warnings for code with try-finally statements|Enhancement
|WORDSCPP-760|Incorrect porting of autoproperties initialization in ctor|Bug
|WORDSCPP-764|Implement AsposeAssert.AreCollectionsEqual assertion|Bug

## Public API and Backward Incompatible Changes ##

1. Stubs to GetFileName(), GetFileLineNumber() and GetFileColumnNumber() methods of System::Diagnostics::StackFrame class were added.
1. Boolean-parameterized System::Diagnostics::StackTrace constructor was implemented.
1. System::Reflection::MethodBase::get_ReflectedType() method was implemented.
1. Implementation of Regex-related classes was changed. Some internally used methods may have been added and/or removed.
1. Stubs for System::Runtime::Serialization::FormatterConverter class were added.
