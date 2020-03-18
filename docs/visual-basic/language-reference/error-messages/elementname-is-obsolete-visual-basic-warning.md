---
title: "'<elementname>' は古い形式です (Visual Basic 警告)"
ms.date: 07/20/2015
f1_keywords:
- vbc40008
- bc40008
helpviewer_keywords:
- BC40008
ms.assetid: 729e3eb5-76ac-4c55-9fdd-78350e0de55e
ms.openlocfilehash: d7d3d86f89ef3b76e958707dd0be2dce8a3e9bf2
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2019
ms.locfileid: "64659813"
---
# <a name="elementname-is-obsolete-visual-basic-warning"></a>'\<elementname >' は廃止されています (Visual Basic 警告)
ステートメントが、<xref:System.ObsoleteAttribute> 属性でマーク付けされているプログラミング要素にアクセスしています。また、ディレクティブがそれを警告として扱うように設定されています。  
  
 <xref:System.ObsoleteAttribute> を適用することで、任意のプログラミング要素を使用しない要素としてマークできます。 これを行う場合は、属性の<xref:System.ObsoleteAttribute.IsError%2A> プロパティを `True` または `False`に設定できます。 `True`に設定した場合、コンパイラは要素を使用する試みをエラーとして扱います。 `False`に設定した場合、または既定値の `False`を使用した場合、コンパイラはこの要素の使用が試行されると、警告を発行します。  
  
 既定では、 <xref:System.ObsoleteAttribute.IsError%2A> の <xref:System.ObsoleteAttribute> プロパティが `False`であるため、このメッセージは警告です。 警告を表示しない方法や、警告をエラーとして扱う方法の詳細については、「[Visual Basic での警告の構成](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC40008  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- ソース コード参照の要素名のスペルが正しいことを確認します。  
  
## <a name="see-also"></a>関連項目

- [属性の概要](../../../visual-basic/programming-guide/concepts/attributes/index.md)
