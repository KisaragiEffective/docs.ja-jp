---
title: '方法: プロシージャ引数の値を変更する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- arguments [Visual Basic], passing by reference
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: 6fad2368-5da7-4c07-8bf8-0f4e65a1be67
ms.openlocfilehash: deac87ca4690990a4d00f63d0ea9b843c3f9a9c4
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75344487"
---
# <a name="how-to-change-the-value-of-a-procedure-argument-visual-basic"></a>方法: プロシージャ引数の値を変更する (Visual Basic)
プロシージャを呼び出すと、指定した各引数は、プロシージャで定義されたパラメーターのいずれかに対応します。 場合によっては、プロシージャコードは、呼び出し元のコードの引数の基になる値を変更できます。 それ以外の場合、プロシージャは引数のローカルコピーだけを変更できます。  
  
 プロシージャを呼び出すと、Visual Basic によって、 [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)で渡されるすべての引数のローカルコピーが作成されます。 [ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)で渡された引数ごとに、Visual Basic は、呼び出し元のコードの引数の基になるプログラミング要素への直接参照をプロシージャコードに付与します。  
  
 呼び出し元のコード内の基になる要素が変更可能な要素であり、引数が `ByRef`渡される場合、プロシージャコードは直接参照を使用して、呼び出し元のコード内の要素の値を変更できます。  
  
## <a name="changing-the-underlying-value"></a>基になる値の変更  
  
#### <a name="to-change-the-underlying-value-of-a-procedure-argument-in-the-calling-code"></a>呼び出し元コードのプロシージャ引数の基になる値を変更するには  
  
1. プロシージャの宣言で、引数に対応するパラメーターに[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)を指定します。  
  
2. 呼び出し元のコードで、変更可能なプログラミング要素を引数として渡します。  
  
3. 呼び出し元のコードでは、引数リストのかっこで引数を囲まないでください。  
  
4. プロシージャコードでは、パラメーター名を使用して、呼び出し元のコードの基になる要素に値を割り当てます。  
  
 デモについては、「例」を参照してください。  
  
## <a name="changing-local-copies"></a>ローカルコピーの変更  
 呼び出し元のコード内の基になる要素が変更不可能な要素である場合、または引数が `ByVal`渡される場合、プロシージャは呼び出し元のコード内の値を変更できません。 ただし、このプロシージャは、このような引数のローカルコピーを変更できます。  
  
#### <a name="to-change-the-copy-of-a-procedure-argument-in-the-procedure-code"></a>プロシージャコードのプロシージャ引数のコピーを変更するには  
  
1. プロシージャの宣言で、引数に対応するパラメーターに[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)を指定します。  
  
     -または-  
  
     呼び出し元のコードの引数リストで、引数をかっこで囲みます。 これにより、対応するパラメーターで `ByRef`が指定されている場合でも、値渡しで引数を渡すことが Visual Basic されます。  
  
2. プロシージャコードでは、パラメーター名を使用して、引数のローカルコピーに値を割り当てます。 呼び出し元のコードの基になる値は変更されません。  
  
## <a name="example"></a>使用例  
 次の例は、配列変数を受け取り、その要素を操作する2つのプロシージャを示しています。 `increase` の手順では、単に各要素に1つを追加します。 `replace` プロシージャは、新しい配列をパラメーター `a()` に割り当て、各要素に1つを追加します。  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#36)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 最初の `MsgBox` の呼び出しでは、"後に増加する (n):11, 21, 31, 41" と表示されます。 配列 `n` は参照型であるため、渡される機構が `ByVal`場合でも、`replace` はそのメンバーを変更できます。  
  
 2番目の `MsgBox` 呼び出しでは、"置換後 (n): 101, 201, 301" と表示されます。 `n` は `ByRef`渡されるので、`replace` は呼び出し元のコードの変数 `n` を変更し、その変数に新しい配列を割り当てることができます。 `n` は参照型であるため、`replace` はそのメンバーを変更することもできます。  
  
 プロシージャが呼び出し元のコードで変数自体を変更しないようにすることができます。 「[方法: プロシージャ引数を値の変更に対して保護](./how-to-protect-a-procedure-argument-against-value-changes.md)する」を参照してください。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 変数を参照渡しで渡す場合は、`ByRef` キーワードを使用してこのメカニズムを指定する必要があります。  
  
 Visual Basic の既定では、値渡しで引数を渡します。 ただし、すべての宣言されたパラメーターに[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)キーワードまたは[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)キーワードを含めることをお勧めします。 これにより、コードが読みやすくなります。  
  
## <a name="net-framework-security"></a>.NET Framework セキュリティ  
 プロシージャが呼び出し元のコードの引数の基になる値を変更できるようにすると、常にリスクが発生する可能性があります。 この値が変更されることが予想されることを確認し、使用前に有効かどうかを確認できるように準備してください。  
  
## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [変更できる引数と変更できない引数の違い](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [引数の値渡しと参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [方法: プロシージャ引数の値が変化しないようにする](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [方法: 引数の値渡しを強制する](./how-to-force-an-argument-to-be-passed-by-value.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
