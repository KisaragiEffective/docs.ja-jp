---
title: '方法: 文字列をバイト配列に変換する'
ms.date: 07/20/2015
helpviewer_keywords:
- string conversion [Visual Basic], arrays
- arrays [Visual Basic], converting strings to
- byte arrays
- examples [Visual Basic], string conversion
- arrays [Visual Basic], byte arrays
ms.assetid: f477d35c-a3fc-4a30-b1d4-cd0d353aae1d
ms.openlocfilehash: 76fde3120ce629ce32f29ca28d90eba24fff726c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351968"
---
# <a name="how-to-convert-strings-into-an-array-of-bytes-in-visual-basic"></a>方法 : Visual Basic で文字列をバイトの配列に変換する
このトピックでは、文字列をバイト配列に変換する方法について説明します。  
  
## <a name="example"></a>例  
 この例では、<xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> encoding クラスの <xref:System.Text.Encoding.GetBytes%2A> メソッドを使用して、文字列をバイト配列に変換します。  
  
 [!code-vb[VbVbalrStrings#74](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#74)]  
  
 文字列をバイト配列に変換するには、いくつかのエンコードオプションから選択できます。  
  
- <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType>: ASCII (7 ビット) 文字セットのエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.BigEndianUnicode%2A?displayProperty=nameWithType>: ビッグエンディアンのバイト順を使用する UTF-16 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.Default%2A?displayProperty=nameWithType>: システムの現在の ANSI コードページのエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType>: リトルエンディアンのバイト順を使用する UTF-16 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF32%2A?displayProperty=nameWithType>: リトルエンディアンのバイト順を使用して、32形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF7%2A?displayProperty=nameWithType>: UTF-7 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF8%2A?displayProperty=nameWithType>: UTF-8 形式のエンコーディングを取得します。  
  
## <a name="see-also"></a>参照

- <xref:System.Text.Encoding?displayProperty=nameWithType>
- <xref:System.Text.Encoding.GetBytes%2A>
- [方法: Visual Basic でバイト配列を文字列に変換する](../../../../visual-basic/programming-guide/language-features/strings/how-to-convert-an-array-of-bytes-into-a-string.md)
