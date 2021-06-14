---
date: "2021-05-19"
author:
display_name: "xwiki:XWiki.denisdetochka"
draft: "false"
toc: true
title: "C++ user-defined exception classes"
linktitle: "C++ user-defined exception classes"
menu:
docs:
parent: "Developer Guide"
weight: "10"
lastmod: "2021-05-20"
weight: "10"
---

## Introduction ##

Due to the fact that C++ allows to allocate an exception on stack, when in C# language exception is a reference type,  it was necessary to design a solution which allows to extend exception instance lifetime and preserve its type without losing an opportunity to use exception in the common for C++ way.

### Problem description ###

Due to the fact that C# exceptions are represented as object type it can be stored or cauth with exception type trimming. Let see sample code:

{{< highlight cs >}}
// C# code
Exception e = null;
try
{
throw new ArgumentException("abc");
}
catch (Exception caught)
{
e = caught; // (1)
}
try
{
throw e; // (2)
}
catch (ArgumentException arg)
{
Console.WriteLine("Caught!");
}
{{< /highlight >}}

If the most common approach for C++ language, when exception instance is stack-allocated, is used thrown exception from the second try catch block won't be caught. To solve this exception type trimming new approach was designed.

## Approach description ##

In order to accomplish defined goals C# exception classes were splitted in 2 abstract instances.

First one, from this point and below lets call it exception body, mostly serves utility functions. It defines exception class functions, contains logic and methods of C# exception classes (such as ToString() and Equals()) as well as contains user-defined logic. In library such classes are prefixed with "Details_" string in class name. Such classes inheritance represents C# exception class hierarchy, instances of such classes are always allocated on heap and are responsible for exception data owning, RTTI, instances comparison and so on. Derived exception body is always inherited from base exception body. In library all such classes must be inherited from System::Details_Exception at least indirectly. It is denied to throw instances of exception body. It is denied to create instance of exception body by calling its constructor.

Second one is ExceptionWrapper template class and its instantiations. From architecture point of view, this classes only contain shared pointer to exception body and provide indirect access to it via calls of pointer dereferencing operator. Also ExceptionWrapper template allows to make upcast and downcast of exception without exception type trimming. Inheritance tree is the same as for exception bodies. Instances of ExceptionWrapper must be stack-allocated only. Aslo only instances of ExceptionWrapper template can be used for throw syntax constructions. ExceptionWrapper class is also inherited from std::exception, which allows to use it in C++ notation.

As an example, lets define new exception class UnknownValueException, which is inherited from System.ArgumentException, which is also inherited from System.Exception. When library user wants to correctly define such exception, he need to define Details_UnknownValueException class, which is inherited from System::Details_ArgumentException, which is already inherited from System::Details_Exception. In exception body library user must provide additional RTTI information (please, see Requiremens section below). Library ExceptionWrapper template class will automatically create necessary inheritance tree by using SFINAE approach. From this point, ExceptionWrapper&lt;Details_UnknownValueException&gt; is inherited from ExceptionWrapper&lt;System::Details_ArgumentException&gt;, which is already inherited from ExceptionWrapper&lt;System::Details_Exception&gt;.

Library user must not create instances of exception body. It is allowed only by creation of ExceptionWrapper instances, which will be owners of the exception body. To throw exception, ExceptionWrapper::Throw() method must be called.

## Requirements ##

There is a requirement to define correct RTTI information for each exception body. This is necessary to allow ExceptionWrapper template create correct inheritance tree using SFINAE. ThisType and BaseType typedefs serve for this purpose.

All of exception body constructors must be defined in protected section to remove possibility of creation instances both on stack and heap. All exception body instances must be created implicitly by creating ExceptionWrapper template instance.

To allow creation of exception body instances on heap by creating ExceptionWrapper instance on stack each protected constructor must be accompanied with MakeObject() member function. Most common way is to use MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION macro in exception body declaration, which accepts exception body class name as a first parameter and an accompanied constructor argument list as a second parameter. In source file it is needed to add MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION, which also accepts exception body class name as a first parameter, an accompanied constructor argument list as a second parameter and list of accompanied constructor argument names. Another way is to use MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION macro, which defines and implements member function object in class declaration. Most common way to use it is for template classes, which are declared and defined in header file. The macro argument list is the same as for MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION.

It is needed to add friend declarations for ExceptionWrapper and its helper class to allow ExceptionWrapper class call protected MakeObject() member functions (defined by macro) of exception body classes. ExceptionWrapper template has variadic template constructor, which passes all arguments to exception body constructor, when allocates it on heap.

All exception bodies must be inherited from other exception body to preserve inheritance hierarchy.

Exception body declaration must be defined after its forward declaration and ExceptionWrapper aliasing. Alias allows to simplify using of ExceptionWrapper classes. As an example, in library System::Exception is an alias for ExceptionWrapper&lt;System::Details_Exception&gt;.

Exception body class name must be prefixed with "Detail_" string to distinguish them from aliased ExceptionWrapper class instances.

It is needed to define override for DoThrow() method for exception body. This method must throw stack-allocated instance of ExceptionWrapper template class instantiation, which must be created by calling constructor overload, which accepts only exception body shared pointer passed to a DoThrow() call as an argument. This method will be called, once ExceptionWrapper&lt;&gt;::Throw() method is called.

But all this requirements are applied only for the case when library user defines exceptions manually. When CodePorting.Native.Cs2Cpp porter is used, all requirements are met automatically.

## Throwing mechanism description ##

Let us have an instance of ExceptionWrapper&lt;System::Details_Exception&gt; class. This instance contains a shared pointer to System::Details_Exception exception body but actually it is an instance of Details_UnknownValueException exception body. Library user calls ExceptionWrapper&lt;System::Details_Exception&gt;::Throw() method. It calls System::Details_Exception::DoThrow() method from exception body by derefencing shared pointer and passed this shared pointer as an argument to System::Details_Exception::DoThrow() function call. Using dynamic polymorphism Details_UnknownValueException::DoThrow() method is actually called. It creates new instance of ExceptionWrapper&lt;Details_UnknownValueException&gt;, which stores shared pointer to the same exception body instance as a caller and throws fresh ExceptionWrapper&lt;Details_UnknownValueException&gt; instance.

## Minimal code samples ##

{{< highlight cs >}}
// C# code sample
public class SomeException : Exception
{
public SomeException() { }

    public SomeException(object message) : base(message == null ? null : message.ToString()) { }
}
{{< /highlight >}}

{{< highlight cpp >}}
// C++ header
class Details_SomeOtherException;
using SomeOtherException = System::ExceptionWrapper<Details_SomeOtherException>;

class Details_SomeOtherException : public System::Details_DivideByZeroException
{
typedef Details_SomeOtherException ThisType;
typedef System::Details_DivideByZeroException BaseType;

    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
    friend class System::ExceptionWrapperHelper;
    template <typename T> friend class System::ExceptionWrapper;

protected:

    [[noreturn]] void DoThrow(const System::ExceptionPtr& self) const override;
    
    Details_SomeOtherException();
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_SomeOtherException, CODEPORTING_ARGS());

    Details_SomeOtherException(System::SharedPtr<System::Object> message);
    
    MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(Details_SomeOtherException, CODEPORTING_ARGS(System::SharedPtr<System::Object> message));

};
{{< /highlight >}}

{{< highlight cpp >}}
// C++ source
RTTI_INFO_IMPL_HASH_NAMED(927407390u, ::Details_SomeException, "SomeException", ThisTypeBaseTypesInfo);

[[noreturn]] void Details_SomeException::DoThrow(const System::ExceptionPtr& self) const
{
throw System::ExceptionWrapper<Details_SomeException>(self);
}

Details_SomeException::Details_SomeException(System::SharedPtr<System::Object> message)
: System::Details_Exception(message == nullptr ? nullptr : System::ObjectExt::ToString(message))
{    
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_SomeException, CODEPORTING_ARGS(System::SharedPtr<System::Object> message), CODEPORTING_ARGS(message));

Details_SomeException::Details_SomeException() : System::Details_Exception()
{
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(Details_SomeException, CODEPORTING_ARGS(), CODEPORTING_ARGS());
{{< /highlight >}}

{{< highlight cpp >}}
// C++ exception throwing
try
{
// Exception is an alias to ExceptionWrapper<Details_Exception>
Exception exception = nullptr;
try
{
// ExceptionWrapper<Details_SomeException> allocated on stack, Details_SomeException allocated on heap by ExceptionWrapper<Details_SomeException>
// Fresh instance of ExceptionWrapper<Details_SomeException> is thrown
throw SomeException();
}
catch (SomeException& e)
{
// It is caught in catch block and reassigned to `exception` variable with exception type trimming.
// But this trimming is applied to ExceptionWrapper<Details_SomeException> only, which still stores shared pointer to Details_SomeException.
exception = e;
// ExceptionWrapper<Details_Exception>::Throw() method is called.
// It calls Details_SomeException::DoThrow() method.
// Fresh ExceptionWrapper<Details_SomeException> instance created inside Details_SomeException::DoThrow() is thrown
exception.Throw();
}
}
catch (SomeException& e)
{
// ExceptionWrapper<Details_SomeException> instance is caught without exception type trimming
}
{{< /highlight >}}