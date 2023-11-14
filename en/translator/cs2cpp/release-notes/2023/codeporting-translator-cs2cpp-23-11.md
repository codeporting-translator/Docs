---
date: "2023-11-14"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 23.11"
linktitle: "CodePorting.Translator Cs2Cpp 23.11"
menu:
  docs:
    parent: "2023"
    weight: "1"
lastmod: "2023-11-14"
weight: "1"
---

## Major Features ##

None.

## Minor fixes ##

1. Forced `TextWriter` flushing in `XmlTextEncoder` to reserve buffer space for the surrogate pair.
1. Implemented `TestCase` with nullable parameter.
1. `InvalidEnumArgumentException` definition is fixed.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3788 | Fix RegressionTests_v23_1.SLIDESNET_43486 test | Task |
| PDFCPP-2557 | TestCase porting for Nullable types | Task |
| PDFCPP-2556 | InvalidEnumArgumentException incorrect definition | Task |

## Public API and Backward Incompatible Changes ##

None.
