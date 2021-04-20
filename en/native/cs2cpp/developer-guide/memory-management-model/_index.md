---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
title: "Memory Management Model"
linktitle: "Memory Management Model"
menu:
  docs:
    parent: "Developer Guide"
    weight: "7"
lastmod: "2019-05-28"
weight: "7"
layout: base-home
---

In C#, memory management rules are simple: all objects allocated on heap exist until references to them exist, and are collected by GC afterwards. In C++, this is not the case.

To enable executing ported code in C++ environment, CodePorting.Native Cs2Cpp wraps all references to objects created on heap into smart pointers of custom System::SmartPtr type. These pointers follow intruisive pointer semantics which means that reference counter resides in allocated object itself and all smart pointers share same reference counter regardless how they were created.

Having same wrapping rules across whole environment allows following native C# memory management rules in ported code. This means, any function defined in any module can allocate any object and use it any way required; the object will be deleted when and only when no more shared references to it exist. The developer shouldn't make any special arrangements to track down object deletion points.

On the other hand, those types which are usually allocated on stack in C#, are considered value types in ported code. No references to them are created by CodePorting.Native Cs2Cpp as they are always passed by value.

When working with ported code, same rules should apply to manually-written one. This means:

1. Use System::MakeObject() function to allocate object if it is allocated on heap in C#.
1. Use System::SmartPtr class to keep references to such objects.
1. Do not allocate C# heap types on stack as this can lead to memory management issues (e. g. calling 'delete' for stack-allocated objects).
1. Allocate C# value types on stack.

There are some exceptions as well.

1. Exception classes should always be allocated on stack in C++. Never create instance of exception class using 'new' or 'System::MakeObject'.
1. Some special C# types such as System::String become unconditional value types in C++.

Managing memory via shared pointers introduces loop reference issues. If two or more objects hold shared pointers to each other, reference loop is created which prevents all objects in charge from being destroyed. To break such a reference loop, use weak pointers. If reference loop comes from C#, use `CppWeakPtr` attribute to do so (see [CodePorting.Native Cs2Cpp attributes](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-attributes/) for details). In C++ you may use System::WeakPtr class directly.

Unfortunately, some usecases that look normal in C# can't be reproduced in terms of shared and weak pointers. For example, consider two classes called Actor and Config. Config class represents Actor configuration, while Actor performs some actions based on referenced Config. Both classes have references to each other. If your API allows the user to create any of these two classes in such a manner that the second one is created automatically, you can't tell which reference is weak and which is strong. In such cases, code refactoring is required to fix memory management issues.