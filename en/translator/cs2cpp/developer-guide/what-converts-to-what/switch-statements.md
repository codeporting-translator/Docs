---
date: "2022-06-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "SwitchStatements"
linktitle: "SwitchStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-06-09"
weight: "1"
---

This example demonstrates how switch statement is translated to C++. Switch statement with stings is translated to C++ if statement.

Additional command-line options passed to CodePorting.Translator.Cs2Cpp: none.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Collections.Generic;

namespace StatementsPorting
{
    public enum SomeEnum
    {
        InitValue = 0,
        Value = 1,
        SomeValue =2,
        OtherValue = 3,
        YetOneValue = 4,
        AnotherValue = 5
    }

    public class SwitchStatements
    {
        public void IntSwitch(int value)
        {
            switch (value)
            {
                case 3:
                    Console.WriteLine("3 branch");
                    break;
                case 4:
                case 5:
                    Console.WriteLine("4-5 branch");
                    break;
                case 6:
                    Console.WriteLine("6 branch with return");
                    return;
                case 7:
                    Console.WriteLine("7 branch");
                    break;
                default:
                    Console.WriteLine("default branch");
                    break;
            }
        }

        public void EnumSwitch(SomeEnum value)
        {
            switch (value)
            {
                case SomeEnum.InitValue:
                    Console.WriteLine("SomeEnum.InitValue branch");
                    break;
                case SomeEnum.Value:
                case SomeEnum.SomeValue:
                    Console.WriteLine("SomeEnum.Value-SomeEnum.SomeValue branch");
                    break;
                case SomeEnum.OtherValue:
                    Console.WriteLine("SomeEnum.OtherValue branch with return");
                    return;
                case SomeEnum.YetOneValue:
                    Console.WriteLine("SomeEnum.YetOneValue branch");
                    break;
                default:
                    Console.WriteLine("default branch");
                    break;
            }
        }

        public void StringSwitch(string value)
        {
            switch (value)
            {
                default:
                    Console.WriteLine("default branch");
                    break;
                case "iddqd":
                    Console.WriteLine("iddqd branch");
                    break;
                case "idkfa":
                case "idkfa2":
                    Console.WriteLine("idkfa-idkfa2 branch");
                    break;
                case "idclip":
                    Console.WriteLine("idclip branch with return");
                    return;
                case "quicken":
                    Console.WriteLine("quicken branch");
                    break;

            }
        }

        public void StringSwitchAdvanced()
        {
            string value = "";
            switch (value + "d")
            {
                default:
                    Console.WriteLine("default branch");
                    break;
                case "iddqd":
                    Console.WriteLine("iddqd branch");
                    break;
                case "idkfa":
                case "idkfa2":
                    Console.WriteLine("idkfa-idkfa2 branch");
                    break;
                case "idclip":
                    Console.WriteLine("idclip branch with return");
                    return;
                case "quicken":
                    {
                        Console.WriteLine("quicken branch");
                        break;
                    }
                case "choice":
                    while (true)
                    {
                        if (value == "iddqd")
                        {
                            break;
                        }
                        break;
                    }
                    if (value == "iddqd")
                    {
                        break;
                    }
                    break;
                case "choice2":
                    Console.WriteLine("choice2 branch");
                    if (value == "iddqd")
                    {
                        break;
                        if (value == "iddqd")
                        {
                            break;
                        }
                    }
                    break;
                case "choice3":
                    if (value == "aaa")
                        break;

                    Console.WriteLine("choice3 branch");
                    break;
            }
        }

        public string Property1 { get { return "circle"; } }
        
        public string Field1 = "a";

        public SwitchStatements NewInstance() { return new SwitchStatements(); }

        public void StringSwitchOnProperties()
        {
            switch (Property1)
            {
                case "abc":
                    break;
                case "def":
                case "a, b, c":
                    break;
                default:
                    break;
            }
        }
        
        public void StringSwitchOnFields()
        {
            switch (Field1)
            {
                case "a":
                    break;
                case "b":
                case "c":
                    break;
                default:
                    break;
            }

            switch (NewInstance().Field1)
            {
                case "a":
                    break;
                case "b":
                case "c":
                    break;
                default:
                    break;
            }
        }
    }
}

{{< /highlight >}}

## Translated Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/string.h>
#include <cstdint>

namespace StatementsPorting {

enum class SomeEnum
{
    InitValue = 0,
    Value = 1,
    SomeValue = 2,
    OtherValue = 3,
    YetOneValue = 4,
    AnotherValue = 5
};

class SwitchStatements : public System::Object
{
    typedef SwitchStatements ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:

    System::String get_Property1();
    
    System::String Field1;
    
    void IntSwitch(int32_t value);
    void EnumSwitch(SomeEnum value);
    void StringSwitch(System::String value);
    void StringSwitchAdvanced();
    System::SharedPtr<SwitchStatements> NewInstance();
    void StringSwitchOnProperties();
    void StringSwitchOnFields();
    
    SwitchStatements();
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "SwitchStatements.h"

#include <system/console.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(3987979893u, ::StatementsPorting::SwitchStatements, ThisTypeBaseTypesInfo);

System::String SwitchStatements::get_Property1()
{
    return u"circle";
}

void SwitchStatements::IntSwitch(int32_t value)
{
    switch (value)
    {
        case 3:
            System::Console::WriteLine(u"3 branch");
            break;
        
        case 4:
        case 5:
            System::Console::WriteLine(u"4-5 branch");
            break;
        
        case 6:
            System::Console::WriteLine(u"6 branch with return");
            return;
        
        case 7:
            System::Console::WriteLine(u"7 branch");
            break;
        
        default: 
            System::Console::WriteLine(u"default branch");
            break;
        
    }
}

void SwitchStatements::EnumSwitch(SomeEnum value)
{
    switch (value)
    {
        case StatementsPorting::SomeEnum::InitValue:
            System::Console::WriteLine(u"SomeEnum.InitValue branch");
            break;
        
        case StatementsPorting::SomeEnum::Value:
        case StatementsPorting::SomeEnum::SomeValue:
            System::Console::WriteLine(u"SomeEnum.Value-SomeEnum.SomeValue branch");
            break;
        
        case StatementsPorting::SomeEnum::OtherValue:
            System::Console::WriteLine(u"SomeEnum.OtherValue branch with return");
            return;
        
        case StatementsPorting::SomeEnum::YetOneValue:
            System::Console::WriteLine(u"SomeEnum.YetOneValue branch");
            break;
        
        default: 
            System::Console::WriteLine(u"default branch");
            break;
        
    }
}

void SwitchStatements::StringSwitch(System::String value)
{
    if (value == u"iddqd")
    {
        System::Console::WriteLine(u"iddqd branch");
    }
    else if (value == u"idkfa" || value == u"idkfa2")
    {
        System::Console::WriteLine(u"idkfa-idkfa2 branch");
    }
    else if (value == u"idclip")
    {
        System::Console::WriteLine(u"idclip branch with return");
        return;
    }
    else if (value == u"quicken")
    {
        System::Console::WriteLine(u"quicken branch");
    }
    else 
    {
        System::Console::WriteLine(u"default branch");
    }
}

void SwitchStatements::StringSwitchAdvanced()
{
    System::String value = u"";
    const System::String& switch_value_0 = value + u"d";
    if (switch_value_0 == u"iddqd")
    {
        System::Console::WriteLine(u"iddqd branch");
    }
    else if (switch_value_0 == u"idkfa" || switch_value_0 == u"idkfa2")
    {
        System::Console::WriteLine(u"idkfa-idkfa2 branch");
    }
    else if (switch_value_0 == u"idclip")
    {
        System::Console::WriteLine(u"idclip branch with return");
        return;
    }
    else if (switch_value_0 == u"quicken")
    {
        System::Console::WriteLine(u"quicken branch");
    }
    else if (switch_value_0 == u"choice")
    {
        while (true)
        {
            if (value == u"iddqd")
            {
                break;
            }
            break;
        }
        if (value == u"iddqd")
        {
            
        }
    }
    else if (switch_value_0 == u"choice2")
    {
        System::Console::WriteLine(u"choice2 branch");
        if (value == u"iddqd")
        {
            goto switch_break_0;
            if (value == u"iddqd")
            {
                goto switch_break_0;
            }
        }
    }
    else if (switch_value_0 == u"choice3")
    {
        if (value == u"aaa")
        {
            goto switch_break_0;
        }
        System::Console::WriteLine(u"choice3 branch");
    }
    else 
    {
        System::Console::WriteLine(u"default branch");
    }
    switch_break_0:;
}

System::SharedPtr<SwitchStatements> SwitchStatements::NewInstance()
{
    return System::MakeObject<SwitchStatements>();
}

void SwitchStatements::StringSwitchOnProperties()
{
    const System::String& switch_value_1 = get_Property1();
    if (switch_value_1 == u"abc")
    {
    }
    else if (switch_value_1 == u"def" || switch_value_1 == u"a, b, c")
    {
    }
    else 
    {
    }
}

void SwitchStatements::StringSwitchOnFields()
{
    if (Field1 == u"a")
    {
    }
    else if (Field1 == u"b" || Field1 == u"c")
    {
    }
    else 
    {
    }
    
    const System::String& switch_value_2 = NewInstance()->Field1;
    if (switch_value_2 == u"a")
    {
    }
    else if (switch_value_2 == u"b" || switch_value_2 == u"c")
    {
    }
    else 
    {
    }
}

SwitchStatements::SwitchStatements() : Field1(u"a")
{
}

} // namespace StatementsPorting

{{< /highlight >}}
