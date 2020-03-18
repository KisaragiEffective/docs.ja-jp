---
title: '方法: 引数の値渡しを強制する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- procedures [Visual Basic], calling
- arguments [Visual Basic], in parentheses
- procedure arguments [Visual Basic], in parentheses
- arguments [Visual Basic], changing value
ms.assetid: 77b4f2d2-1055-4c2f-a521-874d1db86946
ms.openlocfilehash: 047738a2cbadc6b7d72f41aade22bbeff16d1bac
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347585"
---
# <a name="how-to-force-an-argument-to-be-passed-by-value-visual-basic"></a>方法: 引数の値渡しを強制する (Visual Basic)
プロシージャの宣言によって、渡される機構が決まります。 パラメーターが[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)として宣言されている場合、Visual Basic は、対応する引数を参照渡しで渡すことを想定しています。 これにより、プロシージャは、呼び出し元のコードの引数の基になるプログラミング要素の値を変更できます。 このような変更に対して基になる要素を保護する場合は、引数名をかっこで囲むことによって、プロシージャ呼び出しの `ByRef` 渡す機構をオーバーライドできます。 これらのかっこに加えて、呼び出しの引数リストを囲むかっこが追加されます。  
  
 呼び出し元のコードで[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)機構をオーバーライドすることはできません。  
  
### <a name="to-force-an-argument-to-be-passed-by-value"></a>引数を強制的に値によって渡すには  
  
- 対応するパラメーターがプロシージャの `ByVal` として宣言されている場合、追加の手順を実行する必要はありません。 Visual Basic は、既に値渡しで引数を渡すことを想定しています。  
  
- 対応するパラメーターがプロシージャの `ByRef` として宣言されている場合は、プロシージャ呼び出しで引数をかっこで囲みます。  
  
## <a name="example"></a>使用例  
 次の例では、`ByRef` パラメーター宣言をオーバーライドします。 `ByVal`を強制する呼び出しで、2つのレベルのかっこに注意してください。  
  
 [!code-vb[VbVbcnProcedures#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#39)]  
  
 [!code-vb[VbVbcnProcedures#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#40)]  
  
 `str` が引数リスト内の余分なかっこで囲まれている場合、`setNewString` プロシージャは呼び出し元のコードの値を変更できず、"ByVal が渡された場合は置き換えることができません" `MsgBox` と表示されます。 `str` が余分なかっこで囲まれていない場合は、プロシージャによって変更され、"inString 引数の新しい値です" と表示さ `MsgBox` ます。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 変数を参照渡しで渡す場合は、`ByRef` キーワードを使用してこのメカニズムを指定する必要があります。  
  
 Visual Basic の既定では、値渡しで引数を渡します。 ただし、すべての宣言されたパラメーターに[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)キーワードまたは[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)キーワードを含めることをお勧めします。 これにより、コードが読みやすくなります。  
  
## <a name="robust-programming"></a>堅牢性の高いプログラミング  
 プロシージャが[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)パラメーターを宣言する場合、コードの正しい実行は、呼び出し元のコード内の基になる要素を変更できるかどうかに依存する場合があります。 呼び出し元のコードが引数をかっこで囲むことによってこの呼び出し機構をオーバーライドした場合、または変更できない引数を渡した場合、プロシージャは基になる要素を変更できません。 これにより、呼び出し元のコードで予期しない結果が生じる可能性があります。  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 プロシージャが呼び出し元のコードの引数の基になる値を変更できるようにすると、常にリスクが発生する可能性があります。 この値が変更されることが予想されることを確認し、使用前に有効かどうかを確認できるように準備してください。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic におけるプロシージャ](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [変更できる引数と変更できない引数の違い](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [引数の値渡しと参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [方法: プロシージャ引数の値を変更する](./how-to-change-the-value-of-a-procedure-argument.md)
- [方法: プロシージャ引数の値が変化しないようにする](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
