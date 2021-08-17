---
date: "2021-08-14"
author:
  display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.8"
linktitle: "CodePorting.Native Cs2Cpp 21.8"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-08-14"
weight: "1"
---

## Major Features ##

1. The `XslCompiledTransform.Transform(XmlReader input, XsltArgumentList arguments, XmlWriter results)` method is added.
1. The `XsltArgumentList::AddParam` method is fixed. A parameter value is always wrapped into the single quotes.
1. Now the `CppArgumentKind` attribute can be used to pass arguments of the `Type` type by value.
1. Now the porter is capable of generating begin-end methods for the classes that don't implement the `IEnumerable` interface.
1. `ArgumentKind.UnsafeConst` is added. It is used to translate parameters as unsafe pointer to const data.
1. `ArgumentKind.ArrayRawPointer` and `ArgumentKind.ConstArrayRawPointer` are added. They are used to translate parameters as raw pointers (to avoid extra check in [] operator).

## Minor fixes ##

1. Now the porter logs messages when unobfuscatable items are used in obfuscated headers.
1. The performance of the porter was fixed when generating includes list with the `forwarding_if_possible` option being disabled.
1. The bug was fixed in the porter with the replacement of the documentation tags.
1. The bug was fixed in the porter forcing it to keep class comments even if `keep_documentation_comments` option was disabled.
1. The performance of by-tagname XmlNode lookup was improved.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4348 | Generate begin-end for those types not implementing IEnumerable interface | Task |
| PDFCPP-1630 | Comments replacement issue | Task |
| PDFCPP-1626 | Implement XslCompiledTransform.Transform(XmlReader input, XsltArgumentList arguments, XmlWriter results) | Task |
| CSPORTCPP-4447 | Support CodePorting.Native Cs2Cpp | Task |
| SLIDESCPP-3005 | Porter: Allow passing of System.Type arguments by value | Task |
| SLIDESCPP-2994 | Porter: Fix keep_documentation_comments option behavior | Bug |
| CSPORTCPP-4457 | Support CodePorting.Native Cs2Cpp | Task |
| PDFCPP-1642 | Porter Improvment | Task |
| CSPORTCPP-4394 | Support CodePorting.Native Cs2Cpp | Task |
| TASKSCPP-1641 | Optimize XmlNode search by name (use std::string's manipulations instead of System::String's one.) | Enhancement |

## Public API and Backward Incompatible Changes ##

None.
