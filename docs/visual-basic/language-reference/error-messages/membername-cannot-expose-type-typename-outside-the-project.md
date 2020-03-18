---
title: "'<membername>' は、型 '<typename>' を <containertype>'<containertypename>' 経由でプロジェクトの外側に公開できません。"
ms.date: 07/20/2015
f1_keywords:
- bc30909
- vbc30909
helpviewer_keywords:
- BC30909
ms.assetid: ffa7395d-e182-4087-8ce8-079810fdae54
ms.openlocfilehash: ca67e74d7790352bd1842cb8a59fe1525af6e18c
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71700897"
---
# <a name="membername-cannot-expose-type-typename-outside-the-project-through-containertype-containertypename"></a>'\<membername>' は、型 '\<typename>' を \<containertype>'\<containertypename>' 経由でプロジェクトの外側に公開できません。
変数、プロシージャパラメーター、または関数の戻り値は、コンテナーの外部に公開されますが、コンテナーの外部に公開されてはならない型として宣言されています。  
  
 次のスケルトンコードは、このエラーを生成する状況を示しています。  
  
```vb  
Private Class privateClass  
End Class  
Public Class mainClass  
    Public exposedVar As New privateClass  
End Class  
```  
  
 @No__t-0、`Friend`、`Protected Friend`、または `Private` として宣言された型は、宣言コンテキストの外部でアクセスが制限されることを意図しています。 アクセス制限が低い変数のデータ型として使用すると、この目的が損なわれます。 前のスケルトンコードでは、`exposedVar` は `Public` であり、@no__t にアクセスできないようにするコードには、-2 を公開します。  
  
 **エラー ID:** BC30909  
  
## <a name="to-correct-this-error"></a>このエラーを解決するには  
  
- 変数、プロシージャパラメーター、または関数のアクセスレベルを、少なくともそのデータ型のアクセスレベルと同じ制限以上になるように変更します。  
  
## <a name="see-also"></a>関連項目

- [Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)
