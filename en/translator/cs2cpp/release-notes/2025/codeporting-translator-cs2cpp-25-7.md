---
date: "2025-07-15"
author:
  display_name: "Sanzharbek Zhalilov"
draft: "false"
toc: true
title: "CodePorting.Translator Cs2Cpp 25.7"
linktitle: "CodePorting.Translator Cs2Cpp 25.7"
menu:
  docs:
    parent: "2025"
    weight: "4"
lastmod: "2025-07-15"
weight: "4"
---

## Major Features ##

None.

## Minor fixes ##

1. Implemented the Max property for SortedSet.
1. Fixed the behavior of ScopeGuard desctructor logic. Now safely handles exceptions during stack unwinding by wrapping the cleanup call into a try/catch block if an exception is active.
1. Implemented the method System::SharedPtr<RandomNumberGenerator> RandomNumberGenerator::Create().

## Public API and Backward Incompatible Changes ##

1. Removed Emscripten from the release bundle — builds for this platform are no longer included in the release.
 