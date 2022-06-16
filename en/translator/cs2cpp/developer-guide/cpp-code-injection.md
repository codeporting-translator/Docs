---
date: "2021-04-21"
author:
  display_name: "xwiki:XWiki.dmitrybaskakov"
draft: "false"
toc: true
title: "C++ code injection"
linktitle: "C++ code injection"
menu:
  docs:
    parent: "Developer Guide"
    weight: "9"
lastmod: "2021-04-21"
weight: "9"
---

Sometimes there's a need to inject some C++ code into your translated C# code. If translating your code just once, you can do this after the translation comletes. However, if you are setting up a pipeline for translating some code continuously (e. g. to translate each version of your product), it is easier to let translator do this job.

There are several features providing this behavior.

## Definition replacement ##

Placing 'CppSkipDefinition' [attribute](/translator/cs2cpp/developer-guide/codeporting-translator-cs2cpp-attributes/) at some method will remove this method's definition (but not declaration) from translated code. After doing this, you can either provide an alternative definition using 'implementation' [cofiguration file node](/translator/cs2cpp/developer-guide/codeporting-translator-cs2cpp-configuration-file/configuration-file-nodes/), or simply include a *.cpp file containing one directly into your project - when translation is done, this file will be copied into output project unchanged.

## Code line injection ##

Alternatively, you may use a specially formatted comments in your C# code to add small portions of C++ code into your translated code. The example is below.

{{< highlight cs >}}
void Increment(ref int i, ref int j, ref int k)
{
    ++i;
    //CPPCODE: ++j;
    //++k;
}
{{< /highlight >}}

In translated code, both '++i' and '++j' expressions will be present. However, '++k' is just a comment and it will remain this way after translating.