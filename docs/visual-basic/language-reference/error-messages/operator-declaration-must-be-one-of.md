---
title: "'演算子宣言は次のいずれかでなければなりません: +、-、*、\\\\、/、^、&amp;、Like、Mod、And、Or、Xor、Not、<<、>>、=、<>、<、<=、>、 >=、CType、IsTrue、または IsFalse'"
ms.date: 07/20/2015
f1_keywords:
- bc33000
- vbc33000
helpviewer_keywords:
- BC33000
ms.assetid: 15c5d8eb-3a8c-4141-8f41-33151afabf97
ms.openlocfilehash: 4283547109ec312cc4fe07a054bbb8db3bff660f
ms.sourcegitcommit: 9b552addadfb57fab0b9e7852ed4f1f1b8a42f8e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/23/2019
ms.locfileid: "61946602"
---
# <a name="operator-declaration-must-be-one-of----amp-like-mod-and-or-xor-not--"></a>演算子宣言は次のいずれかでなければなりません: +、-、*、\,、/、^、&amp;、Like、Mod、And、Or、Xor、Not、\<\<、>>、=、\<>、\<、\<=、>、 >=、CType、
オーバー ロードできるは、演算子のみを宣言できます。 次の表は、演算子を宣言することができます。  
  
|型|演算子|  
|----------|---------------|  
|単項|`+`, `-`, `IsFalse`, `IsTrue`, `Not`|  
|2 項|`+`, `-`, `*`, `/`, `\`, `&`, `^`, `>>`, `<<`, `=`, `<>`, `>`, `>=`, `<`, `<=`, `And`, `Like`, `Mod`, `Or`, `Xor`|  
|変換 (単項)|`CType`|  
  
 なお、`=`バイナリの一覧で演算子は、比較演算子、代入演算子ではありません。  
  
 **エラー ID:** BC33000  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. 演算子をオーバーロード可能な演算子の中から選択します。  
  
2. 直接オーバーロードできない演算子をオーバーロードする機能が必要な場合は、適切なパラメーターを受け取り適切な値を返す `Function` プロシージャを作成します。  
  
## <a name="see-also"></a>関連項目

- [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)
- [演算子プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/operator-procedures.md)
- [方法: 演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [方法: 変換演算子を定義する](../../../visual-basic/programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
