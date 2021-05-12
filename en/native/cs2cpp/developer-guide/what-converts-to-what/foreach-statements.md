---
date: "2021-05-09"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "ForeachStatements"
linktitle: "ForeachStatements"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2021-05-09"
weight: "1"
---

This example demonstrates how foreach statement is ported to C++. It is translated to C++ for statement.

Additional command-line options passed to CsToCppPorter: -o foreach_as_range_based_for_loop=true.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Collections;
using System.Collections.Generic;
using CsToCppPorter;

namespace StatementsPorting
{
    public class ForeachStatements : IEnumerable<string>
    {
        [CppGenerateBeginEndMethods]
        private List<string> m_collection;

        public IEnumerator<string> GetEnumerator()
        {
            return m_collection.GetEnumerator();
        }

        [CppSkipEntity]
        IEnumerator IEnumerable.GetEnumerator()
        {
            return null;
        }

        public void Foreach(string[] values)
        {
            foreach (string value in values)
            {
                Console.WriteLine(value);
            }
        }

        public void EnclosedForeach(string[][] values)
        {
            foreach (string[] row in values)
                foreach (string value in row)
                {
                    Console.WriteLine(value);
                }
        }

        public void ForeachOverList()
        {
            var list = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in list)
            {
                Console.WriteLine(i);
            }
        }

        public void ForeachOverIList()
        {
            IList<string> list = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in list)
            {
                Console.WriteLine(i);
            }
        }

        public void ForeachOverThis()
        {
            m_collection = new List<string>(new string[] { "1", "2", "3" });
            foreach (string i in this)
            {
                Console.WriteLine(i);
            }
        }
    }
}

{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/collections/list.h>
#include <system/collections/ienumerable.h>
#include <system/array.h>

namespace System { namespace Collections { namespace Generic { template <typename> class IEnumerator; } } }

namespace StatementsPorting {

class ForeachStatements : public System::Collections::Generic::IEnumerable<System::String>
{
    typedef ForeachStatements ThisType;
    typedef System::Collections::Generic::IEnumerable<System::String> BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
public:
    /// A collection type whose iterator types is used as iterator types in the current collection.
    using iterator_holder_type = System::Collections::Generic::List<System::String>;
    /// Iterator type.
    using iterator = typename iterator_holder_type::iterator;
    /// Const iterator type.
    using const_iterator = typename iterator_holder_type::const_iterator;
    
public:

    System::SharedPtr<System::Collections::Generic::IEnumerator<System::String>> GetEnumerator() override;
    void Foreach(System::ArrayPtr<System::String> values);
    void EnclosedForeach(System::ArrayPtr<System::ArrayPtr<System::String>> values);
    void ForeachOverList();
    void ForeachOverIList();
    void ForeachOverThis();
    /// Gets iterator pointing to the first element (if any) of the collection.
    /// @return An iterator pointing to the first element (if any) of the collection
    iterator begin() noexcept;
    /// Gets iterator pointing right after the last element (if any) of the collection.
    /// @return An iterator pointing right after the last element (if any) of the collection
    iterator end() noexcept;
    /// Gets iterator pointing to the first element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing to the first element (if any) of the const-qualified instance of the collection
    const_iterator begin() const noexcept;
    /// Gets iterator pointing right after the last element (if any) of the const-qualified instance of the collection.
    /// @return An iterator pointing right after the last element (if any) of the const-qualified instance of the collection
    const_iterator end() const noexcept;
    /// Gets iterator pointing to the first const-qualified element (if any) of the collection.
    /// @return An iterator pointing to the first const-qualified element (if any) of the collection
    const_iterator cbegin() const noexcept;
    /// Gets iterator pointing right after the last const-qualified element (if any) of the collection.
    /// @return An iterator pointing right after the last const-qualified element (if any) of the collection
    const_iterator cend() const noexcept;
    
protected:

    virtual ~ForeachStatements();
    
    #ifdef ASPOSE_GET_SHARED_MEMBERS
    System::Object::shared_members_type GetSharedMembers() override;
    #endif
    
    
private:

    System::SharedPtr<System::Collections::Generic::List<System::String>> m_collection;
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "ForeachStatements.h"

#include <system/enumerator_adapter.h>
#include <system/console.h>
#include <system/collections/ilist.h>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(2238368721u, ::StatementsPorting::ForeachStatements, ThisTypeBaseTypesInfo);

System::SharedPtr<System::Collections::Generic::IEnumerator<System::String>> ForeachStatements::GetEnumerator()
{
    return m_collection->GetEnumerator();
}

void ForeachStatements::Foreach(System::ArrayPtr<System::String> values)
{
    for (System::String value : values)
    {
        System::Console::WriteLine(value);
    }
    
}

void ForeachStatements::EnclosedForeach(System::ArrayPtr<System::ArrayPtr<System::String>> values)
{
    for (System::ArrayPtr<System::String> row : values)
    {
        for (System::String value : row)
        {
            System::Console::WriteLine(value);
        }
        
    }
    
}

void ForeachStatements::ForeachOverList()
{
    auto list = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (auto i : list)
    {
        System::Console::WriteLine(i);
    }
}

void ForeachStatements::ForeachOverIList()
{
    System::SharedPtr<System::Collections::Generic::IList<System::String>> list = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (auto i : System::IterateOver(list))
    {
        System::Console::WriteLine(i);
    }
}

void ForeachStatements::ForeachOverThis()
{
    m_collection = System::MakeObject<System::Collections::Generic::List<System::String>>(System::MakeArray<System::String>({u"1", u"2", u"3"}));
    for (auto i : *this)
    {
        System::Console::WriteLine(i);
    }
}

ForeachStatements::~ForeachStatements()
{
}

ForeachStatements::iterator ForeachStatements::begin() noexcept
{
    return m_collection->begin();
}

ForeachStatements::iterator ForeachStatements::end() noexcept
{
    return m_collection->end();
}

ForeachStatements::const_iterator ForeachStatements::begin() const noexcept
{
    return m_collection->cbegin();
}

ForeachStatements::const_iterator ForeachStatements::end() const noexcept
{
    return m_collection->cend();
}

ForeachStatements::const_iterator ForeachStatements::cbegin() const noexcept
{
    return m_collection->cbegin();
}

ForeachStatements::const_iterator ForeachStatements::cend() const noexcept
{
    return m_collection->cend();
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type StatementsPorting::ForeachStatements::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::ForeachStatements::m_collection", this->m_collection);
    
    return result;
}
#endif

} // namespace StatementsPorting

{{< /highlight >}}
