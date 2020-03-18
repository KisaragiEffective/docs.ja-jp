---
title: ジェネリック インターフェイスの変性 (C#)
ms.date: 06/06/2019
ms.assetid: 4828a8f9-48c0-4128-9749-7fcd6bf19a06
ms.openlocfilehash: 2020ea54734724de775192a1a438413a73003d17
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79169663"
---
# <a name="variance-in-generic-interfaces-c"></a>ジェネリック インターフェイスの変性 (C#)

.NET Framework 4 では、既存のいくつかのジェネリック インターフェイスに対して、変性のサポートが導入されています。 変性のサポートにより、これらのインターフェイスを実装するクラスの暗黙的な変換が可能になりました。

.NET Framework 4 以降では、次のインターフェイスがバリアントです。

- <xref:System.Collections.Generic.IEnumerable%601> (T は共変です)

- <xref:System.Collections.Generic.IEnumerator%601> (T は共変です)

- <xref:System.Linq.IQueryable%601> (T は共変です)

- <xref:System.Linq.IGrouping%602> (`TKey` と `TElement` は共変です)

- <xref:System.Collections.Generic.IComparer%601> (T は反変です)

- <xref:System.Collections.Generic.IEqualityComparer%601> (T は反変です)

- <xref:System.IComparable%601> (T は反変です)

.NET Framework 4.5 以降では、次のインターフェイスがバリアントです。

- <xref:System.Collections.Generic.IReadOnlyList%601> (T は共変です)

- <xref:System.Collections.Generic.IReadOnlyCollection%601> (T は共変です)

共変性により、メソッドの戻り値の型の派生を、インターフェイスのジェネリック型パラメーターで定義されている型よりも強くすることができます。 ここでは、共変性機能について説明するために、`IEnumerable<Object>` および `IEnumerable<String>` というジェネリック インターフェイスについて考えます。 `IEnumerable<String>` インターフェイスは、`IEnumerable<Object>` インターフェイスを継承しません。 ただし、`String` 型は `Object` 型を継承します。場合によっては、これらのインターフェイスのオブジェクトを、相互に割り当てる必要が生じることもあるでしょう。 このコードの例を次に示します。

```csharp
IEnumerable<String> strings = new List<String>();
IEnumerable<Object> objects = strings;
```

.NET Framework の以前のバージョンでは、C# ではこのコードを実行するとコンパイル エラーが発生し、Visual Basic では `Option Strict` がオンの場合にエラーが発生します。 今後は、`strings` インターフェイスが共変になったので、上記の例のように、`objects` の代わりに <xref:System.Collections.Generic.IEnumerable%601> を使用できるようになりました。

反変性により、メソッドの引数の型の派生を、インターフェイスのジェネリック パラメーターで指定されている型よりも弱くすることができます。 ここでは、反変性について説明するために、`BaseComparer` クラスのインスタンスを比較するための `BaseClass` クラスを作成した場合について考えます。 `BaseComparer` クラスは、`IEqualityComparer<BaseClass>` インターフェイスを実装します。 <xref:System.Collections.Generic.IEqualityComparer%601> インターフェイスが反変になったので、`BaseComparer` を使用して、`BaseClass` クラスを継承するクラスのインスタンスを比較することができます。 このコードの例を次に示します。

```csharp
// Simple hierarchy of classes.
class BaseClass { }
class DerivedClass : BaseClass { }

// Comparer class.
class BaseComparer : IEqualityComparer<BaseClass>
{
    public int GetHashCode(BaseClass baseInstance)
    {
        return baseInstance.GetHashCode();
    }
    public bool Equals(BaseClass x, BaseClass y)
    {
        return x == y;
    }
}
class Program
{
    static void Test()
    {
        IEqualityComparer<BaseClass> baseComparer = new BaseComparer();

        // Implicit conversion of IEqualityComparer<BaseClass> to
        // IEqualityComparer<DerivedClass>.
        IEqualityComparer<DerivedClass> childComparer = baseComparer;
    }
}
```

詳しくは、「[ジェネリック コレクションに対するインターフェイスでの変性の使用 (C#)](./using-variance-in-interfaces-for-generic-collections.md)」をご覧ください。

ジェネリック インターフェイスでの変性がサポートされるのは参照型だけです。 値型は変性をサポートしていません。 たとえば、整数は値型によって表されるため、`IEnumerable<int>` を暗黙的に `IEnumerable<object>` に変換することはできません。

```csharp
IEnumerable<int> integers = new List<int>();
// The following statement generates a compiler error,
// because int is a value type.
// IEnumerable<Object> objects = integers;
```

また、バリアント インターフェイスを実装するクラスは、現在でもインバリアントであることを忘れないようにしてください。 たとえば、<xref:System.Collections.Generic.List%601> が共変インターフェイス <xref:System.Collections.Generic.IEnumerable%601> を実装しても、`List<String>` を `List<Object>` に暗黙的に変換することはできません。 これを次のコード例に示します。

```csharp
// The following line generates a compiler error
// because classes are invariant.
// List<Object> list = new List<String>();

// You can use the interface object instead.
IEnumerable<Object> listObjects = new List<String>();
```

## <a name="see-also"></a>参照

- [ジェネリック コレクションに対するインターフェイスでの変性の使用 (C#)](./using-variance-in-interfaces-for-generic-collections.md)
- [バリアント ジェネリック インターフェイスの作成 (C#)](./creating-variant-generic-interfaces.md)
- [ジェネリック インターフェイス](../../../../standard/generics/interfaces.md)
- [デリゲートの変性 (C#)](./variance-in-delegates.md)
