---
date: "2025-03-10"
author:
  display_name: "Vitaliy Molchanov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 25.3"
linktitle: "CodePorting.Translator Cs2Cpp 25.3"
menu:
  docs:
    parent: "2025"
    weight: "2"
lastmod: "2025-03-10"
weight: "2"
---

## Major Features ##

None.

## Minor fixes ##

1. Singletons are optimized. `std::call_once` and mutex are removed from singleton value initialization code.
1. The license verification mechanism has been updated and fixed.
1. `System::ComponentModel::DoWorkEventHandler` type definition is added in separate header file.
1. Fixed `allow_interface_members_base_class_impl` option for 'out ByRef' prameters case.

## Public API and Backward Incompatible Changes ##

None.
