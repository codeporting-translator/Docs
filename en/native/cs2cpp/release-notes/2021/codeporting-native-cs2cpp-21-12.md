---
date: "2021-12-09"
author:
display_name: "Dmitry Baskakov"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp 21.12"
linktitle: "CodePorting.Native Cs2Cpp 21.12"
menu:
  docs:
    parent: "2021"
    weight: "1"
lastmod: "2021-12-09"
weight: "1"
---

## Major Features ##
1. The compilation issues with GCC 10.0 and Clang 13.0 were fixed across our headers and in the generated code.
1. The performance of the `Stream` and `TextWriter` methods was improved. The methods of these classes no longer copy the smart pointers that are passed to them. The API for these classes and their subclasses was changed accordingly.
1. The `System::Collections::Generic::IEnumerable<T>::get_Current()` method now returns const references to pointers and strings to avoid the copying where possible. This increases the performance. The porter avoids returning reference to the temporary variables by generating service code which assigns such temporary values to enumerator's field. The `CppDisableEnumeratorCurrentValueHolder` and `CppEmitEnumeratorCurrentValueHolder` attributes and the `emit_enumerator_current_value_holder` option were introduced. They can be used to impose manual control over such behavior.
1. The packaging scheme for the Linux OS was changed. The single CodePorting.Native.Cs2Cpp_x86_64_libstdcpp_21.12.zip package is now provided instead of compiler-specific versions. The single libaspose_cpp_x86_64_libstdcpp_libc2.23.so library should now be used with any compiler on the Linux OS.

## Minor fixes ##
1. The exceptions thrown by the `Socket` and `Dns` classes were improved.
1. The `class_ptr_alias` option was supported by the porter. For each public class, the porter generates `Ptr` member type which is an alias to the smart pointer for this class when this option is enabled.
1. The code of `Array::CopyTo()` and `ArrayView::CopyTo()` methods was simplified.
1. The overloads were added to `RandomNumberGenerator` and `RNGCryptoServiceProvider` methods that work with `ArrayView` and `StackArray` instead of `ArrayPtr`.
1. The access violation was fixed in the `HttpWebResponse::GetResponseHeader()` method.
1. The `HMACSHA512` class was implemented.
1. The porter no longer generates unwanted `ToString()` calls when translating the additional test failure message.
1. The porter no longer generates unwanted forward declarations when translating the generic delegates.
1. More usage examples were added to the API reference.
1. The indentation was fixed in the generated code for the member initializers.
1. The conditions of generating the `SetTemplateWeakPtr()` method were fixed.
1. Unwanted Boost symbols are no longer exported from Linux builds.
1. The `System::String` to `std::*string` conversion operators were moved to the `string.h` header.
1. The memory management issue was fixed inside the `System::String::Join()` method.
1. The `force_const_ref_return_type_simple_properties` porter option was improved. When it is enabled, the porter generated incorrect code for the properties where the property type didn't match the type of the field being returned. This was fixed. Also, this option now supports the properties with more than one return statement and with the field of the field (of the field, and so on) being returned. Previously, only a single return statement with this class'es fields were supported.
1. The bug was fixed in the porter. The overload methods with `iostream`-like arguments were ill-formed in case of non-void return types. This was fixed.

## Full List of Issues Covering all Changes in this Release ##
| Key | Summary | Category |
| --- | --- | --- |
| EMAILCPP-316 | Prepearing Aspose.Email for C++ Release 21.10 | Task |
| CSPORTCPP-4689 | Review code changes | Task |
| CSPORTCPP-3916 | Upgrade Linux compilers | Task |
| SLIDESCPP-3177 | Improve System::IO::Stream class performance | Enhancement |
| SLIDESCPP-3178 | Improve System::IO::TextWriter class pefrormance | Enhancement |
| CSPORTCPP-4730 | Remove redundant forward declarations for delegates | Enhancement |
| CSPORTCPP-3867 | Add usage examples for system classes | New feature |
| CSPORTCPP-4731 | Fix styling for constructor initializer list | Enhancement |
| CSPORTCPP-4732 | Fix bug with SetTemplateWeakPtr | Bug |
| WORDSCPP-1140 | lld linker reports ld.lld: warning: found local symbol | Bug |
| CSPORTCPP-4634 | Unify Linux packages | Task |
| TASKSCPP-1681 | Enhance force_const_ref_return_type_simple_properties porter option appliance. | Enhancement |
| TASKSCPP-1682 | Avoid CODEPORTING_CURRENT_RETTYPE macro usage in library and ported code | Enhancement |
| PDFCPP-1732 | Add support to RandomNumberGenerator and depended classes | Task |
| BARCODECPP-478 | Add fixes to Metered License support | Task |

## Public API and Backward Incompatible Changes ##
1. The `get_Current` method of the `IEnumerable` class and its inheritors now returns the value by reference instead of returning by value when a collection stores reference types or strings. The implementations of this method should be updated where applicable.
2. Methods of `Stream`, `TextWriter` and their inheritor classes now accept arguments by const reference instead of copying pointers. The overriding methods should be updated where applicable.
3. The suffix of the library built for Linux OS was changed. Now the code built using any compiler should be linked against libaspose_cpp_x86_64_libstdcpp_libc2.23.so. libaspose_cpp_gcc6.so and libaspose_cpp_clang3_libstdcpp.so are no longer provided.
4. The supported glibc version may be changed in the upcoming releases.
5. The new implementation of the classes that belong to the `System::Xml` namespace will be provided in one of the upcoming releases. Minor API changes are to be expected.
6. The new `IEnumerable`-level iterators and iterators for collections with duck typing will be introduced in one of the upcoming releases. The code which is dependent on `EnumeratorBasedIterator` or `DuckTypedIterator` may require some changes.
7. The `CODEPORTING_CURRENT_RETTYPE` macro was deprecated. It will be removed in one of the upcoming releases.