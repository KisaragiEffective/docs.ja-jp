---
title: プロパティ '<propertyname>' は、すべてのコードのパスでは値を返しません。
ms.date: 07/20/2015
f1_keywords:
- bc42107
- vbc42107
helpviewer_keywords:
- BC42107
ms.assetid: 06800966-9c3b-4844-9f13-83ac95607d32
ms.openlocfilehash: a5cb28a024274e58da1755b437d6ba4ca6712610
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2019
ms.locfileid: "64661711"
---
# <a name="property-propertyname-doesnt-return-a-value-on-all-code-paths"></a>プロパティ '\<propertyname >' は、すべてのコード パスで値を返しません
プロパティ '\<propertyname >' は、すべてのコード パスで値を返しません。 この結果が使用されると、実行時に Null 参照例外が生じる可能性があります。  
  
 プロパティ`Get`手順では、値を返さないコードの少なくとも 1 つの可能なパス。  
  
 プロパティから値を返すことができます`Get`次の方法のいずれかの手順。  
  
- プロパティ名に値を代入し、実行、`Exit Property`ステートメント。  
  
- プロパティ名に値を代入し、実行、`End Get`ステートメント。  
  
- 値を含める、 [Return ステートメント](../../../visual-basic/language-reference/statements/return-statement.md)します。  
  
 制御が`Exit Property`または`End Get`に渡り、プロパティ名に値が割り当てられていないと、`Get`プロシージャはプロパティのデータ型の既定値を返します。 詳細については、[Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)の「動作」を参照してください。  
  
 既定では、このメッセージは警告です。 警告を非表示にする方法や、警告をエラーとして扱う方法の詳細については、「[Configuring Warnings in Visual Basic](/visualstudio/ide/configuring-warnings-in-visual-basic)」を参照してください。  
  
 **エラー ID:** BC42107  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 制御フローのロジックを確認し、すべてが値を返すステートメントの前に値を代入するかどうかを確認します。  
  
     常に使用する場合に、プロシージャからリターンごとに値を返すことを保証する方が簡単、`Return`ステートメント。 この場合、最後のステートメントの前に`End Get`する必要があります、`Return`ステートメント。  
  
## <a name="see-also"></a>関連項目

- [Property プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/property-procedures.md)
- [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)
- [Get ステートメント](../../../visual-basic/language-reference/statements/get-statement.md)
