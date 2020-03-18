---
title: オブジェクト変数の値
ms.date: 07/20/2015
helpviewer_keywords:
- object variables [Visual Basic], values
- values [Visual Basic], of object variables
- data types [Visual Basic], object variable
- variables [Visual Basic], object
ms.assetid: 31555704-58a3-49f1-9a0a-6421f605664f
ms.openlocfilehash: 8b93063d2d97802b1a7fdbc93e01040ff3337753
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74351798"
---
# <a name="object-variable-values-visual-basic"></a>オブジェクト変数の値 (Visual Basic)
[Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)の変数は、任意の型のデータを参照できます。 `Object` 変数に格納した値はメモリ内の他の場所に保持されますが、変数自体はデータへのポインターを保持します。  
  
## <a name="object-classifier-functions"></a>オブジェクト分類子関数  
 Visual Basic には、次の表に示すように、`Object` 変数の参照先に関する情報を返す関数が用意されています。  
  
|関数|オブジェクト変数がを参照している場合に True を返します。|  
|--------------|---------------------------------------------------|  
|<xref:Microsoft.VisualBasic.Information.IsArray%2A>|単一の値ではなく、値の配列。|  
|<xref:Microsoft.VisualBasic.Information.IsDate%2A>|[日付データ型](../../../../visual-basic/language-reference/data-types/date-data-type.md)の値、または日付と時刻の値として解釈できる文字列|  
|<xref:Microsoft.VisualBasic.Information.IsDBNull%2A>|<xref:System.DBNull>型のオブジェクト。存在しないデータまたは存在しないデータを表します。|  
|<xref:Microsoft.VisualBasic.Information.IsError%2A>|<xref:System.Exception> から派生した例外オブジェクト|  
|<xref:Microsoft.VisualBasic.Information.IsNothing%2A>|[何も](../../../../visual-basic/language-reference/nothing.md)ありません。つまり、現在変数にオブジェクトが割り当てられていません。|  
|<xref:Microsoft.VisualBasic.Information.IsNumeric%2A>|数値、または数値として解釈できる文字列|  
|<xref:Microsoft.VisualBasic.Information.IsReference%2A>|参照型 (文字列、配列、デリゲート、またはクラス型など)|  
  
 これらの関数を使用すると、操作またはプロシージャに無効な値が送信されないようにすることができます。  
  
## <a name="typeof-operator"></a>TypeOf 演算子  
 また、 [TypeOf 演算子](../../../../visual-basic/language-reference/operators/typeof-operator.md)を使用して、オブジェクト変数が現在特定のデータ型を参照しているかどうかを判断することもできます。 オペランドの実行時の型がから派生しているか、指定された型を実装している場合、`TypeOf`...`Is` 式は `True` に評価されます。  
  
 次の例では、値型と参照型を参照するオブジェクト変数に対して `TypeOf` を使用します。  
  
```vb  
' The following statement puts a value type (Integer) in an Object variable.  
Dim num As Object = 10  
' The following statement puts a reference type (Form) in an Object variable.  
Dim frm As Object = New Form()  
If TypeOf num Is Long Then Debug.WriteLine("num is Long")  
If TypeOf num Is Integer Then Debug.WriteLine("num is Integer")  
If TypeOf num Is Short Then Debug.WriteLine("num is Short")  
If TypeOf num Is Object Then Debug.WriteLine("num is Object")  
If TypeOf frm Is Form Then Debug.WriteLine("frm is Form")  
If TypeOf frm Is Label Then Debug.WriteLine("frm is Label")  
If TypeOf frm Is Object Then Debug.WriteLine("frm is Object")  
```  
  
 前の例では、次の行を**デバッグ**ウィンドウに書き込みます。  
  
 `num is Integer`  
  
 `num is Object`  
  
 `frm is Form`  
  
 `frm is Object`  
  
 オブジェクト変数 `num` は `Integer`型のデータを参照し、`frm` はクラス <xref:System.Windows.Forms.Form>のオブジェクトを参照します。  
  
## <a name="object-arrays"></a>オブジェクトの配列  
 `Object` 変数の配列を宣言して使用することができます。 これは、さまざまなデータ型およびオブジェクトクラスを処理する必要がある場合に便利です。 配列内のすべての要素は、宣言されたデータ型と同じである必要があります。 このデータ型を `Object` として宣言すると、オブジェクトとクラスインスタンスを配列内の他のデータ型と共に格納できます。  
  
## <a name="see-also"></a>参照

- [オブジェクト変数](../../../../visual-basic/programming-guide/language-features/variables/object-variables.md)
- [オブジェクト変数の宣言](../../../../visual-basic/programming-guide/language-features/variables/object-variable-declaration.md)
- [オブジェクト変数の代入](../../../../visual-basic/programming-guide/language-features/variables/object-variable-assignment.md)
- [方法: オブジェクトの現在のインスタンスを参照する](../../../../visual-basic/programming-guide/language-features/variables/how-to-refer-to-the-current-instance-of-an-object.md)
- [方法: オブジェクト変数で参照している型を確認する](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-what-type-an-object-variable-refers-to.md)
- [方法: 2 つのオブジェクトが関連しているかどうかを決める](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-related.md)
- [方法: 2 つのオブジェクトが同一であるかどうか判別する](../../../../visual-basic/programming-guide/language-features/variables/how-to-determine-whether-two-objects-are-identical.md)
- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
