---
date: "2019-10-11"
author:
  display_name: "xwiki:XWiki.farooqsheikh"
draft: "false"
toc: true
title: "CodePorting.Native Cs2Cpp Configuration File Structure"
linktitle: "CodePorting.Native Cs2Cpp Configuration File Structure"
description: "Basic Structure of Configuration File"
menu:
  docs:
    parent: "CodePorting.Native Cs2Cpp Configuration File"
    weight: "1"
lastmod: "2019-05-28"
weight: "1"
---


### CodePorting.Native Cs2Cpp Configuration File Structure ###

This page describes the basic structure of CodePorting.Native Cs2Cpp Configuration File. There is a default porter.config file supplied alongside with the porter one can refer to.

### General structure ###

The root node of the configuration file must be 'porter'. It has no attributes:

{{< highlight xml >}}
<porter>
    <!-- Some definitions here -->
</porter>
{{< /highlight >}}

### On paths ###

All paths in the configuration file are written related to current .config file, **except**:

* those related to .csproj project dir: \

{{< highlight xml >}}
<exclude file="src\foo*.cs"/>
<only file="src\bar?.cs"/>
{{< /highlight >}}

* those related to nothing, these paths used to generate #include directive: \

{{< highlight xml >}}
<class name="ClassA" file="path/to/include.h"/>
<enum name="EnumB" file="path/to/include.h"/>
{{< /highlight >}}


### Configuration files inclusion ###

Use 'import' element with 'config' attribute to include the file which resides in the same location as your current directory:

{{< highlight xml >}}
<import config="other_config_file.config"/>
{{< /highlight >}}

Use relative paths if you need cross-directory pathing.

To include default configuration file or other files from porter default configuration files directory, use special '%PorterHome%' placeholder:

{{< highlight xml >}}
<import config="%PorterHome%/porter.config"/>
{{< /highlight >}}

### Configuration file splitting ###

It is recommended to move everything related to library support to a separate file and import this file in the configuration file you use to port your project:

{{< highlight xml >}}
<import config="porter.lib_aspose_drawing_skia.config"/>
{{< /highlight >}}

This such approach allows easily switch to different lib implementation:

{{< highlight xml >}}
<import config="porter.lib_aspose_drawing_cario.config"/>
{{< /highlight >}}


Also, one can reuse library configuration file when porting several projects.
