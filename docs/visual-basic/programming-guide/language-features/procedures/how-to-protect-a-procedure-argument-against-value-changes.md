---
title: '方法 : プロシージャ引数の値が変化しないようにする'
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
- procedures [Visual Basic], calling
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: d2b7c766-ce16-4d2c-8d79-3fc0e7ba2227
ms.openlocfilehash: 75c718c83f36e2f0b2c4cfb5504c2d740eaa3520
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347898"
---
# <a name="how-to-protect-a-procedure-argument-against-value-changes-visual-basic"></a>方法: プロシージャ引数の値が変化しないようにする (Visual Basic)
プロシージャがパラメーターを[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)として宣言している場合、Visual Basic は、呼び出し元のコードの引数の基になるプログラミング要素への直接参照をプロシージャコードに付与します。 これにより、プロシージャは、呼び出し元のコードの引数の基になる値を変更できます。 場合によっては、呼び出し元のコードでこのような変更から保護することが必要になることがあります。  
  
 プロシージャ内で対応するパラメーター [ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)を宣言することによって、変更から引数を常に保護することができます。 特定の引数を他のケース以外でも変更できるようにする場合は、その引数を `ByRef` 宣言し、呼び出し元のコードが各呼び出しで渡される機構を決定できるようにします。 これを行うには、対応する引数をかっこで囲み、値で渡します。または、かっこで囲まないようにして、参照渡しで渡すことができます。 詳細については、「[方法: 引数を強制的に値で渡す](./how-to-force-an-argument-to-be-passed-by-value.md)」を参照してください。  
  
## <a name="example"></a>使用例  
 次の例は、配列変数を受け取り、その要素を操作する2つのプロシージャを示しています。 `increase` の手順では、単に各要素に1つを追加します。 `replace` プロシージャは、新しい配列をパラメーター `a()` に割り当て、各要素に1つを追加します。 ただし、再割り当ては、呼び出し元のコード内の基になる配列変数には影響しません。  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#38)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 最初の `MsgBox` の呼び出しでは、"後に増加する (n):11, 21, 31, 41" と表示されます。 配列 `n` は参照型であるため、渡される機構が `ByVal`場合でも、`increase` はそのメンバーを変更できます。  
  
 2番目の `MsgBox` 呼び出しでは、"After replace (n):11, 21, 31, 41" と表示されます。 `n` は `ByVal`渡されるので、`replace` 新しい配列を割り当てることによって、呼び出し元のコードの変数 `n` を変更することはできません。 `replace` によって新しい配列インスタンス `k` 作成され、それがローカル変数 `a`に代入されると、呼び出し元のコードによって渡された `n` への参照が失われます。 `a`のメンバーを変更すると、ローカル配列 `k` のみが影響を受けます。 したがって、`replace` では、呼び出し元のコードの配列 `n` の値はインクリメントされません。  
  
## <a name="compile-the-code"></a>コードのコンパイル  
 Visual Basic の既定では、値渡しで引数を渡します。 ただし、すべての宣言されたパラメーターに[ByVal](../../../../visual-basic/language-reference/modifiers/byval.md)キーワードまたは[ByRef](../../../../visual-basic/language-reference/modifiers/byref.md)キーワードを含めることをお勧めします。 これにより、コードが読みやすくなります。  
  
## <a name="see-also"></a>関連項目

- [手順](./index.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [方法: プロシージャに引数を渡す](./how-to-pass-arguments-to-a-procedure.md)
- [引数の値渡しと参照渡し](./passing-arguments-by-value-and-by-reference.md)
- [変更できる引数と変更できない引数の違い](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [引数の値渡しと参照渡しの違い](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [方法: プロシージャ引数の値を変更する](./how-to-change-the-value-of-a-procedure-argument.md)
- [方法: 引数の値渡しを強制する](./how-to-force-an-argument-to-be-passed-by-value.md)
- [位置と名前による引数渡し](./passing-arguments-by-position-and-by-name.md)
- [Value Types and Reference Types](../../../../visual-basic/programming-guide/language-features/data-types/value-types-and-reference-types.md)
