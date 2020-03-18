---
title: '方法: 日付または時刻を表す文字列を検証する'
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], validating
- String data type [Visual Basic], validation
ms.assetid: ae7d4b29-3436-4032-bdbf-4650eb1c8e19
ms.openlocfilehash: 5321e7a85c45ddb6ce17433bd25ce9ca2165f0a3
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348411"
---
# <a name="how-to-validate-strings-that-represent-dates-or-times-visual-basic"></a>方法: 日付または時刻を表す文字列を検証する (Visual Basic)
次のコード例では、文字列が有効な日付または時刻を表すかどうかを示す `Boolean` 値を設定します。  
  
## <a name="example"></a>使用例  
 [!code-vb[VbVbcnRegEx#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnRegEx/VB/Class1.vb#2)]  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 `("01/01/03")` と `"9:30 PM"` を、検証する日付と時刻に置き換えます。 文字列を別のハードコーディングされた文字列に置き換えたり、`String` 変数を使用したり、文字列を返すメソッド (`InputBox`など) に置き換えることができます。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 `String` を `DateTime` 変数に変換する前に、このメソッドを使用して文字列を検証します。 最初に日付または時刻を確認することで、実行時に例外が生成されないようにすることができます。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.Information.IsDate%2A>
- <xref:Microsoft.VisualBasic.Interaction.InputBox%2A>
- [Visual Basic における文字列の検証](../../../../visual-basic/programming-guide/language-features/strings/validating-strings.md)
