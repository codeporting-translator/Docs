---
date: "2019-10-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "Cpp Code Injection"
linktitle: "Cpp Code Injection"
description: "Limitations for Injecting some C++ Code into your Translated C# Code"
menu:
  docs:
    parent: "Limitations and Bugs"
    weight: "3"
lastmod: "2019-06-27"
weight: "3"
---

Sometimes there's a need to inject some C++ code into your translated C# code. If translating your code just once, you can do this after translation completes. However, if you are setting up a pipeline for translating some code continuously (e. g. to translate each version of your product), it is easier to let translator do this job.

There are several features providing this behavior.

## Definition replacement ##

Placing 'CppSkipDefinition' attribute at some method will remove this method's definition (but not declaration) from translated code. After doing this, you can either provide an alternative definition using 'implementation' codification file node, or simply include a *.cpp file containing one directly into your project - when translating is done, this file will be copied into output project unchanged.

## Code line injection ##


Alternatively, you may use a specially formatted comments in your C# code to add small portions of C++ code into your translated code. The example is below.

{{< highlight cpp >}}
void Increment(ref int i, ref int j, ref int k)

{

    ++i;

    //CPPCODE: ++j;

    //++k;

}
{{< /highlight >}}

In translated code, both '++i' and '++j' expressions will be present. However, '++k' is just a comment and it will remain this way after translating.
