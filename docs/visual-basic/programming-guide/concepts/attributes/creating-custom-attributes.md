---
title: カスタム属性の作成
ms.date: 07/20/2015
ms.assetid: 5c9ef584-6c7c-496b-92a9-6e42f8d9ca28
ms.openlocfilehash: 773a3e8e974f37a1554892dd3441c115681c5bae
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74350145"
---
# <a name="creating-custom-attributes-visual-basic"></a>カスタム属性の作成 (Visual Basic)

属性クラスを定義することで、独自のカスタム属性を作成できます。属性クラスは、<xref:System.Attribute> の直接的または間接的な派生クラスです。これにより、メタデータの中で属性の定義をすばやく簡単に特定できます。 型にそれを記述したプログラマーの名前でタグを付けるとします。 `Author` というカスタム属性クラスを定義します。

```vb
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct)>
Public Class Author
    Inherits System.Attribute
    Private name As String
    Public version As Double
    Sub New(ByVal authorName As String)
        name = authorName
        version = 1.0
    End Sub
End Class
```

クラス名は属性の名前の `Author` です。 このクラスは `System.Attribute` から派生しているので、カスタム属性クラスです。 コンストラクターのパラメーターはカスタム属性の位置指定パラメーターです。 この例では、`name` が位置指定パラメーターになります。 パブリックな読み取り/書き込みフィールドまたはプロパティは名前付きパラメーターです。 この場合は、`version` が唯一の名前付きパラメーターです。 `AttributeUsage` 属性を使用して、クラスと `Author` の宣言に対してのみ `Structure` 属性を有効にしていることに注意してください。

この新しい属性の使用方法は次のとおりです。

```vb
<Author("P. Ackerman", Version:=1.1)>
Class SampleClass
    ' P. Ackerman's code goes here...
End Class
```

`AttributeUsage` には名前付きパラメーター `AllowMultiple` があります。これを使用すると、カスタム属性が 1 回しか指定できない属性か、または複数回指定できる属性かを設定できます。 次のコード例では、複数回指定できる属性が作成されます。

```vb
' multiuse attribute
<System.AttributeUsage(System.AttributeTargets.Class Or
                       System.AttributeTargets.Struct,
                       AllowMultiple:=True)>
Public Class Author
    Inherits System.Attribute
```

次のコード例では、同じ型の複数の属性が 1 つのクラスに適用されます。

```vb
<Author("P. Ackerman", Version:=1.1),
Author("R. Koch", Version:=1.2)>
Class SampleClass
    ' P. Ackerman's code goes here...
    ' R. Koch's code goes here...
End Class
```

> [!NOTE]
> 属性クラスにプロパティが含まれている場合、そのプロパティは読み取り/書き込み可能である必要があります。

## <a name="see-also"></a>参照

- <xref:System.Reflection>
- [Visual Basic のプログラミング ガイド](../../../../visual-basic/programming-guide/index.md)
- [カスタム属性の記述](../../../../standard/attributes/writing-custom-attributes.md)
- [リフレクション (Visual Basic)](../../../../visual-basic/programming-guide/concepts/reflection.md)
- [属性 (Visual Basic)](../../../../visual-basic/language-reference/attributes.md)
- [リフレクションを使用した属性へのアクセス (Visual Basic)](../../../../visual-basic/programming-guide/concepts/attributes/accessing-attributes-by-using-reflection.md)
- [AttributeUsage (Visual Basic)](../../../../visual-basic/programming-guide/concepts/attributes/attributeusage.md)
