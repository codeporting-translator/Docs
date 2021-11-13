---
date: "2021-03-10"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.4"
linktitle: "CodePorting.Native Cs2Cpp 21.4"
menu:
  docs:
    parent: "2021"
    weight: "9"
lastmod: "2021-03-10"
weight: "9"
---

## Major Features ##

1. Code documentation was improved significantly. Some previously undocumented members now have descriptions.
1. The `headers_dir_name` and `sources_dir_name` options were added. They are used to rename the include and source directories respectively.
1. Missing documentation for the exception classes was added.
1. The `string.Join(dlm, IEnumerable<string>)` method was implemented.
1. The `generate_begin_end_methods` option was added. This option is used to generate the `begin` and `end` methods for the classes and structures that implement the `IEnumberable` generic interface.
1. The `CppGenerateBeginEndMethods` attribute was added. This attribute is applied to fields and auto properties of any classes and structures. It is used to generate the `begin` and `end` methods. The important condition: types of fields and auto properties must contain the `begin` and `end` methods.
1. The `CppNoBeginEndMethods` was added. This attribute is used to disable generation of the `begin` and `end` methods when the `generate_begin_end_methods` option is enabled.

## Minor fixes ##

1. The invalid behavior of the `String.LastIndexOf(substr, stringComparison)` method was fixed.
1. The invalid behavior of the `XPathNavigator::GetAttribute(name. nsURI)` method was fixed for the case when the `nsURI` param is empty or blank.
1. The performance of the `Path::HasInvalidChars` method was improved.
1. Missing `#include <cstdint>` was added for some cases.
1. The following code `foreach (T item in this)` was ported to `for (T item: this)` without dereferencing for cases when the `foreach` loop is ported to `range-based for`. Fixed.
1. An access operator was changed for cases when code examples from comments were translated.
1. Now a chain of methods calls in code examples from comments can be translated.
1. Now co-variant array conversion at the method call is translated correctly.
1. When building an SDK-style project that contained the reference to another SDK-style project using Buildalyzer, product dll was deleted during porting of the first project, and porting of the second project failed because a product assembly was not found. Fixed.
1. Drawing of dash caps for closed curves was fixed for cases when `LineCap` had the `LineCap::NoAnchor` type.
1. Content of `codeporting.native.cs2cpp-config.cmake` was improved.
1. The `PRODUCT_NAME` and `ORIGINAL_FILE_NAME` properties were deleted from `version_defines.h`.
1. Logs of the comments translator were optimized.
1. Translation of fields in code examples from comments was improved.
1. The performance of converting of the `Decimal` type to integral types was improved.
1. The missing `GC.Collect` and `GC.get_MaxGeneration` methods were added.
1. The support of `Environment.GetFolderPath(Environment.SpecialFolder.UserProfile)` was added.
1. The `BrotliStream` class is marked as final. The type of some class fields was changed from `void *` to `std::unique_ptr<void, std::function<const void(void*)>>`.
1. The `Region::get_Clip` method implementation was fixed.
1. The `GraphicsPath::NeedConvertCurves` method implementation was fixed.
1. The performance of the `XmlDocument::idx_get`, `XmlElement::GetAttribute`, `XmlElement::HasAttribute`, `XmlElement::SetAttribute`, and `XmlElement::idx_get` methods was improved.
1. Now the `Func` class inherits `MulticastDelegate`.
1. A wildcard for a return type didn't work in the `CppSkipEntity` attribute when the porter couldn't resolve a return type. Fixed.
1. Enum underlying types deduction is moved to pre-translate analysis phase for proper includes generation. Race condition is fixed.
1. The `TimeZone::GetDaylightChanges` method was added.
1. The `CppForceInclude` attribute loop reference lookup override was fixed.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| WORDSCPP-1063 | Simplify null-conditional operators | Task |
| CSPORTCPP-4149 | Support CodePorting.Native Cs2Cpp | Task |
| TASKSCPP-1570 | Fix invalid behaviour of String.LastIndexOf(substr, stringComparison) in asposecpplib | Task |
| TASKSCPP-1571 | Fix XPathNavigator::GetAttribute(name. nsURI) for case when nsURI is empty or blank. | Task |
| WORDSCPP-1065 | Add porter options to rename source directories | Task |
| TASKSCPP-1569 | Improve Path::HasInvalidChars performance | Task |
| PDFCPP-1546 | CsToCppPorter - attribute condition checking for case returnType='*' | Task |
| PDFCPP-1549 | CsToCppPorter - not generated "#include <cstdint>" | Task |
| CSPORTCPP-4134 | Compilation error (Dictionary foreach) | Bug |
| CSPORTCPP-3104 | Fix net namespace comments | Enhancement |
| CSPORTCPP-4178 | "foreach in this" porting bug | Bug | 
| SLIDESCPP-2847 | Change an access operator when translating comments | Task |
| SLIDESCPP-2852 | Translate call of methods chain when porting comments | Task |
| WORDSCPP-1070 | Co-variant array conversion at the method call doesn't work | Task |
| EMAILCPP-280 | Provide examples of wrong porter work after "fewer includes" MR to the porter team | Task |
| EMAILCPP-283 | Fixing Porter to process SDK-Style C# project files | Task |
| PDFCPP-1518 | Unable to convert PDF to Word | Bug |
| SLIDESCPP-2836 | Fix RegressionTests_v21_3.SLIDESNET_42212 test | Task |
| CSPORTCPP-4083 | Improve contents of codeporting.native.cs2cpp-config.cmake | Enhancement |
| CSPORTCPP-3364 | Add Doxygen comments for exception classes | Enhancement |
| SLIDESCPP-2826 | Fix warnings when generating "API Reference" | Task |
| TASKSCPP-1583 | Implement string.Join(dlm, IEnumerable<string>) method | Task |
| CSPORTCPP-4188 | Exclude PRODUCT_NAME and ORIGINAL_FILE_NAME from version_defines.h | Task |
| TASKSCPP-1584 | Optimize xml navigation/move ICU_TO_XMLSTR_PARAM out from cycles body | Task |
| CSPORTCPP-4200 | asposecpp gets rebuilt after executing git commit and git push | Bug |
| SLIDESCPP-2874 | Optimize logs of comments translator | Task |
| TASKSCPP-1588 | Investigate asposecpplib's Decimal casting to numeric types. | Task |
| PDFCPP-1564 | Fix GC - missing methods for porting | Task |
| CSPORTCPP-4193 | Support CodePorting.Native Cs2Cpp | Task |
| PDFCPP-1560| Include Pdf2HtmlConverter to porting into C++| Investigation |

## Public API and Backward Incompatible Changes ##

None.
