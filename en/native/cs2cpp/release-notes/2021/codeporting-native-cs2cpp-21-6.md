---
date: "2021-06-11"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.6"
linktitle: "CodePorting.Native Cs2Cpp 21.6"
menu:
  docs:
    parent: "2021"
    weight: "4"
lastmod: "2021-03-10"
weight: "4"
---

## Major Features ##

1. Multiple new classes from the `System::Xml::Schema` namespace are implemented.
1. The Boost C++ Libraries are updated to version 1.76.
1. The obfuscation mechanism is improved. Now members that cannot be accessed beyond the current class are deleted from the public headers if possible. If deleting such members may lead to malfunction, stubs are generated.
1. Usage examples are added for the collections.

## Minor fixes ##

1. The `ASPOSECPP_SHARED_API` macro is removed from documentation.
1. The default constructor is marked as deleted for the ported static classes.
1. The foreach-based loop performance in a ported code is improved when the `foreach_as_range_based_for_loop` option is enabled. This optimization affects all containers that contains elements with the `SmartPtr<>` or `String` type except for arrays.
1. The `force_const_ref_parameters` option is added. When this option is enabled, the non-virtual methods/constructors/setters/operators parameters with `String` or `SmartPtr<>` types are translated as `const T&` instead of `T`.
1. The porter didn't translate 'foreach' statements properly if the container in question didn't define begin()/end() methods directly, but inherited them instead. This bug was fixed.
1. Now the `XmlTextReader` class contains the `InitLibXml2EntitySubstitution` method used to initialize `libxml2` before use.
1. The begin-end methods generation is improved.
1. The static initialization order is fixed in the `HttpWebRequest` class.
1. Sometimes the porter skips the `break` statement in the switch-case statement. Fixed.
1. A missing include is fixed when return means casting.
1. Operators for the `System::Xml::Schema` namespace enums are fixed.
1. Implementation of the `SslStream` class stalls in handshake process. It is rewritten using `Boost::beast` and `Botan::TLS::Stream`.
1. Now decimals are passed by reference when possible.
1. The conversion functions to make CodePorting.Native Cs2Cpp types work with Qt environment and examples to them were added to [examples repository](https://github.com/codeporting-native/codeporting-native-cs2cpp). Qt Creator debugger helper files allowing the user to inspect contents of CodePorting.Native Cs2Cpp types were added to [examples repository](https://github.com/codeporting-native/codeporting-native-cs2cpp/tree/master/qtcreator_debugging_helpers). [Documentation on how to use these](https://docs.codeporting.com/native/cs2cpp/developer-guide/qt-support/) was added.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4005 | Remove ASPOSECPP_SHARED_API from the html documentation | Task |
| CSPORTCPP-3545 | Speed up Linux builds | Task |
| CSPORTCPP-4024 | The porter must mark the default constructror as deleted for the static C# classes | Task |
| TASKSCPP-1607 | Improve foreach based loop performance in ported code. | Task |
| TASKSCPP-1610 | Implement performance test to fix up foreach performance in ported code. | Task |
| CSPORTCPP-4119 | Add Qt helpers to samples repository and documentation | Task |
| CSPORTCPP-4305 | Fixing the issue of building of asposecpplib using the gcc compiler in the debug configuration | Task |
| TASKSCPP-1611 | Fix foreach performance for string containers. | Task |
| WORDSCPP-1078 | Translate methods arguments as `const T&` instead of `T` | Task |
| SLIDESCPP-2710 | Port the dynamic parser to C++ | Task |
| CSPORTCPP-4332 | TimerTest.TimerTest1 is unstable | Bug |
| CSPORTCPP-4306 | Porter doesn't recognize begin-end methods on containers' descendants | Bug |
| CSPORTCPP-3284 | Develop strategy to improve 'develop' branch testing | Task |
| CSPORTCPP-4007 | Update 3rd party libraries | Task |
| TASKSCPP-1594 | Improve System::Drawing::Imaging::Metafile functional. | Task |
| CSPORTCPP-4341 | Support CodePorting.Native Cs2Cpp | Task |
| WORDSCPP-1081	| Tests which depend on auckland.dynabic.com resources doesn't work | Task |
| WORDSCPP-1082 | Regression in the switch statement porting | Task |
| TASKSCPP-1625 | Apply UNITY_BUILD option to speed up builds | Task |
| EMAILCPP-303 | Refactor SslStream using Boost::beast and Botan::TLS::Stream | Task |
| CSPORTCPP-3867 | Add usage examples for system classes | Task |
| TASKSCPP-1628 | Optimize Decimal class API - pass Decimals by reference when possible. | Task |
| WORDSCPP-815 | Implement XmlReader.CanResolveEntity property | Task |

## Public API and Backward Incompatible Changes ##

None.
