---
date: "2020-05-06"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "Configuration file Nodes"
linktitle: "Configuration file Nodes"
description: "List of all Tags Allowed in Configuration File"
menu:
  docs:
    parent: "CodePorting Native Cs2Cpp Configuration File"
    weight: "3"
lastmod: "2020-05-06"
weight: "3"
---



This sectiolists all tags allowed in configuration file.

## csproj ##

{{< highlight xml >}}
<csproj path="Path/ProjectName.csproj" cfg="Configuration" platform="Platform"/>
{{< /highlight >}}

Refernce to input project file.

Attributes:

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| path | Path to project file | Yes |
| cfg | Configuration used when parsing source C# code | No | Debug
| platform | Platform to use specific settings for when parsing the project | No | Default or first available platform as defined in input project file

This attribute overrides path to project given to a porter application via command line.

## outdir ##

{{< highlight xml >}}
<outdir path="Path"/>
{{< /highlight >}}

Reference to output directory.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| path | Path to output directory | Yes |

## embedded_proj ##

{{< highlight xml >}}
<embedded_proj path="path/to/project"/>
{{< /highlight >}}

Defines paths to embedded projects used in project being ported.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| path | Full or relative path to embedded project | Yes |

## cppproj ##

{{< highlight xml >}}
<cppproj name="ProjectName"/>
{{< /highlight >}}

Allows it to specify output project name.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Output project name | Yes |

## opt ##

{{< highlight xml >}}
<opt name="Name" value="Value"/>
{{< /highlight >}}

Porter option. See [Configuration file options](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/) for details on what options are available.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Option name | Yes |
| value | Option value | Yes |
| _Option-specific attributes_ | Some options can require additional attributes to be specified | Defined by option | Defined by option

## additional_source ##

{{< highlight xml >}}
<additional_source>
    <copy dir="dir1"/>
    <copy dir="dir2"/>
</additional_source>
{{< /highlight >}}

Directories to copy additional C++ sources from.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'copy' subnodes are allowed inside 'additional_source' one.

### copy ###

{{< highlight xml >}}
<copy dir="dir1"/>
{{< /highlight >}}

Single directory to copy additional C++ source files from.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| dir | Path to directory (absolute or relative to configuration file; can also be relative to porter directory if 'use_porter_home_directory_while_resolving_path' option is enabled) | Yes |

## import ##

{{< highlight xml >}}
<import config="details.config"/>
{{< /highlight >}}

Imports configuration file as if all its contents were added into the current one. Imported configuration file must have valid structure ('porter' node in the root and so on).

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| config | Path to configuration file (absolute or relative to current configuration file; can also be relative to porter directory if 'use_porter_home_directory_while_resolving_path' option is enabled) | Yes |

# libs, style

{{< highlight xml >}}
<libs>
    <lib name="a">
        ...
    </lib>
</libs>
<style>
    <opt name="b" value="c"/>
    ...
</style>
{{< /highlight >}}

Logical grouping. 'libs' element defines configuration file section which contains instructions on libraries attachment. 'style' element is meant to group style-related options.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'opt' and 'lib' nodes are allowed inside both tags. Using 'opt' and 'lib' outside of 'libs' or 'style' is also allowed.

## lib ##

{{< highlight xml >}}
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
{{< /highlight >}}

Imports library to use against current project.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Name of the imported project in C++ | Yes |

Meaning of allowed subnodes is explained below.

### tag ###

{{< highlight xml >}}
<tag path="path/to/ns2/headers">Namespace::Namespace2::</tag>
{{< /highlight >}}

Sets up header lookup rule. When porter meets a name from namespace given, it generates includes based on path specified, following the below rules:

* Each namespace inside tagged namespace is translated to a separate subdirectory;
* Class name corresponds to header name;
* Extension of file being included is .h;
* Namespaces and classes names are converted to underscore-delimited format even if they originally follow e. g. camelCase.

More specific rules (with innermore namespaces mentioned) override less specific ones. Explicit rules for individual types override tags.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Namespace to add rule for | Yes |
| path | Beginning of generated inclusion path | Yes |

For example, the above example implements such rule that references to class called 'Namespace::Namespace2::Namespace3::FooBarClass' will generate the following include:

{{< highlight xml >}}
#include "path/to/ns2/headers/namespace3/foo_bar_class.h"
{{< /highlight >}}

Tag syntax is useful when implementing library manually. When using ported library, use typemap configuration file inclusion instead.

### cmake_part_template ###

{{< highlight xml >}}
<cmake_part_template>
    find_botan()
</cmake_part_template>
{{< /highlight >}}

Cmake rules to allow for includes and other neccessary setups like 3rd party libraries lookup.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Bare cmake code to be used both for libraries and executables | No |

### cmake_link_template ###

{{< highlight xml >}}
<cmake_link_template>
    target_link_libraries(Project_name PUBLIC botan::botan LibProject::LibProject)
</cmake_link_template>
{{< /highlight >}}

Cmake rules to allow for linkable units (DLLs, EXEs)

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Bare cmake code to be used for linkable items (executables, shared libraries) | |

### defines ###

{{< highlight xml >}}
<defines>DEFINE1 DEFINE2;DEFINE3=VALUE</defines>
{{< /highlight >}}

Defines to be added to the project. Put library-related defines here rather than to global section of your configuration file as this make unnecessary define go once you exclude library-related file. Another benefit is keeping everything that relates to the library in a single place.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Space- and semicolon-separated list of defines with optional values | |

### includes ###

{{< highlight xml >}}
<includes>path/to/dir1 path/to/dir2;path/to/dir3</includes>
{{< /highlight >}}

Paths to include directories, related to the library. Again, allows keeping everything that is related to the library in a single place.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Space- and semicolon-separated list of paths for cmake to use | |

### libdirs ###

{{< highlight xml >}}
<libdirs>path/to/dir1 path/to/dir2;path/to/dir3</libdirs>
{{< /highlight >}}

Paths to library directories, related to the library.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Space- and semicolon-separated list of paths | |

### class ###

{{< highlight xml >}}
<class name="LibProject::Class1" path="libproject/class1.h" shortptr="true"/>
{{< /highlight >}}

Sets class-specific header path, useful for classes not covered by any tag rules.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Class name with namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |
| shortptr | Whether class provides ClassNamePtr-formed alias for SharedPtr&lt;ClassName&gt;, must be 'true' or 'false' | No | false

### enum ###

{{< highlight xml >}}
<enum name="LibProject::Enum1" path="libproject/enum1.h"/>
{{< /highlight >}}

Sets enum-specific header path, useful for enums not covered by any tag rules.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Enum name with namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |

### namespace ###

{{< highlight xml >}}
<namespace name="LibProject::Namespace1" path="libproject/namespace1.h" shortptr="true"/>
{{< /highlight >}}

Sets namespace-specific header path, useful for enums not covered by any tag rules.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Full namespace | Yes |
| path | Full header path to be inserted into 'include' directive | Yes |

### replace_user_types ###

{{< highlight xml >}}
<replace_user_types>false</replace_user_types>
{{< /highlight >}}

Forces the types in current project that are covered by any of the library tags to be ignored (skipped) - library-provided ones will be used instead. Useful if you add library to replace some types available in current build.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | 'true' to exclude types or 'false' to let them be | false | false

## files ##

{{< highlight xml >}}
<files>
    <exclude file="/mask"/>
    <include file="/mask"/>
    <only file="/mask"/>
</files>
{{< /highlight >}}

Adds file include and exclude masks. Masks should be prefixed with '/'. When these filters apply, priorities are as follows:

| Directive | Priority | Effect | Overrides directives | Is overriden by directives
---| ---| ---| ---| ---|
| include | High | Adds masked files to porting even if they are excluded by 'exclude' or 'only' rules | All | None
| exclude | Medium | Excludes masked files from porting even if they are included by 'only' rules but unless they are included by 'include' rule | only | include
| only | Low | If present, excludes all files but those masked, unless they are excluded by 'exclude' rule, plus those added by 'include' rule | None | All

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Allowed subnodes are listed below.

### exclude ###

{{< highlight xml >}}
<exclude file="path/foo_*.cs"/>
{{< /highlight >}}

Makes porter ignore all files in project that match the specified mask.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

### include ###

{{< highlight xml >}}
<include file="path/foo_bar_*.cs"/>
{{< /highlight >}}

Stops porter from ignoring files that match the specified mask even if ignored otherwise (by 'exclude' or 'only' subnode).

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

### only ###

{{< highlight xml >}}
<only file="path/bar_foo_*.cs"/>
{{< /highlight >}}

Makes porter ignore all files that do not match the specified mask (or any of the masks specified in 'only' subnodes if multiple ones are present).

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| file | Filename mask with '*' and '?' substitutions allowed. | Yes |

## cut_namespaces ##

{{< highlight xml >}}
<cut_namespaces>
    <exclude file="mask"/>
    <include file="mask"/>
    <only file="mask"/>
</cut_namespaces>
{{< /highlight >}}

Enable/disable namespaces cutting for all types, defined in specific files. The subnodes rules are identical with &lt;files&gt; option.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


So, if the file 'foo.cs' containing type Bar.Foo is included using cut_namespace element, this type will be referred to as 'Foo' in translated code. Otherwise, it will be referred to as 'Bar::Foo'.

## typemap ##

{{< highlight xml >}}
<typemap>
    <class csname="Namespace1.Class1" cppname="Namespace2.Class2" box="false"/>
    ...
</typemap>
{{< /highlight >}}

Maps C# type names into C++ ones. Useful if the default mapping fails.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


The only allowed subnode is 'class'.

### class ###

{{< highlight xml >}}
<class csname="Namespace1.Class1" cppname="Namespace2.Class2" box="false"/>
{{< /highlight >}}

Denotes a single mapping rule of how C# typename should be translated to C++.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| csname | Class name with namespace in C# | Yes |
| cppname | Class name with namespace in C++ | Yes |
| box | Boolean flag that shows whether the type requires boxing | No | false

## includes ##

{{< highlight xml >}}
<includes>
    <class name="LibProject::Class1" path="libproject/class1.h" shortptr="true"/>
    <enum name="LibProject::Enum1" path="libproject/enum1.h"/>
    <namespace name="LibProject::Namespace1" path="libproject/namespace1.h"/>
    ...
</includes>
{{< /highlight >}}

Specifies includes for specific types. Include rules, same as in "lib" section.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


## force_value_types ##

{{< highlight xml >}}
<force_value_types>
    <class name="Namespace1::Class1"/>
    <class name="Namespace2::Class2" box="true"/>
    ...
</force_value_types>
{{< /highlight >}}

Lists the types to be treated as value types. Only 'class' subnodes are allowed.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


### class ###

{{< highlight xml >}}
<class name="Namespace1::Class1" box="false"/>
{{< /highlight >}}

Class to be treated as value type.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Type in C++ with namespace | Yes |
| box | Boolean value which defines whether type is subject for boxing | No | false

## references ##

{{< highlight xml >}}
<references>
    <assembly name="Assembly.Name" path="path\to\assembly.dll"/>
</references>
{{< /highlight >}}

Specifies the references to external assemblies to import symbols from.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'assembly' nodes are allowed here.

### assembly ###

{{< highlight xml >}}
<assembly name="Assembly.Name" path="path\to\assembly.dll"/>
{{< /highlight >}}

Single assembly to import symbols from.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | C# assembly name. | Yes |
| path | Path to assembly. | Yes |

## dont_wrap_params ##

{{< highlight xml >}}
<dont_wrap_params>
    <class  name="Namespace::Class1"/>
    <method name="Namespace::Class2::Method1"/>
    ...
</dont_wrap_params>
{{< /highlight >}}

Disables parameters wrapping (matching actual parameters against format ones with neccessary casts) against specific method or all methods of specific class. This is required if the method uses variadic template arguments and actual wrapping would fail.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'class' and 'method' subnodes are allowed here.

### class ###

{{< highlight xml >}}
<class name="Namespace::Class1"/>
{{< /highlight >}}

Single class, all methods of which will not be wrapping parameters for.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Name of the class in C++ with namespace. | Yes |

### method ###

{{< highlight xml >}}
<method name="Namespace::Class2::Method1"/>
{{< /highlight >}}

Single method to not wrap parameters for. All methods with this name will be affected - there's no way disabling parameter wrapping for single overload.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Name of the method in C++ with namespace and class name. | Yes |

## disable_boxing ##

{{< highlight xml >}}
<disable_boxing>
    <class name="System::Convert"/>
    <method name="System::Enum::Parse"/>
    ...
</disable_boxing>
{{< /highlight >}}

Disables parameters boxing when type conversions occur (usually when native C++ implementations exist with better signatures). The syntax is same as for 'dont_wrap_param'.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


## restricted_tokens ##

{{< highlight xml >}}
<restricted_tokens mask="*">
    <token from="OldName" to="NewName"/>
    ...
</restricted_tokens>
{{< /highlight >}}

Replaces all identifiers specified globally in selected files.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| mask | File name or path mask - only selected files will be affected. | No | *

### token ###

{{< highlight xml >}}
<token from="OldName" to="NewName"/>
{{< /highlight >}}

Single rule for identifier replacement.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| from | Identifier in original C# code to be replaced | Yes |
| to | Replacement for identifier specified in 'from' attribute | Yes |

## assembly_with_restricted_tokens ##

{{< highlight xml >}}
<assembly_with_restricted_tokens>AssemblyName</assembly_with_restricted_tokens>
{{< /highlight >}}

Forces the names in the referenced assembly to be replaced as per all restricted_tokens rules. By default, only the tokens in current assembly are replaced. No mask matching is performed.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | Name of the dependence assembly in C# | Yes |

## skip_definitions ##

{{< highlight xml >}}
<skip_definitions stub="false" only_public_api="true"/>
{{< /highlight >}}

Global version of 'CppSkipDefinition' attribute. If enabled, no class member definitions are processed (but declarations are still generated for them).

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| stub | Whether to generate stubs for dropped definitions, 'true' or 'false' | false | true
| only_public_api | Whether porting application should leave public API only ('true') or process private and internal classes as well ('false') | false | false

## implementation ##

{{< highlight xml >}}
<implementation type="MyNamespace.MyClass" entity="MyMethod" includes="*someglobalheader;*otherglobalheader.h;path1/path2/header1.h;path1/path2/header2.h">
    <![CDATA[
        return 2+2;
    ]]>
</implementation>

<implementation file="OriginalFileName.cpp" to="source"/>
{{< /highlight >}}

Substitutes some C++ implementation instead of translated one. First form allows it to store the implementation for the specified method in config itself. The second one copies the C++ file to destination directory.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| type | C# type name to substitute member for | In the first form |
| entity | C# method name (no overloads supported) | In the first form |
| includes | Semicolon delimited list of headers, used for the custom implementation (if header started with '*' character, then header is the global, else header is the local) | Optional (used in the first form only) |
| _Element contents_ | CDATA with code | In the first form |
| file | Path to C++ file with additional implementations to copy to your target project | In the second form |
| to | Subdirectory to copy C++ file to | In the second form |

Alternatively, you can simply include .cpp file into your C# project. It will be copied to output project during porting.

Please note that all methods you want to replace implementations for must be marked with CppSkipDefinition attribute.

## nunit_categories ##

{{< highlight xml >}}
<nunit_categories>
    <include name="Category1"/>
    <exclude name="Category2"/>
</nunit_categories>
{{< /highlight >}}

Allows it to filter tests by the category specified in NUnit.Framework.Category attribute. 'exclude' filter excludes single category. 'include' filter, if present, excludes all categories except for the one specified and for the ones specified in other 'include' filters, if present. 'include' directive has higher priority than 'execlude' one.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'include' and 'exclude' elements are allowed inside 'nunit_categories' one.

For example, there are 'A', 'B', 'C' and 'D' categories and some uncategorized tests. If we exclude 'A' and 'B' categories, then only 'C', 'D' and uncategorized tests will be ported. If we additionally include 'A', then only 'A' category will be ported, as 'include' tag overweights any 'exclude' ones and, if present, excludes everything that is not included explicitly. If now we include '*' category, then only 'A', 'C', 'D' and uncategorized tests are ported as including '*' includes everything that was not excluded explicitly.

### include ###

{{< highlight xml >}}
<include name="Category1"/>
{{< /highlight >}}

Category for inclusion.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | NUnit test category name | Yes |

A special value of '*' can be assigned to the 'name' attribute of 'include' tag. If present, such tag changes the logic here. By default, everything that is not included explicitly, is excluded if there is at least one 'include' tag present. If there is a tag including '*' category, then everything that is not excluded explicitly gets included, instead.

### exclude ###

{{< highlight xml >}}
<exclude name="Category1"/>
{{< /highlight >}}

Category for exclusion.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | NUnit test category name | Yes |

## cmake_commands ##

{{< highlight xml >}}
<cmake_commands>
<![CDATA[
    if (MSVC)
        set_target_properties(${PROJECT_NAME} PROPERTIES LINK_FLAGS "/INCREMENTAL:NO")
    endif()
]]>
</cmake_commands>
{{< /highlight >}}

Allows it to push custom commands to resulting CMakeLists.txt file.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| _Element contents_ | CDATA with cmake code | Yes |

## cmake_files ##

{{< highlight xml >}}
<cmake_files>
    <file name="file1.cmake" />
    ...
</cmake_files>
{{< /highlight >}}

Lists cmake files to be copied from 'cmake' directory of porting application installation to target project directory. Please note that each copy of this node clears all previously set ones, including the default values.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'file' nodes are allowed inside.

### file ###

{{< highlight xml >}}
<file name="file1.cmake" />
{{< /highlight >}}

Single file to be copied.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | File name | Yes |

## enum_underlying_types ##

{{< highlight xml >}}
<enum_underlying_types>
    <type name="Enum1" value="int" />
    ...
</enum_underlying_types>
{{< /highlight >}}

Maps enums underlying types. Mostly used by porter typemap when processing dependent projects.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'type' nodes are allowed inside.

### type ###

{{< highlight xml >}}
<type name="Enum1" value="int" />
{{< /highlight >}}

Individual underlying type mapping rule.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Enum type name with namespace | Yes |
| value | Integer type name | Yes |

## attribute ##

{{< highlight xml >}}
<attribute name="CppAttributeName" ... />
{{< /highlight >}}

Single attribute record. See [Attributes in configuration file](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/attributes-in-configuration-file/) for more details.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Attribute name | Yes |
| _Other attributes_ | Individual per attribute. Some of them set attribute parameters, some other ones specify scope. | Depends on attribute | Depends on attribute

## forced_include ##

{{< highlight xml >}}
<forced_include>
    <class name="Namespace::ClassName" />
    <class name="Namespace::ClassName1" />
</forced_include>
{{< /highlight >}}

Include header instead of forward declaration for an argument type, same as `CppForceInclude` attribute.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


Only 'class' nodes are allowed inside.

### class ###

{{< highlight xml >}}
<class name="Namespace::ClassName" />
{{< /highlight >}}

Individual class to force includes for.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Class name with namespace | Yes |

## documentation_comments_translation ##

{{< highlight xml >}}
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
{{< /highlight >}}

Parameters for code documentation.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


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

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


### cref ###

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| cstext | Type name in C# comments | Yes |
| cpptext | Corresponding type name in C++ comments | Yes |

### summary_text_map ###

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


### property ###

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| cstext | Text to be replaced as it appears in C# property documentation | Yes |
| getter_text | Replacement text for C++ getter function documentation | Yes |
| setter_text | Replacement text for C++ setter function documentation | Yes |

### options ###

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


### replacements ###

Defines exact replacement for contents of specific tag in documentation for type or type member.

### translate_code_from_comments ###

Enables the porter replacing references to C# types and members with C++ analogs.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| value | Boolean flag that enables or disables this behavior. | Yes | false

**Since version:** 21.2

### translate_code_from_comments_system_dlls ###

Adds specified DLLs to symbol lookup when replacing references in documentation comments. No longer required since 21.3 version.

#### system_dll ####

Specifies a single DLL to add to this list.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| name | Name of the DLL to use symbols from when performing code comments changes. | Yes |

**Since version:** 21.2

## if ##

{{< highlight xml >}}
<if defined="my_var">
    <opt name="additional_defines" value="SOME_DEFINES"/>
    <else>
        <opt name="additional_defines" value="SOME_OTHER_DEFINES"/>
    </else>
</if>
{{< /highlight >}}

Allows to switch on or off some part of the config conditionally, based on command line parameters passed to porter: if '-d' command line parameter followed by definition name is passed, all contents of 'if' tag except for 'else' subtag is executed (in the above example - first 'opt' element); otherwise, only 'else' tag is executed (in the above example - second 'opt' tag).

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|
| defined | Name of parameter to be evaluated | Yes |

### else ###

{{< highlight xml >}}
<else>
    <opt name="additional_defines" value="SOME_OTHER_DEFINES"/>
</else>
{{< /highlight >}}

Part of the config which should be executed if 'if' element evaluation fails.

| Attribute | Meaning | Mandatory | Default value
---| ---| ---| ---|


## msbuild_global_properties ##

{{< highlight xml >}}
<msbuild_global_properties>
    <property name="TargetFramework" value="net20"/>
</msbuild_global_properties>
{{< /highlight >}}

Allows to define MSBuild properties directly to satisfy conditions in csproj file.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|


Allowed sub-items:

### property ###

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name | MSBuild property name | Yes | -
| value | MSBUild property value | Yes | -

## rename_files ##

{{< highlight xml >}}
<rename_files>
    <file name_without_extension="FileToRename" to="RenamedFile"/>
</rename_files>
{{< /highlight >}}

Allows to rename files generated by porter. Impacts both '.h' and '.cpp' file (if exist).

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|


Allowed sub-items:

### file ###

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name_without_extension | Name of file generated by porter by default. | Yes | -
| to | File name the file to be renamed to. | Yes | -

## allowed_heap_only_types ##

{{< highlight xml >}}
<allowed_heap_only_types>
    <class name="System.Xml.XPath.XPathNodeIterator" />
</allowed_heap_only_types>
{{< /highlight >}}

Makes porter generate code that produces compilation error if specified class is being allocated on stack. Useful for the classes that create shared pointers to themselves and thus are incompatible with automatic memory management.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|


Since version: 20.1.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name | Fully qualified name of C# class. | Yes | -

## types_with_begin_and_end_methods ##

{{< highlight xml >}}
<types_with_begin_and_end_methods>
    <class name="System.Collections.Generic.Dictionary" />
</types_with_begin_and_end_methods>
{{< /highlight >}}

Makes porter generate simplier code for 'foreach' statements, if ['foreach_as_range_based_for_loop' option](/native/cs2cpp/developer-guide/codeporting-native-cs2cpp-configuration-file/configuration-file-options/) is enabled.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|


Since version: 20.8.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name | Fully qualified name of C# class. | Yes | -

## assembly_types ##

{{< highlight xml >}}
<assembly_types>
   <class name="Namespace.ClassName" />
</assembly_types>
{{< /highlight >}}

Makes porter register specified types in the Assembly object which is associated with current project. Unless the type is registered, TypeInfo::get_Assembly() doesn't work for it.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|


Since version: 20.10.

Allowed sub-items:

### class ###

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name | Fully qualified name of C# class. | Yes | -

## unity_build ##

{{< highlight xml >}}
<unity_build batch_size="16">
  <excluded_files>
    <file name="main.cpp" />
  </excluded_files>
</unity_build>
{{< /highlight >}}

Enables the building process using UNITY_BUILD for the ported project.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| batch_size | Sets the batch size of UNITY_BUILD. The value must be greater that 0. Otherwise, UNITY_BUILD will be disabled. | Yes | -

Since version: 21.6.

Allowed sub-items:

### excluded_files ###

Contains a list of files that must be excluded from UNITY_BUILD.

Allowed sub-items:

#### file ####

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| name | A path to a file that must be excluded from UNITY_BUILD. | Yes | -

## external_include ##

{{< highlight xml >}}
<external_include file="AddFunctionArgument_PassFunctionArgument_Tests.cs" include="function_traits.hpp" include_to="source" include_as="local"/>
{{< /highlight >}}

Makes the porter generate additional include directives into the translated version of the specified cs file. Can do the same thing CppForceInclude attribute does, but can also include any file the porter doesn't know about.

| Attribute | Meaning | Mandatory | Default Value
---| ---| ---| ---|
| file | Cs file the inclusion should be added to the translated version of. | Yes | -
| include | Include text (header name with all directories required). | Yes | -
| include_to | Whether to add the include into header ('header') file or into the source ('source') file. | Yes | -
| include_as | Whether to use local inclusion syntax (double quotes, 'local') or global inclusion syntax (angle brackets, 'global'). | Yes | -

**Since version**: 21.9.