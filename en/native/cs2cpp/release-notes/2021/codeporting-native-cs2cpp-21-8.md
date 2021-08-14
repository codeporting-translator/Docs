---
date: "2021-08-14"
author:
display_name: "Dmitry Baskakov"
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
1. Now the `GenerateBeginEndMethods` attribute can be applied to classes that don't implement the `IEnumerable` interface.
1. `ArgumentKind.UnsafeConst` is added. It is used to translate parameters as unsafe pointer to const data.
1. `ArgumentKind.ArrayRawPointer` and `ArgumentKind.ConstArrayRawPointer` are added. They are used to translate parameters as raw pointers (for avoid extra check in [] operator).

## Minor fixes ##

1. Now the porter logs messages when unobfuscatable items are used in obfuscated headers.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-4348 | Generate begin-end for those types not implementing IEnumerable interface | Task |
| PDFCPP-1630 | Comments replacement issue | Task |
| PDFCPP-1626 | Implement XslCompiledTransform.Transform(XmlReader input, XsltArgumentList arguments, XmlWriter results) | Task |
| CSPORTCPP-4447 | Support CodePorting.Native Cs2Cpp | Task |
| SLIDESCPP-3005 | Porter: Allow passing of System.Type arguments by value | Task |
| CSPORTCPP-4457 | Support CodePorting.Native Cs2Cpp | Task |
| PDFCPP-1642 | Porter Improvment | Task |

## Public API and Backward Incompatible Changes ##

None.
