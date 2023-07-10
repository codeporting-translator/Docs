---
date: "2023-05-15"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.5"
linktitle: "CodePorting.Translator Cs2Cpp 23.5"
menu:
  docs:
    parent: "2023"
    weight: "3"
lastmod: "2023-05-15"
weight: "3"
---

## Major Features ##

None.

## Minor fixes ##

1. Fixed delegate initializer generation via `return` statement.
1. Fixed `System::String::LastIndexOf` misbehavior resulting in `ArgumentOutOfRangeException`.
1. The perfomance of `MemoryStream` has improved.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| CSPORTCPP-5818 | Delegate initializer do not generate for return expression | Bug  |
| PDFCPP-2285 | Improve performance of MemoryStream | Task |

## Public API and Backward Incompatible Changes ##

None.
