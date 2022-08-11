---
date: "2022-08-11"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 22.8"
linktitle: "CodePorting.Translator Cs2Cpp 22.8"
menu:
  docs:
    parent: "2022"
    weight: "1"
lastmod: "2022-08-11"
weight: "1"
---

## Major Features ##
1. The assertion mechanism is updated. Now `#include <system/collections/list.h>` is required when the `System::Diagnostics::Debug` class is used. The translator adds this `#include` automatically.
1. The used openssl version is updated.
1. The `Native` is replaced to `Translator` in the 3rd party license file.


## Minor Fixes ##
1. The `CODEPORTING_WINDOWS`, `CODEPORTING_MACOS`, `CODEPORTING_LINUX`, and `CODEPORTING_WEBASSEMBLY` macroses used to detect OS are added.
1. The collecting of test case data is moved to the proper stage.
1. The cocurrrency issues of collecting `TestCase` and `TestCaseData` information are fixed.
1. The `insert_code_to_tests` configuration file node is added. It is used to add a specified C++ code into the beginning of each Google.Test `TEST_F`/`TEST_P` method that is translated from the NUnit tests.
1. The translator generated incorrect arguments when the '+=' operator was used to subscribe to events. Now the cast operator is added in case of mismatching types.
1. The `-ns` command line argument is removed. It was used to add support for nanoseconds in DateTime.
1. The `-ls` command line argument is added. It is used to list project files instead of running the porter. The GUI application uses the transator with the `-ls` command line argument to get a list of project files.
1. Now the translator finds proper netstandard/netcore references.
1. Locking of named mutex worked incorrectly on Windows 10. Fixed.
1. Now the TextureBrush constructors throw necessary exceptions.
1. The `force_const_ref_parameters` option is taken into account by the transator when generating the IOStreamWrapper constructor. The translator also adds the `*_SHARED_API` keyword to the IOStreamWrapper constructor.
1. The `DISABLE_ASPOSECPPLIB_UNITY_BUILD` option is added to the generated CMakeLists.txt. It is used to disable UNITY_BUILD when project is being built.
1. The `ignore_constraints` option is added to a configuration file. It is used to skip generation of asserts that constrain types in the C++ template translated from the C# generic.
1. The `FontFamily::Clone` and `StringFormat::GetCharacterRangesCount` methods are added.


## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4607 | Review asserts in asposecpplib | Task |
| CSPORTCPP-4397 | Unify OS macros | Task |
| CSPORTCPP-5426 | Rebuild OpenSSL for all platforms | Task |
| SLIDESCPP-2782 | Implement an option to insert custom code into the GoogleTest methods | Task |
| CSPORTCPP-5213 | Update public sites | Task |
| SLIDESCPP-3438 | Incorrect MulticastDelegate::connect() arguments generation | Task |
| CSPORTCPP-5382 | GUI crashes on some projects | Bug |
| CSPORTCPP-5533 | Porter doesn't process the project properly | Bug |
| PAGECPP-74 | review failed NamedMutexTests of asposecpplib on local | Task |
| PAGECPP-75 | fix TextureBrush ctor behavior to .NET | Task |
| CSPORTCPP-5528 | Fix issues of building of asposecpplib when UNITY_BUILD is disabled | Task |
| SLIDESCPP-3519 | Porter: Implement an option to disable the 'where' keyword translation | Task |
| SLIDESCPP-3500 | Investigate the SLIDESCPP-3211 task status | Task |


## Public API and Backward Incompatible Changes ##
1. The `DynamicCast`, `DynamicCast_noexcept`, `StaticCast`, `StaticCast_noexcept` methods use `dynamic_cast` that looks uncorrectly. These methods will be marked as deprecated and removed in the upcoming releases. The `ExplicitCast` and `AsCast` methods will be used instead of them. The translator will generate a code that uses the new casts.
