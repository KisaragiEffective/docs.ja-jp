---
title: バリアント ジェネリック インターフェイスの作成 (C#)
ms.date: 07/20/2015
ms.assetid: 30330ec4-9df2-4838-a535-6c406d0ed4df
ms.openlocfilehash: 4ba72f28cd2ddd800f169387cc2c742159d4cb1b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "69595315"
---
# <a name="creating-variant-generic-interfaces-c"></a>バリアント ジェネリック インターフェイスの作成 (C#)

インターフェイスのジェネリック型パラメーターは、共変または反変として宣言できます。 "*共変性*" により、インターフェイス メソッドの戻り値の型の派生を、ジェネリック型パラメーターで定義されている型よりも強くすることができます。 "*反変性*" により、インターフェイス メソッドの引数の型の派生を、ジェネリック パラメーターで指定されている型よりも弱くすることができます。 共変または反変のジェネリック型パラメーターを持つジェネリック インターフェイスは、"*バリアント*" と呼ばれます。

> [!NOTE]
> .NET Framework 4 では、既存のいくつかのジェネリック インターフェイスに対して、変性のサポートが導入されています。 .NET Framework のバリアント インターフェイスの一覧については、「[ジェネリック インターフェイスの変性 (C#)](./variance-in-generic-interfaces.md)」を参照してください。

## <a name="declaring-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの宣言

バリアント ジェネリック インターフェイスは、ジェネリック型パラメーターの `in` キーワードと `out` キーワードを使用して宣言できます。

> [!IMPORTANT]
> C# の `ref`、`in`、`out` パラメーターは、バリアントにすることはできません。 また、値の型も変性をサポートしていません。

ジェネリック型パラメーターを共変として宣言するには、`out` キーワードを使用します。 共変の型は、次の条件を満たす必要があります。

- 型がインターフェイス メソッドの戻り値の型としてのみ使用され、メソッド引数の型として使用されない。 この例を以下に示します。ここでは、型 `R` が共変として宣言されています。

    ```csharp
    interface ICovariant<out R>
    {
        R GetSomething();
        // The following statement generates a compiler error.
        // void SetSomething(R sampleArg);

    }
    ```

    この規則には例外が 1 つあります。 反変の汎用デリゲートをメソッド パラメーターとして使用する場合は、型をデリゲートのジェネリック型パラメーターとして使用できます。 次の例では、型 `R` によって示します。 詳細については、次を参照してください。[デリゲート (Visual Basic) の変性](./variance-in-delegates.md)と[Func および Action 汎用デリゲート (Visual Basic) を使用して変性](./using-variance-for-func-and-action-generic-delegates.md)します。

    ```csharp
    interface ICovariant<out R>
    {
        void DoSomething(Action<R> callback);
    }
    ```

- 型がインターフェイス メソッドのジェネリック制約として使用されない。 これを次のコードに示します。

    ```csharp
    interface ICovariant<out R>
    {
        // The following statement generates a compiler error
        // because you can use only contravariant or invariant types
        // in generic constraints.
        // void DoSomething<T>() where T : R;
    }
    ```

ジェネリック型パラメーターを反変として宣言するには、`in` キーワードを使用します。 反変の型は、メソッド引数の型としてのみ使用でき、インターフェイス メソッドの戻り値の型として使用できません。 また、反変の型はジェネリック制約にも使用できます。 次のコードでは、反変のインターフェイスを宣言し、そのメソッドの 1 つにジェネリック制約を使用する方法を示します。

```csharp
interface IContravariant<in A>
{
    void SetSomething(A sampleArg);
    void DoSomething<T>() where T : A;
    // The following statement generates a compiler error.
    // A GetSomething();
}
```

次のコード例に示すように、同一インターフェイス内の異なる型パラメーターで、共変性と反変性の両方をサポートすることもできます。

```csharp
interface IVariant<out R, in A>
{
    R GetSomething();
    void SetSomething(A sampleArg);
    R GetSetSomethings(A sampleArg);
}
```

## <a name="implementing-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの実装

バリアント ジェネリック インターフェイスをクラスに実装する場合は、インバリアント インターフェイスに使用するのと同じ構文を使用します。 次のコード例では、共変のインターフェイスをジェネリック クラスに実装する方法を示します。

```csharp
interface ICovariant<out R>
{
    R GetSomething();
}
class SampleImplementation<R> : ICovariant<R>
{
    public R GetSomething()
    {
        // Some code.
        return default(R);
    }
}
```

バリアント インターフェイスを実装するクラスは不変です。 次に例を示します。

```csharp
// The interface is covariant.
ICovariant<Button> ibutton = new SampleImplementation<Button>();
ICovariant<Object> iobj = ibutton;

// The class is invariant.
SampleImplementation<Button> button = new SampleImplementation<Button>();
// The following statement generates a compiler error
// because classes are invariant.
// SampleImplementation<Object> obj = button;
```

## <a name="extending-variant-generic-interfaces"></a>バリアント ジェネリック インターフェイスの拡張

バリアント ジェネリック インターフェイスを拡張するときは、`in` キーワードと `out` キーワードを使用して、派生インターフェイスで変性をサポートするかどうかを明示的に指定する必要があります。 コンパイラでは、拡張されているインターフェイスからの変性の推論は行われません。 たとえば、次のようなインターフェイスがあるとします。

```csharp
interface ICovariant<out T> { }
interface IInvariant<T> : ICovariant<T> { }
interface IExtCovariant<out T> : ICovariant<T> { }
```

`IInvariant<T>` インターフェイスのジェネリック型パラメーター `T` は不変であるのに対して、`IExtCovariant<out T>` の型パラメーターは共変ですが、どちらのインターフェイスでも同じインターフェイスを拡張します。 これと同じ規則が、反変のジェネリック型パラメーターにも当てはまります。

拡張インターフェイスのジェネリック型パラメーター `T` が不変であれば、ジェネリック型パラメーター `T` が共変のインターフェイスと反変のインターフェイスの両方を拡張する 1 つのインターフェイスを作成できます。 これを次のコード例に示します。

```csharp
interface ICovariant<out T> { }
interface IContravariant<in T> { }
interface IInvariant<T> : ICovariant<T>, IContravariant<T> { }
```

ただし、一方のインターフェイスでジェネリック型パラメーター `T` が共変として宣言されている場合は、それを拡張インターフェイスで反変として宣言することはできません。その逆についても同様です。 これを次のコード例に示します。

```csharp
interface ICovariant<out T> { }
// The following statement generates a compiler error.
// interface ICoContraVariant<in T> : ICovariant<T> { }
```

### <a name="avoiding-ambiguity"></a>あいまいさの回避

バリアント ジェネリック インターフェイスの実装時には、変性によってあいまいさが発生することがあります。 このような状況は回避する必要があります。

たとえば、同一のバリアント ジェネリック インターフェイスを、異なるジェネリック型パラメーターを使用して 1 つのクラスに明示的に実装すると、あいまいさが発生する可能性があります。 この場合、コンパイラでエラーは生成されませんが、実行時にどのインターフェイスの実装が選択されるかは明確ではありません。 これにより、コードで特定しにくいバグが発生する可能性があります。 次のコード例について考えます。

```csharp
// Simple class hierarchy.
class Animal { }
class Cat : Animal { }
class Dog : Animal { }

// This class introduces ambiguity
// because IEnumerable<out T> is covariant.
class Pets : IEnumerable<Cat>, IEnumerable<Dog>
{
    IEnumerator<Cat> IEnumerable<Cat>.GetEnumerator()
    {
        Console.WriteLine("Cat");
        // Some code.
        return null;
    }

    IEnumerator IEnumerable.GetEnumerator()
    {
        // Some code.
        return null;
    }

    IEnumerator<Dog> IEnumerable<Dog>.GetEnumerator()
    {
        Console.WriteLine("Dog");
        // Some code.
        return null;
    }
}
class Program
{
    public static void Test()
    {
        IEnumerable<Animal> pets = new Pets();
        pets.GetEnumerator();
    }
}
```

この例では、`pets.GetEnumerator` メソッドで `Cat` と `Dog` がどのように選択されるかが明らかではありません。 そのため、コードで問題が発生する可能性があります。

## <a name="see-also"></a>参照

- [ジェネリック インターフェイスの変性 (C#)](./variance-in-generic-interfaces.md)
- [Func および Action 汎用デリゲートでの分散の使用 (C#)](./using-variance-for-func-and-action-generic-delegates.md)
