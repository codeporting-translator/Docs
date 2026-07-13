---
date: "2026-07-13"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 26.7"
linktitle: "CodePorting.Translator Cs2Cpp 26.7"
menu:
  docs:
    parent: "2026"
    weight: "1"
lastmod: "2026-07-13"
weight: "1"
---

## Major Features ##

1. Added "inherit" configuration attribute parameter to control whether it applies to inherited classes or not.
1. 'using var d = new SomeDisposable()' syntax is supported now.
1. Implemented 'LINQ_Average' and 'LINQ_Skip' methods.
1. List patterns like 'collection is [1, var center, 10]' are implemented.
1. Indexes and ranges are supported now.

## Minor fixes ##

1. Fixed some 'using' constructs inside asynchronous methods.
1. Delegate generation code refactored. Fixed some rare errors.
1. Implemented some nUnit constructs that were not supported previously.
1. Many fixes are done for 'force_const_ref_parameters' option.
