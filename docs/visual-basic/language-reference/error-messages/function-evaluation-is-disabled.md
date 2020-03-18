---
title: 前の関数の評価がタイムアウトしたため、関数の評価は無効になりました。
ms.date: 07/20/2015
f1_keywords:
- bc30957
- vbc30957
helpviewer_keywords:
- BC30957
ms.assetid: 561e593a-f50a-4b72-a708-4cab60ec7b28
ms.openlocfilehash: d004c89b742944622ce45e6a2be8d96116252745
ms.sourcegitcommit: 5a28f8eb071fcc09b045b0c4ae4b96898673192e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/31/2019
ms.locfileid: "73197568"
---
# <a name="function-evaluation-is-disabled-because-a-previous-function-evaluation-timed-out"></a>前の関数の評価がタイムアウトしたため、関数の評価は無効になりました。
以前の関数の評価がタイムアウトしたため、関数の評価が無効になっています。関数の評価を再度有効にするには、ステップを再度実行するか、デバッグを再起動してください。  
  
 Visual Studio デバッガーで、プロシージャ呼び出しが式に指定されていますが、別の評価がタイムアウトしています。  
  
 プロシージャ呼び出しでタイムアウトが発生する原因として、無限ループまたは*無限ループ*が考えられます。 詳細については、「」を参照して[ください。次のステートメント](../../../visual-basic/language-reference/statements/for-next-statement.md)。  
  
 無限ループの特殊なケースは*再帰*的です。 詳細については、「[再帰プロシージャ](../../../visual-basic/programming-guide/language-features/procedures/recursive-procedures.md)」を参照してください。  
  
 **エラー ID:** BC30957  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
1. 可能であれば、以前の関数の評価がどのようなものか、およびそれがタイムアウトになった原因を特定します。それ以外の場合は、このエラーが再度発生する可能性があります。  
  
2. デバッガーのステップをもう一度実行するか、デバッガーを終了させて再起動します。  
  
## <a name="see-also"></a>関連項目

- [Visual Studio でのデバッグ](/visualstudio/debugger/debugger-feature-tour)
- [デバッガーでのコード間の移動](/visualstudio/debugger/navigating-through-code-with-the-debugger)
