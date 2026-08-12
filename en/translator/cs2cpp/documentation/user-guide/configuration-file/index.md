---
order: "0"
navTitle: "Configuration file"
---

# Configuration file reference #

There is a default *translator.config* file supplied alongside with the translator one can refer to. For simple projects, this is sufficient, but for more complex ones, you'll need your own file (or several files) that extend the default settings. This section describes how to do that.

## General structure ##

CodePorting.Translator Cs2Cpp translator configuration files are of plain XML format. This section describes the meaning of allowed elements and attributes. The root node of the configuration file must be \<porter\>. It has no attributes:

```xml
<porter>
    <!-- Some definitions here -->
</porter>
```

## On paths ##

All paths in the configuration file are written related to current .config file. However, there are some exceptions.

1. Pathes of project files are related to .csproj project dir:

```xml
<exclude file="src\foo*.cs"/>
<only file="src\bar?.cs"/>
```

1. Include pathes are related to nothing, they are just put to translated code as they are and then get resolved by compiler itself.

```xml
<class name="ClassA" file="path/to/include.h"/>
<enum name="EnumB" file="path/to/include.h"/>
```

## Configuration files inclusion ##

Use \<import\> element with 'config' attribute to include the file which resides in the same location as your current directory:

```xml
<import config="other_config_file.config"/>
```

Use relative paths if you need cross-directory pathing.

To include default configuration file or other files from translator default configuration files directory, use special `%PorterHome%` placeholder:

```xml
<import config="%PorterHome%/translator.config"/>
```

## Configuration file splitting ##

It is recommended to move everything related to library support to a separate file and import this file in the configuration file you use to translate your project:

```xml
<import config="translator.lib_aspose_drawing_skia.config"/>
```

This such approach allows easily switch to different lib implementation:

```xml
<import config="translator.lib_aspose_drawing_cario.config"/>
```

Also, one can reuse library configuration file when translating several projects.

See also:

* [Nodes](configuration-file/nodes.md) - node-specific options and their meaning.
* [Options](configuration-file/options.md) - all configuration options their and defaults.
* [Attributes](configuration-file/attributes.md) - attribute settings available in the config file.
