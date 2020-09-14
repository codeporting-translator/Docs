---
date: "2019-10-11"
author:
  display_name: "muhammadrizwan"
draft: "false"
toc: true
title: "Using aliasing constructor to create a smart pointer"
linktitle: "Using aliasing constructor to create a smart pointer"
menu:
  docs:
    parent: "Memory Management Model"
    weight: "1"
lastmod: "2019-06-27"
weight: "1"
---

## Key agreement ##

The **Document** object is the root in the object tree. As the result, we can guarantee correctness of work and safety of use for objects from the object tree while the parent **Document** object is alive. So, the following approach will violate our key agreement: creation of the **Document** object in some inner region (e.g., in the some method) and return the some child node (e.g., paragraph) of the **Document** object from this inner region (because, the lifetime of the returning child node will be longer than lifetime of the parent **Document** object). In other word, our memory management model will be similar to [Qt memory management model](http://doc.qt.io/qt-5/objecttrees.html).

## Aliasing constructor ##

To solve this problem, a new constructor for smart pointer has been implemented. This constructor has a signature:

{{< highlight cpp >}}
template <typename P>
SmartPtr(const SmartPtr<P> &ptr, Pointee_ *p, SmartPtrMode mode # SmartPtrMode::Shared)
{{< /highlight >}}

A smart pointer created with this constructor shares ownership of the owned object. In our case, the following is obtained: a smart pointer points to a child node, but shares ownership of the root document.
This allows you to create a temporary document, save a smart pointer to its part and be sure that the document will be alive as long as this smart pointer is alive.

{{< highlight cpp >}}
System::SharedPtr<System::Xml::XmlNode> node;
{
    auto document = System::MakeObject<System::Xml::XmlDocument>(); // document.shared_count # 1
    ...
    node = System::SmartPtr<System::Xml::XmlNode>(doc, doc->get_DocumentElement()->SelectSingleNode(u"node")); // document.shared_count # 2
} // document.shared_count # 1
auto document = node->get_OwnerDocument(); // valid document
{{< /highlight >}}

## **Method** ##

**Description**
To use the aliasing constructor in the generated code, a special method was implemented.
In C# code, this method simply returns the target object, because in C# code, no similar functionality is needed.

{{< highlight cs >}}
public static T1 BindLifetime<T1, T2>(T1 target, T2 source)
{
    return target;
}
{{< /highlight >}}

In C+ code, this method creates a smart pointer using the aliasing constructor.

{{< highlight cpp >}}
template<typename T1, typename T2>
static T1 BindLifetime(const T1& target, const T2& source)
{
    return T1(source, target.get());
}
{{< /highlight >}}

**Using**
In C # code, in places where it is necessary to create a smart pointer using an aliasing constructor, simply place a call to this method.

## **Caution** ##

Aliasing constructor should be used carefully. There are various situations where its use can lead to crashes or memory leaks.
~1. Aliasing constructor cannot be used when creating parent-child relationships. This will lead to the fact that the parent itself increases the lifetime and is not destroyed even if there is not a single external strong reference to it.

{{< highlight cpp >}}
class A : public System::Object{};
class B : public System::Object
{
public:
    B()
    {
        a = System::SmartPtr<A>(System::SmartPtr<B>(this), new A());
    }

private:
    SmartPtr<A> a;
};
...

void foo()
{
    auto b = System::MakeObject<B>(); // b.shared_count # 2
} // b.shared_count # 1, b still alive
{{< /highlight >}}

2. If you create an object and save it to a smart pointer without sharing of ownership over it. And then save it to a second smart pointer that shares ownership over it. Then after the second smart pointer is destroyed, the object itself is also destroyed, and the first smart pointer will refer to the garbage.

{{< highlight cpp >}}
class A : public System::Object{};
class B : public System::Object{};
...
void foo()
{
    A *a = new A(); // a.shared_count # 0
    System::SmartPtr<B> sb = System::MakeObject<B>();
    System::SmartPtr<A> sa1 = System::SmartPtr<A>(sb, a); // a.shared_count # 0
    {
        System::SmartPtr<A> sa2 = System::SmartPtr<A>(a); // a.shared_count # 1
    } // a.shared_count # 0, a will be destroyed
}
{{< /highlight >}}
