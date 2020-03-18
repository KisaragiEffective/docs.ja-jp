---
title: Boolean データ型
ms.date: 07/20/2015
f1_keywords:
- vb.FALSE
- vb.TRUE
- vb.Boolean
helpviewer_keywords:
- Boolean data type
- Boolean values [Visual Basic], False keyword
- False keyword [Visual Basic]
- True keyword [Visual Basic]
- Boolean values [Visual Basic], True keyword
ms.assetid: 4858e630-4813-4216-a55e-f4d0feb884e4
ms.openlocfilehash: 5d05514207c5d07e81aab897f40f728570f6bd87
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347842"
---
# <a name="boolean-data-type-visual-basic"></a>Boolean データ型 (Visual Basic)

`True` または `False`のみ可能な値を保持します。 キーワード `True` と `False` は `Boolean` 変数の2つの状態に対応しています。  
  
## <a name="remarks"></a>コメント  

 [Boolean データ型 (Visual Basic)](../../../visual-basic/language-reference/data-types/boolean-data-type.md) を使用して、true/false、yes/no、on/off などの 2 つの状態の値を格納します。  
  
 `Boolean` の既定値は `False` です。  
  
 `Boolean` 値は数値として格納されず、格納されている値は数値と等価であるとは見なされません。 `True` と `False`の等価の数値に依存するコードを記述することは避けてください。 可能な限り、`Boolean` 変数の使用は、設計対象の論理値に制限する必要があります。  
  
## <a name="type-conversions"></a>型変換  

 Visual Basic 数値データ型の値を `Boolean`に変換する場合、0は `False` になり、その他のすべての値は `True`になります。 `Boolean` 値を数値型に変換 Visual Basic 場合、`False` は0になり、`True` は-1 になります。  
  
 `Boolean` 値と数値データ型の間で変換を行う場合、.NET Framework の変換メソッドでは、必ずしも Visual Basic の変換キーワードと同じ結果が生成されないことに注意してください。 これは、Visual Basic 変換は、以前のバージョンと互換性のある動作を保持するためです。 詳細については、「[データ型のトラブルシューティング](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)」の「ブール型が数値型に正しく変換されない」を参照してください。  
  
## <a name="programming-tips"></a>プログラミングのヒント  
  
- **負の数。** `Boolean` は数値型ではなく、負の値を表すことはできません。 どのような場合でも、`Boolean` を使用して数値を保持しないでください。  
  
- **型文字。** `Boolean` には、リテラルの型文字または識別子の型文字がありません。  
  
- **Framework の型。** .NET Framework において対応する型は、<xref:System.Boolean?displayProperty=nameWithType> 構造体です。  
  
## <a name="example"></a>例  

 次の例では、`runningVB` は、単純な yes/no 設定を格納する `Boolean` 変数です。  
  
```vb  
Dim runningVB As Boolean  
' Check to see if program is running on Visual Basic engine.  
If scriptEngine = "VB" Then  
    runningVB = True  
End If  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Boolean?displayProperty=nameWithType>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [トラブルシューティング (データ型)](../../../visual-basic/programming-guide/language-features/data-types/troubleshooting-data-types.md)
- [CType 関数](../../../visual-basic/language-reference/functions/ctype-function.md)
