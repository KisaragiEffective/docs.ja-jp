---
title: デリゲートの分散
ms.date: 07/20/2015
ms.assetid: 38e9353f-74f8-4211-a8f0-7a495414df4a
ms.openlocfilehash: 11146bc4a60f55fc0373f0b5dfa5d44dcf748a5b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348998"
---
# <a name="variance-in-delegates-visual-basic"></a>デリゲートの変性 (Visual Basic)

.NET Framework 3.5 では、および Visual Basic のC#すべてのデリゲートのデリゲート型を使用して、メソッドシグネチャの照合がサポートされるようになりました。 つまり、シグネチャが一致するメソッドだけでなく、デリゲート型で指定された型よりも強い派生型を返す (共変性) メソッドや、弱い派生型のパラメーターを受け取る (反変性) メソッドを、デリゲートに割り当てることができます。 これには、汎用デリゲートと非汎用デリゲートの両方が含まれます。

たとえば、次のコードについて考えます。このコードには、2 つのクラスと、汎用と非汎用の 2 つのデリゲートが含まれています。

```vb
Public Class First
End Class

Public Class Second
    Inherits First
End Class

Public Delegate Function SampleDelegate(ByVal a As Second) As First
Public Delegate Function SampleGenericDelegate(Of A, R)(ByVal a As A) As R
```

`SampleDelegate` 型または `SampleDelegate(Of A, R)` 型のデリゲートを作成する場合、そのデリゲートには、次のいずれかのメソッドを割り当てることができます。

```vb
' Matching signature.
Public Shared Function ASecondRFirst(
    ByVal second As Second) As First
    Return New First()
End Function

' The return type is more derived.
Public Shared Function ASecondRSecond(
    ByVal second As Second) As Second
    Return New Second()
End Function

' The argument type is less derived.
Public Shared Function AFirstRFirst(
    ByVal first As First) As First
    Return New First()
End Function

' The return type is more derived
' and the argument type is less derived.
Public Shared Function AFirstRSecond(
    ByVal first As First) As Second
    Return New Second()
End Function
```

次のコード例は、メソッド シグネチャとデリゲート型の間の暗黙的な変換を示しています。

```vb
' Assigning a method with a matching signature
' to a non-generic delegate. No conversion is necessary.
Dim dNonGeneric As SampleDelegate = AddressOf ASecondRFirst
' Assigning a method with a more derived return type
' and less derived argument type to a non-generic delegate.
' The implicit conversion is used.
Dim dNonGenericConversion As SampleDelegate = AddressOf AFirstRSecond

' Assigning a method with a matching signature to a generic delegate.
' No conversion is necessary.
Dim dGeneric As SampleGenericDelegate(Of Second, First) = AddressOf ASecondRFirst
' Assigning a method with a more derived return type
' and less derived argument type to a generic delegate.
' The implicit conversion is used.
Dim dGenericConversion As SampleGenericDelegate(Of Second, First) = AddressOf AFirstRSecond
```

その他の例については、「[デリゲートの分散の使用」 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates.md)および「 [Func と Action 汎用デリゲートの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)」を参照してください。

## <a name="variance-in-generic-type-parameters"></a>ジェネリック型パラメーターの変性

.NET Framework 4 以降では、ジェネリック型パラメーターによって指定された型が異なる汎用デリゲートを相互に割り当てることができるように、デリゲート間で暗黙的な変換を有効にできます。sv.

暗黙的な変換を有効にするには、`in` キーワードまたは `out` キーワードを使用して、デリゲートでジェネリック パラメーターを共変または反変として明示的に宣言する必要があります。

次のコード例は、共変のジェネリック型パラメーターが指定されたデリゲートを作成する方法を示しています。

```vb
' Type T is declared covariant by using the out keyword.
Public Delegate Function SampleGenericDelegate(Of Out T)() As T
Sub Test()
    Dim dString As SampleGenericDelegate(Of String) = Function() " "
    ' You can assign delegates to each other,
    ' because the type T is declared covariant.
    Dim dObject As SampleGenericDelegate(Of Object) = dString
End Sub
```

変性サポートのみを使用してメソッド シグネチャをデリゲート型に一致させ、`in` キーワードと `out` キーワードを使用しない場合、同等のラムダ式かメソッドを使用すれば、デリゲートをインスタンス化できることがありますが、デリゲートを別のデリゲートに割り当てることはできません。

次のコード例では、`SampleGenericDelegate(Of String)` を `SampleGenericDelegate(Of Object)`に明示的に変換することはできませんが、`String` は `Object`を継承します。 この問題を修正するには、ジェネリック パラメーター `T` を `out` キーワードでマークします。

```vb
Public Delegate Function SampleGenericDelegate(Of T)() As T
Sub Test()
    Dim dString As SampleGenericDelegate(Of String) = Function() " "

    ' You can assign the dObject delegate
    ' to the same lambda expression as dString delegate
    ' because of the variance support for
    ' matching method signatures with delegate types.
    Dim dObject As SampleGenericDelegate(Of Object) = Function() " "

    ' The following statement generates a compiler error
    ' because the generic type T is not marked as covariant.
    ' Dim dObject As SampleGenericDelegate(Of Object) = dString

End Sub
```

### <a name="generic-delegates-that-have-variant-type-parameters-in-the-net-framework"></a>.NET Framework のバリアント型パラメーターが含まれる汎用デリゲート

.NET Framework 4 では、既存の複数の汎用デリゲートで、ジェネリック型パラメーターに対して分散サポートが導入されました。

- `Action` 名前空間の <xref:System> デリゲート。<xref:System.Action%601>、<xref:System.Action%602> など

- `Func` 名前空間の <xref:System> デリゲート。<xref:System.Func%601>、<xref:System.Func%602> など

- <xref:System.Predicate%601> デリゲート

- <xref:System.Comparison%601> デリゲート

- <xref:System.Converter%602> デリゲート

詳細と例については、「 [Func と Action 汎用デリゲートの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)」を参照してください。

### <a name="declaring-variant-type-parameters-in-generic-delegates"></a>汎用デリゲートのバリアント型パラメーターの宣言

汎用デリゲートに共変または反変のジェネリック型パラメーターがある場合、そのデリゲートは "*バリアント汎用デリゲート*" と呼ばれます。

汎用デリゲートのジェネリック型パラメーターを共変として宣言するには、`out` キーワードを使用します。 共変の型は、メソッドの戻り値の型としてのみ使用できます。メソッド引数の型として使用することはできません。 共変の汎用デリゲートを宣言する方法を次のコード例に示します。

```vb
Public Delegate Function DCovariant(Of Out R)() As R
```

汎用デリゲートのジェネリック型パラメーターを反変として宣言するには、`in` キーワードを使用します。 反変の型は、メソッド引数の型としてのみ使用できます。メソッドの戻り値の型として使用することはできません。 反変の汎用デリゲートを宣言する方法を次のコード例に示します。

```vb
Public Delegate Sub DContravariant(Of In A)(ByVal a As A)
```

> [!IMPORTANT]
> Visual Basic 内の `ByRef` パラメーターをバリアントとしてマークすることはできません。

同じデリゲートで、型パラメーターが異なる場合は、分散と共変性の両方をサポートすることもできます。 これを次の例に示します。

```vb
Public Delegate Function DVariant(Of In A, Out R)(ByVal a As A) As R
```

### <a name="instantiating-and-invoking-variant-generic-delegates"></a>バリアント汎用デリゲートのインスタンス化と呼び出し

バリアント デリゲートのインスタンス化および呼び出しは、インバリアント デリゲートのインスタンス化および呼び出しと同様に行うことができます。 次の例では、ラムダ式によってデリゲートをインスタンス化します。

```vb
Dim dvariant As DVariant(Of String, String) = Function(str) str + " "
dvariant("test")
```

### <a name="combining-variant-generic-delegates"></a>バリアント汎用デリゲートの結合

バリアント デリゲートの結合はお勧めしません。 <xref:System.Delegate.Combine%2A> メソッドはバリアント デリゲートの変換をサポートしていないため、デリゲートが厳密に同じ型である必要があります。 この場合、次のコード例に示すように、<xref:System.Delegate.Combine%2A> メソッド ( C#および Visual Basic) を使用するか、`+` 演算子 (のC#) を使用してデリゲートを結合すると、実行時例外が発生する可能性があります。

```vb
Dim actObj As Action(Of Object) = Sub(x) Console.WriteLine("object: {0}", x)
Dim actStr As Action(Of String) = Sub(x) Console.WriteLine("string: {0}", x)

' The following statement throws an exception at run time.
' Dim actCombine = [Delegate].Combine(actStr, actObj)
```

## <a name="variance-in-generic-type-parameters-for-value-and-reference-types"></a>値型と参照型でのジェネリック型パラメーターの分散

ジェネリック型パラメーターの変性がサポートされるのは参照型だけです。 たとえば、整数は値型であるため、`DVariant(Of Int)`を `DVariant(Of Object)` または `DVariant(Of Long)`に暗黙的に変換することはできません。

次の例は、値型ではジェネリック型パラメーターの分散がサポートされないことを示しています。

```vb
' The type T is covariant.
Public Delegate Function DVariant(Of Out T)() As T
' The type T is invariant.
Public Delegate Function DInvariant(Of T)() As T
Sub Test()
    Dim i As Integer = 0
    Dim dInt As DInvariant(Of Integer) = Function() i
    Dim dVariantInt As DVariant(Of Integer) = Function() i

    ' All of the following statements generate a compiler error
    ' because type variance in generic parameters is not supported
    ' for value types, even if generic type parameters are declared variant.
    ' Dim dObject As DInvariant(Of Object) = dInt
    ' Dim dLong As DInvariant(Of Long) = dInt
    ' Dim dVariantObject As DInvariant(Of Object) = dInt
    ' Dim dVariantLong As DInvariant(Of Long) = dInt
End Sub
```

## <a name="relaxed-delegate-conversion-in-visual-basic"></a>Visual Basic での緩やかになったデリゲート変換

厳密でないデリゲート変換を使用すると、メソッドシグネチャをデリゲート型と照合する際の柔軟性が向上します。 たとえば、メソッドをデリゲートに割り当てるときに、パラメーターの指定を省略したり、関数の戻り値を省略したりすることができます。 詳細については、「[緩やかなデリゲート変換](../../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)」を参照してください。

## <a name="see-also"></a>参照

- [ジェネリック](../../../../standard/generics/index.md)
- [Func および Action 汎用デリゲートでの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)
