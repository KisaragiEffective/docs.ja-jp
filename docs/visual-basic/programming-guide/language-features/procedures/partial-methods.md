---
title: 部分メソッド
ms.date: 07/20/2015
f1_keywords:
- vb.PartialMethod
- PartialMethod
helpviewer_keywords:
- custom logic into code [Visual Basic]
- partial methods [Visual Basic]
- partial [Visual Basic], methods [Visual Basic]
- methods [Visual Basic], partial methods
- inserting custom logic into code
ms.assetid: 74b3368b-b348-44a0-a326-7d7dc646f4e9
ms.openlocfilehash: 7abf0565a985f1fb44fcf2bb91b9220d57a10f20
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352636"
---
# <a name="partial-methods-visual-basic"></a>部分メソッド (Visual Basic)
部分メソッドを使用すると、開発者はカスタムロジックをコードに挿入できます。 通常、このコードはデザイナーによって生成されるクラスの一部です。 部分メソッドは、コードジェネレーターによって作成される部分クラスで定義され、通常は、何らかの変更が行われたことを通知するために使用されます。 これにより、開発者は、変更に応じてカスタム動作を指定できます。  
  
 コードジェネレーターのデザイナーは、メソッドシグネチャと、メソッドの1つ以上の呼び出しのみを定義します。 開発者は、生成されたコードの動作をカスタマイズする場合に、メソッドの実装を提供できます。 実装が指定されていない場合、メソッドの呼び出しはコンパイラによって削除され、その結果、追加のパフォーマンスオーバーヘッドが発生しません。  
  
## <a name="declaration"></a>宣言  
 生成されたコードは、署名行の先頭に `Partial` キーワードを配置することで、部分メソッドの定義をマークします。  
  
```vb  
Partial Private Sub QuantityChanged()  
End Sub  
```  
  
 定義は、次の条件を満たしている必要があります。  
  
- メソッドは、`Function`ではなく、`Sub`である必要があります。  
  
- メソッドの本体は空のままにする必要があります。  
  
- アクセス修飾子は `Private`である必要があります。  
  
## <a name="implementation"></a>実装  
 実装は、主に部分メソッドの本体を埋めることによって構成されます。 実装は、通常、定義とは別の部分クラスにあり、生成されたコードを拡張する開発者によって記述されます。  
  
```vb  
Private Sub QuantityChanged()  
'    Code for executing the desired action.  
End Sub  
```  
  
 前の例では、宣言のシグネチャを正確に複製しますが、バリエーションを使用することもできます。 特に、`Overloads` や `Overrides`などの他の修飾子を追加できます。 1つの `Overrides` 修飾子のみが許可されます。 メソッド修飾子の詳細については、「 [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)」を参照してください。  
  
## <a name="use"></a>新しく使用する機能  
 部分メソッドは、他の `Sub` プロシージャと同じように呼び出すことができます。 メソッドが実装されている場合は、引数が評価され、メソッドの本体が実行されます。 ただし、部分メソッドの実装は省略可能であることに注意してください。 メソッドが実装されていない場合、その呼び出しは無効であり、メソッドに引数として渡された式は評価されません。  
  
## <a name="example"></a>例  
 .Vb という名前のファイルで、`Quantity` プロパティを持つ `Product` クラスを定義します。  
  
 [!code-vb[VbVbalrPartialMeths#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#4)]  
  
 Product .vb という名前のファイルに、`QuantityChanged`の実装を指定します。  
  
 [!code-vb[VbVbalrPartialMeths#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#5)]  
  
 最後に、プロジェクトの Main メソッドで、`Product` インスタンスを宣言し、その `Quantity` プロパティの初期値を指定します。  
  
 [!code-vb[VbVbalrPartialMeths#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrPartialMeths/VB/Class1.vb#6)]  
  
 メッセージボックスが表示され、次のメッセージが表示されます。  
  
 `Quantity was changed to 100`  
  
## <a name="see-also"></a>関連項目

- [Sub ステートメント](../../../../visual-basic/language-reference/statements/sub-statement.md)
- [Sub プロシージャ](./sub-procedures.md)
- [省略可能なパラメーター](./optional-parameters.md)
- [Partial](../../../../visual-basic/language-reference/modifiers/partial.md)
- [LINQ to SQL でのコード生成](../../../../framework/data/adonet/sql/linq/code-generation-in-linq-to-sql.md)
- [部分メソッドによるビジネス ロジックの追加](../../../../framework/data/adonet/sql/linq/adding-business-logic-by-using-partial-methods.md)
