---
order: "2"
navTitle: "Next-gen translator"
---

# About next-gen translator #

Actual **CodePorting.Translator Cs2Cpp** is an C# to C++ transpiler based on open-source Roslyn (.NET Compiler Platform) project. The old-generation porter used NRefactory, which is a legacy library and limited to C# 5 - Roslyn is a project in active development, and it supports the [most modern version](new-features.md) of the language for now - C# 13 (and there is a high probability, that it will support future versions of C#). It means, that the next-generation translator could potentially support the porting of modern C# in the future: actually, it is the main motivation of the development of this product for now. Developers of the translator are trying to save the same code style of translated C++ code used in the old-generation porter to make the future migration process to the next-generation porter easier.

The format and command line flags of the console application of the new translator are the same as those of the old translator (except for some minor ones). The new translator works with the same configuration files, uses the same attributes in C# code as the old one and generates code that is as similar to it as possible. The formatting, order of **#includes**, some rules for simplifying type names or generation of anonymous functions may differ, but the code should be semantically identical. The migration from old translator to actual one is quite easy and described in [mirgration](migration.md) page.

The console output of the new translator is different. It also displays the main stages of translation, warnings and translation errors, including internal errors of the translator itself, but in a completely different form. If you'll see some error message (they are displayed in red in the Windows console), the development team should be notified, as correct code translation is not guaranteed in this case.
