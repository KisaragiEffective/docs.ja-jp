---
title: 再帰プロシージャ
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic code, procedures
- procedures [Visual Basic], that call themselves
- procedures [Visual Basic], recursive
- procedures [Visual Basic], calling
- recursive procedures
- functions [Visual Basic], calling recursively
- recursion
ms.assetid: ba1d3962-b4c3-48d3-875e-96fdb4198327
ms.openlocfilehash: 646d4e29ed7a0b6367d4b35a7f8641bcf659e616
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352552"
---
# <a name="recursive-procedures-visual-basic"></a>再帰プロシージャ (Visual Basic)

*再帰*プロシージャは、それ自体を呼び出すものです。 一般に、これは Visual Basic コードを記述するための最も効果的な方法ではありません。  
  
 次の手順では、再帰を使用して、元の引数の階乗を計算します。  
  
 [!code-vb[VbVbcnProcedures#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#51)]  
  
## <a name="considerations-with-recursive-procedures"></a>再帰プロシージャに関する考慮事項

 **条件の制限。 再帰プロシージャは、再帰を終了できる条件が少なくとも1つ必要であることをテストするように設計する必要があります。また、適切な数の再帰呼び出しで条件が満たされない場合にも処理する必要があります。 失敗せずに 1 つ以上の条件を満たすことができない場合、プロシージャは無限ループで実行されるリスクが高くなります。

 **メモリ使用状況。 ** アプリケーションのローカル変数には、限られた量の領域があります。 プロシージャがそれ自体を呼び出すたびに、そのローカル変数のコピーを追加するために、その領域を使用します。 この処理が無期限に続行されると、最終的に <xref:System.StackOverflowException> エラーが発生します。

 **効率。 ** ほとんどの場合、再帰のためにループを置き換えることができます。 ループには、引数を渡したり、追加のストレージを初期化したり、値を返したりするオーバーヘッドがありません。 再帰呼び出しを行わないと、パフォーマンスが大幅に向上します。

 **相互再帰**。 2つのプロシージャが互いに呼び出す場合、パフォーマンスが低下したり、無限ループが発生したりする可能性があります。 このような設計では、単一の再帰プロシージャと同じ問題が発生しますが、検出とデバッグが困難な場合があります。

 **かっこを使った呼び出し。  **   `Function` プロシージャがそれ自体を再帰的に呼び出す場合は、引数リストがない場合でも、プロシージャ名の後にかっこを指定する必要があります。 それ以外の場合、関数名は関数の戻り値を表すものとして取得されます。

 **テスト。 ** 再帰プロシージャを記述する場合は、常に一定の制限条件を満たしていることを確認するために、慎重にテストする必要があります。 また、再帰呼び出しが多すぎるため、メモリが不足することがないようにする必要があります。

## <a name="see-also"></a>参照

- <xref:System.StackOverflowException>
- [Visual Basic におけるプロシージャ](index.md)
- [Sub プロシージャ](sub-procedures.md)
- [Function プロシージャ](function-procedures.md)
- [Property プロシージャ](property-procedures.md)
- [演算子プロシージャ](operator-procedures.md)
- [プロシージャのパラメーターと引数](procedure-parameters-and-arguments.md)
- [プロシージャのオーバーロード](procedure-overloading.md)
- [プロシージャのトラブルシューティング](troubleshooting-procedures.md)
- [ループ構造](../control-flow/loop-structures.md)
