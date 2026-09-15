---
date: "2026-09-15"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 26.9"
linktitle: "CodePorting.Translator Cs2Cpp 26.9"
menu:
  docs:
    parent: "2026"
    weight: "1"
lastmod: "2026-09-15"
weight: "1"
---

## Major Features ##

1. Updated [online documentation](https://products.codeporting.com/translator/csharp-to-cpp/docs/) has been added.

## Minor fixes ##

1. The logging logic has been significantly reworked: problem identifiers were added, a way to control severity via command-line flags was introduced, and the internal architecture was redesigned.
1. Fixed the `force_static_cast` option in some cases.
1. Fixed on-stack arrays when declared using the **var** keyword.
