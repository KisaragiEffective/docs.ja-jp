---
title: ディレクティブ
ms.date: 07/20/2015
helpviewer_keywords:
- directives, Visual Basic compiler
- Visual Basic code, directives
- directives
ms.assetid: 20d5fe65-490a-4c23-88c2-ee4f490ed762
ms.openlocfilehash: b5e857198351b30c0d7a38dce1a9e6d1209b5258
ms.sourcegitcommit: a4f9b754059f0210e29ae0578363a27b9ba84b64
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/05/2019
ms.locfileid: "74838144"
---
# <a name="directives-visual-basic"></a>ディレクティブ (Visual Basic)

このセクションのトピックでは、Visual Basic ソース コードのコンパイラ ディレクティブについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  

 [#Const ディレクティブ](../../../visual-basic/language-reference/directives/const-directive.md)--コンパイラ定数を定義します。  
  
 [#ExternalSource ディレクティブ](../../../visual-basic/language-reference/directives/externalsource-directive.md)--ソース行とソース外部のテキストとの間のマッピングを示します。  
  
 [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)--選択したコードブロックをコンパイルします  
  
 [#Region ディレクティブ](../../../visual-basic/language-reference/directives/region-directive.md)--Visual Studio エディターでコードのセクションを折りたたんで非表示にします  
  
 **#Disable、#Enable** --コードの領域に対して特定の警告を有効または無効にします。  
  
```vb  
#Disable Warning BC42356 ' suppress warning about no awaits in this method  
    Async Function TestAsync() As Task  
        Console.WriteLine("testing")  
    End Function  
#Enable Warning BC42356  
```  
  
 警告コードのコンマ区切りリストを無効および有効にすることもできます。  
  
## <a name="related-sections"></a>関連セクション  

 [Visual Basic の言語リファレンス](../../../visual-basic/language-reference/index.md)  
  