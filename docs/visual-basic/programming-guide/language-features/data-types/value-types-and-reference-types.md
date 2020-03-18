---
title: 値型と参照型
ms.date: 07/20/2015
helpviewer_keywords:
- reference data types [Visual Basic]
- reference types [Visual Basic]
- value types [Visual Basic]
- value data types [Visual Basic]
- types [Visual Basic]
- data types [Visual Basic], value types
- data types [Visual Basic], reference types
ms.assetid: fc82ce15-5a40-4c5c-a1e1-a556830e7391
ms.openlocfilehash: 466bb5386235917705344d35c5141c8bf779218d
ms.sourcegitcommit: 1f12db2d852d05bed8c53845f0b5a57a762979c8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/18/2019
ms.locfileid: "72582642"
---
# <a name="value-types-and-reference-types"></a>値型と参照型
Visual Basic には、参照型と値型という2種類の型があります。 参照型の変数はデータ (オブジェクト) への参照を格納するのに対して、値型の変数はデータを直接格納します。 参照型の場合、2 つの変数が同じオブジェクトを参照できるため、ある変数に対する演算によって、他の変数が参照しているオブジェクトが影響を受ける可能性があります。 値型の場合、各変数にはデータの独自のコピーがあり、一方の変数に対する操作がもう一方の変数に影響を与えることはできません ([パラメーターの ByRef 修飾子](../../../language-reference/modifiers/byref.md)の場合を除く)。
  
## <a name="value-types"></a>値型  
 データ型は、独自のメモリ割り当て内にデータを保持する場合は*値型*です。 値の種類は次のとおりです。  
  
- すべての数値データ型  
  
- `Boolean`、`Char`、および `Date`  
  
- メンバーが参照型の場合でも、すべての構造体  
  
- 列挙型。基になる型は常に `SByte`、`Short`、`Integer`、`Long`、`Byte`、`UShort`、`UInteger`、または `ULong`  
  
 すべての構造体は、参照型のメンバーが含まれている場合でも、値型です。 このため、`Char` や `Integer` などの値型は .NET Framework 構造体によって実装されます。  
  
 予約済みのキーワードを使用して値型を宣言できます (`Decimal` など)。 @No__t_0 キーワードを使用して値型を初期化することもできます。 これは、型にパラメーターを受け取るコンストラクターがある場合に特に便利です。 この例として、<xref:System.Decimal.%23ctor%28System.Int32%2CSystem.Int32%2CSystem.Int32%2CSystem.Boolean%2CSystem.Byte%29> コンストラクターがあります。このコンストラクターは、指定されたパーツから新しい `Decimal` 値を構築します。  
  
## <a name="reference-types"></a>参照型  
 *参照型*は、そのデータへの参照を格納します。 参照型には、次のものがあります。  
  
- `String`  
  
- すべての配列 (要素が値型の場合でも)  
  
- クラスの型 (<xref:System.Windows.Forms.Form> など)  
  
- デリゲート  
  
 クラスは*参照型*です。 すべての配列が参照型であることに注意してください。これは、そのメンバーが値型の場合でも同様です。  
  
 参照型はすべて、基になる .NET Framework クラスを表しているため、初期化時に [New 演算子](../../../../visual-basic/language-reference/operators/new-operator.md) キーワードを使用する必要があります。 次のステートメントは、配列を初期化します。  
  
```vb  
Dim totals() As Single = New Single(8) {}  
```  
  
## <a name="elements-that-are-not-types"></a>型ではない要素  
 次のプログラミング要素は、宣言された要素のデータ型として指定できないため、型としては使用できません。  
  
- 名前空間  
  
- モジュール  
  
- イベント  
  
- プロパティとプロシージャ  
  
- 変数、定数、およびフィールド  
  
## <a name="working-with-the-object-data-type"></a>オブジェクトのデータ型の操作  
 @No__t_0 データ型の変数には、参照型または値型のいずれかを割り当てることができます。 @No__t_0 変数は、データ自体ではなく、常にデータへの参照を保持します。 ただし、`Object` 変数に値型を割り当てた場合は、それが独自のデータを保持しているかのように動作します。 詳細については、「[Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)」を参照してください。  
  
 @No__t_0 変数が参照型または値型として機能しているかどうかを確認するには、<xref:Microsoft.VisualBasic?displayProperty=nameWithType> 名前空間の <xref:Microsoft.VisualBasic.Information> クラスの <xref:Microsoft.VisualBasic.Information.IsReference%2A> メソッドに渡します。 `Object` 変数の内容が参照型を表している場合、<xref:Microsoft.VisualBasic.Information.IsReference%2A?displayProperty=nameWithType> は `True` を返します。  
  
## <a name="see-also"></a>関連項目

- [null 許容値型](../../../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md)
- [Visual Basic での型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [Structure ステートメント](../../../../visual-basic/language-reference/statements/structure-statement.md)
- [データ型の有効な使用方法](../../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
- [Object データ型](../../../../visual-basic/language-reference/data-types/object-data-type.md)
- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
