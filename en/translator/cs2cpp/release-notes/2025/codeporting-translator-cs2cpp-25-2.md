---
date: "2025-02-10"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 25.2"
linktitle: "CodePorting.Translator Cs2Cpp 25.2"
menu:
  docs:
    parent: "2025"
    weight: "8"
lastmod: "2025-02-10"
weight: "8"
---

## Major Features ##

None.

## Minor fixes ##

1. Updated `HashAlgorithm::ComputeHash` method implementation to use abstract `HashCore` and `HashFinal` methods when `m_bt_hash_function` is empty.
1. `System::Details::ArrayView` is now passed to functions using a constant reference rather than by value.

## Public API and Backward Incompatible Changes ##

None.
