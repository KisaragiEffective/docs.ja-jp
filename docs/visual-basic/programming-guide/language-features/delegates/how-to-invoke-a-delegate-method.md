---
title: '方法: デリゲート メソッドを呼び出す'
ms.date: 07/20/2015
ms.assetid: b56866ae-abf9-4a5a-a855-486359455e9c
ms.openlocfilehash: 520bacfbe6103490e0459cd5af149c1d55a8fce4
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345266"
---
# <a name="how-to-invoke-a-delegate-method-visual-basic"></a>方法: デリゲート メソッドを呼び出す (Visual Basic)

この例では、メソッドをデリゲートに関連付けて、デリゲートを使用してそのメソッドを呼び出す方法を示します。

### <a name="create-the-delegate-and-matching-procedures"></a>デリゲートと一致するプロシージャを作成する

1. `MySubDelegate`という名前のデリゲートを作成します。

    ```vb
    Delegate Sub MySubDelegate(ByVal x As Integer)
    ```

2. デリゲートと同じシグネチャを持つメソッドを含むクラスを宣言します。

    ```vb
    Class class1
        Sub Sub1(ByVal x As Integer)
            MsgBox("The value of x is: " & CStr(x))
        End Sub
    End Class
    ```

3. デリゲートのインスタンスを作成し、組み込みの `Invoke` メソッドを呼び出すことによって、デリゲートに関連付けられているメソッドを呼び出すメソッドを定義します。

    ```vb
    Protected Sub DelegateTest()
        Dim c1 As New class1
        ' Create an instance of the delegate.
        Dim msd As MySubDelegate = AddressOf c1.Sub1
        ' Call the method.
        msd.Invoke(10)
    End Sub
    ```

## <a name="see-also"></a>参照

- [Delegate ステートメント](../../../../visual-basic/language-reference/statements/delegate-statement.md)
- [デリゲート](../../../../visual-basic/programming-guide/language-features/delegates/index.md)
- [イベント](../../../../visual-basic/programming-guide/language-features/events/index.md)
- [マルチスレッド アプリケーション](../../../../standard/threading/using-threads-and-threading.md)
