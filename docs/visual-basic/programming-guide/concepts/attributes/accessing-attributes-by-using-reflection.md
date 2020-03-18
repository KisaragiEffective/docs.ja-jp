---
title: リフレクションを使用した属性へのアクセス
ms.date: 07/20/2015
ms.assetid: c56e41da-5433-464f-a7bf-2a722e78bc9f
ms.openlocfilehash: 94352f07cf1f7e4a35f023503f138596ae5ac227
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353556"
---
# <a name="accessing-attributes-by-using-reflection-visual-basic"></a>リフレクションを使用した属性へのアクセス (Visual Basic)

カスタム属性を定義し、それらをソース コード内に配置することができても、その情報を取得して操作する手段がなければ、ほとんど価値はありません。 リフレクションを使用すれば、カスタム属性を使用して定義された情報を取得することができます。 鍵となるメソッドは `GetCustomAttributes` です。このメソッドは、ソース コード属性の実行時の等価オブジェクトを配列で返します。 このメソッドには、いくつかのオーバー ロード バージョンがあります。 詳細については、「 <xref:System.Attribute>」を参照してください。

次のような属性指定は、

```vb
<Author("P. Ackerman", Version:=1.1)>
Class SampleClass
    ' P. Ackerman's code goes here...
End Class
```

 概念的には次の記述と同じです。

```vb
Dim anonymousAuthorObject As Author = New Author("P. Ackerman")
anonymousAuthorObject.version = 1.1
```

ただし、属性について `SampleClass` が照会されるまで、コードは実行されません。 `GetCustomAttributes` について `SampleClass` を呼び出すと、`Author` オブジェクトが作成され、上記のように初期化されます。 クラスに他の属性がある場合は、他の属性オブジェクトも同様に作成されます。 `GetCustomAttributes` はその後、`Author` オブジェクトと配列内の他の属性オブジェクトを返します。 その後、この配列を反復処理し、各配列要素の型に基づいてどの属性が適用されたかを確認して、属性オブジェクトから情報を抽出することができます。

## <a name="example"></a>例

完全な例を次に示します。 カスタム属性が定義され、複数のエンティティに適用された後、リフレクションを使用して取得されています。

```vb
' Multiuse attribute
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct,
                       AllowMultiple:=True)>
Public Class Author
    Inherits System.Attribute
    Private name As String
    Public version As Double
    Sub New(ByVal authorName As String)
        name = authorName

        ' Default value
        version = 1.0
    End Sub

    Function GetName() As String
        Return name
    End Function
End Class

' Class with the Author attribute
<Author("P. Ackerman")>
Public Class FirstClass
End Class

' Class without the Author attribute
Public Class SecondClass
End Class

' Class with multiple Author attributes.
<Author("P. Ackerman"), Author("R. Koch", Version:=2.0)>
Public Class ThirdClass
End Class

Class TestAuthorAttribute
    Sub Main()
        PrintAuthorInfo(GetType(FirstClass))
        PrintAuthorInfo(GetType(SecondClass))
        PrintAuthorInfo(GetType(ThirdClass))
    End Sub

    Private Shared Sub PrintAuthorInfo(ByVal t As System.Type)
        System.Console.WriteLine("Author information for {0}", t)

        ' Using reflection
        Dim attrs() As System.Attribute = System.Attribute.GetCustomAttributes(t)

        ' Displaying output
        For Each attr In attrs
            Dim a As Author = CType(attr, Author)
            System.Console.WriteLine("   {0}, version {1:f}", a.GetName(), a.version)
        Next
    End Sub

    ' Output:
    '   Author information for FirstClass
    '     P. Ackerman, version 1.00
    ' Author information for SecondClass
    ' Author information for ThirdClass
    '  R. Koch, version 2.00
    '  P. Ackerman, version 1.00

End Class
```

## <a name="see-also"></a>参照

- <xref:System.Reflection>
- <xref:System.Attribute>
- [Visual Basic のプログラミング ガイド](../../../../visual-basic/programming-guide/index.md)
- [属性に格納されている情報の取得](../../../../standard/attributes/retrieving-information-stored-in-attributes.md)
- [リフレクション (Visual Basic)](../../../../visual-basic/programming-guide/concepts/reflection.md)
- [属性 (Visual Basic)](../../../../visual-basic/language-reference/attributes.md)
- [カスタム属性の作成 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/attributes/creating-custom-attributes.md)
