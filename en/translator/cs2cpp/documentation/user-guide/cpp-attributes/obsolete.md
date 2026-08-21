---
order: "9"
navTitle: "Obsolete"
---

# Obsolete C++ Attributes #

This section lists translator attributes that were previously used but are now ignored for one reason or another. These attributes still remain in the assembly and adding them does not produce a compilation error, but they are marked with the `[System.Obsolete]` attribute and will therefore generate a corresponding warning. Do not use them and remove them from your C# code and config files as soon as possible.

## CppAllowBoxing ##

**Used on**: Structures

**Arguments**: None

Allows boxing for this type. This requires the type to implement operator == (), ToString() const and GetHashCode() const.

**Obsolete since version**: 26.8

**Reason for obsolescence**: All structures now automatically support boxing by inheriting from the `System::Details::BoxableObjectBase` tag type.

## CppDisableEnumeratorCurrentValueHolder ##

**Used on**: Class or structure

**Arguments**: None

Disables value holding in a particular enumerator class.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Not in use.

## CppDoNotObfuscate ##

**Used on**: Entity

**Arguments**: None

Disables entity obfuscation.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Obfuscation is no longer supported.

## CppEmitEnumeratorCurrentValueHolder ##

**Used on**: Class or structure

**Arguments**: None

Emits value holding in a particular enumerator class.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Not in use.

## CppExactArrayInitializer ##

**Used on**: Array type field

**Arguments**: None

Passes array initializer expression directly from C# to C++ without parsing and code generation. Speeds up long initializers (like thousands of string elements, etc.).

**Obsolete since version**: 26.8

**Reason for obsolescence**: Not in use.

## CppForceDynamicCastFromTypeParam ##

**Used on**: Generic type or method

**Argument**: String name of template parameter to affect

Forces dynamic casts from argument type parameter to Object instead of static casts used by default.

**Obsolete since version**: 22.9

**Reason for obsolescence**: The need for dynamic casting is now determined at C++ code compilation time, not at translation time, so such attributes are no longer required.

## CppForceDynamicCastToTypeParam ##

**Used on**: Generic type or method

**Argument**: String name of template parameter to affect

Forces dynamic casts from Object to argument type parameter instead of static casts used by default.

**Obsolete since version**: 22.9

**Reason for obsolescence**: The need for dynamic casting is now determined at C++ code compilation time, not at translation time, so such attributes are no longer required.

## CppForceObfuscate ##

**Used on**: Entities

**Arguments**: None

Forces attributed entity to be obfuscated if 'obfuscate_cpp_headers' option is enabled.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Obfuscation is no longer supported.

## CppNoAbstract ##

**Used on**: Class

**Arguments**: None

Omit 'abstract' mark when translating the class.

**Obsolete since version**: 26.8

**Reason for obsolescence**: `ABSTRACT` macro never generate for abstract C++ types from now.

## CppOverride ##

**Used on**: Method

**Arguments**: None

Makes translator mark method with 'override' qualifier.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Not in use.

## CppUnknownTypeParam ##

**Used on**: Generic type or method

**Argument**: String name of template parameter to treat as unknown type

Force treating type argument as unknown type: calling ObjectExt::UnknownToObject() and ObjectExt::ObjectToUnknown() whenever the conversion is required. Fixes some type conversion issues with type parameters.

**Obsolete since version**: 22.9

**Reason for obsolescence**: The need for dynamic casting is now determined at C++ code compilation time, not at translation time, so such attributes are no longer required.

## CppValueTypeParam ##

**Used on**: Generic type or method

**Argument**: String name of template parameter to treat as value type

Force treating type argument as value type: boxing, etc.

**Obsolete since version**: 26.8

**Reason for obsolescence**: Not in use.

## CppStaticMethod ##

**Used on**: Method

**Argument**: None

Forces method which is otherwise non-static to be translated as static. All members of the class that are accessed by such a method must also be made static.

**Obsolete since version**: 26.9

**Reason for obsolescence**: This attribute is too complex to fully implement, and its boundaries and purposes are difficult to formalize. It was not fully implemented in the old translator and was never used for its intended purpose.
