# Configuration file nodes #

This section lists all tags allowed in configuration file.

[TOC]

## csproj ##

```xml
<csproj path="Path/ProjectName.csproj" cfg="Configuration" platform="Platform"/>
```

Reference to input project file.

Attributes:

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| path | Path to project file | Yes |
| cfg | Configuration used when parsing source C# code | No (default `Debug`) |
| platform | Platform to use specific settings for when parsing the project | No (default or first available platform as defined in input project file) |

This attribute overrides path to project given to a translator application via command line.

## outdir ##

```xml
<outdir path="Path"/>
```

Reference to output directory.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| path | Path to output directory | Yes |

## embedded_proj ##

```xml
<embedded_proj path="path/to/project"/>
```

Defines paths to embedded projects used in project being translated.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| path | Full or relative path to embedded project | Yes |

## cppproj ##

```xml
<cppproj name="ProjectName"/>
```

Allows it to specify output project name.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Output project name | Yes |

## opt ##

```xml
<opt name="Name" value="Value"/>
```

Translator option. See [configuration file options](options.md) for details on what options are available.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Option name | Yes |
| value | Option value | Yes |
| _Option-specific attributes_ | Some options can require additional attributes to be specified | Defined by option |

## additional_source ##

```xml
<additional_source>
    <copy dir="dir1"/>
    <copy dir="dir2"/>
</additional_source>
```

Directories to copy additional C++ sources from. Only 'copy' subnodes are allowed inside 'additional_source' one.

### copy ###

```xml
<copy dir="dir1"/>
```

Single directory to copy additional C++ source files from.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| dir | Path to directory (absolute or relative to configuration file; can also be relative to translator directory if 'use_porter_home_directory_while_resolving_path' option is enabled) | Yes |

## import ##

```xml
<import config="details.config"/>
```

Imports configuration file as if all its contents were added into the current one. Imported configuration file must have valid structure ('porter' node in the root and so on).

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| config | Path to configuration file (absolute or relative to current configuration file; can also be relative to translator directory if 'use_porter_home_directory_while_resolving_path' option is enabled) | Yes |

## libs, style ##

```xml
<libs>
    <lib name="a">
        ...
    </lib>
</libs>
<style>
    <opt name="b" value="c"/>
    ...
</style>
```

Logical grouping. 'libs' element defines configuration file section which contains instructions on libraries attachment. 'style' element is meant to group style-related options.

Only 'opt' and 'lib' nodes are allowed inside both tags. Using 'opt' and 'lib' outside of 'libs' or 'style' is also allowed.

### lib ###

```xml
<lib name="LibProject.Cpp">
    <tag path="path/path">Namespace::Namespace2::</tag>
    <cmake_part_template>
        find_botan()
    </cmake_part_template>
    <cmake_link_template>
        target_link_libraries(Project_name PUBLIC botan::botan LibProject::LibProject)
    </cmake_link_template>
    <defines>DEFINE1 DEFINE2;DEFINE3=VALUE</defines>
    <includes>path/to/dir1 path/to/dir2;path/to/dir3</includes>
    <libdirs>path/to/dir1 path/to/dir2;path/to/dir3</libdirs>
    <class name="LibProject::Class1" path="libproject/class1.h" shortptr="true"/>
    <enum name="LibProject::Enum1" path="libproject/enum1.h"/>
    <namespace name="LibProject::Namespace1" path="libproject/namespace1.h" shortptr="true"/>
    <replace_user_types>false</replace_user_types>
</lib>
```

Imports library to use against current project.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Name of the imported project in C++ | Yes |

Meaning of allowed subnodes is explained below.

#### tag ####

```xml
<tag path="path/to/ns2/headers">Namespace::Namespace2::</tag>
```

Sets up header lookup rule. When translator meets a name from namespace given, it generates includes based on path specified, following the below rules:

* Each namespace inside tagged namespace is translated to a separate subdirectory;
* Class name corresponds to header name;
* Extension of file being included is .h;
* Namespaces and classes names are converted to underscore-delimited format even if they originally follow e. g. camelCase.

More specific rules (with innermore namespaces mentioned) override less specific ones. Explicit rules for individual types override tags.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Namespace to add rule for | Yes |
| path | Beginning of generated inclusion path | Yes |

For example, the above example implements such rule that references to class called 'Namespace::Namespace2::Namespace3::FooBarClass' will generate the following include:

```xml
#include "path/to/ns2/headers/namespace3/foo_bar_class.h"
```

Tag syntax is useful when implementing library manually. When using translated library, use typemap configuration file inclusion instead.

#### cmake_part_template ####

```xml
<cmake_part_template>
    find_botan()
</cmake_part_template>
```

CMake rules to allow for includes and other necessary setups like 3rd party libraries lookup.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Bare CMake code to be used both for libraries and executables | No |

#### cmake_link_template ####

```xml
<cmake_link_template>
    target_link_libraries(Project_name PUBLIC botan::botan LibProject::LibProject)
</cmake_link_template>
```

CMake rules to allow for linkable units (DLLs, EXEs)

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Bare CMake code to be used for linkable items (executables, shared libraries) | |

#### defines ####

```xml
<defines>DEFINE1 DEFINE2;DEFINE3=VALUE</defines>
```

Defines to be added to the project. Put library-related defines here rather than to global section of your configuration file as this make unnecessary define go once you exclude library-related file. Another benefit is keeping everything that relates to the library in a single place.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Space- and semicolon-separated list of defines with optional values | |

#### includes ####

```xml
<includes>path/to/dir1 path/to/dir2;path/to/dir3</includes>
```

Paths to include directories, related to the library. Again, allows keeping everything that is related to the library in a single place.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Space- and semicolon-separated list of paths for cmake to use | |

#### libdirs ####

```xml
<libdirs>path/to/dir1 path/to/dir2;path/to/dir3</libdirs>
```

Paths to library directories, related to the library.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Space- and semicolon-separated list of paths | |

#### class ####

```xml
<class name="LibProject::Class1" path="libproject/class1.h" shortptr="true"/>
```

Sets class-specific header path, useful for classes not covered by any tag rules.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Class name with namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |
| shortptr | Whether class provides ClassNamePtr-formed alias for SharedPtr\<ClassName\>, must be 'true' or 'false' | No (default `false`) |

#### enum ####

```xml
<enum name="LibProject::Enum1" path="libproject/enum1.h"/>
```

Sets enum-specific header path, useful for enums not covered by any tag rules.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Enum name with namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |

#### namespace ####

```xml
<namespace name="LibProject::Namespace1" path="libproject/namespace1.h" shortptr="true"/>
```

Sets namespace-specific header path, useful for enums not covered by any tag rules.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Full namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |

#### replace_user_types ####

```xml
<replace_user_types>false</replace_user_types>
```

Forces the types in current project that are covered by any of the library tags to be ignored (skipped) - library-provided ones will be used instead. Useful if you add library to replace some types available in current build.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | 'true' to exclude types or 'false' to let them be | Node (default `false`) |

## files ##

```xml
<files>
    <exclude file="/mask"/>
    <include file="/mask"/>
    <only file="/mask"/>
</files>
```

Adds file include and exclude masks. Masks should be prefixed with '/'. When these filters apply, priorities are as follows:

| Directive | Priority | Effect | Overrides directives | Is overriden by directives
| --- | --- | --- | ---|
| include | High | Adds masked files to translating even if they are excluded by 'exclude' or 'only' rules | All | None
| exclude | Medium | Excludes masked files from translating even if they are included by 'only' rules but unless they are included by 'include' rule | only | include
| only | Low | If present, excludes all files but those masked, unless they are excluded by 'exclude' rule, plus those added by 'include' rule | None | All

| Attribute | Meaning | Mandatory |
| --- | --- | --- |

Allowed subnodes are listed below.

### exclude ###

```xml
<exclude file="path/foo_*.cs"/>
```

Makes translator ignore all files in project that match the specified mask.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

### include ###

```xml
<include file="path/foo_bar_*.cs"/>
```

Stops translator from ignoring files that match the specified mask even if ignored otherwise (by 'exclude' or 'only' subnode).

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

### only ###

```xml
<only file="path/bar_foo_*.cs"/>
```

Makes translator ignore all files that do not match the specified mask (or any of the masks specified in 'only' subnodes if multiple ones are present).

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

## cut_namespaces ##

```xml
<cut_namespaces>
    <exclude file="mask"/>
    <include file="mask"/>
    <only file="mask"/>
</cut_namespaces>
```

Enable/disable namespaces cutting for all types, defined in specific files. The subnodes rules are identical with \<files\> option.

So, if the file 'foo.cs' containing type Bar.Foo is included using cut_namespace element, this type will be referred to as 'Foo' in translated code. Otherwise, it will be referred to as 'Bar::Foo'.

## typemap ##

```xml
<typemap>
    <class csname="Namespace1.Class1" cppname="Namespace2.Class2" box="false"/>
    ...
</typemap>
```

Maps C# type names into C++ ones. Useful if the default mapping fails. The only allowed subnode is 'class'.

### class ###

```xml
<class csname="Namespace1.Class1" cppname="Namespace2.Class2" box="false"/>
```

Denotes a single mapping rule of how C# typename should be translated to C++.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| csname | Class name with namespace in C# | Yes |
| cppname | Class name with namespace in C++ | Yes |
| box | Boolean flag that shows whether the type requires boxing | No (default `false`) |

## includes ##

```xml
<includes>
    <class name="LibProject::Class1" path="libproject/class1.h" shortptr="true"/>
    <enum name="LibProject::Enum1" path="libproject/enum1.h"/>
    <namespace name="LibProject::Namespace1" path="libproject/namespace1.h"/>
    ...
</includes>
```

Specifies includes for specific types. Include rules, same as in \<lib\> section.

## force_value_types ##

```xml
<force_value_types>
    <class name="Namespace1::Class1"/>
    <class name="Namespace2::Class2" box="true"/>
    ...
</force_value_types>
```

Lists the types to be treated as value types. Only 'class' subnodes are allowed.

### class ###

```xml
<class name="Namespace1::Class1" box="false"/>
```

Class to be treated as value type.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Type in C++ with namespace | Yes |
| box | Boolean value which defines whether type is subject for boxing | No (default `false`) |

## references ##

```xml
<references>
    <assembly name="Assembly.Name" path="path\to\assembly.dll"/>
</references>
```

Specifies the references to external assemblies to import symbols from. Only 'assembly' nodes are allowed here.

### assembly ###

```xml
<assembly name="Assembly.Name" path="path\to\assembly.dll"/>
```

Single assembly to import symbols from.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | C# assembly name. | Yes |
| path | Path to assembly. | Yes |

## dont_wrap_params ##

```xml
<dont_wrap_params>
    <class  name="Namespace::Class1"/>
    <method name="Namespace::Class2::Method1"/>
    ...
</dont_wrap_params>
```

Disables parameters wrapping (matching actual parameters against format ones with necessary casts) against specific method or all methods of specific class. This is required if the method uses variadic template arguments and actual wrapping would fail.

Only 'class' and 'method' subnodes are allowed here.

### class ###

```xml
<class name="Namespace::Class1"/>
```

Single class, all methods of which will not be wrapping parameters for.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Name of the class in C++ with namespace. | Yes |

### method ###

```xml
<method name="Namespace::Class2::Method1"/>
```

Single method to not wrap parameters for. All methods with this name will be affected - there's no way disabling parameter wrapping for single overload.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Name of the method in C++ with namespace and class name. | Yes |

## disable_boxing ##

```xml
<disable_boxing>
    <class name="System::Convert"/>
    <method name="System::Enum::Parse"/>
    ...
</disable_boxing>
```

Disables parameters boxing when type conversions occur (usually when native C++ implementations exist with better signatures). The syntax is same as for 'dont_wrap_param'.

## restricted_tokens ##

```xml
<restricted_tokens mask="*">
    <token from="OldName" to="NewName"/>
    ...
</restricted_tokens>
```

Replaces all identifiers specified globally in selected files.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| mask | File name or path mask - only selected files will be affected. | No (default `*`) |

### token ###

```xml
<token from="OldName" to="NewName"/>
```

Single rule for identifier replacement.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| from | Identifier in original C# code to be replaced | Yes |
| to | Replacement for identifier specified in 'from' attribute | Yes |

## assembly_with_restricted_tokens ##

```xml
<assembly_with_restricted_tokens>AssemblyName</assembly_with_restricted_tokens>
```

Forces the names in the referenced assembly to be replaced as per all restricted_tokens rules. By default, only the tokens in current assembly are replaced. No mask matching is performed.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | Name of the dependence assembly in C# | Yes |

## skip_definitions ##

```xml
<skip_definitions stub="false" only_public_api="true"/>
```

Global version of 'CppSkipDefinition' attribute. If enabled, no class member definitions are processed (but declarations are still generated for them).

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| stub | Whether to generate stubs for dropped definitions, 'true' or 'false' | No (default `true`) |
| only_public_api | Whether translator should leave public API only ('true') or process private and internal classes as well ('false') | No (default `false`) |

## implementation ##

```xml
<implementation type="MyNamespace.MyClass" entity="MyMethod" includes="*someglobalheader;*otherglobalheader.h;path1/path2/header1.h;path1/path2/header2.h">
    <![CDATA[
        return 2+2;
    ]]>
</implementation>

<implementation file="OriginalFileName.cpp" to="source"/>
```

Substitutes some C++ implementation instead of translated one. First form allows it to store the implementation for the specified method in config itself. The second one copies the C++ file to destination directory.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| type | C# type name to substitute member for | In the first form |
| entity | C# method name (no overloads supported) | In the first form |
| includes | Semicolon delimited list of headers, used for the custom implementation (if header started with '*' character, then header is the global, else header is the local) | Optional (used in the first form only) |
| _Element contents_ | CDATA with code | In the first form |
| file | Path to C++ file with additional implementations to copy to your target project | In the second form |
| to | Subdirectory to copy C++ file to | In the second form |

Alternatively, you can simply include .cpp file into your C# project. It will be copied to output project during translating.

Please note that all methods you want to replace implementations for must be marked with CppSkipDefinition attribute.

## nunit_categories ##

```xml
<nunit_categories>
    <include name="Category1"/>
    <exclude name="Category2"/>
</nunit_categories>
```

Allows it to filter tests by the category specified in NUnit.Framework.Category attribute. 'exclude' filter excludes single category. 'include' filter, if present, excludes all categories except for the one specified and for the ones specified in other 'include' filters, if present. 'include' directive has higher priority than 'execlude' one.

Only 'include' and 'exclude' elements are allowed inside 'nunit_categories' one.

For example, there are 'A', 'B', 'C' and 'D' categories and some uncategorized tests. If we exclude 'A' and 'B' categories, then only 'C', 'D' and uncategorized tests will be translated. If we additionally include 'A', then only 'A' category will be translated, as 'include' tag overweights any 'exclude' ones and, if present, excludes everything that is not included explicitly. If now we include '*' category, then only 'A', 'C', 'D' and uncategorized tests are translated as including '\*' includes everything that was not excluded explicitly.

### include ###

```xml
<include name="Category1"/>
```

Category for inclusion.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | NUnit test category name | Yes |

A special value of '*' can be assigned to the 'name' attribute of 'include' tag. If present, such tag changes the logic here. By default, everything that is not included explicitly, is excluded if there is at least one 'include' tag present. If there is a tag including '\*' category, then everything that is not excluded explicitly gets included, instead.

### exclude ###

```xml
<exclude name="Category1"/>
```

Category for exclusion.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | NUnit test category name | Yes |

## cmake_commands ##

```xml
<cmake_commands>
<![CDATA[
    if (MSVC)
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "/INCREMENTAL:NO")
    endif()
]]>
</cmake_commands>
```

Allows it to push custom commands to resulting CMakeLists.txt file.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| _Element contents_ | CDATA with cmake code | Yes |

## cmake_files ##

```xml
<cmake_files>
    <file name="file1.cmake" />
    ...
</cmake_files>
```

Lists cmake files to be copied from 'cmake' directory of translator installation to target project directory. Please note that each copy of this node clears all previously set ones, including the default values.

Only 'file' nodes are allowed inside.

### file ###

```xml
<file name="file1.cmake" />
```

Single file to be copied.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | File name | Yes |

## enum_underlying_types ##

```xml
<enum_underlying_types>
    <type name="Enum1" value="int" />
    ...
</enum_underlying_types>
```

Maps enums underlying types. Mostly used by translator typemap when processing dependent projects.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |

Only 'type' nodes are allowed inside.

### type ###

```xml
<type name="Enum1" value="int" />
```

Individual underlying type mapping rule.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Enum type name with namespace | Yes |
| value | Integer type name | Yes |

## attribute ##

```xml
<attribute name="CppAttributeName" ... />
```

Single attribute record. See [attributes in configuration file](attributes.md) for more details.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Attribute name | Yes |
| _Other attributes_ | Individual per attribute. Some of them set attribute parameters, some other ones specify scope. | Depends on attribute |

## forced_include ##

```xml
<forced_include>
    <class name="Namespace::ClassName" />
    <class name="Namespace::ClassName1" />
</forced_include>
```

Include header instead of forward declaration for an argument type, same as `CppForceInclude` attribute.

Only 'class' nodes are allowed inside.

### class ###

```xml
<class name="Namespace::ClassName" />
```

Individual class to force includes for.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Class name with namespace | Yes |

## documentation_comments_translation ##

```xml
<documentation_comments_translation>
    <cref_typemap>
        <cref cstext="sbyte" cpptext="int8_t" />
    </cref_typemap>
    <summary_text_map>
        <property cstext="Gets or sets" getter_text="Gets" setter_text="Sets" />
    </summary_text_map>
    <options>
        <opt name="fix_setter_return_tag" value="true" />
    </options>
    <replacements>
        <comment  type="SomeNS.Enum1" member="" tag="code">
            <![CDATA[
C++ code Enum1
]]>
        </comment>
    </replacements>
    <translate_code_from_comments value="true"/>
    <translate_code_from_comments_system_dlls>
        <system_dll name="System.Something.dll"/>
    </translate_code_from_comments_system_dlls>
</documentation_comments_translation>
```

Parameters for code documentation.

The following subsections are allowed:

* **cref_typemap** - list of type references to be translated and supplied to Doxygen. The following child nodes are allowed:
  * **cref** - one type; **cstext** attribute names type in C# and **cpptext** names corresponding type in C++.
* **summary_text_map** - list of substrings which should be replaced when generating individual getter and setter comments from joint getter+setter comment in C#.
  * **property** - individual replacement entry. **cstext** attribute means text to be replaced, **getter_text** is replacement for getter and **setter_text** is replacement for setter.
* **options** - list of options related to documentation generation. The following options are allowed:
  * **fix_setter_return_tag** - remove 'return' tag from setter documentation.
* **replacements** - list of allowed replacements for the specified tags in comments to specified items.
  * **comment** - the contents of this item will replace the contents of specified tag of the specified member of specified tag. If both type and member are unset, this replacement will be done by default to all occurrances of mentioned tag where no other replacements apply.

For mentioned subtags attributes, see below.

### cref_typemap ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |

### cref ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| cstext | Type name in C# comments | Yes |
| cpptext | Corresponding type name in C++ comments | Yes |

### summary_text_map ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |

### property ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| cstext | Text to be replaced as it appears in C# property documentation | Yes |
| getter_text | Replacement text for C++ getter function documentation | Yes |
| setter_text | Replacement text for C++ setter function documentation | Yes |

### options ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |

### replacements ###

Defines exact replacement for contents of specific tag in documentation for type or type member.

### translate_code_from_comments ###

Enables the translator replacing references to C# types and members with C++ analogs.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| value | Boolean flag that enables or disables this behavior. | Yes |

### translate_code_from_comments_system_dlls ###

Adds specified DLLs to symbol lookup when replacing references in documentation comments. No longer required since 21.3 version.

#### system_dll ####

Specifies a single DLL to add to this list.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Name of the DLL to use symbols from when performing code comments changes. | Yes |

## if ##

```xml
<if defined="my_var">
    <opt name="additional_defines" value="SOME_DEFINES"/>
    <else>
        <opt name="additional_defines" value="SOME_OTHER_DEFINES"/>
    </else>
</if>
```

Allows to switch on or off some part of the config conditionally, based on command line parameters passed to translator: if '-d' command line parameter followed by definition name is passed, all contents of 'if' tag except for 'else' subtag is executed (in the above example - first 'opt' element); otherwise, only 'else' tag is executed (in the above example - second 'opt' tag).

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| defined | Name of parameter to be evaluated | Yes |

### else ###

```xml
<else>
    <opt name="additional_defines" value="SOME_OTHER_DEFINES"/>
</else>
```

Part of the config which should be executed if 'if' element evaluation fails.

## msbuild_global_properties ##

```xml
<msbuild_global_properties>
    <property name="TargetFramework" value="net20"/>
</msbuild_global_properties>
```

Allows to define MSBuild properties directly to satisfy conditions in csproj file.

Allowed sub-items:

### property ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | MSBuild property name | Yes |
| value | MSBUild property value | Yes |

## rename_files ##

```xml
<rename_files>
    <file name_without_extension="FileToRename" to="RenamedFile"/>
</rename_files>
```

Allows to rename files generated by translator. Impacts both '.h' and '.cpp' file (if exist).

Allowed sub-items:

### file ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name_without_extension | Name of file generated by translator by default. | Yes |
| to | File name the file to be renamed to. | Yes |

## allowed_heap_only_types ##

```xml
<allowed_heap_only_types>
    <class name="System.Xml.XPath.XPathNodeIterator" />
</allowed_heap_only_types>
```

Makes translator generate code that produces compilation error if specified class is being allocated on stack. Useful for the classes that create shared pointers to themselves and thus are incompatible with automatic memory management.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Fully qualified name of C# class. | Yes |

## types_with_begin_and_end_methods ##

```xml
<types_with_begin_and_end_methods>
    <class name="System.Collections.Generic.Dictionary" />
</types_with_begin_and_end_methods>
```

Makes translator generate simplier code for 'foreach' statements, if [['foreach_as_range_based_for_loop' option|doc:Codeporting.Dynabic\.csPorter for Cpp.Documentation and Support Materials.Production documentation storage point.Developer Guide.CodePorting\.Translator Cs2Cpp configuration file.Configuration file options.WebHome]] is enabled.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Fully qualified name of C# class. | Yes |

## assembly_types ##

```xml
<assembly_types>
   <class name="Namespace.ClassName" />
</assembly_types>
```

Makes translator register specified types in the Assembly object which is associated with current project. Unless the type is registered, TypeInfo::get_Assembly() doesn't work for it.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Fully qualified name of C# class. | Yes |

## unity_build ##

```xml
<unity_build batch_size="16">
  <excluded_files>
    <file name="main.cpp" />
  </excluded_files>
</unity_build>
```

Enables the building process using UNITY_BUILD for the translated project.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| batch_size | Sets the batch size of UNITY_BUILD. The value must be greater that 0. Otherwise, UNITY_BUILD will be disabled. | Yes |

Allowed sub-items:

### excluded_files ###

Contains a list of files that must be excluded from UNITY_BUILD.

Allowed sub-items:

#### file ####

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | A path to a file that must be excluded from UNITY_BUILD. | Yes |

## external_include ##

```xml
<external_include file="AddFunctionArgument_PassFunctionArgument_Tests.cs" include="function_traits.hpp" include_to="source" include_as="local"/>
```

Makes the translator generate additional include directives into the translated version of the specified cs file. Can do the same thing [CppForceInclude](../cpp-attributes/reference.md#cppforceinclude) attribute does, but can also include any file the translator doesn't know about.

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| file | Cs file the inclusion should be added to the translated version of. | Yes |
| include | Include text (header name with all directories required). | Yes |
| include_to | Whether to add the include into header ('header') file or into the source ('source') file. | Yes |
| include_as | Whether to use local inclusion syntax (double quotes, 'local') or global inclusion syntax (angle brackets, 'global'). | Yes |

## events_with_custom_accessors ##

```xml
<events_with_custom_accessors>
    <event name="System.Xml.XmlReaderSettings.ValidationEventHandler"/>
    <event name="System.Xml.XmlValidatingReader.ValidationEventHandler"/>
    <event name="System.Xml.Schema.XmlSchemaSet.ValidationEventHandler"/>
</events_with_custom_accessors>
```

Makes the translator generate \<PropertyName\>_add()/\<PropertyName\>_remove() calls instead of +=/-= operators for specified custom events.

Allowed sub-items:

### event ###

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| name | Event name (with namespace and class name) | Yes |

## insert_code_to_tests ##

```xml
<insert_code_to_tests include="myinclude.h">
<![CDATA[
// Any C++ code
]]>
</insert_code_to_tests>
```

Adds some snippet of C++ code into beginning of each Google.Test Recommended to wrap C++ code to XML `<![CDATA[...]]>` to avoid problems during parsing configuration

| Attribute | Meaning | Mandatory |
| --- | --- | --- |
| include | Name of header file that will be included in every C++ source file contains tests affected by \<insert_code_to_tests\> | No |

Multiple usage of tag is allowed and will add all code snippets and all includes consequentially.
