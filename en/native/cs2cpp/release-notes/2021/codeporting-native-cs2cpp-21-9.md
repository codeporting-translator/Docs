---
date: "2021-09-10"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.9"
linktitle: "CodePorting.Native Cs2Cpp 21.9"
menu:
  docs:
    parent: "2021"
    weight: "4"
lastmod: "2021-09-10"
weight: "4"
---

## Major Features ##

1. The new CppArrayOnStack attribute was supported by the porter. This attribute allows allocating constant length arrays on stack in the ported code.
1. The macros were introduced to simplify defining custom exceptions. See [C++ user-defined exception classes](https://docs.codeporting.com/native/cs2cpp/developer-guide/cpp-user-defined-exception-classes/) for more details.
1. The new CppArrayInnerIndexer attribute was supported by the porter which allows the ported code to use the members of internal vector to access array elements instead of .Net-styled accessors implemented at System::Array and System::ArrayPtr level. This increases the performance by the cost of skipping the boundary checks.
1. The GroupBy LINQ method was supported.
1. The forward declarations in the generated code are now grouped by namespaces and ordered to avoid violating C++ declare-before-use rule. Several related inclusion generation issues were fixed.
1. The external_include configuration file node was added which makes the porter to insert custom inclusion lines into generated files.
1. The CppAddFunctionArgument and CppPassFunctionArgument attributes were added which make the porter to generate and pass additional arguments for the methods being translated.

## Minor fixes ##

1. The CppWeakPtr attribute now works with the generic argument-typed fields and properties.
1. The force_const_ref_return_type_for_auto_properties option now works with the generic argument-typed properties.
1. The previously uncompiled references to null exceptions in the ported code are now replaced with references to System::Exception::NullException which doesn't create any ambiguity with its type.
1. Some code smell issues were fixed in the porter's code. Some code was moved between the DLLs.
1. The CppInline attribute now works with property getters and setters.
1. The work of the CppPortConstStringAsWChar was fixed for some cases.
1. The UnsafeConst value of the CppArgumentKind attribute was fixed for the char pointers.
1. The size of the CodePorting.Native Cs2Cpp API library and ported binaries was decreased. Implementation of some internal library members were changed.
1. The types referred by the in-config attribute declarations can now use short C# notation (like 'int' or 'char') instead of fully qualified one (like 'System.Int32' or 'System.Char').
1. The GraphicsPath rendering was fixed for some cases of closed figures.
1. The support of the System::Tuple class in the porter was improved. It is now possible to replace custom tuples with the system one using typemap records in the configuration file.
1. The rendering of bitmaps with different DPIs at the same bitmap was fixed.
1. Some compilation issues for various compilers were fixed in the library code.
1. The compilation of the translated foreach statement for duck-typed collections was fixed in some cases.
1. Some unneccessary locking was fixed in the porter which improves performance in some cases on multicored machines.
1. The export of protected internal members was fixed in the porter.
1. The unneccessary export of template class members in the ported code was fixed.
1. The System::Text::Encoding::GetEncoding() method now throws ArgumentException if there are non-ASCII symbols in the encoding name.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| SLIDESCPP-3003 | Implement String.Join(String, Object[]) method | New feature |
| TASKSCPP-1647 | Implement CppWeakPtr attribute appliance to properties/fields which type is a template parameter | Task |
| SLIDESCPP-3088 | Porter: Improve the translation of exceptions | Enhancement |
| CSPORTCPP-4501 | Do small fixes to code | Enhancement |
| PDFCPP-1648 | Implement new attribute ArrayOnStack for porter | New feature |
| CSPORTCPP-4451 | Review user-defined exceptions: article and approach | Enhancement |
| SLIDESCPP-3083 | Implement BinaryWriter.Write(String) method | New feature |
| SLIDESCPP-3107 | Porter: Fix CppInline attribute usage for getters/setters | Enhancement |
| SLIDESCPP-3108 | Porter: Fix CppPortConstStringAsWChar attribute usage for code within System namespace | Bug |
| SLIDESCPP-3109 | Porter: Fix CppArgumentKind attribute with UnsafeConst parameter usage for pointers to char | Bug |
| WORDSCPP-1120 | Analyze and reduce the size of the binary artifacts | Enhancement |
| PDFCPP-1660 | Fix porter attribute reader for reading short CSharp types like uint, ulong, e.t.c | Enhancement |
| PDFCPP-1661 | CppArrayInnerIndexer - submit on MR. | New feature |
| EMAILCPP-309 | Implementing Linq GroupBy functionality in asposecpplib | New feature |
| PAGECPP-36 | Create test and MR for GraphicsPath::AddPath fix | Bug |
| SLIDESCPP-3113 | Porter: Improve System.Tuple class handling | Enhancement |
| SLIDESCPP-3102 | Improve Graphics::DrawImage() and Graphics::DrawImageUnscaled() methods | Bug |
| CSPORTCPP-4528 | Separate code dependent on NRefactory from CsToCppPorter.Settings | Task |
| CSPORTCPP-4537 | foreach translation doesn't compile | Bug |
| EMAILCPP-310 | Prepearing Aspose.Email for C++ Release 21.7 | Task |
| PDFCPP-1669 | Compatibility between CppArgumentKind raw pointers and CppArrayOnStack attributes | Task |
| CSPORTCPP-4536 | Support CodePorting.Native Cs2Cpp | Task |
| SLIDESCPP-3119 | Porter: Fix 'protected internal' access modifier translation | Bug |
| CSPORTCPP-3296 | Group forward declarations by namespace | Enhancement |
| CSPORTCPP-4550 | Check and fix release | Task |

## Public API and Backward Incompatible Changes ##

1. The new overload of System::String::Join() was supported.
1. The string overloads of the System::IO::BinaryWriter::Write() method were implemented.
1. The CURRENT_NAMESPACE variable introduced by EXCEPTION_NAMESPACE macro was replaced with a local function.
1. The new constructors were added into System::TypeInfoPtr structure.
1. The System::static_holder class was changed into a function.
1. Some classes from System::Net were given missing virtual inheritance.
