---
date: "2020-12-10"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 20.12"
linktitle: "CodePorting.Native Cs2Cpp 20.12"
menu:
  docs:
    parent: "2020"
    weight: "1"
lastmod: "2020-12-11"
weight: "1"
---

## Major Features ##

1. Porter is now capable of porting several classes overloaded by generic parameter count. These go into partial specialization of variadic template.
1. Porter can now report an error if an in-config attribute does not apply to any item during the project porting. To make it do so, use 'error_if_unused' syntax as described [at 'Attributes in configuration file' page](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/attributes-in-configuration-file/).
1. UDP sockets now handle timeouts properly. The implementation now uses async calls.
1. The porter is now capable of porting 'Assert.That(expr, Throws.TypeOf<exc>)' and 'Assert.That(expr, Is.Empty)' Nunit checks.
1. A new 'enable_fast_rtti' [porter option](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/#Henable_fast_rtti) was added making it possible to speed up casting operations (translations of c-style cast, 'as' and 'is' operators).

## Minor fixes ##

1. A new 'unexpected_override_as_warning' [porter option](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/#Hunexpected_override_as_warning) was added making it possible to switch 'override is unexpected' message from error to warning.
1. LinkedList container was completely rewritten. It no longer leaks the memory.
1. RefCount class doesn't produce assertion fault if the counter goes beyond zero.
1. Some previously missing types were added into porter's typemap.
1. Graphics::MeasureString now wokrs properly with the characters that are absent from the font being used and loaded from fallback one instead.
1. Some cases of line cap drawing were fixed.
1. Incorrect paths in cmake target files for Visual Studio projects were fixed.
1. The performance of some Dictionary operations was improved.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-3866 | Make porter tests fail on porter errors | Enhancement |
| BARCODECPP-426 | Support Aspose.BarCode for C++ | Task |
| CSPORTCPP-3102 | Add warnings for unused config attributes | Enhancement |
| CSPORTCPP-3526 | LinkedList leaks memory | Bug |
| CSPORTCPP-3854 | MakeObject leakage detection improvement | Enhancement |
| SLIDESCPP-2661 | Porter: Using of CppIgnoreBaseType attribute mistakenly ignores System::Object base type | Bug |
| SLIDESCPP-2664 | Weekly meeting and current working activity (2020/49) | Task |
| SLIDESCPP-2631 | Fix indentation after ellipsis | Task |
| SLIDESCPP-2662 | Incorrect LineCap::NoAnchor handling in GraphicsPath::Widen() method | Bug |
| EMAILCPP-267 | UdpClient does not properly handle timeouts | Bug |
| WORDSCPP-966 | Support of Assert.That(expr, Throws.TypeOf<exc>) expressions | New feature |
| CSPORTCPP-3924 | Fix cmake targets files | Bug |
| SLIDESCPP-2596 | Improve performance of System::Cast and System::ObjectExt::Is operations | Enhancement |
| WORDSCPP-967 | Support of Assert.That(expr, Is.Empty) expressions | New feature |
| SLIDESCPP-2595 | Improve performance of Dictionary<string, string> accessing operations | Enhancement |

## Public API and Backward Incompatible Changes ##

1. CppIgnoreBaseType [attribute](https://docs.codeporting.com/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/#HCppIgnoreBaseType) no longer can ignore System.Object inheritance implicitly. Explicit ignore still works.
