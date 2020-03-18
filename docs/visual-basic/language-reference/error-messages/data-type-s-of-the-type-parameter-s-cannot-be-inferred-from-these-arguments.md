---
title: 型パラメーターのデータ型を、これらの引数から推論することはできません
ms.date: 07/20/2015
f1_keywords:
- bc36644
- bc36647
- vbc36647
- vbc36644
helpviewer_keywords:
- BC36644
- BC36647
ms.assetid: 0e0050f2-2039-4311-b260-f0ebfde84189
ms.openlocfilehash: 97b03874489473482554078958c7bfd29d87c095
ms.sourcegitcommit: 4f4a32a5c16a75724920fa9627c59985c41e173c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/17/2019
ms.locfileid: "72523993"
---
# <a name="data-types-of-the-type-parameters-cannot-be-inferred-from-these-arguments"></a>型パラメーターのデータ型を、これらの引数から推論することはできません

型パラメーターのデータ型をこれらの引数から推論することはできません。 データ型を明示的に指定すると、このエラーを修正できる場合があります。

このエラーは、オーバーロードの解決法が失敗したときに発生します。 これは、特定のオーバーロード候補が削除された理由を示す従属メッセージとして発生します。 エラーメッセージは、コンパイラが型の推定を使用して型パラメーターのデータ型を検索できないことを示しています。

> [!NOTE]
> 引数の指定がオプションではない場合 (たとえば、クエリ式内のクエリ演算子など)、エラー メッセージの 2 つ目の文は表示されません。

次のコードにこのエラーを示します。

```vb
Module Module1

    Sub Main()

        '' Not Valid.
        'OverloadedGenericMethod("Hello", "World")

    End Sub

    Sub OverloadedGenericMethod(Of T)(ByVal x As String,
                                      ByVal y As InterfaceExample(Of T))
    End Sub

    Sub OverloadedGenericMethod(Of T, R)(ByVal x As T,
                                         ByVal y As InterfaceExample(Of R))
    End Sub

End Module

Interface InterfaceExample(Of T)
End Interface
```

**エラー ID:** BC36647 と BC36644

## <a name="to-correct-this-error"></a>このエラーを解決するには

型の推定に依存せずに、型パラメーターまたはパラメーターのデータ型を指定できる場合があります。

## <a name="see-also"></a>関連項目

- [厳密でないデリゲート変換](../../../visual-basic/programming-guide/language-features/delegates/relaxed-delegate-conversion.md)
- [Visual Basic におけるジェネリック プロシージャ](../../../visual-basic/programming-guide/language-features/data-types/generic-procedures.md)
- [Visual Basic における型変換](../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
