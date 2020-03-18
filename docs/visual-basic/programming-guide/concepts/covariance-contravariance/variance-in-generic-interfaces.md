---
title: ジェネリック インターフェイスの分散
ms.date: 07/20/2015
ms.assetid: cf4096d0-4bb3-45a9-9a6b-f01e29a60333
ms.openlocfilehash: 1f6913b322e2d3d9ec2234e556e63d67324277e5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348989"
---
# <a name="variance-in-generic-interfaces-visual-basic"></a>ジェネリック インターフェイス (Visual Basic) の変性

.NET Framework 4 では、既存のいくつかのジェネリック インターフェイスに対して、変性のサポートが導入されています。 変性のサポートにより、これらのインターフェイスを実装するクラスの暗黙的な変換が可能になりました。 次のインターフェイスは、新たにバリアントになりました。

- <xref:System.Collections.Generic.IEnumerable%601> (T は共変です)

- <xref:System.Collections.Generic.IEnumerator%601> (T は共変です)

- <xref:System.Linq.IQueryable%601> (T は共変です)

- <xref:System.Linq.IGrouping%602> (`TKey` と `TElement` は共変です)

- <xref:System.Collections.Generic.IComparer%601> (T は反変です)

- <xref:System.Collections.Generic.IEqualityComparer%601> (T は反変です)

- <xref:System.IComparable%601> (T は反変です)

共変性により、メソッドの戻り値の型の派生を、インターフェイスのジェネリック型パラメーターで定義されている型よりも強くすることができます。 ここでは、共変性機能について説明するために、`IEnumerable(Of Object)` および `IEnumerable(Of String)` というジェネリック インターフェイスについて考えます。 `IEnumerable(Of String)` インターフェイスは、`IEnumerable(Of Object)` インターフェイスを継承しません。 ただし、`String` 型は `Object` 型を継承します。場合によっては、これらのインターフェイスのオブジェクトを、相互に割り当てる必要が生じることもあるでしょう。 これを次のコード例に示します。

```vb
Dim strings As IEnumerable(Of String) = New List(Of String)
Dim objects As IEnumerable(Of Object) = strings
```

以前のバージョンの .NET Framework では、このコードにより `Option Strict On`で Visual Basic にコンパイルエラーが発生します。 今後は、`strings` インターフェイスが共変になったので、上記の例のように、`objects` の代わりに <xref:System.Collections.Generic.IEnumerable%601> を使用できるようになりました。

反変性により、メソッドの引数の型の派生を、インターフェイスのジェネリック パラメーターで指定されている型よりも弱くすることができます。 ここでは、反変性について説明するために、`BaseComparer` クラスのインスタンスを比較するための `BaseClass` クラスを作成した場合について考えます。 `BaseComparer` クラスは、`IEqualityComparer(Of BaseClass)` インターフェイスを実装します。 <xref:System.Collections.Generic.IEqualityComparer%601> インターフェイスが反変になったので、`BaseComparer` を使用して、`BaseClass` クラスを継承するクラスのインスタンスを比較することができます。 これを次のコード例に示します。

```vb
' Simple hierarchy of classes.
Class BaseClass
End Class

Class DerivedClass
    Inherits BaseClass
End Class

' Comparer class.
Class BaseComparer
    Implements IEqualityComparer(Of BaseClass)

    Public Function Equals1(ByVal x As BaseClass,
                            ByVal y As BaseClass) As Boolean _
                            Implements IEqualityComparer(Of BaseClass).Equals
        Return (x.Equals(y))
    End Function

    Public Function GetHashCode1(ByVal obj As BaseClass) As Integer _
        Implements IEqualityComparer(Of BaseClass).GetHashCode
        Return obj.GetHashCode
    End Function
End Class
Sub Test()
    Dim baseComparer As IEqualityComparer(Of BaseClass) = New BaseComparer
    ' Implicit conversion of IEqualityComparer(Of BaseClass) to
    ' IEqualityComparer(Of DerivedClass).
    Dim childComparer As IEqualityComparer(Of DerivedClass) = baseComparer
End Sub
```

その他の例については、「[ジェネリックコレクションのインターフェイスでの分散の使用」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-interfaces-for-generic-collections.md)を参照してください。

ジェネリック インターフェイスでの変性がサポートされるのは参照型だけです。 値型は変性をサポートしていません。 たとえば、整数は値型によって表されるため、`IEnumerable(Of Integer)` を暗黙的に `IEnumerable(Of Object)` に変換することはできません。

```vb
Dim integers As IEnumerable(Of Integer) = New List(Of Integer)
' The following statement generates a compiler error
' with Option Strict On, because Integer is a value type.
' Dim objects As IEnumerable(Of Object) = integers
```

また、バリアント インターフェイスを実装するクラスは、現在でもインバリアントであることを忘れないようにしてください。 たとえば、<xref:System.Collections.Generic.List%601> が共変インターフェイス <xref:System.Collections.Generic.IEnumerable%601> を実装しても、`List(Of Object)` を `List(Of String)` に暗黙的に変換することはできません。 これを次のコード例に図示します。

```vb
' The following statement generates a compiler error
' because classes are invariant.
' Dim list As List(Of Object) = New List(Of String)

' You can use the interface object instead.
Dim listObjects As IEnumerable(Of Object) = New List(Of String)
```

## <a name="see-also"></a>参照

- [ジェネリック コレクションに対するインターフェイスでの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-interfaces-for-generic-collections.md)
- [バリアント ジェネリック インターフェイスの作成 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/creating-variant-generic-interfaces.md)
- [ジェネリック インターフェイス](../../../../standard/generics/interfaces.md)
- [デリゲートの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)
