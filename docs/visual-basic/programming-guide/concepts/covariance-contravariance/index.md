---
title: 共変性と反変性
ms.date: 07/20/2015
ms.assetid: 59224c46-9931-466b-8c6e-3648c3e609c6
ms.openlocfilehash: a75970d98890cb1fb363d4672bd90d376bccf89c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352156"
---
# <a name="covariance-and-contravariance-visual-basic"></a>共変性と反変性 (Visual Basic)

Visual Basic では、共変性と反変性により、配列型、デリゲート型、およびジェネリック型引数の暗黙の参照変換が可能になります。 共変性は代入互換性を維持し、反変性はこれを反転させます。

次のコードでは、代入互換性、共変性、および反変性の違いについて説明します。

```vb
' Assignment compatibility.
Dim str As String = "test"
' An object of a more derived type is assigned to an object of a less derived type.
Dim obj As Object = str

' Covariance.
Dim strings As IEnumerable(Of String) = New List(Of String)()
' An object that is instantiated with a more derived type argument
' is assigned to an object instantiated with a less derived type argument.
' Assignment compatibility is preserved.
Dim objects As IEnumerable(Of Object) = strings

' Contravariance.
' Assume that there is the following method in the class:
' Shared Sub SetObject(ByVal o As Object)
' End Sub
Dim actObject As Action(Of Object) = AddressOf SetObject

' An object that is instantiated with a less derived type argument
' is assigned to an object instantiated with a more derived type argument.
' Assignment compatibility is reversed.
Dim actString As Action(Of String) = actObject
```

配列の共変性により、強い派生型の配列から弱い派生型の配列への暗黙の型変換が可能になります。 ただし、次のコード例に示すように、この操作はタイプ セーフではありません。

```vb
Dim array() As Object = New String(10) {}
' The following statement produces a run-time exception.
' array(0) = 10
```

メソッド グループの共変性と反変性のサポートにより、メソッド シグネチャをデリゲート型と一致させることができます。 これにより、一致するシグネチャを持つメソッドだけでなく、デリゲート型で指定された型よりも強い派生型 (共変性) を返すメソッドや、弱い派生型 (反変性) のパラメーターを受け取るメソッドを、デリゲートに割り当てることができます。 詳細については、「[Variance in Delegates (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」(デリゲートの分散 (Visual Basic)) および「[Using Variance in Delegates (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates.md)」(デリゲートの分散の使用 (Visual Basic)) を参照してください。

次のコード例は、メソッド グループでの共変性と反変性のサポートを示しています。

```vb
Shared Function GetObject() As Object
    Return Nothing
End Function

Shared Sub SetObject(ByVal obj As Object)
End Sub

Shared Function GetString() As String
    Return ""
End Function

Shared Sub SetString(ByVal str As String)

End Sub

Shared Sub Test()
    ' Covariance. A delegate specifies a return type as object,
    ' but you can assign a method that returns a string.
    Dim del As Func(Of Object) = AddressOf GetString

    ' Contravariance. A delegate specifies a parameter type as string,
    ' but you can assign a method that takes an object.
    Dim del2 As Action(Of String) = AddressOf SetObject
End Sub
```

.NET Framework 4 以降では、Visual Basic ジェネリックインターフェイスとデリゲートでの共変性と反変性がサポートされ、ジェネリック型パラメーターの暗黙の型変換が可能になります。 詳細については、「[Variance in Generic Interfaces (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)」(ジェネリック インターフェイスの分散 (Visual Basic)) および「[Variance in Delegates (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」(デリゲートの分散 (Visual Basic)) を参照してください。

次のコード例は、ジェネリック インターフェイスの暗黙の参照変換を示しています。

```vb
Dim strings As IEnumerable(Of String) = New List(Of String)
Dim objects As IEnumerable(Of Object) = strings
```

ジェネリック インターフェイスや汎用デリゲートは、そのジェネリック パラメーターが共変または反変として宣言されている場合、*バリアント*と呼ばれます。 Visual Basic では、独自のバリアント インターフェイスおよびデリゲートを作成できます。 詳細については、「[Creating Variant Generic Interfaces (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/creating-variant-generic-interfaces.md)」(バリアント ジェネリック インターフェイスの作成 (Visual Basic)) および「[Variance in Delegates (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)」(デリゲートの分散 (Visual Basic)) を参照してください。

## <a name="related-topics"></a>関連トピック

|タイトル|説明|
|-----------|-----------------|
|[ジェネリック インターフェイスの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-generic-interfaces.md)|ジェネリック インターフェイスでの共変性と反変性について説明し、.NET Framework でのバリアント ジェネリック インターフェイスの一覧を示します。|
|[バリアント ジェネリック インターフェイスの作成 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/creating-variant-generic-interfaces.md)|カスタムのバリアント インターフェイスを作成する方法を示します。|
|[ジェネリック コレクションに対するインターフェイスでの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-interfaces-for-generic-collections.md)|<xref:System.Collections.Generic.IEnumerable%601> および <xref:System.IComparable%601> インターフェイスでの共変性と反変性のサポートがコードの再利用にどのように役立つかを示します。|
|[デリゲートの分散 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/variance-in-delegates.md)|汎用および非汎用デリゲートでの共変性と反変性について説明し、.NET Framework でのバリアント汎用デリゲートの一覧を示します。|
|[デリゲートの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-in-delegates.md)|非汎用デリゲートでの共変性と反変性のサポートを使用して、メソッド シグネチャをデリゲート型に一致させる方法について説明します。|
|[Func および Action 汎用デリゲートでの分散の使用 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/covariance-contravariance/using-variance-for-func-and-action-generic-delegates.md)|`Func` および `Action` デリゲートでの共変性と反変性のサポートがコードの再利用にどのように役立つかを示します。|
