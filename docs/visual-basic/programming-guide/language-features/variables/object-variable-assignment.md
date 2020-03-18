---
title: オブジェクト変数の代入
ms.date: 07/20/2015
helpviewer_keywords:
- Nothing keyword [Visual Basic], object variable assignment
- object variables [Visual Basic], initializing
- variables [Visual Basic], initializing
- objects [Visual Basic], current instance
- object variables [Visual Basic], assigning
- variables [Visual Basic], object variables
- current instance [Visual Basic], defined
- variables [Visual Basic], assigning
- assignment statements [Visual Basic], object variable assignment
- Me keyword [Visual Basic], as object variable
ms.assetid: 3706811d-fd40-44fe-8727-d692e8e55d6d
ms.openlocfilehash: 93de17490935d6d5cad01000e9ee3e2fe55bd16c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351830"
---
# <a name="object-variable-assignment-visual-basic"></a>オブジェクト変数への代入 (Visual Basic)

オブジェクトをオブジェクト変数に割り当てるには、通常の代入ステートメントを使用します。 次の例に示すように、オブジェクト式または[Nothing](../../../../visual-basic/language-reference/nothing.md)キーワードを割り当てることができます。

```vb
Dim thisObject As Object
' The following statement assigns an object reference.
thisObject = Form1
' The following statement discontinues association with any object.
thisObject = Nothing
```

`Nothing` は、変数に現在割り当てられているオブジェクトがないことを意味します。

## <a name="initialization"></a>初期化

コードの実行が開始されると、オブジェクト変数が `Nothing`に初期化されます。 宣言を含む宣言がある場合は、宣言ステートメントの実行時に指定した値に再初期化されます。

[新しい](../../../../visual-basic/language-reference/operators/new-operator.md)キーワードを使用して、宣言に初期化を含めることができます。 次の宣言ステートメントでは、オブジェクト変数を宣言し、`ver` して、特定のオブジェクトを割り当て `testUri` します。 各は、適切なクラスのオーバーロードされたコンストラクターの1つを使用してオブジェクトを初期化します。

```vb
Dim testUri As New System.Uri("https://www.microsoft.com")
Dim ver As New System.Version(6, 1, 0)
```

## <a name="disassociation"></a>関連付け

オブジェクト変数を `Nothing` に設定すると、変数と特定のオブジェクトとの関連付けが中断されます。 これにより、変数を変更しても誤ってオブジェクトが変更されることがなくなります。 また、次の例に示すように、オブジェクト変数が有効なオブジェクトを指しているかどうかをテストすることもできます。

```vb
If otherObject IsNot Nothing Then
    ' otherObject refers to a valid object, so your code can use it.
End If
```

変数が参照しているオブジェクトが別のアプリケーション内にある場合、このテストでは、そのアプリケーションが終了したか、またはオブジェクトを無効にしただけであるかどうかを判断できません。

`Nothing` の値を持つオブジェクト変数は、 *null 参照*とも呼ばれます。

## <a name="current-instance"></a>現在のインスタンス

オブジェクトの*現在のインスタンス*は、コードが現在実行されているものです。 すべてのコードはプロシージャ内で実行されるため、現在のインスタンスはプロシージャが呼び出されたものです。

`Me` キーワードは、現在のインスタンスを参照するオブジェクト変数として機能します。 プロシージャが[共有](../../../../visual-basic/language-reference/modifiers/shared.md)されていない場合は、`Me` キーワードを使用して、現在のインスタンスへのポインターを取得できます。 共有プロシージャをクラスの特定のインスタンスに関連付けることはできません。

`Me` の使用は、現在のインスタンスを別のモジュール内のプロシージャに渡す場合に特に便利です。 たとえば、いくつかの XML ドキュメントがあり、それらすべてに標準テキストを追加したいとします。 次の例では、これを実行するプロシージャを定義します。

```vb
Sub addStandardText(XmlDoc As System.Xml.XmlDocument)
    XmlDoc.CreateTextNode("This text goes into every XML document.")
End Sub
```

その後、すべての XML ドキュメントオブジェクトがプロシージャを呼び出し、その現在のインスタンスを引数として渡すことができます。 次に例を示します。

```vb
addStandardText(Me)
```

## <a name="see-also"></a>参照

- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の宣言](../../../../visual-basic/programming-guide/language-features/variables/object-variable-declaration.md)
- [オブジェクト変数の値](../../../../visual-basic/programming-guide/language-features/variables/object-variable-values.md)
- [方法: オブジェクト変数を宣言し、その変数にオブジェクトを割り当てる Visual Basic](../../../../visual-basic/programming-guide/language-features/variables/how-to-declare-an-object-variable-and-assign-an-object-to-it.md)
- [方法: オブジェクト変数がインスタンスを参照しないようにする](../../../../visual-basic/programming-guide/language-features/variables/how-to-make-an-object-variable-not-refer-to-any-instance.md)
- [Me、My、MyBase、および MyClass](../../../../visual-basic/programming-guide/program-structure/me-my-mybase-and-myclass.md)
