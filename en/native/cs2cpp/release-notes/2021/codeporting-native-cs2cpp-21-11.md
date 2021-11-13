---
date: "2021-11-11"
author:
display_name: "Nikolay Shestakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.11"
linktitle: "CodePorting.Native Cs2Cpp 21.11"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-11-11"
weight: "1"
---

## Major Features ##
1. API reference was improved for multiple classes. Usage examples were added. Some misprints were fixed.
2. The Skia version used was updated.
3. Now the `CODEPORTING_CURRENT_RETTYPE` macro is used as the return type of the `get_Current` method. Now it's just a stub that do not modify its argument type, but in one of upcoming releases, it will return a reference instead of a shared pointer copy in the collections that store reference types or strings.
4. Forward declarations are sorted in alphabetical order as much as it's possible without violating topological sorting.
5. Namespaces that contain forward declarations are opened and closed as rare as possible.
6. Now Skia is built without the OpenGL support.
7. Now the porter supports using of the `CppArgumentKind` attribute with an argument type name parameter.
8. Now the `CppArrayOnStack` attribute can be applied to multidimensional arrays.
9. Implemented text alignment when using Graphics::DrawString with a given rectangle.

## Minor fixes ##
1. `NullReferenceException` is thrown when sorting elements of the `List`- and `Array`-class instances that contain null elements. Fixed.
2. An incorrect behaviour of the cycles detection mechanism is fixed.
3. The `IPAddress`- and `IPEndPoint`-class static data initialization order is fixed.
4. Now `ArrayView` can be passed as an argument to the `Array.Copy` method.
5. `CheckedCast` doesn't throw `OverflowException` when the casted number is out of the value range. It also causes the compilation error (by warning C4018).
6. The `Bitmap::set_Palette` method is optimized. It worked slowly with large images.
7. The new template was added for the case when the setter accepts `const PropT&` and the getter returns `PropT`. It happens when `force_const_ref_parameters = true`.
8. The delegate translation was improved.
9. The number of `GraphicsPath` points was fixed when the `AddString` method is used to add the tabulation symbol.
10. The `begin` and `end` methods were added to the `ArrayView` and `StackArray` classes.
11. Now the `StackArray`-class instance is used instead of `std::array` as a parameter in the `ArrayView` constructor.
12. Now the porter supports extension methods for the primitive types.
13. The `System.Drawing.Drawing2D.AdjustableArrowCap` class was implemented.
14. The `TextureBrush` constructor now behaves as in .Net.
15. The implicit `object.Equals(object, object)` method call is ported to `Equals(left, right)` instead of `System::Object::Equals(left, right)`.
16. The porter doesn't translate the `cref` tag when another project contains the specified type. Fixed.
17. Now the `force_const_ref_return_type_for_auto_properties` option is applied to the simple getters that contain only one return statement.
18. Now the porter supports global enums with the `Flags` attribute.

## Full List of Issues Covering all Changes in this Release ##
| Key | Summary | Category |
| --- | --- | --- |
| WORDSCPP-1135 | Regular memory leaks check | Task |
| CSPORTCPP-3867 | Add usage examples for system classes | Enhancement |
| SLIDESCPP-3202 | Fix cycles detection mechanism failure | Task |
| SLIDESCPP-3197 | Fix IPAddress and IPEndPoint classes static data initialization order | Task |
| PDFCPP-1712 | Add Array.Copy support for ArrayView class [asposecpplib] | Task |
| SLIDESCPP-3201 | Fix CheckedCast compilation and behavior | Task |
| PDFCPP-1647 | HTML.Builder optimization | Task |
| SLIDESCPP-3191 | Porter: Improve delegates translation | Task |
| SLIDESCPP-3154 | Fix RegressionTests_v21_10.SLIDESNET_42542 test | Task |
| PDFCPP-1705 | Changes in asposecpplib for ArrayView class | Task |
| WORDSCPP-1138 | Add support for porting extension methods for value types (int, long, etc.) | Task |
| SLIDESCPP-2949 | Implement System.Drawing.Drawing2D.AdjustableArrowCap class | Task |
| PUBCPP-33 | modify wrap mode in TextureBrush constructor | Task |
| WORDSCPP-1139 | Incorrect porting of implicit object.Equals(a,b) invocation expression | Task |
| CSPORTCPP-4651 | Investigate Linux so files size growth | Investigation |
| SLIDESCPP-3171 | Porter: Improve cref tags translation | Task |
| CSPORTCPP-4661 | Support global enums with Flags attribute | Task |
| CSPORTCPP-4557 | Sort forward declarations where possible | Enhancement |
| WORDSCPP-1125 | Build skia without OpenGL support | Task |
| SLIDESCPP-3170 | Porter: Enhance CppArgumentKind attribute in config with an argument type name parameter | Task |

## Public API and Backward Incompatible Changes ##
