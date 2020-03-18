---
title: アクセスするインスタンスがインターフェイス型であるため、遅延バインドされたオーバーロードの解決は '<procedurename>' に適用されません。
ms.date: 07/20/2015
f1_keywords:
- vbc30933
- bc30933
helpviewer_keywords:
- overload resolution [Visual Basic], with late-bound argument
- BC30933
ms.assetid: 8182eea0-dd34-4d6e-9ca0-41d8713e9dc4
ms.openlocfilehash: 1fe3c4a29b542302b3615459142a3c565aa8244f
ms.sourcegitcommit: 463f3f050cecc0b6403e67f19a61f870fb8e7b7d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/26/2019
ms.locfileid: "68513026"
---
# <a name="latebound-overload-resolution-cannot-be-applied-to-procedurename-because-the-accessing-instance-is-an-interface-type"></a>アクセスするインスタンスがインターフェイス型であるため、遅延バインドされたオーバーロードの解決は '\<procedurename>' に適用されません

コンパイラは、オーバーロードされたプロパティまたはプロシージャへの参照を解決しようとしていますが、引数`Object`が型であり、参照元のオブジェクトがインターフェイスのデータ型を持っているため、参照は失敗します。 引数`Object`は、コンパイラが参照を遅延バインディングとして解決することを強制します。

このような場合、コンパイラは、基になるインターフェイスを使用するのではなく、実装するクラスを使用してオーバーロードを解決します。 クラスがオーバーロードされたバージョンの1つの名前を変更した場合、コンパイラはそのバージョンの名前が異なるため、そのバージョンがオーバーロードであるとは見なされません。 これにより、コンパイラは、参照を解決するための正しい選択が行われている可能性がある場合に、名前が変更されたバージョンを無視します。

**エラー ID:** BC30933

## <a name="to-correct-this-error"></a>このエラーを解決するには

- 引数`CType`をから`Object`呼び出すオーバーロードのシグネチャで指定された型にキャストするには、を使用します。

  参照元のオブジェクトを基になるインターフェイスにキャストすることはできないことに注意してください。 このエラーを回避するには、引数をキャストする必要があります。

## <a name="example"></a>例

次の例は、コンパイル時にこの`Sub`エラーを発生させるオーバーロードされたプロシージャの呼び出しを示しています。

```vb
Module m1
    Interface i1
        Sub s1(ByVal p1 As Integer)
        Sub s1(ByVal p1 As Double)
    End Interface
    Class c1
        Implements i1
        Public Overloads Sub s1(ByVal p1 As Integer) Implements i1.s1
        End Sub
        Public Overloads Sub s2(ByVal p1 As Double) Implements i1.s1
        End Sub
    End Class
    Sub Main()
        Dim refer As i1 = New c1
        Dim o1 As Object = 3.1415
        ' The following reference is INVALID and causes a compiler error.
        refer.s1(o1)
    End Sub
End Module
```

前の例では、コンパイラがの呼び出しを`s1`書き込み済みとして許可した場合、インターフェイス`i1`で`c1`はなくクラスを使用して解決が行われます。 これは、で定義されている`s2` `i1`正しい選択であっても`c1`、コンパイラではその名前が異なるために考慮されないことを意味します。

このエラーを修正するには、次のいずれかのコード行の呼び出しを変更します。

```vb
refer.s1(CType(o1, Integer))
refer.s1(CType(o1, Double))
```

前のコード行では、オーバーロードに対し`Object`て`o1`定義されているいずれかのパラメーター型に変数を明示的にキャストします。

## <a name="see-also"></a>関連項目

- [プロシージャのオーバーロード](../../../visual-basic/programming-guide/language-features/procedures/procedure-overloading.md)
- [オーバーロードの解決](../../../visual-basic/programming-guide/language-features/procedures/overload-resolution.md)
- [CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)
