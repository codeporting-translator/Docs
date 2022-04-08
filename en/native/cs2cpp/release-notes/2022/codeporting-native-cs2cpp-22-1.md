---
date: "2022-01-14"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 22.1"
linktitle: "CodePorting.Native Cs2Cpp 22.1"
menu:
  docs:
    parent: "2022"
    weight: "4"
lastmod: "2022-01-14"
weight: "4"
---

## Major Features ##

1. The new implementation for System::Xml was provided. Previously, we had custom implementation which was based on the libxml2 and libxslt libraries. The new version is based on the ported CoreFX code.
1. The Readme files were added to the Nuget and downloadable packages.
1. The double-conversion library was upgraded to 3.1.7 version.
1. The `events_with_custom_accessors` node was supported in the porter configuration files. It makes it possible to inform the porter on custom events in non-ported code.

## Minor fixes ##

1. The compound assignment operators now work with the ported structures containing corresponding arithmetic operators.
1. The comparison operators for the structures that have `IsNull` method were fixed.
1. The `force_const_ref_parameters` option was imrpoved. If applied to the class constructor, it also affects generated member `MakeObject` function (if any), thus improving the performance of the ported code.
1. Unwanted dependencies were removed from `make_const_ref.h`.
1. This-comparison was optimized in the ported code. `SmartPtr` instance is no longer created.
1. The Qt conversion functions were updated at GitHub.
1. The shaping for symbolic fonts was disabled, like in .Net.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4664 | Add README to the Nuget and Downloads packages | Enhancement |
| SLIDESCPP-3274 | Improve arithmetic operators translation | Bug |
| SLIDESCPP-3278 | Fix comparison of structures with defined IsNull method | Bug |
| CSPORTCPP-4632 | force_const_ref_parameters doesn't affect MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION | Bug |
| SLIDESCPP-3061 | Integrate ported System.Xml code into CodePorting.Native.Cs2Cpp library | New feature |
| SLIDESCPP-3115 | Add the ported code to CodePorting.Native.Cs2Cpp library | Task |
| SLIDESCPP-3116 | Resolve asposecpplib porter's tests compilation errors | Task |
| SLIDESCPP-3120 | Fix failed ported tests | Task |
| SLIDESCPP-3131 | Find and apply profitable porter options | Task |
| SLIDESCPP-3132 | Replace 'for' loops with 'foreach' loops where possible | Task |
| SLIDESCPP-3135 | Resolve compilation errors in Linux | Task |
| SLIDESCPP-3142 | Find and try to enable disabled System.Xml tests | Task |
| SLIDESCPP-3151 | Fix failed product's tests | Task |
| SLIDESCPP-3068 | Improve the ported code readability | Task |
| SLIDESCPP-3166 | Apply performance improvements to System.Xml implementation | Task |
| SLIDESCPP-2867 | Add documentation comments for System.Xml code ported from CoreFX | Task |
| SLIDESCPP-3223 | Rework CollectionBase implementation in CoreFX code used for System.Xml porting | Task |
| SLIDESCPP-3195 | Fix merge request issues with System.Xml code ported from CoreFX | Task |
| SLIDESCPP-3193 | Fix regressions occurred after performance optimizations in System.Xml port | Task |
| SLIDESCPP-3264 | Synchronize System.Xml ported code with the latest CodePorting.Native.Cs2Cpp library changes (2021/49) | Task |
| SLIDESCPP-3076 | Fix failed aspose_xml_gtest tests | Task |
| SLIDESCPP-3136 | Fix failed tests in Linux | Task |
| SLIDESCPP-3208 | Fix newly detected reference cycles in System.Xml code ported from CoreFX | Task |
| CSPORTCPP-4767 | Fix Regex issue with the new System::Xml | Task |
| SLIDESCPP-3266 | Fix XmlRegexIssue.TestXmlRegexIssue test | Task |
| SLIDESCPP-3286 | Investigate System.Xml runtime errors reported by Tasks team | Task |
| CSPORTCPP-4818 | Review code changes | Task |
| CSPORTCPP-4618 | Add Double-conversion build job for Windows | Task |
| TASKSCPP-1692 | Avoid excess SmartPtr creation when compare with 'this' pointer. | Enhancement |
| CSPORTCPP-4846 | Fix tests for 'develop' branch | Bug |
| CSPORTCPP-4835 | Drop libxml2 and libxslt licenses from pdf file | Task |
| CSPORTCPP-4090 | Add tests for Qt converters to Jenkins sequence | Task |
| SLIDESCPP-3296 | Fix RegressionTests_v22_1.SLIDESNET_42964 | Bug |

## Public API and Backward Incompatible Changes ##

1. The implementation of `System::Xml` classes was completely reworked. Some APIs may have become unavailable. Some APIs may have changed. The new API is much closer to such of .Net. Please use the API reference for more information.
2. The supported glibc version will be changed in the upcoming releases.
3. The new `IEnumerable`-level iterators and iterators for collections with duck typing will be introduced in one of the upcoming releases. The code which is dependent on `EnumeratorBasedIterator` or `DuckTypedIterator` may require some changes.
4. Some declarations (e. g. `StreamPtr`) will be removed from the global namespace in the upcoming releases.