---
title: 関数 '<procedurename>' の戻り値の型は CLS に準拠していません。
ms.date: 07/20/2015
f1_keywords:
- bc40027
- vbc40027
helpviewer_keywords:
- BC40027
ms.assetid: 33c088c7-48e7-400c-920e-6d8967e1f3fc
ms.openlocfilehash: 881726ea2cfb23493d85097635adb15608ed741d
ms.sourcegitcommit: 8699383914c24a0df033393f55db3369db728a7b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/15/2019
ms.locfileid: "65642255"
---
# <a name="return-type-of-function-procedurename-is-not-cls-compliant"></a>関数 '\<procedurename >' の戻り値の型は CLS に準拠していません。
A`Function`プロシージャがマーク`<CLSCompliant(True)>`としてマークされている型を返しますが、 `<CLSCompliant(False)>`、マークされていない、または非準拠の型であるためには修飾されません。  
  
 プロシージャを[言語への非依存性および言語非依存コンポーネント](../../../standard/language-independence-and-language-independent-components.md) (CLS) に準拠させるには、CLS 準拠型のみを使用する必要があります。 これは、パラメーターの型、戻り値の型、およびすべてのローカル変数の型に適用されます。  
  
 次の Visual Basic データ型は CLS 準拠ではありません。  
  
- [SByte データ型](../../../visual-basic/language-reference/data-types/sbyte-data-type.md)  
  
- [UInteger データ型](../../../visual-basic/language-reference/data-types/uinteger-data-type.md)  
  
- [ULong データ型](../../../visual-basic/language-reference/data-types/ulong-data-type.md)  
  
- [UShort データ型](../../../visual-basic/language-reference/data-types/ushort-data-type.md)  
  
 プログラミング要素に <xref:System.CLSCompliantAttribute> を適用する場合は、準拠または非準拠を示すために、属性の `isCompliant` パラメーターを `True` または `False` のどちらかに設定します。 このパラメーターには既定値がありません。値を指定する必要があります。  
  
 要素に <xref:System.CLSCompliantAttribute> を適用しないと、その要素は非準拠と見なされます。  
  
 既定では、このメッセージは警告です。 警告を非表示にする方法や、警告をエラーとして扱う方法の詳細については、「[Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)」をご覧ください。  
  
 **エラー ID:** BC40027  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 場合、`Function`プロシージャがこの特定の型を返す、削除する必要があります、<xref:System.CLSCompliantAttribute>します。 プロシージャは CLS 準拠になりません。  
  
- 場合、`Function`プロシージャが CLS に準拠する、最も近い CLS 準拠型に戻り値の型を変更する必要があります。 たとえば、2,147,483,647 を超える値の範囲が不要な場合は、 `UInteger` の代わりに `Integer` を使用できます。 拡張範囲が必要な場合は、 `UInteger` の代わりに `Long`を使用できます。  
  
- オートメーションまたは COM オブジェクトをやり取りする場合は、一部の種類がある .NET Framework の別のデータ幅よりも注意してください。 たとえば、他の多くの環境では `int` は 16 ビットです。 このようなコンポーネントに 16 ビット整数を返す場合は宣言として`Short`の代わりに`Integer`管理対象の Visual Basic コードです。
