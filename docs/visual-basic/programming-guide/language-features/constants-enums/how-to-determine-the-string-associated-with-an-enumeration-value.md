---
title: '方法 : 列挙値に関連付けられている文字列を確認する'
ms.date: 07/20/2015
helpviewer_keywords:
- enumerations [Visual Basic]
- strings [Visual Basic], enumeration values
- values [Visual Basic], enumeration members
ms.assetid: 9253e7c8-579c-49a2-8f26-392b20ea99eb
ms.openlocfilehash: c396a4e2ebe973f5be65c3fab5f22cdedb29be1a
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351133"
---
# <a name="how-to-determine-the-string-associated-with-an-enumeration-value-visual-basic"></a>方法: 列挙値に関連付けられている文字列を確認する (Visual Basic)
<xref:System.Enum.GetValues%2A> メソッドと <xref:System.Enum.GetNames%2A> メソッドを使用すると、列挙メンバーに関連付けられている文字列と値を確認できます。  
  
### <a name="to-determine-the-string-associated-with-an-enumeration"></a>列挙体に関連付けられている文字列を確認するには  
  
- <xref:System.Enum.GetNames%2A> メソッドを使用して、列挙体のメンバーに関連付けられている文字列を取得します。 この例では、列挙型を宣言し、`flavorEnum`、<xref:System.Enum.GetNames%2A> メソッドを使用して、各メンバーに関連付けられている文字列を表示します。  
  
     [!code-vb[VbEnumsTask#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#2)]  
  
## <a name="see-also"></a>参照

- <xref:System.Enum.GetValues%2A>
- <xref:System.Enum.GetNames%2A>
- <xref:System.Enum>
- [方法: 列挙型を宣言する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-declare-enumerations.md)
- [方法 : 列挙型のメンバーを参照する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [方法: Visual Basic 内の列挙体を反復処理する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-iterate-through-an-enumeration.md)
- [列挙型を使用する状況](../../../../visual-basic/programming-guide/language-features/constants-enums/when-to-use-an-enumeration.md)
- [Enum ステートメント](../../../../visual-basic/language-reference/statements/enum-statement.md)
