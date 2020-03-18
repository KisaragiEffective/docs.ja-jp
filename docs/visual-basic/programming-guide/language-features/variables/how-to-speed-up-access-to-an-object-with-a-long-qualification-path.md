---
title: '方法 : 長い修飾パスをもつオブジェクトへのアクセス時間を短縮する'
ms.date: 07/20/2015
helpviewer_keywords:
- variables [Visual Basic], accessing
- variables [Visual Basic], object
- With statement [Visual Basic]
- With block
- object variables [Visual Basic], accessing
ms.assetid: 3eb7657f-c9fe-4e05-8bc3-4bb14d5ae585
ms.openlocfilehash: 83670ae6af0904156b08398024658cf504b7663f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346814"
---
# <a name="how-to-speed-up-access-to-an-object-with-a-long-qualification-path-visual-basic"></a>方法: 長い修飾パスを持つオブジェクトに対するアクセス時間を短縮する (Visual Basic)

複数のメソッドおよびプロパティの修飾パスを必要とするオブジェクトに頻繁にアクセスする場合は、修飾パスを繰り返さないことでコードを高速化できます。

修飾パスの繰り返しを回避するには、2つの方法があります。 変数にオブジェクトを割り当てることも、`With`...`End With` ブロックで使用することもできます。

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-assigning-it-to-a-variable"></a>変数に割り当てて、高い修飾オブジェクトへのアクセスを高速化するには

1. 頻繁にアクセスするオブジェクトの型の変数を宣言します。 宣言の初期化部分に修飾パスを指定します。

    ```vb
    Dim ctrlActv As Control = someForm.ActiveForm.ActiveControl
    ```

2. オブジェクトのメンバーにアクセスするには、変数を使用します。

    ```vb
    ctrlActv.Text = "Test"
    ctrlActv.Location = New Point(100, 100)
    ctrlActv.Show()
    ```

### <a name="to-speed-up-access-to-a-heavily-qualified-object-by-using-a-withend-with-block"></a>With... を使用して、高い修飾オブジェクトへのアクセスを高速化するには末尾をブロックにする

1. `With` ステートメントに修飾パスを配置します。

    ```vb
    With someForm.ActiveForm.ActiveControl
    ```

2. `End With` ステートメントの前に、`With` ブロック内のオブジェクトのメンバーにアクセスします。

    ```vb
        .Text = "Test"
        .Location = New Point(100, 100)
        .Show()
    End With
    ```

## <a name="see-also"></a>参照

- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [With...End With ステートメント](../../../../visual-basic/language-reference/statements/with-end-with-statement.md)
