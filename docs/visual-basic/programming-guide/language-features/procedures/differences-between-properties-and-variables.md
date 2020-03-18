---
title: プロパティと変数の違い
ms.date: 07/20/2015
helpviewer_keywords:
- property values [Visual Basic]
- variables [Visual Basic]
- Visual Basic code, procedures
- values [Visual Basic], properties
- variables [Visual Basic], definition
- Visual Basic code, variables
- Visual Basic code, properties
- properties [Visual Basic], values
- properties [Visual Basic], defined
- variables [Visual Basic], and properties
- properties [Visual Basic], and variables
ms.assetid: 7a03a8be-5381-431f-bd7c-16e887e4e07b
ms.openlocfilehash: bbed3248840803d36607a67c8373fed15c07445f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74341222"
---
# <a name="differences-between-properties-and-variables-in-visual-basic"></a>Visual Basic のプロパティと変数の違い
変数とプロパティはどちらも、アクセスできる値を表します。 ただし、ストレージと実装には違いがあります。  
  
## <a name="variables"></a>変数:  
 *変数*は、メモリ位置に直接対応します。 1つの宣言ステートメントで変数を定義します。 変数は、プロシージャ内で定義され、そのプロシージャ内でのみ使用可能な*ローカル変数*にすることができます。また、モジュール、クラス、または構造体で定義されているが、プロシージャ内には定義されていない*メンバー変数*にすることもできます。 メンバー変数は*フィールド*とも呼ばれます。  
  
## <a name="properties"></a>[プロパティ]  
 *プロパティ*は、モジュール、クラス、または構造体で定義されたデータ要素です。 `Property` と `End Property` ステートメント間のコードブロックを使用して、プロパティを定義します。 コードブロックには、`Get` プロシージャ、`Set` プロシージャ、またはその両方が含まれています。 これらのプロシージャは、*プロパティプロシージャ*または*プロパティアクセサー*と呼ばれます。 プロパティの値を取得または格納するだけでなく、アクセスカウンターの更新などのカスタムアクションを実行することもできます。  
  
## <a name="differences"></a>[残領域]  
 変数とプロパティのいくつかの重要な違いを次の表に示します。  
  
|相違点|変数|プロパティ|  
|-------------------------|--------------|--------------|  
|宣言|単一の宣言ステートメント|コードブロック内の一連のステートメント|  
|実装|1つのストレージの場所|実行可能コード (プロパティプロシージャ)|  
|ストレージ|変数の値に直接関連付けられている|通常、内部ストレージは、プロパティの含まれているクラスまたはモジュールの外部では使用できません。<br /><br /> プロパティの値が、格納されている要素として存在しないか、存在しない可能性があります<sup>1</sup>|  
|実行可能コード|なし|少なくとも1つのプロシージャが必要です|  
|読み取りと書き込みのアクセス権|読み取り/書き込みまたは読み取り専用|読み取り/書き込み、読み取り専用、または書き込み専用|  
|カスタムアクション (値の受け入れまたは返却に加えて)|無理です|プロパティ値の設定または取得の一部として実行できます。|  
  
 <sup>1</sup>変数とは異なり、プロパティの値がストレージの1つの項目に直接対応していない場合があります。 利便性やセキュリティを高めるためにストレージが分割されている場合や、値が暗号化された形式で格納されている場合があります。 このような場合、`Get` プロシージャは、ピースをアセンブルするか、格納されている値の暗号化を解除し、`Set` プロシージャは新しい値を暗号化するか、構成ストレージに分割します。 プロパティ値は一時的なものである可能性があります。この場合、`Get` プロシージャは、プロパティにアクセスするたびに、その時刻を計算します。  
  
## <a name="see-also"></a>関連項目

- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)
- [Dim ステートメント](../../../../visual-basic/language-reference/statements/dim-statement.md)
- [方法 : プロパティを作成する](./how-to-create-a-property.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
