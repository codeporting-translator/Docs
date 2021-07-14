---
date: "2021-07-12"
author:
  display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.7"
linktitle: "CodePorting.Native Cs2Cpp 21.7"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-07-12"
weight: "1"
---

## Major Features ##

1. Properties (of pointer types), methods and indexers are now capable of returning constant references to backed fields. This behavior can be enabled for auto-properties using the new 'force_const_ref_return_type_for_auto_properties' option. For other properties, methods and indexers, use the CppConstRefReturnType attribute (only pointers and generic arguments are supported).
1. The System.Tuple class was supported.
1. The 'default' replecement of comment tag contents was introduced, see description of the 'documentation_comments_translation' configuration file node.
1. The 'force_enum_flags_attribute' option was supported by the porter. It generates the Flags-like operators for all enums being ported.

## Minor fixes ##

1. The porter used to crash with System.Runtime.CompilerServices.Unsafe lookup error on some machines. This was fixed.
1. The code generated for this object's indexer access was optimized.
1. Decimal rounding performance was improved.
1. In some schemes of inheritance, the porter didn't generate proper friend declarations to allow for internal members access. This was fixed.
1. Several issues were fixed with generating method overloads that accept the STL streams instead of Aspose ones. The CppIOStreamWrapperAttribute attribute was revised.
1. The System::Xml::Xsl::XslCompiledTransform::Transform() method used to throw exceptions on some documents. This was fixed.
1. Generation of the SetTemplateWeakPtr method on the nested classes was fixed.
1. User-defined conversion operators translation was fixed for the case of the generic types.
1. Compilability of the property compound assignment operators on GCC 6 was fixed.
1. The CppConstWrapper attribute misplaced method comments in translated code. This was fixed.
1. Inclusion related to assembly reflection support was fixed to avoid including internal headers from the public ones.

Please consult respective sections of our wiki for more information.

## Full List of Issues Covering all Changes in this Release ##

| Key | Summary | Category |
| --- | --- | --- |
| TASKSCPP-1631 | Implement porter option for auto-properties getters, which made return type const SmartPtr&lt;&gt;&amp; (for reference types) | Enhancement |
| CSPORTCPP-4370 | System.Runtime.CompilerServices.Unsafe: file or assembly not found | Bug |
| CSPORTCPP-4371 | Missing friend declaration caused by inheritance | Bug |
| CSPORTCPP-3945 | Improve stream wrappers generation. | Enhancement |
| TASKSCPP-1591 | Enable CppConstRefReturnType attribute appliance to indexer properties and methods. | Task |
| TASKSCPP-1634 | Fix decimal construction from floating point types for NaN/INF cases. | Bug |
| SLIDESCPP-2980 | Implement System.Tuple class | New feature |
| PDFCPP-1613 | XslCompiledTransform::Transform throws exception | Bug |
| SLIDESCPP-2999 | Porter: Fix SetTemplateWeakPtr method export for nested classes | Bug |
| SLIDESCPP-2981 | Improve translation of user-defined conversion operators | Enhancement |
| SLIDESCPP-2996 | Porter: Fix CppIgnoreBaseType attribute behavior | Task |
| CSPORTCPP-4402 | Fix comments generated when CppConstWrapper attribute is applied | Bug |
| PDFCPP-1611 | C# examples in API Reference for C++ | New feature |
| SLIDESCPP-2997 | Porter: Fix binary operations porting for enumerations without Flags attribute | New feature |
| TASKSCPP-1639 | Implement CppConstRefReturnType support for generic parameters | New feature |
| PDFCPP-1620 | Preparing to release, test "Release build for build-21.7.0.337" | Task |

## Public API and Backward Incompatible Changes ##

1. The CppIOStreamWrapper attribute now has argumentless constructor. 2-argumented form was retired - use 3 or 1 arguments instead.
1. The behavior of the CppIgnoreBaseType attribute was made consistent. From now on, C++-styled type names are not supported as arguments, and System.Object inheritance can only be ignored explicitly.