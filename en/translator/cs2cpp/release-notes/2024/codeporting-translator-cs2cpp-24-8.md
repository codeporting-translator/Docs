---
date: "2024-08-12"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 24.8"
linktitle: "CodePorting.Translator Cs2Cpp 24.8"
menu:
  docs:
    parent: "2024"
    weight: "2"
lastmod: "2024-08-12"
weight: "2"
---

## Major Features ##

None.

## Minor fixes ##

1. Fixed parsing of delegate expressions inside of lambda expressions.
1. `CppLambdaPass...` attributes adding now available in config files.
1. Options is added to exclude `DECLARE_ENUM` macros from appear in Doxygen output as functions.
1. Fixed initialization of a delegates with parameters by anonymous functions without them.

## Public API and Backward Incompatible Changes ##

None.
