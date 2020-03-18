---
title: '方法: バイト配列を文字列に変換する'
ms.date: 07/20/2015
helpviewer_keywords:
- string conversion [Visual Basic], arrays
- byte arrays [Visual Basic], converting to strings
- examples [Visual Basic], strings
- arrays [Visual Basic], converting to strings
ms.assetid: d0dc8317-9ab3-4324-99f7-3f5788c0e72a
ms.openlocfilehash: 8c1d9d1d2e89390873bc1c3dbb9623f047433a9a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351987"
---
# <a name="how-to-convert-an-array-of-bytes-into-a-string-in-visual-basic"></a>方法 : Visual Basic でバイトの配列を文字列に変換する
このトピックでは、バイト配列のバイトを文字列に変換する方法について説明します。  
  
## <a name="example"></a>例  
 この例では、<xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType> encoding クラスの <xref:System.Text.Encoding.GetString%2A> メソッドを使用して、バイト配列のすべてのバイトを文字列に変換します。  
  
 [!code-vb[VbVbalrStrings#72](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#72)]  
  
 複数のエンコードオプションから選択して、バイト配列を文字列に変換することができます。  
  
- <xref:System.Text.Encoding.ASCII%2A?displayProperty=nameWithType>: ASCII (7 ビット) 文字セットのエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.BigEndianUnicode%2A?displayProperty=nameWithType>: ビッグエンディアンのバイト順を使用する UTF-16 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.Default%2A?displayProperty=nameWithType>: システムの現在の ANSI コードページのエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.Unicode%2A?displayProperty=nameWithType>: リトルエンディアンのバイト順を使用する UTF-16 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF32%2A?displayProperty=nameWithType>: リトルエンディアンのバイト順を使用して、32形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF7%2A?displayProperty=nameWithType>: UTF-7 形式のエンコーディングを取得します。  
  
- <xref:System.Text.Encoding.UTF8%2A?displayProperty=nameWithType>: UTF-8 形式のエンコーディングを取得します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Text.Encoding?displayProperty=nameWithType>
- <xref:System.Text.Encoding.GetString%2A>
- [方法: Visual Basic で文字列をバイト配列に変換する](../../../../visual-basic/programming-guide/language-features/strings/how-to-convert-strings-into-an-array-of-bytes.md)
