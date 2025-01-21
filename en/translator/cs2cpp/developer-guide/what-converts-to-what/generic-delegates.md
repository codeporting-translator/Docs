---
date: "2025-01-15"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "GenericDelegates"
linktitle: "GenericDelegates"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2025-01-15"
weight: "1"
---

This example demonstrates how generic delegates are translated to C++. They are declared using special type System::MulticastDelegate<T> from asposecpplib.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;

namespace TypesPorting
{
    public delegate TOut GenericDelegate<TIn, TOut>(TIn value);

    public delegate TOut GenericDelegateWithTypeConstraint<TIn, TOut>(TIn value) where TIn : ICloneable where TOut : IConvertible;

    public delegate TOut GenericDelegateWithClassConstraint<TIn, TOut>(TIn value) where TIn : class where TOut : class;

    public delegate TOut GenericDelegateWithStructConstraint<TIn, TOut>(TIn value) where TIn : struct where TOut : struct;

    public delegate TOut GenericDelegateWithNewConstraint<TIn, TOut>(TIn value) where TIn : new() where TOut : new();

    public delegate TOut GenericDelegateWithSeveralConstraints<TIn, TOut>(TIn value) where TIn : class, ICloneable, new() where TOut : struct, IConvertible;
}
{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/multicast_delegate.h>

namespace TypesPorting {

template <typename TIn, typename TOut> using GenericDelegate = System::MulticastDelegate<TOut(TIn)>;

template <typename TIn, typename TOut> using GenericDelegateWithTypeConstraint = System::MulticastDelegate<TOut(TIn)>;

template <typename TIn, typename TOut> using GenericDelegateWithClassConstraint = System::MulticastDelegate<TOut(TIn)>;

template <typename TIn, typename TOut> using GenericDelegateWithStructConstraint = System::MulticastDelegate<TOut(TIn)>;

template <typename TIn, typename TOut> using GenericDelegateWithNewConstraint = System::MulticastDelegate<TOut(TIn)>;

template <typename TIn, typename TOut> using GenericDelegateWithSeveralConstraints = System::MulticastDelegate<TOut(TIn)>;
} // namespace TypesPorting



{{< /highlight >}}
