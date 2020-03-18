---
title: 0から始まる文字列アクセスと1から始まる文字列アクセス
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], indexing
ms.assetid: 0ed39f35-d68e-421d-ae14-460a5c0373b8
ms.openlocfilehash: 97e60038bc7ec0f030939d0980b786bffebcfb9a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354300"
---
# <a name="zero-based-vs-one-based-string-access-in-visual-basic"></a>Visual Basic における文字列アクセスのインデックス番号
このトピックでは、Visual Basic と .NET Framework が文字列内の文字へのアクセスを提供する方法を比較します。 .NET Framework は、文字列内の文字へのゼロベースのアクセスを常に提供します。一方、Visual Basic は、関数に応じて、0から始まる、1から始まるアクセスを提供します。  
  
## <a name="one-based"></a>1から始まる  
 1ベースの Visual Basic 関数の例については、`Mid` 関数を検討してください。 位置1から始まる部分文字列の開始位置を示す引数を受け取ります。 .NET Framework <xref:System.String.Substring%2A?displayProperty=nameWithType> メソッドは、位置0から開始して、部分文字列を開始する文字列内の文字のインデックスを取得します。 したがって、文字列 "ABCDE...Z" を使用している場合、個々の文字には、`Mid` 関数で使用するために1、2、3、4、5の番号が付けられますが、<xref:System.String.Substring%2A?displayProperty=nameWithType> メソッドで使用する場合は0、1、2、3、4になります。  
  
## <a name="zero-based"></a>0から始まる  
 0から始まる Visual Basic 関数の例については、`Split` 関数を検討してください。 文字列を分割し、部分文字列を含む配列を返します。 また、.NET Framework <xref:System.String.Split%2A?displayProperty=nameWithType> メソッドは文字列を分割し、部分文字列を含む配列を返します。 `Split` 関数と <xref:System.String.Split%2A> メソッドは .NET Framework 配列を返すため、0から始まる必要があります。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Strings.Mid%2A>
- <xref:Microsoft.VisualBasic.Strings.Split%2A>
- <xref:System.String.Substring%2A>
- <xref:System.String.Split%2A>
- [Visual Basic の文字列の概要](../../../../visual-basic/programming-guide/language-features/strings/introduction-to-strings.md)
