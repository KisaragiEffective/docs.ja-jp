---
title: '方法 : プロパティを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- Visual Basic code, properties
- properties [Visual Basic]
ms.assetid: 4d229712-6be8-4c5c-bac5-06995ce9185a
ms.openlocfilehash: ee5a9f687765ce064eb3c3f84218ed36eb916d9d
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349710"
---
# <a name="how-to-create-a-property-visual-basic"></a>方法: プロパティを作成する (Visual Basic)
`Property` ステートメントと `End Property` ステートメントの間でプロパティ定義を囲みます。 この定義では、`Get` プロシージャ、`Set` プロシージャ、またはその両方を定義します。 すべてのプロパティのコードは、これらのプロシージャの中にあります。  
  
 `Get` プロシージャはプロパティの値を取得し、`Set` プロシージャは値を格納します。 プロパティに読み取り/書き込みアクセスを許可する場合は、両方のプロシージャを定義する必要があります。 読み取り専用プロパティの場合は、`Get`だけを定義し、書き込み専用プロパティについては `Set`のみを定義します。  
  
### <a name="to-create-a-property"></a>プロパティを作成するには  
  
1. プロパティまたはプロシージャの外部では、 [Property ステートメント](../../../../visual-basic/language-reference/statements/property-statement.md)を使用し、その後に `End Property` ステートメントを使用します。  
  
2. プロパティがパラメーターを受け取る場合は、`Property` キーワードの後にプロシージャの名前を入力し、次にかっこで囲んでパラメーターリストを指定します。  
  
3. かっこの後に `As` 句を入力して、プロパティの値のデータ型を指定します。 書き込み専用プロパティについても、データ型を指定する必要があります。  
  
4. 必要に応じて、`Get` および `Set` プロシージャを追加します。 次の手順を参照してください。  
  
### <a name="to-create-a-get-procedure-that-retrieves-a-property-value"></a>プロパティ値を取得する Get プロシージャを作成するには  
  
1. `Property` ステートメントと `End Property` ステートメントの間に、 [Get ステートメント](../../../../visual-basic/language-reference/statements/get-statement.md)を記述し、その後に `End Get` ステートメントを記述します。 `Get` プロシージャのパラメーターを定義する必要はありません。  
  
2. `Get` と `End Get` ステートメントの間でプロパティの値を取得するコードステートメントを配置します。 このコードには、プロパティの値を生成して返すだけでなく、他の計算やデータ操作を含めることができます。  
  
3. `Return` ステートメントを使用して、呼び出し元のコードにプロパティの値を返します。  
  
 読み取り/書き込みプロパティと読み取り専用プロパティの `Get` プロシージャを記述する必要があります。 書き込み専用プロパティの `Get` プロシージャを定義することはできません。  
  
### <a name="to-create-a-set-procedure-that-writes-a-propertys-value"></a>プロパティの値を書き込む Set プロシージャを作成するには  
  
1. `Property` ステートメントと `End Property` ステートメントの間に、 [Set ステートメント](../../../../visual-basic/language-reference/statements/set-statement.md)を記述し、その後に `End Set` ステートメントを記述します。  
  
2. `Set` ステートメントで、`Set` キーワードの後にかっこで囲んだパラメーターリストを指定します。 このパラメーターリストには、呼び出し元のコードで渡される値の値パラメーターを少なくとも1つ含める必要があります。 この値パラメーターの既定の名前は `Value`ですが、必要に応じて別の名前を使用することもできます。 値パラメーターは、プロパティ自体と同じデータ型である必要があります。  
  
3. コードステートメントを配置して、`Set` と `End Set` ステートメントの間のプロパティに値を格納します。 このコードには、プロパティの値の検証と格納に加えて、他の計算とデータ操作を含めることができます。  
  
4. 値パラメーターを使用して、呼び出し元のコードによって指定された値を受け入れます。 この値は、代入ステートメントに直接格納するか、式で使用して格納される内部値を計算することができます。  
  
 読み取り/書き込みプロパティと書き込み専用プロパティの `Set` プロシージャを記述する必要があります。 読み取り専用プロパティの `Set` プロシージャを定義することはできません。  
  
## <a name="example"></a>例  
 次の例では、完全名を2つの構成名、名、姓を格納する読み取り/書き込みプロパティを作成します。 呼び出し元のコードが `fullName`を読み取る場合、`Get` プロシージャは2つの構成名を結合し、完全な名前を返します。 呼び出し元のコードによって新しい完全名が割り当てられると、`Set` プロシージャは2つの構成名に分割しようとします。 スペースが見つからない場合は、そのすべてが最初の名前として格納されます。  
  
 [!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]  
  
 次の例は、`fullName`のプロパティプロシージャの一般的な呼び出しを示しています。 最初の呼び出しでは、プロパティ値を設定し、2番目の呼び出しでそれを取得します。  
  
 [!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>参照

- [Visual Basic におけるプロシージャ](./index.md)
- [Property プロシージャ](./property-procedures.md)
- [プロシージャのパラメーターと引数](./procedure-parameters-and-arguments.md)
- [Visual Basic のプロパティと変数の違い](./differences-between-properties-and-variables.md)
- [方法 : 複数のアクセス レベルを持つプロパティを宣言する](./how-to-declare-a-property-with-mixed-access-levels.md)
- [方法 : プロパティ プロシージャを呼び出す](./how-to-call-a-property-procedure.md)
- [方法: Visual Basic で既定のプロパティを宣言して呼び出す](./how-to-declare-and-call-a-default-property.md)
- [方法 : プロパティに値を格納する](./how-to-put-a-value-in-a-property.md)
- [方法 : プロパティから値を取得する](./how-to-get-a-value-from-a-property.md)
