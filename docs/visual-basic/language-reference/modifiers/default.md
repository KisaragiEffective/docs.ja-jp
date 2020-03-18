---
title: Default (Visual Basic)
ms.date: 07/20/2015
f1_keywords:
- vb.Default
helpviewer_keywords:
- defaults, properties
- properties [Visual Basic], default
- default properties, in Visual Basic
- Default keyword [Visual Basic]
- default properties
ms.assetid: 45fce9b9-d212-4b2d-ab86-6e359b8b57af
ms.openlocfilehash: ad4c9528f208cc2c31f07b0506d1b049a7568c86
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351577"
---
# <a name="default-visual-basic"></a>Default (Visual Basic)
クラス、構造体、またはインターフェイスの既定のプロパティとしてプロパティを識別します。  
  
## <a name="remarks"></a>コメント  
 クラス、構造体、またはインターフェイスは、そのプロパティが少なくとも1つのパラメーターを受け取ることができる限り、そのプロパティを*既定のプロパティ*として1つだけ指定できます。 コードがメンバーを指定せずにクラスまたは構造体への参照を作成した場合、Visual Basic は既定のプロパティへの参照を解決します。  
  
 既定のプロパティを使用すると、ソースコード文字が小さくなることがありますが、コードの読み取りが困難になる可能性があります。 呼び出し元のコードがクラスまたは構造体に精通していない場合、クラスまたは構造体の名前への参照を作成するときに、その参照がクラスまたは構造体自体、または既定のプロパティにアクセスするかどうかを特定できません。 これにより、コンパイラエラーまたは微妙な実行時のロジックエラーが発生する可能性があります。  
  
 [Option Strict ステートメント](../../../visual-basic/language-reference/statements/option-strict-statement.md)を常に使用してコンパイラの型チェックを `On`に設定することにより、既定のプロパティエラーが発生する可能性を多少減らすことができます。  
  
 定義済みのクラスまたは構造体をコード内で使用する予定の場合は、既定のプロパティがあるかどうかを判断し、存在する場合はその名前を確認する必要があります。  
  
 これらの欠点があるため、既定のプロパティを定義しないことを検討してください。 コードを読みやすくするために、常にすべてのプロパティを明示的に参照することも検討する必要があります。既定のプロパティも同様です。  
  
 このコンテキストでは、`Default` 修飾子を使用できます。  
  
 [Property ステートメント](../../../visual-basic/language-reference/statements/property-statement.md)  
  
## <a name="see-also"></a>関連項目

- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](../../../visual-basic/programming-guide/language-features/procedures/how-to-declare-and-call-a-default-property.md)
- [キーワード](../../../visual-basic/language-reference/keywords/index.md)
