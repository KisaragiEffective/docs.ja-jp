---
title: 関数 '<procedurename>' すべてのコード パス上では値を返しません。
ms.date: 07/20/2015
f1_keywords:
- bc42105
- vbc42105
helpviewer_keywords:
- BC42105
ms.assetid: b6929bf4-a365-4a70-8dc9-6b0fc09e1468
ms.openlocfilehash: 5564f95048f6b44a48229c7e5be9331839803439
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2019
ms.locfileid: "64662100"
---
# <a name="function-procedurename-doesnt-return-a-value-on-all-code-paths"></a>関数 '\<procedurename >' は、すべてのコード パスで値を返しません
関数 '\<procedurename >' は、すべてのコード パスで値を返しません。 'Return' ステートメントが見当たりませんか。  
  
 A`Function`手順では、値を返さないコードの少なくとも 1 つの可能なパス。  
  
 値を返すことができます、`Function`次の方法のいずれかの手順。  
  
- 値を含める、 [Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)します。  
  
- 値を代入します、`Function`プロシージャ名前を指定し、実行、`Exit Function`ステートメント。  
  
- 値を代入します、`Function`プロシージャ名前を指定し、実行、`End Function`ステートメント。  
  
 制御が`Exit Function`または`End Function`に渡り、プロシージャ名に任意の値が割り当てられていないと、戻り値のデータ型の既定値を返します。 詳細については、[Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md) の「動作」を参照してください。  
  
 既定では、このメッセージは警告です。 警告を非表示にする方法や、警告をエラーとして扱う方法の詳細については、「[Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC42105  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 制御フローのロジックを確認し、すべてが値を返すステートメントの前に値を代入するかどうかを確認します。  
  
     常に使用する場合に、プロシージャからリターンごとに値を返すことを保証する方が簡単、`Return`ステートメント。 この場合、最後のステートメントの前に`End Function`する必要があります、`Return`ステートメント。  
  
## <a name="see-also"></a>関連項目

- [Function プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/function-procedures.md)
- [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)
- [[コンパイル] ページ、プロジェクト デザイナー (Visual Basic)](/visualstudio/ide/reference/compile-page-project-designer-visual-basic)
