---
title: 属性 '<attributename>' を複数回適用することはできません。
ms.date: 07/20/2015
f1_keywords:
- bc30663
- vbc30663
helpviewer_keywords:
- BC30663
ms.assetid: 3760e7ff-7238-40a1-8676-77d858a64fc0
ms.openlocfilehash: f2f4dc428a247275f9919c4a8b6e6944a558eef0
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968226"
---
# <a name="attribute-attributename-cannot-be-applied-multiple-times"></a>属性 '\<attributename > ' を複数回適用することはできません

属性を適用できるのは一度だけです。 `AttributeUsage` 属性は、属性を複数回適用できるかどうかを決定します。  
  
 **エラー ID:** BC30663  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. 属性が適用されるのは一度だけであることを確認してください。  
  
2. 開発したカスタム属性を使用している場合は、次の例のように、複数の属性の使用を許可するように `AttributeUsage` 属性を変更することを検討してください。  
  
```vb  
<AttributeUsage(AllowMultiple := True)>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.AttributeUsageAttribute>
- [カスタム属性の作成](../../../visual-basic/programming-guide/concepts/attributes/creating-custom-attributes.md)
- [AttributeUsage](../../../visual-basic/programming-guide/concepts/attributes/attributeusage.md)
