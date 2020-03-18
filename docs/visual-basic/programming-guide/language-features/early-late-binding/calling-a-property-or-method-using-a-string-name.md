---
title: 文字列名によるプロパティまたはメソッドの呼び出し
ms.date: 07/20/2015
helpviewer_keywords:
- passing operators [Visual Basic]
- strings [Visual Basic], passing new operators as
- objects [Visual Basic], setting properties
- setting properties, object properties at run time
- method calls [Visual Basic], strings
- methods [Visual Basic], calling with string names
- calling methods [Visual Basic], string names
- properties [Visual Basic], setting at run time
- CallByName function
ms.assetid: 79a7b8b4-b8c7-4ad8-aca8-12a9a2b32f03
ms.openlocfilehash: cb584f0dfbd905ca071f9a86b1eab231f3017538
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345209"
---
# <a name="calling-a-property-or-method-using-a-string-name-visual-basic"></a>文字列名によるプロパティまたはメソッドの呼び出し (Visual Basic)
ほとんどの場合、オブジェクトのプロパティとメソッドをデザイン時に検出し、それらを処理するコードを記述できます。 ただし、場合によっては、オブジェクトのプロパティやメソッドを事前に把握していない場合や、エンドユーザーが実行時にプロパティを指定したりメソッドを実行したりできるようにすることが必要になる場合もあります。  
  
## <a name="callbyname-function"></a>CallByName 関数  
 たとえば、演算子を COM コンポーネントに渡すことによってユーザーによって入力された式を評価するクライアントアプリケーションなどを考えてみます。 新しい演算子を必要とする新しい関数をコンポーネントに常に追加するとします。 標準のオブジェクトアクセス手法を使用する場合は、新しい演算子を使用する前に、クライアントアプリケーションを再コンパイルして再配布する必要があります。 これを回避するには、`CallByName` 関数を使用して、アプリケーションを変更せずに、新しい演算子を文字列として渡すことができます。  
  
 `CallByName` 関数を使用すると、実行時に文字列を使用してプロパティまたはメソッドを指定できます。 `CallByName` 関数のシグネチャは次のようになります。  
  
 *結果* = `CallByName`(*Object*、 *ProcedureName*、 *CallType*、 *Arguments*())  
  
 最初の引数*object*は、操作対象のオブジェクトの名前を受け取ります。 *ProcedureName*引数は、呼び出されるメソッドまたはプロパティプロシージャの名前を含む文字列を受け取ります。 *CallType*引数は、呼び出すプロシージャの種類を表す定数を受け取ります。メソッド (`Microsoft.VisualBasic.CallType.Method`)、読み取りプロパティ (`Microsoft.VisualBasic.CallType.Get`)、またはプロパティセット (`Microsoft.VisualBasic.CallType.Set`) です。 *Arguments*引数 (省略可能) は、プロシージャの引数を含む `Object` 型の配列を受け取ります。  
  
 現在のソリューションではクラスと共に `CallByName` を使用できますが、ほとんどの場合、.NET Framework アセンブリから COM オブジェクトまたはオブジェクトにアクセスするために使用されます。  
  
 次のコードに示すように、`MathClass`という名前のクラスを含むアセンブリへの参照を追加するとします。このクラスには、`SquareRoot`という名前の新しい関数があります。  
  
 [!code-vb[VbVbalrOOP#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#53)]  
  
 アプリケーションでは、テキストボックスコントロールを使用して、呼び出すメソッドとその引数を制御できます。 たとえば、評価する式が `TextBox1` に含まれていて、`TextBox2` を使用して関数の名前を入力した場合は、次のコードを使用して `TextBox1`の式で `SquareRoot` 関数を呼び出すことができます。  
  
 [!code-vb[VbVbalrOOP#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#54)]  
  
 `TextBox2`の `TextBox1`"SquareRoot" に「64」と入力し、`CallMath` プロシージャを呼び出すと、`TextBox1` 内の数値の平方根が評価されます。 この例のコードでは、`SquareRoot` 関数 (必須の引数として評価される式を含む文字列を受け取ります) を呼び出し、`TextBox1` (64 の平方根) で "8" を返します。 もちろん、ユーザーが `TextBox2`に無効な文字列を入力した場合、文字列にメソッドではなくプロパティの名前が含まれている場合、またはメソッドに追加の必須引数がある場合は、実行時エラーが発生します。 `CallByName` を使用してこれらのエラーやその他のエラーを予測する場合は、堅牢なエラー処理コードを追加する必要があります。  
  
> [!NOTE]
> `CallByName` 関数は役に立つ場合がありますが、パフォーマンスへの影響を考慮する必要があります。つまり、`CallByName` を使用してプロシージャを呼び出すと、遅延バインディングの呼び出しより少し遅くなります。 ループの内部など、繰り返し呼び出される関数を呼び出す場合、`CallByName` はパフォーマンスに重大な影響を与える可能性があります。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Interaction.CallByName%2A>
- [オブジェクトの型の決定](../../../../visual-basic/programming-guide/language-features/early-late-binding/determining-object-type.md)
