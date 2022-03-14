---
date: "2022-03-14"
author:
  display_name: "Wiki code generator"
draft: "false"
toc: true
title: "LambdaExpressions"
linktitle: "LambdaExpressions"
menu:
  docs:
    parent: "What Converts to What"
    weight: "1"
lastmod: "2022-03-14"
weight: "1"
---

This example demonstrates how C# lambda expressions are ported to C++.

Additional command-line options passed to CsToCppPorter: none.

## Source C# Code ##

{{< highlight cs >}}
using System;
using System.Collections.Generic;
using System.Linq;

namespace StatementsPorting
{
    public class LambdaExpressions
    {
        public void LambdaWithReturnValueExpressions()
        {
            int delta = 767;
            SomeCalculation(SomeSelector);
            SomeCalculation(value => value + 2);
            SomeCalculation(value => value + delta);
            SomeCalculation(value => value + mSomeDelta);
            SomeCalculation(_ => 777);
            Func<int, int> selector1 = SomeSelector;
            SomeCalculation(selector1);
            Func<int, int> selector2 = value => value + 2;
            SomeCalculation(selector2);
            Func<int, int> selector3 = value => value + delta;
            SomeCalculation(selector3);
            Func<int, int> selector4 = value => value + mSomeDelta;
            SomeCalculation(selector4);
            Func<int, int> selector5 = _ => 777;
            SomeCalculation(selector5);
        }

        public void LambdaWithoutReturnValueExpressions()
        {
            int delta = 767;
            SomeProcessor(SomeAction);
            SomeProcessor(value => Console.WriteLine(value + 2));
            SomeProcessor(value => Console.WriteLine(value + delta));
            SomeProcessor(value => Console.WriteLine(value + mSomeDelta));
            SomeProcessor(_ => Console.WriteLine(777));
            Action<int> action1 = SomeAction;
            SomeProcessor(action1);
            Action<int> action2 = value => Console.WriteLine(value + 2);
            SomeProcessor(action2);
            Action<int> action3 = value => Console.WriteLine(value + delta);
            SomeProcessor(action3);
            Action<int> action4 = value => Console.WriteLine(value + mSomeDelta);
            SomeProcessor(action4);
            Action<int> action5 = _ => Console.WriteLine(777);
            SomeProcessor(action5);
        }

        public void LambdaCurrying()
        {
            Func<int, int, int> someFun = (value1, value2) => value1 + value2;
            Func<int, int> simpleFun1 = value => someFun(value, 666);
            Func<int, int> simpleFun2 = value => someFun(777, value);
            Action<int, int> someAction = (value1, value2) => Console.WriteLine(value1 + value2);
            Action<int> simpleAction1 = value => someAction(value, 666);
            Action<int> simpleAction2 = value => someAction(777, value);
        }

        public void LambdaReturnedFromFunctionExpressions()
        {
            Func<int, int> lambda1 = CreateFun(666);
            Func<int, int> lambda2 = CreateFun(-13);
        }

        delegate void VoidVoidDelegate();

        [CsToCppPorter.CppLambdaPassByValue("value")]
        public void PassVariableByValue()
        {
            var value = 10;
            VoidVoidDelegate lambda = () => Console.WriteLine(value);
        }

        [CsToCppPorter.CppLambdaPassByValue("value1")]
        [CsToCppPorter.CppLambdaPassByValue("value2")]
        public void PassTwoVariablesByValue()
        {
            var value1 = 10;
            var value2 = 20;
            VoidVoidDelegate lambda = () =>
            {
                Console.WriteLine(value1);
                Console.WriteLine(value2);
            };
        }

        [CsToCppPorter.CppLambdaPassByReference("value1")]
        [CsToCppPorter.CppLambdaPassByValue("value2")]
        [CsToCppPorter.CppLambdaUseHolder("value3")]
        public void AllCaptureMechanisms()
        {
            var value1 = 10;
            var value2 = 20;
            var value3 = 30;
            VoidVoidDelegate lambda = () =>
            {
                Console.WriteLine(value1);
                Console.WriteLine(value2);
                Console.WriteLine(value3);
            };
        }

        public void LambdaCapturesForLoopInitializers()
        {
            const int itemsCount = 10;
            for (int i = 0, j = 0; i < itemsCount; i++)
            {
                Func<int> lambda = () => i;
            }
        }

        public void LambdaCapturesLocalVariableInForLoop()
        {
            var value = 10;
            for (int i = 0; i < 20; i++)
            {
                Func<int> lambda = () => i * value;
            }
        }

        public void LambdaCapturesMethodParam(int param)
        {
            Func<int> lambda1 = () => 2 * param;
            Func<int> lambda2 = () => 3 * param;
        }

        public void LambdaCapturesThis1()
        {
            Func<int> lambda = () => this.mSomeDelta;
            lambda();
        }

        public Func<LambdaExpressions> LambdaCapturesThis2()
        {
            Func<LambdaExpressions> lambda1 = () => this;
            Func<LambdaExpressions> lambda2 = () => new LambdaExpressions();
            lambda2 = lambda1;
            return lambda2;
        }

        public LambdaExpressions LambdaCapturesThis3()
        {
            Func<LambdaExpressions> lambda = () => this;
            return lambda();
        }

        public void LambdaCapturesThis4()
        {
            Func<LambdaExpressions> lambda = () => this;
            var instance = lambda();
        }

        public Func<LambdaExpressions> LambdaCapturesThis5()
        {
            Func<LambdaExpressions> lambda = () => new LambdaExpressions();
            lambda = () => this;
            return lambda;
        }

        class LambdaCapturesThis6
        {
            private int m_field = 10;

            public Func<int> Foo()
            {
                try
                {
                    return () => m_field;
                }
                catch (Exception)
                {
                }

                return () => m_field;
            }
        }

        class LambdaCapturesThis7
        {
            private int m_field = 10;
            private bool m_condition = false;

            public Func<int> Foo()
            {
                if (m_condition)
                {
                    return () => 2 * m_field;
                }
                else
                {
                    return () => 3 * m_field;
                }
            }
        }

        class LambdaCapturesThisByValue
        {
            private int m_value = 30;

            [CsToCppPorter.CppLambdaPassByValue("this")]
            public void Foo1()
            {
                Func<int> lambda = () => m_value;
            }

            [CsToCppPorter.CppLambdaPassByValue("this")]
            [CsToCppPorter.CppUseWeakPtrToCaptureThis]
            public void Foo2()
            {
                Func<int> lambda = () => m_value;
            }
        }

        class LambdaCapturesThisByReference
        {
            private Func<int> m_lambda1;
            private Func<LambdaCapturesThisByReference> m_lambda2;
            private int m_value;

            [CsToCppPorter.CppLambdaPassByReference("this")]
            public void Foo1()
            {
                const int value = 30;
                m_lambda1 = () => m_value * value;
            }

            [CsToCppPorter.CppLambdaPassByReference("this")]
            public void Foo2()
            {
                const int value = 30;
                m_lambda2 = () => this;
            }
        }

        public void LambdaCapturesClassField()
        {
            m_lambda = () => mSomeDelta;
        }

        public void LambdaCapturesByte()
        {
            byte b = 10;
            Func<int> lambda = () => b * 2;
        }

        int this[int index]
        {
            get
            {
                Func<int> lambda = () => 2 * index;
                return lambda();
            }
            set
            {
                Func<int> lambda = () => index * value;
                this.mSomeDelta = lambda();
            }
        }

        public void LambdaCapturesBoxedDateTime()
        {
            object value = new DateTime(2020, 08, 31);
            Func<int> lambda = () => value.GetHashCode();
        }

        public void LambdaCapturesBoxedString()
        {
            object value = "1e-02";
            Func<bool> lambda = () => value.Equals("1e-02");
        }

        int SomeDelta
        {
            get { return mSomeDelta; }
            set { mSomeDelta = value; }
        }

        public void LambdaCapturesClassProperty()
        {
            Func<int> lambda = () => SomeDelta;
        }

        class SomeClass1
        {
            SomeClass1()
            {
                Func<SomeClass1> lambda = () => this;
            }

            void Foo()
            {
                Func<SomeClass1> lambda = () => this;
            }
        }

        class SomeClass2
        {
            private Func<int> m_lambda;
            private int m_data;

            [CsToCppPorter.CppUseWeakPtrToCaptureThis]
            SomeClass2()
            {
                m_lambda = () => this.m_data;
            }

            void Foo()
            {
                m_lambda = () => this.m_data;
            }
        }

        [CsToCppPorter.CppUseWeakPtrToCaptureThis]
        class SomeClass3
        {
            private Func<int> m_lambda;
            private int m_data;

            SomeClass3()
            {
                m_lambda = () => this.m_data;
            }

            void Foo()
            {
                m_lambda = () => this.m_data;
            }
        }

        void LambdaCapturesLocalVariables1()
        {
            int value = 30;
            Func<int> lambda1 = () => value;
            var lambda2 = lambda1;
            var result = lambda2();
        }

        Func<int> LambdaCapturesLocalVariables2()
        {
            int value = 30;
            Func<int> lambda1 = () => value;
            var lambda2 = lambda1;
            return lambda2;
        }

        int LambdaCapturesLocalVariables3()
        {
            int value = 30;
            Func<int> lambda = () => value;
            return lambda() + 3;
        }

        public void LambdaIsPassedAsLinqParam1()
        {
            var list = new List<int> {1, 2, 3, 4};
            var multiplier = 10;
            Func<int, int> lambda = (x) => x * multiplier;
            list = list.Select(lambda).ToList();
        }

        public void LambdaIsPassedAsLinqParam2()
        {
            var list = new List<int> {1, 2, 3, 4};
            var multiplier = 10;
            list = list.Select((x) => x * multiplier).ToList();
        }

        private Func<int> m_lambda;

        public void LambdaIsAssignedToClassField1()
        {
            var value = 30;
            Func<int> lambda = () => value;
            this.m_lambda = lambda;
        }

        public void LambdaIsAssignedToClassField2()
        {
            var value = 30;
            Func<int> lambda = () => value;
            m_lambda = lambda;
        }

        public void LambdasCycle1()
        {
            int value = 10;
            Func<int> lambda1 = () => value;
            Func<int> lambda2 = () => 20;
            lambda2 = lambda1;
            lambda1 = lambda2;
        }

        public void LambdasCycle2()
        {
            int value = 10;
            Func<int> lambda1 = () => value;
            Func<int> lambda2 = () => 20;
            lambda2 = lambda1;
            lambda1 = lambda2;
            m_lambda = lambda1;
        }

        public void LambdaAsParam(Func<int, int> lambdaParam)
        {
            var value = lambdaParam(30);
        }

        public void LambdaCapturesIntParam(int param)
        {
            LambdaAsParam((x) => param * x);
        }

        public void CallLambdaCapturesIntParam()
        {
            const int value = 10;
            LambdaCapturesIntParam(value);
        }

        public void LambdaCapturesAnonymousFunction(Func<int, int> lambdaParam)
        {
            LambdaAsParam((x) => x * lambdaParam(15));
        }

        public void CallLambdaCapturesAnonymousFunction()
        {
            const int value = 10;
            Func<int, int> lambda = (x) => x * value;
            LambdaCapturesAnonymousFunction(lambda);
        }

        public void LambdaParamChainToLocal()
        {
            const int value = 10;
            Func<int, int> lambda = (x) => x * value;
            LambdaParamChainToLocal1(lambda);
        }

        public void LambdaParamChainToLocal1(Func<int, int> lambdaParam)
        {
            LambdaParamChainToLocal2(lambdaParam);
        }

        public void LambdaParamChainToLocal2(Func<int, int> lambdaParam)
        {
            var value = lambdaParam(30);
        }

        public void LambdaParamChainToField()
        {
            const int value = 10;
            Func<int> lambda = () => value;
            LambdaParamChainToField1(lambda);
        }

        public void LambdaParamChainToField1(Func<int> lambdaParam)
        {
            LambdaParamChainToField2(lambdaParam);
        }

        public void LambdaParamChainToField2(Func<int> lambdaParam)
        {
            m_lambda = lambdaParam;
        }

        class LambdaPasses1
        {
            public LambdaPasses1(Func<int> lambdaParam)
            {
                var value = lambdaParam();
            }

            public void Foo1(Func<int> lambdaParam)
            {
                var value = lambdaParam();
            }
        }

        class LambdaPasses2
        {
            public LambdaPasses2()
            {
                const int value = 20;
                Func<int> lambda = () => value;
                Foo1(lambda, 3);
            }

            public int Property
            {
                get
                {
                    const int value = 20;
                    Func<int> lambda = () => value;
                    Foo1(lambda, 3);
                    return 0;
                }
                set
                {
                    Func<int> lambda = () => value;
                    Foo1(lambda, 3);
                }
            }

            public int this[int index]
            {
                get
                {
                    Func<int> lambda = () => index;
                    Foo1(lambda, 3);
                    return index;
                }
                set
                {
                    Func<int> lambda = () => index * value;
                    Foo1(lambda, 3);
                }
            }

            public void Foo1(Func<int> lambdaParam1, int count)
            {
                if (count > 0)
                {
                    Foo2(lambdaParam1, --count);
                }
                else
                {
                    var value = lambdaParam1();
                }
            }

            public void Foo2(Func<int> lambdaParam2, int count)
            {
                Foo1(lambdaParam2, count);
            }
        }

        class LambdaPasses3
        {
            private Func<int> m_lambda;

            public LambdaPasses3()
            {
                const int value = 20;
                Func<int> lambda = () => value;
                Foo1(lambda, 3);
            }

            public int Property
            {
                get
                {
                    const int value = 20;
                    Func<int> lambda = () => value;
                    Foo1(lambda, 3);
                    return 0;
                }
                set
                {
                    Func<int> lambda = () => value;
                    Foo1(lambda, 3);
                }
            }

            public int this[int index]
            {
                get
                {
                    Func<int> lambda = () => index;
                    Foo1(lambda, 3);
                    return index;
                }
                set
                {
                    Func<int> lambda = () => index * value;
                    Foo1(lambda, 3);
                }
            }

            public void Foo2(Func<int> lambdaParam2, int count)
            {
                Foo1(lambdaParam2, count);
            }

            public void Foo1(Func<int> lambdaParam1, int count)
            {
                if (count > 0)
                {
                    Foo2(lambdaParam1, --count);
                }
                else
                {
                    m_lambda = lambdaParam1;
                }
            }

            public void Foo3(int param)
            {
                Func<int> lambda = () => param;
                Foo1(lambda, 3);
            }
        }

        public class LambdaPasses4
        {
            private Func<int> m_lambda;

            public LambdaPasses4(int param)
            {
                Func<int> lambda1 = () => param;
                Func<int> lambda2 = () => param;
                Foo(lambda1, lambda2);
            }

            public void Foo()
            {
                const int value = 20;
                Func<int> lambda = () => value;
                Foo(lambda, lambda);
            }

            public void Foo(Func<int> lambdaParam1, Func<int> lambdaParam2)
            {
                var value = lambdaParam1();
                m_lambda = lambdaParam2;
            }
        }

        private void MustCaptureByReference([CsToCppPorter.CppLambdaShouldCaptureByReference]
            Func<int> lambdaParam)
        {
            m_lambda = lambdaParam;
        }

        private void MustUseHolders(Func<int> lambdaParam)
        {
            m_lambda = lambdaParam;
        }

        public void CheckCppLambdaMustCaptureByReferenceAttribute()
        {
            const int value = 20;
            MustCaptureByReference(() => value);
            MustUseHolders(() => value);
        }

        class GenericClass<T>
        {
            private T m_value;

            public GenericClass(T value)
            {
                m_value = value;
            }

            public T Foo(Func<T, T> lambda)
            {
                return lambda(m_value);
            }

            public void Foo()
            {
            }
        }

        public void GenericClassTest()
        {
            const int multiplier = 20;
            Func<int, int> lambda = (value) => multiplier * value;

            var instance = new GenericClass<int>(10);
            var result = instance.Foo(lambda);
        }

        private void SomeCalculation(Func<int, int> selector)
        {
        }

        private int SomeSelector(int value)
        {
            return 777;
        }

        private void SomeProcessor(Action<int> action)
        {
        }

        private void SomeAction(int value)
        {
        }

        private Func<int, int> CreateFun(int delta)
        {
            return value => value + delta;
        }

        private int mSomeDelta = 3343;

        class FinallyCapturesThisBase
        {
            protected void Foo1()
            {
            }
        }

        class FinallyCapturesThis : FinallyCapturesThisBase
        {
            void Foo1()
            {
                base.Foo1();
                try
                {
                    base.Foo1();
                }
                catch (Exception e)
                {
                    base.Foo1();
                    System.Console.WriteLine(e);
                    throw;
                }
                finally
                {
                    base.Foo1();
                }
            }

            void Foo2()
            {
            }

            void Foo3()
            {
                this.Foo2();
                try
                {
                    this.Foo2();
                }
                catch (Exception e)
                {
                    this.Foo2();
                    System.Console.WriteLine(e);
                    throw;
                }
                finally
                {
                    this.Foo2();
                }
            }

            void Foo4()
            {
                Foo2();
                try
                {
                    Foo2();
                }
                catch (Exception e)
                {
                    Foo2();
                    System.Console.WriteLine(e);
                    throw;
                }
                finally
                {
                    Foo2();
                }
            }
        }

        class FinallyCapturesThis2
        {
            private int m_value = 10;

            Func<int> Foo1()
            {
                return () =>
                {
                    try
                    {
                    }
                    catch (Exception)
                    {
                    }
                    finally
                    {
                        m_value = 20;
                    }

                    return m_value;
                };
            }

            Func<int> Foo2()
            {
                Func<int> result = () => m_value;

                try
                {
                }
                catch (Exception)
                {
                }
                finally
                {
                    m_value = 30;
                }

                return result;
            }
        }

        class MultipleLambdas
        {
            private int m_value = 10;
            private Func<int> m_lambda;

            Func<int> Foo()
            {
                Func<int> result = () => m_value;
                Func<int> localLambda = () => m_value;
                m_lambda = () => m_value;
                return result;
            }
        }


        public Func<int> LambdaInsideLambda1
        {
            get
            {
                const int value = 10;
                Func<int> lambda = () =>
                {
                    this.SomeAction(value);
                    VoidVoidDelegate voidDelegate = () => SomeAction(20);
                    voidDelegate();
                    VoidVoidDelegate voidDelegate2 = () =>
                    {
                        SomeAction(30);
                        VoidVoidDelegate voidDelegate3 = () =>
                        {
                            var smth = value;
                            this.SomeAction(smth);
                        };
                        voidDelegate3();
                    };
                    return value;
                };
                return lambda;
            }
            set
            {
                const int smth = 10;
                Func<int> lambda = () =>
                {
                    this.SomeAction(smth);
                    return smth;
                };
            }
        }

        VoidVoidDelegate LambdaInsideLambda2(int param)
        {
            VoidVoidDelegate lambda = () =>
            {
                VoidVoidDelegate lambda2 = () =>
                {
                    var value = param;
                };
            };

            return lambda;
        }

        class LambdaInsideLambda3
        {
            Func<LambdaInsideLambda3> Foo(int param)
            {
                try
                {
                    return () =>
                    {
                        Func<LambdaInsideLambda3> lambda = () =>
                        {
                            var value = param;
                            return this;
                        };
                        return lambda();
                    };
                }
                catch (Exception e)
                {
                    Console.WriteLine(e);
                    throw;
                }
                finally
                {
                    Func<LambdaInsideLambda3> lambdaInsideFinally = () =>
                    {
                        Func<LambdaInsideLambda3> lambda = () =>
                        {
                            var value = param;
                            return this;
                        };
                        return lambda();
                    };
                }
            }
        }

        void LambdaInsideLambda4()
        {
            Func<int, int> lambda = (x) =>
            {
                Func<int> lambda2 = () =>
                {
                    Func<int> lambda3 = () => x;

                    return 0;
                };

                return 0;
            };
        }

        class HasAmbiguityInsideLambdaBase
        {
            public void Foo(int x)
            {
            }
        }

        class HasAmbiguityInsideLambda : HasAmbiguityInsideLambdaBase
        {
            private int m_field = 10;

            public Func<int> Foo()
            {
                return () =>
                {
                    Func<int> lambda = () =>
                    {
                        Foo(10);
                        return m_field;
                    };
                    return lambda();
                };
            }
        }

        class PointF
        {
            public PointF(float x, float y)
            {
            }
        }

        class DataPointElement
        {
            public PointF Center
            {
                get { return new PointF(0, 0); }
            }
        }

        void AddLine(PointF left, PointF right) {}

        public void VariableNameIsSameAsLambdaParameter()
        {
            List<DataPointElement> linePoints = new List<DataPointElement>();
            var pointsTmp = linePoints.ConvertAll(i => i.Center).ToArray();
            PointF[] points = new PointF[pointsTmp.Length];
            pointsTmp.CopyTo(points, 0);

            for (int i = 1; i < linePoints.Count; i++)
            {
                AddLine(points[i - 1], points[i]);
            }
        }

        class ReusingVariable
        {
            private Func<int> m_lambda;

            void Foo()
            {
                var i = 0;
                m_lambda = () => i;
                for (i = 0; i < 10; i++) {}
            }
        }
    }
}

{{< /highlight >}}

## Ported Code ##

### C++ Header ###

{{< highlight cpp >}}
#pragma once

#include <system/object.h>
#include <system/multicast_delegate.h>
#include <system/func.h>
#include <system/details/pointer_collection_helpers.h>
#include <system/details/lambda_capture_holder.h>
#include <system/action.h>
#include <cstdint>

namespace StatementsPorting {

class LambdaExpressions : public System::Object
{
    typedef LambdaExpressions ThisType;
    typedef System::Object BaseType;
    
    typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
    RTTI_INFO_DECL();
    
private:

    using VoidVoidDelegate = System::MulticastDelegate<void()>;
    
    
public:

    class LambdaPasses4 : public System::Object
    {
        typedef LambdaPasses4 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        LambdaPasses4(int32_t param);
        
        void Foo();
        void Foo(System::Func<int32_t> lambdaParam1, System::Func<int32_t> lambdaParam2);
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda;
        
    };
    
    
private:

    class LambdaCapturesThis6 : public System::Object
    {
        typedef LambdaCapturesThis6 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        System::Func<int32_t> Foo();
        
        LambdaCapturesThis6();
        
    private:
    
        int32_t m_field;
        
    };
    
    class LambdaCapturesThis7 : public System::Object
    {
        typedef LambdaCapturesThis7 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        System::Func<int32_t> Foo();
        
        LambdaCapturesThis7();
        
    private:
    
        int32_t m_field;
        bool m_condition;
        
    };
    
    class LambdaCapturesThisByValue : public System::Object
    {
        typedef LambdaCapturesThisByValue ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        void Foo1();
        void Foo2();
        
        LambdaCapturesThisByValue();
        
    private:
    
        int32_t m_value;
        
    };
    
    class LambdaCapturesThisByReference : public System::Object
    {
        typedef LambdaCapturesThisByReference ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        void Foo1();
        void Foo2();
        
        LambdaCapturesThisByReference();
        
    protected:
    
        virtual ~LambdaCapturesThisByReference();
        
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda1;
        System::Func<System::SharedPtr<LambdaExpressions::LambdaCapturesThisByReference>> m_lambda2;
        int32_t m_value;
        
    };
    
    class SomeClass1 : public System::Object
    {
        typedef SomeClass1 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        virtual ~SomeClass1();
        
    private:
    
        SomeClass1();
        
        MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(SomeClass1, CODEPORTING_ARGS());
        void Foo();
        
    };
    
    class SomeClass2 : public System::Object
    {
        typedef SomeClass2 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda;
        int32_t m_data;
        
        SomeClass2();
        
        MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(SomeClass2, CODEPORTING_ARGS());
        void Foo();
        
    };
    
    class SomeClass3 : public System::Object
    {
        typedef SomeClass3 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda;
        int32_t m_data;
        
        SomeClass3();
        
        MEMBER_FUNCTION_MAKE_OBJECT_DECLARATION(SomeClass3, CODEPORTING_ARGS());
        void Foo();
        
    };
    
    class LambdaPasses1 : public System::Object
    {
        typedef LambdaPasses1 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        LambdaPasses1(System::Func<int32_t> lambdaParam);
        
        void Foo1(System::Func<int32_t> lambdaParam);
        
    };
    
    class LambdaPasses2 : public System::Object
    {
        typedef LambdaPasses2 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        int32_t get_Property();
        void set_Property(int32_t value);
        
        LambdaPasses2();
        
        int32_t idx_get(int32_t index);
        void idx_set(int32_t index, int32_t value);
        
        void Foo1(System::Func<int32_t> lambdaParam1, int32_t count);
        void Foo2(System::Func<int32_t> lambdaParam2, int32_t count);
        
    };
    
    class LambdaPasses3 : public System::Object
    {
        typedef LambdaPasses3 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        int32_t get_Property();
        void set_Property(int32_t value);
        
        LambdaPasses3();
        
        int32_t idx_get(int32_t index);
        void idx_set(int32_t index, int32_t value);
        
        void Foo2(System::Func<int32_t> lambdaParam2, int32_t count);
        void Foo1(System::Func<int32_t> lambdaParam1, int32_t count);
        void Foo3(int32_t param);
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda;
        
    };
    
    template<typename T>
    class GenericClass : public System::Object
    {
        typedef GenericClass<T> ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_TEMPLATE_CLASS(ThisType, ThisTypeBaseTypesInfo);
        
        friend class LambdaExpressions;
        
    public:
    
        GenericClass(T value) : m_value(T())
        {
            m_value = value;
        }
        
        T Foo(System::Func<T, T> lambda)
        {
            return lambda(m_value);
        }
        
        void Foo()
        {
        }
        
        void SetTemplateWeakPtr(uint32_t argument) override
        {
            switch (argument)
            {
                case 0:
                    System::Details::CollectionHelpers::SetWeakPointer(m_value);
                    break;
                    
            }
        }
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override
        {
            auto result = System::Object::GetSharedMembers();
            
            result.Add("StatementsPorting::LambdaExpressions::GenericClass::m_value", this->m_value);
            
            return result;
        }
        #endif
        
        
        
    private:
    
        T m_value;
        
    };
    
    class FinallyCapturesThisBase : public System::Object
    {
        typedef FinallyCapturesThisBase ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        void Foo1();
        
    };
    
    class FinallyCapturesThis : public LambdaExpressions::FinallyCapturesThisBase
    {
        typedef FinallyCapturesThis ThisType;
        typedef LambdaExpressions::FinallyCapturesThisBase BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    private:
    
        void Foo1();
        void Foo2();
        void Foo3();
        void Foo4();
        
    };
    
    class FinallyCapturesThis2 : public System::Object
    {
        typedef FinallyCapturesThis2 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        FinallyCapturesThis2();
        
    private:
    
        int32_t m_value;
        
        System::Func<int32_t> Foo1();
        System::Func<int32_t> Foo2();
        
    };
    
    class MultipleLambdas : public System::Object
    {
        typedef MultipleLambdas ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        MultipleLambdas();
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        int32_t m_value;
        System::Func<int32_t> m_lambda;
        
        System::Func<int32_t> Foo();
        
    };
    
    class LambdaInsideLambda3 : public System::Object
    {
        typedef LambdaInsideLambda3 ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        virtual ~LambdaInsideLambda3();
        
    private:
    
        System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>> Foo(int32_t param);
        
    };
    
    class HasAmbiguityInsideLambdaBase : public System::Object
    {
        typedef HasAmbiguityInsideLambdaBase ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        void Foo(int32_t x);
        
    };
    
    class HasAmbiguityInsideLambda : public LambdaExpressions::HasAmbiguityInsideLambdaBase
    {
        typedef HasAmbiguityInsideLambda ThisType;
        typedef LambdaExpressions::HasAmbiguityInsideLambdaBase BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        System::Func<int32_t> Foo();
        
        HasAmbiguityInsideLambda();
        
    private:
    
        int32_t m_field;
        
    };
    
    class PointF : public System::Object
    {
        typedef PointF ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        PointF(float x, float y);
        
    };
    
    class DataPointElement : public System::Object
    {
        typedef DataPointElement ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    public:
    
        System::SharedPtr<LambdaExpressions::PointF> get_Center();
        
    };
    
    class ReusingVariable : public System::Object
    {
        typedef ReusingVariable ThisType;
        typedef System::Object BaseType;
        
        typedef ::System::BaseTypesInfo<BaseType> ThisTypeBaseTypesInfo;
        RTTI_INFO_DECL();
        
    protected:
    
        #ifdef ASPOSE_GET_SHARED_MEMBERS
        System::Object::shared_members_type GetSharedMembers() override;
        #endif
        
        
    private:
    
        System::Func<int32_t> m_lambda;
        
        void Foo();
        
    };
    
    
public:

    System::Func<int32_t> get_LambdaInsideLambda1();
    void set_LambdaInsideLambda1(System::Func<int32_t> value);
    
    void LambdaWithReturnValueExpressions();
    void LambdaWithoutReturnValueExpressions();
    void LambdaCurrying();
    void LambdaReturnedFromFunctionExpressions();
    void PassVariableByValue();
    void PassTwoVariablesByValue();
    void AllCaptureMechanisms();
    void LambdaCapturesForLoopInitializers();
    void LambdaCapturesLocalVariableInForLoop();
    void LambdaCapturesMethodParam(int32_t param);
    void LambdaCapturesThis1();
    System::Func<System::SharedPtr<LambdaExpressions>> LambdaCapturesThis2();
    System::SharedPtr<LambdaExpressions> LambdaCapturesThis3();
    void LambdaCapturesThis4();
    System::Func<System::SharedPtr<LambdaExpressions>> LambdaCapturesThis5();
    void LambdaCapturesClassField();
    void LambdaCapturesByte();
    void LambdaCapturesBoxedDateTime();
    void LambdaCapturesBoxedString();
    void LambdaCapturesClassProperty();
    void LambdaIsPassedAsLinqParam1();
    void LambdaIsPassedAsLinqParam2();
    void LambdaIsAssignedToClassField1();
    void LambdaIsAssignedToClassField2();
    void LambdasCycle1();
    void LambdasCycle2();
    void LambdaAsParam(System::Func<int32_t, int32_t> lambdaParam);
    void LambdaCapturesIntParam(int32_t param);
    void CallLambdaCapturesIntParam();
    void LambdaCapturesAnonymousFunction(System::Func<int32_t, int32_t> lambdaParam);
    void CallLambdaCapturesAnonymousFunction();
    void LambdaParamChainToLocal();
    void LambdaParamChainToLocal1(System::Func<int32_t, int32_t> lambdaParam);
    void LambdaParamChainToLocal2(System::Func<int32_t, int32_t> lambdaParam);
    void LambdaParamChainToField();
    void LambdaParamChainToField1(System::Func<int32_t> lambdaParam);
    void LambdaParamChainToField2(System::Func<int32_t> lambdaParam);
    void CheckCppLambdaMustCaptureByReferenceAttribute();
    void GenericClassTest();
    void VariableNameIsSameAsLambdaParameter();
    
    LambdaExpressions();
    
protected:

    virtual ~LambdaExpressions();
    
    #ifdef ASPOSE_GET_SHARED_MEMBERS
    System::Object::shared_members_type GetSharedMembers() override;
    #endif
    
    
private:

    int32_t get_SomeDelta();
    void set_SomeDelta(int32_t value);
    
    System::Func<int32_t> m_lambda;
    int32_t mSomeDelta;
    
    int32_t idx_get(int32_t index);
    void idx_set(int32_t index, int32_t value);
    
    void LambdaCapturesLocalVariables1();
    System::Func<int32_t> LambdaCapturesLocalVariables2();
    int32_t LambdaCapturesLocalVariables3();
    void MustCaptureByReference(System::Func<int32_t> lambdaParam);
    void MustUseHolders(System::Func<int32_t> lambdaParam);
    void SomeCalculation(System::Func<int32_t, int32_t> selector);
    int32_t SomeSelector(int32_t value);
    void SomeProcessor(System::Action<int32_t> action);
    void SomeAction(int32_t value);
    System::Func<int32_t, int32_t> CreateFun(int32_t delta);
    LambdaExpressions::VoidVoidDelegate LambdaInsideLambda2(int32_t param);
    void LambdaInsideLambda4();
    void AddLine(System::SharedPtr<LambdaExpressions::PointF> left, System::SharedPtr<LambdaExpressions::PointF> right);
    
};

} // namespace StatementsPorting



{{< /highlight >}}

### C++ Source Code ###

{{< highlight cpp >}}
#include "LambdaExpressions.h"

#include <system/scope_guard.h>
#include <system/object_ext.h>
#include <system/linq/enumerable.h>
#include <system/exceptions.h>
#include <system/date_time.h>
#include <system/converter.h>
#include <system/console.h>
#include <system/collections/list.h>
#include <system/collections/ienumerable.h>
#include <system/array.h>
#include <functional>

namespace StatementsPorting {

RTTI_INFO_IMPL_HASH(3460900502u, ::StatementsPorting::LambdaExpressions::ReusingVariable, ThisTypeBaseTypesInfo);

void LambdaExpressions::ReusingVariable::Foo()
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_i = 0;
    int32_t &i = _lch_i.GetCapture();
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_i, &i]() -> int32_t
    {
        return i;
    })).template AddHeldVariable<System::Func<int32_t>>("i", i);
    for (i = 0; i < 10; i++)
    {
    }
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::ReusingVariable::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::ReusingVariable::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(599952173u, ::StatementsPorting::LambdaExpressions::DataPointElement, ThisTypeBaseTypesInfo);

System::SharedPtr<LambdaExpressions::PointF> LambdaExpressions::DataPointElement::get_Center()
{
    return System::MakeObject<LambdaExpressions::PointF>(0.0f, 0.0f);
}

RTTI_INFO_IMPL_HASH(3201930733u, ::StatementsPorting::LambdaExpressions::PointF, ThisTypeBaseTypesInfo);

LambdaExpressions::PointF::PointF(float x, float y)
{
}

RTTI_INFO_IMPL_HASH(2620931565u, ::StatementsPorting::LambdaExpressions::HasAmbiguityInsideLambda, ThisTypeBaseTypesInfo);

System::Func<int32_t> LambdaExpressions::HasAmbiguityInsideLambda::Foo()
{
    auto self = System::SharedPtr<LambdaExpressions::HasAmbiguityInsideLambda>(this);
    return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&self]() -> int32_t
        {
            self->LambdaExpressions::HasAmbiguityInsideLambdaBase::Foo(10);
            return self->m_field;
        }));
        return lambda();
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

LambdaExpressions::HasAmbiguityInsideLambda::HasAmbiguityInsideLambda() : m_field(10)
{
}

RTTI_INFO_IMPL_HASH(3349675710u, ::StatementsPorting::LambdaExpressions::HasAmbiguityInsideLambdaBase, ThisTypeBaseTypesInfo);

void LambdaExpressions::HasAmbiguityInsideLambdaBase::Foo(int32_t x)
{
}

RTTI_INFO_IMPL_HASH(3489867922u, ::StatementsPorting::LambdaExpressions::LambdaInsideLambda3, ThisTypeBaseTypesInfo);

System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>> LambdaExpressions::LambdaInsideLambda3::Foo(int32_t param)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_param = param;
    int32_t& _param = _lch_param.GetCapture();
    
    {
        auto __finally_guard_0 = ::System::MakeScopeGuard([&param, this]()
        {
            System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>> lambdaInsideFinally = System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>()>>([&param, this]() -> System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>
            {
                System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>> lambda = System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>()>>([&param, this]() -> System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>
                {
                    int32_t value = param;
                    return System::MakeSharedPtr(this);
                }));
                return lambda();
            }));
        });
        
        try
        {
            auto self = System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>(this);
            return System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>()>>([_lch_param, &_param, self]() -> System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>
            {
                System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>> lambda = System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>()>>([&_param, &self]() -> System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>
                {
                    int32_t value = _param;
                    return self;
                }));
                return lambda();
            })).template AddHeldVariable<System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>>("_param", _param).template AddHeldVariable<System::Func<System::SharedPtr<LambdaExpressions::LambdaInsideLambda3>>>("self", self);
        }
        catch (System::Exception& e)
        {
            System::Console::WriteLine(e);
            throw;
        }
    }
}

LambdaExpressions::LambdaInsideLambda3::~LambdaInsideLambda3()
{
}

RTTI_INFO_IMPL_HASH(2208338373u, ::StatementsPorting::LambdaExpressions::MultipleLambdas, ThisTypeBaseTypesInfo);

System::Func<int32_t> LambdaExpressions::MultipleLambdas::Foo()
{
    auto self = System::SharedPtr<LambdaExpressions::MultipleLambdas>(this);
    System::Func<int32_t> result = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    System::Func<int32_t> localLambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([this]() -> int32_t
    {
        return m_value;
    }));
    // TO DO: The lambda expression captures this by the strong reference, and is assigned to the class field or property
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    return result;
}

LambdaExpressions::MultipleLambdas::MultipleLambdas() : m_value(10)
{
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::MultipleLambdas::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::MultipleLambdas::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(1180203451u, ::StatementsPorting::LambdaExpressions::FinallyCapturesThis2, ThisTypeBaseTypesInfo);

System::Func<int32_t> LambdaExpressions::FinallyCapturesThis2::Foo1()
{
    auto self = System::SharedPtr<LambdaExpressions::FinallyCapturesThis2>(this);
    return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        
        {
            auto __finally_guard_0 = ::System::MakeScopeGuard([&self]()
            {
                self->m_value = 20;
            });
            
            try
            {
            }
            catch (System::Exception& )
            {
            }
        }
        
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

System::Func<int32_t> LambdaExpressions::FinallyCapturesThis2::Foo2()
{
    auto self = System::SharedPtr<LambdaExpressions::FinallyCapturesThis2>(this);
    System::Func<int32_t> result = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    
    
    {
        auto __finally_guard_0 = ::System::MakeScopeGuard([this]()
        {
            m_value = 30;
        });
        
        try
        {
        }
        catch (System::Exception& )
        {
        }
    }
    
    return result;
}

LambdaExpressions::FinallyCapturesThis2::FinallyCapturesThis2() : m_value(10)
{
}

RTTI_INFO_IMPL_HASH(4274230327u, ::StatementsPorting::LambdaExpressions::FinallyCapturesThis, ThisTypeBaseTypesInfo);

void LambdaExpressions::FinallyCapturesThis::Foo1()
{
    StatementsPorting::LambdaExpressions::FinallyCapturesThisBase::Foo1();
    
    {
        auto __finally_guard_0 = ::System::MakeScopeGuard([this]()
        {
            StatementsPorting::LambdaExpressions::FinallyCapturesThisBase::Foo1();
        });
        
        try
        {
            StatementsPorting::LambdaExpressions::FinallyCapturesThisBase::Foo1();
        }
        catch (System::Exception& e)
        {
            StatementsPorting::LambdaExpressions::FinallyCapturesThisBase::Foo1();
            System::Console::WriteLine(e);
            throw;
        }
    }
}

void LambdaExpressions::FinallyCapturesThis::Foo2()
{
}

void LambdaExpressions::FinallyCapturesThis::Foo3()
{
    this->Foo2();
    
    {
        auto __finally_guard_0 = ::System::MakeScopeGuard([this]()
        {
            this->Foo2();
        });
        
        try
        {
            this->Foo2();
        }
        catch (System::Exception& e)
        {
            this->Foo2();
            System::Console::WriteLine(e);
            throw;
        }
    }
}

void LambdaExpressions::FinallyCapturesThis::Foo4()
{
    Foo2();
    
    {
        auto __finally_guard_0 = ::System::MakeScopeGuard([this]()
        {
            Foo2();
        });
        
        try
        {
            Foo2();
        }
        catch (System::Exception& e)
        {
            Foo2();
            System::Console::WriteLine(e);
            throw;
        }
    }
}

RTTI_INFO_IMPL_HASH(3379612424u, ::StatementsPorting::LambdaExpressions::FinallyCapturesThisBase, ThisTypeBaseTypesInfo);

void LambdaExpressions::FinallyCapturesThisBase::Foo1()
{
}

RTTI_INFO_IMPL_HASH(47662935u, ::StatementsPorting::LambdaExpressions::LambdaPasses4, ThisTypeBaseTypesInfo);

LambdaExpressions::LambdaPasses4::LambdaPasses4(int32_t param)
{
    //Self Reference+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    System::Details::ThisProtector __local_self_ref(this);
    //---------------------------------------------------------Self Reference
    
    System::Details::LambdaCaptureHolder<int32_t> _lch_param = param;
    int32_t& _param = _lch_param.GetCapture();
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&param]() -> int32_t
    {
        return param;
    }));
    System::Func<int32_t> lambda2 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_param, &_param]() -> int32_t
    {
        return _param;
    })).template AddHeldVariable<System::Func<int32_t>>("_param", _param);
    Foo(lambda1, lambda2);
}

void LambdaExpressions::LambdaPasses4::Foo()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 20;
    const int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    Foo(lambda, lambda);
}

void LambdaExpressions::LambdaPasses4::Foo(System::Func<int32_t> lambdaParam1, System::Func<int32_t> lambdaParam2)
{
    int32_t value = lambdaParam1();
    m_lambda = lambdaParam2;
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::LambdaPasses4::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::LambdaPasses4::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(47662934u, ::StatementsPorting::LambdaExpressions::LambdaPasses3, ThisTypeBaseTypesInfo);

int32_t LambdaExpressions::LambdaPasses3::get_Property()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 20;
    const int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    Foo1(lambda, 3);
    return 0;
}

void LambdaExpressions::LambdaPasses3::set_Property(int32_t value)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = value;
    int32_t& _value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &_value]() -> int32_t
    {
        return _value;
    })).template AddHeldVariable<System::Func<int32_t>>("_value", _value);
    Foo1(lambda, 3);
}

LambdaExpressions::LambdaPasses3::LambdaPasses3()
{
    //Self Reference+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    System::Details::ThisProtector __local_self_ref(this);
    //---------------------------------------------------------Self Reference
    
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 20;
    const int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    Foo1(lambda, 3);
}

int32_t LambdaExpressions::LambdaPasses3::idx_get(int32_t index)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_index = index;
    int32_t& _index = _lch_index.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_index, &_index]() -> int32_t
    {
        return _index;
    })).template AddHeldVariable<System::Func<int32_t>>("_index", _index);
    Foo1(lambda, 3);
    return index;
}

void LambdaExpressions::LambdaPasses3::idx_set(int32_t index, int32_t value)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_index = index;
    int32_t& _index = _lch_index.GetCapture();
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = value;
    int32_t& _value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_index, &_index, _lch_value, &_value]() -> int32_t
    {
        return _index * _value;
    })).template AddHeldVariable<System::Func<int32_t>>("_index", _index).template AddHeldVariable<System::Func<int32_t>>("_value", _value);
    Foo1(lambda, 3);
}

void LambdaExpressions::LambdaPasses3::Foo2(System::Func<int32_t> lambdaParam2, int32_t count)
{
    Foo1(lambdaParam2, count);
}

void LambdaExpressions::LambdaPasses3::Foo1(System::Func<int32_t> lambdaParam1, int32_t count)
{
    if (count > 0)
    {
        Foo2(lambdaParam1, --count);
    }
    else
    {
        m_lambda = lambdaParam1;
    }
}

void LambdaExpressions::LambdaPasses3::Foo3(int32_t param)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_param = param;
    int32_t& _param = _lch_param.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_param, &_param]() -> int32_t
    {
        return _param;
    })).template AddHeldVariable<System::Func<int32_t>>("_param", _param);
    Foo1(lambda, 3);
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::LambdaPasses3::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::LambdaPasses3::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(47662933u, ::StatementsPorting::LambdaExpressions::LambdaPasses2, ThisTypeBaseTypesInfo);

int32_t LambdaExpressions::LambdaPasses2::get_Property()
{
    const int32_t value = 20;
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    Foo1(lambda, 3);
    return 0;
}

void LambdaExpressions::LambdaPasses2::set_Property(int32_t value)
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    Foo1(lambda, 3);
}

LambdaExpressions::LambdaPasses2::LambdaPasses2()
{
    //Self Reference+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    System::Details::ThisProtector __local_self_ref(this);
    //---------------------------------------------------------Self Reference
    
    const int32_t value = 20;
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    Foo1(lambda, 3);
}

int32_t LambdaExpressions::LambdaPasses2::idx_get(int32_t index)
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&index]() -> int32_t
    {
        return index;
    }));
    Foo1(lambda, 3);
    return index;
}

void LambdaExpressions::LambdaPasses2::idx_set(int32_t index, int32_t value)
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&index, &value]() -> int32_t
    {
        return index * value;
    }));
    Foo1(lambda, 3);
}

void LambdaExpressions::LambdaPasses2::Foo1(System::Func<int32_t> lambdaParam1, int32_t count)
{
    if (count > 0)
    {
        Foo2(lambdaParam1, --count);
    }
    else
    {
        int32_t value = lambdaParam1();
    }
}

void LambdaExpressions::LambdaPasses2::Foo2(System::Func<int32_t> lambdaParam2, int32_t count)
{
    Foo1(lambdaParam2, count);
}

RTTI_INFO_IMPL_HASH(47662932u, ::StatementsPorting::LambdaExpressions::LambdaPasses1, ThisTypeBaseTypesInfo);

LambdaExpressions::LambdaPasses1::LambdaPasses1(System::Func<int32_t> lambdaParam)
{
    //Self Reference+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    System::Details::ThisProtector __local_self_ref(this);
    //---------------------------------------------------------Self Reference
    
    int32_t value = lambdaParam();
}

void LambdaExpressions::LambdaPasses1::Foo1(System::Func<int32_t> lambdaParam)
{
    int32_t value = lambdaParam();
}

RTTI_INFO_IMPL_HASH(2223103238u, ::StatementsPorting::LambdaExpressions::SomeClass3, ThisTypeBaseTypesInfo);

LambdaExpressions::SomeClass3::SomeClass3() : m_data(0)
{
    auto self = System::WeakPtr<LambdaExpressions::SomeClass3>(this);
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_data;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(LambdaExpressions::SomeClass3, CODEPORTING_ARGS(), CODEPORTING_ARGS());

void LambdaExpressions::SomeClass3::Foo()
{
    auto self = System::WeakPtr<LambdaExpressions::SomeClass3>(this);
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_data;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::SomeClass3::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::SomeClass3::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(2223103237u, ::StatementsPorting::LambdaExpressions::SomeClass2, ThisTypeBaseTypesInfo);

LambdaExpressions::SomeClass2::SomeClass2() : m_data(0)
{
    auto self = System::WeakPtr<LambdaExpressions::SomeClass2>(this);
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_data;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(LambdaExpressions::SomeClass2, CODEPORTING_ARGS(), CODEPORTING_ARGS());

void LambdaExpressions::SomeClass2::Foo()
{
    auto self = System::SharedPtr<LambdaExpressions::SomeClass2>(this);
    // TO DO: The lambda expression captures this by the strong reference, and is assigned to the class field or property
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_data;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::SomeClass2::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::SomeClass2::m_lambda", this->m_lambda);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(2223103236u, ::StatementsPorting::LambdaExpressions::SomeClass1, ThisTypeBaseTypesInfo);

LambdaExpressions::SomeClass1::SomeClass1()
{
    //Self Reference+++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    System::Details::ThisProtector __local_self_ref(this);
    //---------------------------------------------------------Self Reference
    
    System::Func<System::SharedPtr<LambdaExpressions::SomeClass1>> lambda = System::Func<System::SharedPtr<LambdaExpressions::SomeClass1>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::SomeClass1>()>>([this]() -> System::SharedPtr<LambdaExpressions::SomeClass1>
    {
        return System::MakeSharedPtr(this);
    }));
}

MEMBER_FUNCTION_MAKE_OBJECT_DEFINITION(LambdaExpressions::SomeClass1, CODEPORTING_ARGS(), CODEPORTING_ARGS());

void LambdaExpressions::SomeClass1::Foo()
{
    System::Func<System::SharedPtr<LambdaExpressions::SomeClass1>> lambda = System::Func<System::SharedPtr<LambdaExpressions::SomeClass1>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::SomeClass1>()>>([this]() -> System::SharedPtr<LambdaExpressions::SomeClass1>
    {
        return System::MakeSharedPtr(this);
    }));
}

LambdaExpressions::SomeClass1::~SomeClass1()
{
}

RTTI_INFO_IMPL_HASH(1767181163u, ::StatementsPorting::LambdaExpressions::LambdaCapturesThisByReference, ThisTypeBaseTypesInfo);

void LambdaExpressions::LambdaCapturesThisByReference::Foo1()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 30;
    const int32_t &value = _lch_value.GetCapture();
    m_lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value, this]() -> int32_t
    {
        return m_value * value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
}

void LambdaExpressions::LambdaCapturesThisByReference::Foo2()
{
    const int32_t value = 30;
    m_lambda2 = static_cast<std::function<System::SharedPtr<LambdaExpressions::LambdaCapturesThisByReference>()>>([this]() -> System::SharedPtr<LambdaExpressions::LambdaCapturesThisByReference>
    {
        return System::MakeSharedPtr(this);
    });
}

LambdaExpressions::LambdaCapturesThisByReference::LambdaCapturesThisByReference() : m_value(0)
{
}

LambdaExpressions::LambdaCapturesThisByReference::~LambdaCapturesThisByReference()
{
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::LambdaCapturesThisByReference::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::LambdaCapturesThisByReference::m_lambda1", this->m_lambda1);
    result.Add("StatementsPorting::LambdaExpressions::LambdaCapturesThisByReference::m_lambda2", this->m_lambda2);
    
    return result;
}
#endif

RTTI_INFO_IMPL_HASH(692807633u, ::StatementsPorting::LambdaExpressions::LambdaCapturesThisByValue, ThisTypeBaseTypesInfo);

void LambdaExpressions::LambdaCapturesThisByValue::Foo1()
{
    auto self = System::SharedPtr<LambdaExpressions::LambdaCapturesThisByValue>(this);
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

void LambdaExpressions::LambdaCapturesThisByValue::Foo2()
{
    auto self = System::WeakPtr<LambdaExpressions::LambdaCapturesThisByValue>(this);
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_value;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

LambdaExpressions::LambdaCapturesThisByValue::LambdaCapturesThisByValue() : m_value(30)
{
}

RTTI_INFO_IMPL_HASH(48475182u, ::StatementsPorting::LambdaExpressions::LambdaCapturesThis7, ThisTypeBaseTypesInfo);

System::Func<int32_t> LambdaExpressions::LambdaCapturesThis7::Foo()
{
    if (m_condition)
    {
        auto self = System::SharedPtr<LambdaExpressions::LambdaCapturesThis7>(this);
        return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
        {
            return 2 * self->m_field;
        })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    }
    else
    {
        auto self = System::SharedPtr<LambdaExpressions::LambdaCapturesThis7>(this);
        return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
        {
            return 3 * self->m_field;
        })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    }
}

LambdaExpressions::LambdaCapturesThis7::LambdaCapturesThis7() : m_field(10), m_condition(false)
{
}

RTTI_INFO_IMPL_HASH(48475181u, ::StatementsPorting::LambdaExpressions::LambdaCapturesThis6, ThisTypeBaseTypesInfo);

System::Func<int32_t> LambdaExpressions::LambdaCapturesThis6::Foo()
{
    try
    {
        auto self = System::SharedPtr<LambdaExpressions::LambdaCapturesThis6>(this);
        return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
        {
            return self->m_field;
        })).template AddHeldVariable<System::Func<int32_t>>("self", self);
    }
    catch (System::Exception& )
    {
    }
    
    
    auto self = System::SharedPtr<LambdaExpressions::LambdaCapturesThis6>(this);
    return System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->m_field;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

LambdaExpressions::LambdaCapturesThis6::LambdaCapturesThis6() : m_field(10)
{
}


RTTI_INFO_IMPL_HASH(539392951u, ::StatementsPorting::LambdaExpressions, ThisTypeBaseTypesInfo);

int32_t LambdaExpressions::get_SomeDelta()
{
    return mSomeDelta;
}

void LambdaExpressions::set_SomeDelta(int32_t value)
{
    mSomeDelta = value;
}

System::Func<int32_t> LambdaExpressions::get_LambdaInsideLambda1()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 10;
    const int32_t &value = _lch_value.GetCapture();
    auto self = System::SharedPtr<LambdaExpressions>(this);
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value, self]() -> int32_t
    {
        self->SomeAction(value);
        LambdaExpressions::VoidVoidDelegate voidDelegate = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([&self]() -> void
        {
            self->SomeAction(20);
        }));
        voidDelegate();
        LambdaExpressions::VoidVoidDelegate voidDelegate2 = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([&value, &self]() -> void
        {
            self->SomeAction(30);
            LambdaExpressions::VoidVoidDelegate voidDelegate3 = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([&value, &self]() -> void
            {
                int32_t smth = value;
                self->SomeAction(smth);
            }));
            voidDelegate3();
        }));
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value).template AddHeldVariable<System::Func<int32_t>>("self", self);
    return lambda;
}

void LambdaExpressions::set_LambdaInsideLambda1(System::Func<int32_t> value)
{
    const int32_t smth = 10;
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&smth, this]() -> int32_t
    {
        this->SomeAction(smth);
        return smth;
    }));
}

void LambdaExpressions::LambdaWithReturnValueExpressions()
{
    int32_t delta = 767;
    SomeCalculation(static_cast<System::Func<int32_t, int32_t>>(std::bind(&LambdaExpressions::SomeSelector, this, std::placeholders::_1)));
    SomeCalculation(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t value)>>([](int32_t value) -> int32_t
    {
        return value + 2;
    })));
    SomeCalculation(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t value)>>([&delta](int32_t value) -> int32_t
    {
        return value + delta;
    })));
    SomeCalculation(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t value)>>([this](int32_t value) -> int32_t
    {
        return value + mSomeDelta;
    })));
    SomeCalculation(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t _)>>([](int32_t _) -> int32_t
    {
        return 777;
    })));
    System::Func<int32_t, int32_t> selector1 = System::Func<int32_t, int32_t>(std::bind(&LambdaExpressions::SomeSelector, this, std::placeholders::_1));
    SomeCalculation(selector1);
    System::Func<int32_t, int32_t> selector2 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([](int32_t value) -> int32_t
    {
        return value + 2;
    }));
    SomeCalculation(selector2);
    System::Func<int32_t, int32_t> selector3 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([&delta](int32_t value) -> int32_t
    {
        return value + delta;
    }));
    SomeCalculation(selector3);
    System::Func<int32_t, int32_t> selector4 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([this](int32_t value) -> int32_t
    {
        return value + mSomeDelta;
    }));
    SomeCalculation(selector4);
    System::Func<int32_t, int32_t> selector5 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t _)>>([](int32_t _) -> int32_t
    {
        return 777;
    }));
    SomeCalculation(selector5);
}

void LambdaExpressions::LambdaWithoutReturnValueExpressions()
{
    int32_t delta = 767;
    SomeProcessor(static_cast<System::Action<int32_t>>(std::bind(&LambdaExpressions::SomeAction, this, std::placeholders::_1)));
    SomeProcessor(static_cast<System::Action<int32_t>>(static_cast<std::function<void(int32_t value)>>([](int32_t value) -> void
    {
        System::Console::WriteLine(value + 2);
    })));
    SomeProcessor(static_cast<System::Action<int32_t>>(static_cast<std::function<void(int32_t value)>>([&delta](int32_t value) -> void
    {
        System::Console::WriteLine(value + delta);
    })));
    SomeProcessor(static_cast<System::Action<int32_t>>(static_cast<std::function<void(int32_t value)>>([this](int32_t value) -> void
    {
        System::Console::WriteLine(value + mSomeDelta);
    })));
    SomeProcessor(static_cast<System::Action<int32_t>>(static_cast<std::function<void(int32_t _)>>([](int32_t _) -> void
    {
        System::Console::WriteLine(777);
    })));
    System::Action<int32_t> action1 = System::Action<int32_t>(std::bind(&LambdaExpressions::SomeAction, this, std::placeholders::_1));
    SomeProcessor(action1);
    System::Action<int32_t> action2 = System::Action<int32_t>(static_cast<std::function<void(int32_t value)>>([](int32_t value) -> void
    {
        System::Console::WriteLine(value + 2);
    }));
    SomeProcessor(action2);
    System::Action<int32_t> action3 = System::Action<int32_t>(static_cast<std::function<void(int32_t value)>>([&delta](int32_t value) -> void
    {
        System::Console::WriteLine(value + delta);
    }));
    SomeProcessor(action3);
    System::Action<int32_t> action4 = System::Action<int32_t>(static_cast<std::function<void(int32_t value)>>([this](int32_t value) -> void
    {
        System::Console::WriteLine(value + mSomeDelta);
    }));
    SomeProcessor(action4);
    System::Action<int32_t> action5 = System::Action<int32_t>(static_cast<std::function<void(int32_t _)>>([](int32_t _) -> void
    {
        System::Console::WriteLine(777);
    }));
    SomeProcessor(action5);
}

void LambdaExpressions::LambdaCurrying()
{
    System::Func<int32_t, int32_t, int32_t> someFun = System::Func<int32_t, int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value1, int32_t value2)>>([](int32_t value1, int32_t value2) -> int32_t
    {
        return value1 + value2;
    }));
    System::Func<int32_t, int32_t> simpleFun1 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([&someFun](int32_t value) -> int32_t
    {
        return someFun(value, 666);
    }));
    System::Func<int32_t, int32_t> simpleFun2 = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([&someFun](int32_t value) -> int32_t
    {
        return someFun(777, value);
    }));
    System::Action<int32_t, int32_t> someAction = System::Action<int32_t, int32_t>(static_cast<std::function<void(int32_t value1, int32_t value2)>>([](int32_t value1, int32_t value2) -> void
    {
        System::Console::WriteLine(value1 + value2);
    }));
    System::Action<int32_t> simpleAction1 = System::Action<int32_t>(static_cast<std::function<void(int32_t value)>>([&someAction](int32_t value) -> void
    {
        someAction(value, 666);
    }));
    System::Action<int32_t> simpleAction2 = System::Action<int32_t>(static_cast<std::function<void(int32_t value)>>([&someAction](int32_t value) -> void
    {
        someAction(777, value);
    }));
}

void LambdaExpressions::LambdaReturnedFromFunctionExpressions()
{
    System::Func<int32_t, int32_t> lambda1 = System::Func<int32_t, int32_t>(CreateFun(666));
    System::Func<int32_t, int32_t> lambda2 = System::Func<int32_t, int32_t>(CreateFun(-13));
}

void LambdaExpressions::PassVariableByValue()
{
    int32_t value = 10;
    LambdaExpressions::VoidVoidDelegate lambda = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([value]() -> void
    {
        System::Console::WriteLine(value);
    })).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("value", value);
}

void LambdaExpressions::PassTwoVariablesByValue()
{
    int32_t value1 = 10;
    int32_t value2 = 20;
    LambdaExpressions::VoidVoidDelegate lambda = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([value1, value2]() -> void
    {
        System::Console::WriteLine(value1);
        System::Console::WriteLine(value2);
    })).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("value1", value1).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("value2", value2);
}

void LambdaExpressions::AllCaptureMechanisms()
{
    int32_t value1 = 10;
    int32_t value2 = 20;
    System::Details::LambdaCaptureHolder<int32_t> _lch_value3 = 30;
    int32_t &value3 = _lch_value3.GetCapture();
    LambdaExpressions::VoidVoidDelegate lambda = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([&value1, value2, _lch_value3, &value3]() -> void
    {
        System::Console::WriteLine(value1);
        System::Console::WriteLine(value2);
        System::Console::WriteLine(value3);
    })).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("value2", value2).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("value3", value3);
}

void LambdaExpressions::LambdaCapturesForLoopInitializers()
{
    const int32_t itemsCount = 10;
    for (int32_t i = 0, j = 0; i < itemsCount; i++)
    {
        System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&i]() -> int32_t
        {
            return i;
        }));
    }
}

void LambdaExpressions::LambdaCapturesLocalVariableInForLoop()
{
    int32_t value = 10;
    for (int32_t i = 0; i < 20; i++)
    {
        System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&i, &value]() -> int32_t
        {
            return i * value;
        }));
    }
}

void LambdaExpressions::LambdaCapturesMethodParam(int32_t param)
{
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&param]() -> int32_t
    {
        return 2 * param;
    }));
    System::Func<int32_t> lambda2 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&param]() -> int32_t
    {
        return 3 * param;
    }));
}

void LambdaExpressions::LambdaCapturesThis1()
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([this]() -> int32_t
    {
        return this->mSomeDelta;
    }));
    lambda();
}

System::Func<System::SharedPtr<LambdaExpressions>> LambdaExpressions::LambdaCapturesThis2()
{
    auto self = System::SharedPtr<LambdaExpressions>(this);
    System::Func<System::SharedPtr<LambdaExpressions>> lambda1 = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([self]() -> System::SharedPtr<LambdaExpressions>
    {
        return self;
    })).template AddHeldVariable<System::Func<System::SharedPtr<LambdaExpressions>>>("self", self);
    System::Func<System::SharedPtr<LambdaExpressions>> lambda2 = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([]() -> System::SharedPtr<LambdaExpressions>
    {
        return System::MakeObject<LambdaExpressions>();
    }));
    lambda2 = lambda1;
    return lambda2;
}

System::SharedPtr<LambdaExpressions> LambdaExpressions::LambdaCapturesThis3()
{
    System::Func<System::SharedPtr<LambdaExpressions>> lambda = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([this]() -> System::SharedPtr<LambdaExpressions>
    {
        return System::MakeSharedPtr(this);
    }));
    return lambda();
}

void LambdaExpressions::LambdaCapturesThis4()
{
    System::Func<System::SharedPtr<LambdaExpressions>> lambda = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([this]() -> System::SharedPtr<LambdaExpressions>
    {
        return System::MakeSharedPtr(this);
    }));
    auto instance = lambda();
}

System::Func<System::SharedPtr<LambdaExpressions>> LambdaExpressions::LambdaCapturesThis5()
{
    System::Func<System::SharedPtr<LambdaExpressions>> lambda = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([]() -> System::SharedPtr<LambdaExpressions>
    {
        return System::MakeObject<LambdaExpressions>();
    }));
    auto self = System::SharedPtr<LambdaExpressions>(this);
    // TO DO: The lambda expression captures this by the strong reference, and is assigned to the class field or property
    lambda = System::Func<System::SharedPtr<LambdaExpressions>>(static_cast<std::function<System::SharedPtr<LambdaExpressions>()>>([self]() -> System::SharedPtr<LambdaExpressions>
    {
        return self;
    })).template AddHeldVariable<System::Func<System::SharedPtr<LambdaExpressions>>>("self", self);
    return lambda;
}

void LambdaExpressions::LambdaCapturesClassField()
{
    auto self = System::SharedPtr<LambdaExpressions>(this);
    // TO DO: The lambda expression captures this by the strong reference, and is assigned to the class field or property
    m_lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([self]() -> int32_t
    {
        return self->mSomeDelta;
    })).template AddHeldVariable<System::Func<int32_t>>("self", self);
}

void LambdaExpressions::LambdaCapturesByte()
{
    uint8_t b = 10;
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&b]() -> int32_t
    {
        return b * 2;
    }));
}

int32_t LambdaExpressions::idx_get(int32_t index)
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&index]() -> int32_t
    {
        return 2 * index;
    }));
    return lambda();
}

void LambdaExpressions::idx_set(int32_t index, int32_t value)
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&index, &value]() -> int32_t
    {
        return index * value;
    }));
    this->mSomeDelta = lambda();
}

void LambdaExpressions::LambdaCapturesBoxedDateTime()
{
    System::SharedPtr<System::Object> value = System::ObjectExt::Box<System::DateTime>(System::DateTime(2020, 8, 31));
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return System::ObjectExt::GetHashCode(value);
    }));
}

void LambdaExpressions::LambdaCapturesBoxedString()
{
    System::SharedPtr<System::Object> value = System::ObjectExt::Box<System::String>(u"1e-02");
    System::Func<bool> lambda = System::Func<bool>(static_cast<std::function<bool()>>([&value]() -> bool
    {
        return System::ObjectExt::Equals(value, System::ObjectExt::Box<System::String>(u"1e-02"));
    }));
}

void LambdaExpressions::LambdaCapturesClassProperty()
{
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([this]() -> int32_t
    {
        return get_SomeDelta();
    }));
}

void LambdaExpressions::LambdaCapturesLocalVariables1()
{
    int32_t value = 30;
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    System::Func<int32_t> lambda2 = System::Func<int32_t>(lambda1);
    int32_t result = lambda2();
}

System::Func<int32_t> LambdaExpressions::LambdaCapturesLocalVariables2()
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = 30;
    int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    System::Func<int32_t> lambda2 = System::Func<int32_t>(lambda1);
    return lambda2;
}

int32_t LambdaExpressions::LambdaCapturesLocalVariables3()
{
    int32_t value = 30;
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    return lambda() + 3;
}

void LambdaExpressions::LambdaIsPassedAsLinqParam1()
{
    auto list = [&]{ int32_t init_0[] = {1, 2, 3, 4}; auto list_0 = System::MakeObject<System::Collections::Generic::List<int32_t>>(); list_0->AddInitializer(4, init_0); return list_0; }();
    int32_t multiplier = 10;
    System::Func<int32_t, int32_t> lambda = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t x)>>([&multiplier](int32_t x) -> int32_t
    {
        return x * multiplier;
    }));
    list = list->LINQ_Select(lambda)->LINQ_ToList();
}

void LambdaExpressions::LambdaIsPassedAsLinqParam2()
{
    auto list = [&]{ int32_t init_1[] = {1, 2, 3, 4}; auto list_1 = System::MakeObject<System::Collections::Generic::List<int32_t>>(); list_1->AddInitializer(4, init_1); return list_1; }();
    int32_t multiplier = 10;
    list = list->LINQ_Select(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t x)>>([&multiplier](int32_t x) -> int32_t
    {
        return x * multiplier;
    })))->LINQ_ToList();
}

void LambdaExpressions::LambdaIsAssignedToClassField1()
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = 30;
    int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    this->m_lambda = lambda;
}

void LambdaExpressions::LambdaIsAssignedToClassField2()
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = 30;
    int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    m_lambda = lambda;
}

void LambdaExpressions::LambdasCycle1()
{
    int32_t value = 10;
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    }));
    System::Func<int32_t> lambda2 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([]() -> int32_t
    {
        return 20;
    }));
    lambda2 = lambda1;
    lambda1 = lambda2;
}

void LambdaExpressions::LambdasCycle2()
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_value = 10;
    int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda1 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    System::Func<int32_t> lambda2 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([]() -> int32_t
    {
        return 20;
    }));
    lambda2 = lambda1;
    lambda1 = lambda2;
    m_lambda = lambda1;
}

void LambdaExpressions::LambdaAsParam(System::Func<int32_t, int32_t> lambdaParam)
{
    int32_t value = lambdaParam(30);
}

void LambdaExpressions::LambdaCapturesIntParam(int32_t param)
{
    LambdaAsParam(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t x)>>([&param](int32_t x) -> int32_t
    {
        return param * x;
    })));
}

void LambdaExpressions::CallLambdaCapturesIntParam()
{
    const int32_t value = 10;
    LambdaCapturesIntParam(value);
}

void LambdaExpressions::LambdaCapturesAnonymousFunction(System::Func<int32_t, int32_t> lambdaParam)
{
    LambdaAsParam(static_cast<System::Func<int32_t, int32_t>>(static_cast<std::function<int32_t(int32_t x)>>([&lambdaParam](int32_t x) -> int32_t
    {
        return x * lambdaParam(15);
    })));
}

void LambdaExpressions::CallLambdaCapturesAnonymousFunction()
{
    const int32_t value = 10;
    System::Func<int32_t, int32_t> lambda = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t x)>>([&value](int32_t x) -> int32_t
    {
        return x * value;
    }));
    LambdaCapturesAnonymousFunction(lambda);
}

void LambdaExpressions::LambdaParamChainToLocal()
{
    const int32_t value = 10;
    System::Func<int32_t, int32_t> lambda = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t x)>>([&value](int32_t x) -> int32_t
    {
        return x * value;
    }));
    LambdaParamChainToLocal1(lambda);
}

void LambdaExpressions::LambdaParamChainToLocal1(System::Func<int32_t, int32_t> lambdaParam)
{
    LambdaParamChainToLocal2(lambdaParam);
}

void LambdaExpressions::LambdaParamChainToLocal2(System::Func<int32_t, int32_t> lambdaParam)
{
    int32_t value = lambdaParam(30);
}

void LambdaExpressions::LambdaParamChainToField()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 10;
    const int32_t &value = _lch_value.GetCapture();
    System::Func<int32_t> lambda = System::Func<int32_t>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    })).template AddHeldVariable<System::Func<int32_t>>("value", value);
    LambdaParamChainToField1(lambda);
}

void LambdaExpressions::LambdaParamChainToField1(System::Func<int32_t> lambdaParam)
{
    LambdaParamChainToField2(lambdaParam);
}

void LambdaExpressions::LambdaParamChainToField2(System::Func<int32_t> lambdaParam)
{
    m_lambda = lambdaParam;
}

void LambdaExpressions::MustCaptureByReference(System::Func<int32_t> lambdaParam)
{
    m_lambda = lambdaParam;
}

void LambdaExpressions::MustUseHolders(System::Func<int32_t> lambdaParam)
{
    m_lambda = lambdaParam;
}

void LambdaExpressions::CheckCppLambdaMustCaptureByReferenceAttribute()
{
    const System::Details::LambdaCaptureHolder<int32_t> _lch_value = 20;
    const int32_t &value = _lch_value.GetCapture();
    MustCaptureByReference(static_cast<System::Func<int32_t>>(static_cast<std::function<int32_t()>>([&value]() -> int32_t
    {
        return value;
    })));
    MustUseHolders(System::Func<int32_t>(static_cast<System::Func<int32_t>>(static_cast<std::function<int32_t()>>([_lch_value, &value]() -> int32_t
    {
        return value;
    }))).template AddHeldVariable<System::Func<int32_t>>("value", value));
}

void LambdaExpressions::GenericClassTest()
{
    const int32_t multiplier = 20;
    System::Func<int32_t, int32_t> lambda = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([&multiplier](int32_t value) -> int32_t
    {
        return multiplier * value;
    }));
    
    auto instance = System::MakeObject<LambdaExpressions::GenericClass<int32_t>>(10);
    int32_t result = instance->Foo(lambda);
}

void LambdaExpressions::SomeCalculation(System::Func<int32_t, int32_t> selector)
{
}

int32_t LambdaExpressions::SomeSelector(int32_t value)
{
    return 777;
}

void LambdaExpressions::SomeProcessor(System::Action<int32_t> action)
{
}

void LambdaExpressions::SomeAction(int32_t value)
{
}

System::Func<int32_t, int32_t> LambdaExpressions::CreateFun(int32_t delta)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_delta = delta;
    int32_t& _delta = _lch_delta.GetCapture();
    return System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t value)>>([_lch_delta, &_delta](int32_t value) -> int32_t
    {
        return value + _delta;
    })).template AddHeldVariable<System::Func<int32_t, int32_t>>("_delta", _delta);
}

LambdaExpressions::VoidVoidDelegate LambdaExpressions::LambdaInsideLambda2(int32_t param)
{
    System::Details::LambdaCaptureHolder<int32_t> _lch_param = param;
    int32_t& _param = _lch_param.GetCapture();
    LambdaExpressions::VoidVoidDelegate lambda = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([_lch_param, &_param]() -> void
    {
        LambdaExpressions::VoidVoidDelegate lambda2 = LambdaExpressions::VoidVoidDelegate(static_cast<std::function<void()>>([&_param]() -> void
        {
            int32_t value = _param;
        }));
    })).template AddHeldVariable<LambdaExpressions::VoidVoidDelegate>("_param", _param);
    
    return lambda;
}

void LambdaExpressions::LambdaInsideLambda4()
{
    System::Func<int32_t, int32_t> lambda = System::Func<int32_t, int32_t>(static_cast<std::function<int32_t(int32_t x)>>([](int32_t x) -> int32_t
    {
        System::Func<int32_t> lambda2 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&x]() -> int32_t
        {
            System::Func<int32_t> lambda3 = System::Func<int32_t>(static_cast<std::function<int32_t()>>([&x]() -> int32_t
            {
                return x;
            }));
            
            return 0;
        }));
        
        return 0;
    }));
}

void LambdaExpressions::AddLine(System::SharedPtr<LambdaExpressions::PointF> left, System::SharedPtr<LambdaExpressions::PointF> right)
{
}

void LambdaExpressions::VariableNameIsSameAsLambdaParameter()
{
    System::SharedPtr<System::Collections::Generic::List<System::SharedPtr<LambdaExpressions::DataPointElement>>> linePoints = System::MakeObject<System::Collections::Generic::List<System::SharedPtr<LambdaExpressions::DataPointElement>>>();
    auto pointsTmp = linePoints->ConvertAll<System::SharedPtr<LambdaExpressions::PointF>>(static_cast<System::Converter<System::SharedPtr<LambdaExpressions::DataPointElement>, System::SharedPtr<LambdaExpressions::PointF>>>(static_cast<std::function<System::SharedPtr<LambdaExpressions::PointF>(System::SharedPtr<LambdaExpressions::DataPointElement> i)>>([](System::SharedPtr<LambdaExpressions::DataPointElement> i) -> System::SharedPtr<LambdaExpressions::PointF>
    {
        return i->get_Center();
    })))->ToArray();
    System::ArrayPtr<System::SharedPtr<LambdaExpressions::PointF>> points = System::MakeArray<System::SharedPtr<StatementsPorting::LambdaExpressions::PointF>>(pointsTmp->get_Length());
    pointsTmp->CopyTo(points, 0);
    
    for (int32_t i = 1; i < linePoints->get_Count(); i++)
    {
        AddLine(points[i - 1], points[i]);
    }
}

LambdaExpressions::LambdaExpressions() : mSomeDelta(3343)
{
}

LambdaExpressions::~LambdaExpressions()
{
}

#ifdef ASPOSE_GET_SHARED_MEMBERS
System::Object::shared_members_type LambdaExpressions::GetSharedMembers()
{
    auto result = System::Object::GetSharedMembers();
    
    result.Add("StatementsPorting::LambdaExpressions::m_lambda", this->m_lambda);
    
    return result;
}
#endif

} // namespace StatementsPorting

{{< /highlight >}}
