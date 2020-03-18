---
title: '#Region ディレクティブ'
ms.date: 07/20/2015
f1_keywords:
- vb.Region
- vb.#Region
helpviewer_keywords:
- Visual Basic compiler, compiler directives
- '#region directive'
- region directive (#region)
- '#Region keyword [Visual Basic]'
ms.assetid: 90a6a104-3cbf-47d0-bdc4-b585d0921b87
ms.openlocfilehash: 4cf9b103486378d001b588aa285f590980b51bb8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343789"
---
# <a name="region-directive"></a>#Region ディレクティブ

Visual Basic ファイルのコードのセクションを折りたたんで非表示にします。  
  
## <a name="syntax"></a>構文  

```vb
#Region "identifier_string"  
#End Region  
```  
  
## <a name="parts"></a>指定項目  
  
|用語|Definition|  
|---|---|  
|`identifier_string`|必須。 領域が折りたたまれたときにその領域のタイトルとして機能する文字列です。 既定では、領域は折りたたまれています。|  
|`#End Region`|`#Region` ブロックを終了します。|  
  
## <a name="remarks"></a>コメント  

 Visual Studio Code エディターのアウトライン機能を使うときに展開または折りたたみの対象となるコード ブロックを指定するには、`#Region` ディレクティブを使用します。 他の領域内の領域を配置または*入れ子*にして、類似した領域をまとめてグループ化することができます。  
  
## <a name="example"></a>例  

 `#Region` ディレクティブの使用例を次に示します。  
  
 [!code-vb[VbVbalrConditionalComp#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#4)]  
  
## <a name="see-also"></a>参照

- [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)
- [アウトライン](/visualstudio/ide/outlining)
- [方法 : コードのセクションを折りたたんで非表示にする](../../../visual-basic/programming-guide/program-structure/how-to-collapse-and-hide-sections-of-code.md)
