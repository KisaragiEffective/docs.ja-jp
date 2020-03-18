---
title: MyBase
ms.date: 07/20/2015
f1_keywords:
- vb.MustOverride
- MustOverride
helpviewer_keywords:
- virtual elements [Visual Basic], pure
- elements [Visual Basic], pure virtual
- properties [Visual Basic], redefining
- procedures [Visual Basic], overriding
- overriding, MustOverride keyword
- procedures [Visual Basic], redefining
- pure virtual elements [Visual Basic]
- MustOverride keyword [Visual Basic]
- properties [Visual Basic], overriding
ms.assetid: 6e9d9ad6-bb64-433f-b32b-3ef84293bf96
ms.openlocfilehash: dc6a153a604fd0e5cee9d7d46ebcd63294f33628
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351492"
---
# <a name="mustoverride-visual-basic"></a>MustOverride (Visual Basic)
プロパティまたはプロシージャがこのクラスで実装されておらず、派生クラスでオーバーライドされてから使用できるようにする必要があることを指定します。  
  
## <a name="remarks"></a>コメント  
 `MustOverride` は、プロパティまたはプロシージャの宣言ステートメントでのみ使用できます。 `MustOverride` を指定するプロパティまたはプロシージャは、クラスのメンバーである必要があります。また、クラスは[MustInherit](../../../visual-basic/language-reference/modifiers/mustinherit.md)に設定する必要があります。  
  
## <a name="rules"></a>ルール  
  
- **不完全な宣言です。** `MustOverride`を指定すると、`End Function`、`End Property`、または `End Sub` ステートメントではなく、プロパティまたはプロシージャに追加のコード行を指定することはできません。  
  
- **結合された修飾子。** 同じ宣言内で `NotOverridable`、`Overridable`、または `Shared` と共に `MustOverride` を指定することはできません。  
  
- **シャドウとオーバーライド。** シャドウとオーバーライドは、どちらも継承された要素を再定義しますが、その方法は大きく異なります。 詳細については、「 [Visual Basic でのシャドウ](../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)」を参照してください。  
  
- **代替語句。** オーバーライドでは使用できない要素は、*純粋仮想*要素と呼ばれることもあります。  
  
 `MustOverride` 修飾子は、次のコンテキストで使用できます。  
  
 [Function ステートメント](../../../visual-basic/language-reference/statements/function-statement.md)  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
 [Sub ステートメント](../../../visual-basic/language-reference/statements/sub-statement.md)  
  
## <a name="see-also"></a>関連項目

- [NotOverridable](../../../visual-basic/language-reference/modifiers/notoverridable.md)
- [Overridable](../../../visual-basic/language-reference/modifiers/overridable.md)
- [Overrides](../../../visual-basic/language-reference/modifiers/overrides.md)
- [MustInherit](../../../visual-basic/language-reference/modifiers/mustinherit.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
- [Visual Basic でのシャドウ処理](../../../visual-basic/programming-guide/language-features/declared-elements/shadowing.md)
