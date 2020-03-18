---
title: 文字列操作メソッドの種類
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], manipulating [Visual Basic]
- string manipulation
ms.assetid: 905055cd-7f50-48fb-9eed-b0995af1dc1f
ms.openlocfilehash: a02278abfb71efb2f31f239a89a22ad1c8ee7a18
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346270"
---
# <a name="types-of-string-manipulation-methods-in-visual-basic"></a>Visual Basic における文字列操作メソッドの種類
文字列を分析して操作するには、いくつかの方法があります。 一部のメソッドは Visual Basic 言語の一部であり、`String` クラスに固有のものです。  
  
## <a name="visual-basic-language-and-the-net-framework"></a>Visual Basic 言語と .NET Framework  
 Visual Basic メソッドは、言語の固有の関数として使用されます。 これらは、コード内で修飾なしで使用することができます。 次の例は、Visual Basic 文字列操作コマンドの一般的な使用法を示しています。  
  
 [!code-vb[VbVbalrStrings#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#44)]  
  
 この例では、`Mid` 関数が `aString` に対して直接操作を実行し `bString`に値を代入します。  
  
 Visual Basic 文字列操作メソッドの一覧については、「[文字列操作の概要](../../../../visual-basic/language-reference/keywords/string-manipulation-summary.md)」を参照してください。  
  
### <a name="shared-methods-and-instance-methods"></a>共有メソッドとインスタンスメソッド  
 `String` クラスのメソッドを使用して、文字列を操作することもできます。 `String`には、*共有*メソッドと*インスタンス*メソッドという2種類のメソッドがあります。  
  
#### <a name="shared-methods"></a>共有メソッド  
 共有メソッドは、`String` クラス自体からのメソッドであり、そのクラスのインスタンスが動作するためには必要ありません。 これらのメソッドは、`String` クラスのインスタンスではなく、クラスの名前 (`String`) で修飾できます。 例 :  
  
 [!code-vb[VbVbalrStrings#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#45)]  
  
 前の例では、<xref:System.String.Copy%2A?displayProperty=nameWithType> メソッドは静的メソッドです。このメソッドは、指定された式に対して動作し、結果として得られる値を `bString`に代入します。  
  
#### <a name="instance-methods"></a>インスタンスメソッド  
 これに対して、インスタンスメソッドは、`String` の特定のインスタンスからのものであり、インスタンス名で修飾する必要があります。 例 :  
  
 [!code-vb[VbVbalrStrings#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#46)]  
  
 この例では、<xref:System.String.Substring%2A?displayProperty=nameWithType> メソッドは、`String` のインスタンス (つまり、`aString`) のメソッドです。 `aString` に対して操作を実行し、その値を `bString`に割り当てます。  
  
 詳細については、<xref:System.String> クラスのドキュメントを参照してください。  
  
## <a name="see-also"></a>参照

- [Visual Basic の文字列の概要](../../../../visual-basic/programming-guide/language-features/strings/introduction-to-strings.md)
