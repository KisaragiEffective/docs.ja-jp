---
title: IsNot 演算子
ms.date: 07/20/2015
f1_keywords:
- vb.isnot
helpviewer_keywords:
- IsNot operator [Visual Basic]
ms.assetid: 8dd2bcdb-0166-48a2-9094-60dfb448f36c
ms.openlocfilehash: 616506f64d20e1f150b443433f1b69040136a5ba
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74336074"
---
# <a name="isnot-operator-visual-basic"></a>IsNot 演算子 (Visual Basic)

2つのオブジェクト参照変数を比較します。

## <a name="syntax"></a>構文

```vb
result = object1 IsNot object2
```

## <a name="parts"></a>指定項目
 `result` 必須。 `Boolean` 値。

 `object1` 必須。 任意の `Object` 変数または式。

 `object2` 必須。 任意の `Object` 変数または式。

## <a name="remarks"></a>コメント
 `IsNot` 演算子は、2つのオブジェクト参照が異なるオブジェクトを参照するかどうかを決定します。 ただし、値の比較は実行されません。 `object1` と `object2` 両方がまったく同じオブジェクトインスタンスを参照している場合、`result` は `False`です。そうでない場合は、`result` が `True`ます。

 `IsNot` は、`Is` 演算子とは逆です。 `IsNot` の利点は、`Not` と `Is`での不適切な構文を避けることができることです。これは読みにくくなる可能性があります。

 `Is` 演算子と `IsNot` 演算子を使用すると、事前バインディングオブジェクトと遅延バインディングオブジェクトの両方をテストできます。

## <a name="example"></a>例
 次のコード例では、`Is` 演算子と `IsNot` 演算子の両方を使用して、同じ比較を実行します。

 [!code-vb[VbVbalrOperators#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#29)]

## <a name="see-also"></a>参照

- [Is 演算子](is-operator.md)
- [TypeOf 演算子](typeof-operator.md)
- [Visual Basic における演算子の優先順位](operator-precedence.md)
- [方法: 2 つのオブジェクトが等しいかどうかをテストする](../../programming-guide/language-features/operators-and-expressions/how-to-test-whether-two-objects-are-the-same.md)
