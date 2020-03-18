---
title: Widening and Narrowing Conversions
ms.date: 07/20/2015
helpviewer_keywords:
- widening conversions [Visual Basic]
- narrowing conversions [Visual Basic]
- conversions [Visual Basic], type
- data types [Visual Basic], changing
- variables [Visual Basic], changing data type
- conversions [Visual Basic], exceptions during conversion
- type conversion [Visual Basic], exceptions during conversion
- conversions [Visual Basic], data type
- conversions [Visual Basic], narrowing
- type conversion [Visual Basic], narrowing
- data type conversion [Visual Basic], widening
- data type conversion [Visual Basic], narrowing
- changing data types [Visual Basic]
- type conversion [Visual Basic], widening
- data type conversion [Visual Basic], exceptions during conversion
- conversions [Visual Basic], widening
ms.assetid: 058c3152-6c28-4268-af44-2209e774f0bd
ms.openlocfilehash: 72cc1a2179db4f057c45cd2ff4de82a6437d6b58
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74348686"
---
# <a name="widening-and-narrowing-conversions-visual-basic"></a>拡大変換と縮小変換 (Visual Basic)
型変換に関する重要な考慮事項は、変換の結果が変換先のデータ型の範囲内にあるかどうかです。  
  
 *拡大変換*では、元のデータの任意の値を使用できるデータ型に値が変更されます。  拡大変換ではソース値が保持されますが、その表現を変更することができます。 このエラーは、整数型から `Decimal`に変換した場合、または `Char` から `String`に変換した場合に発生します。  
  
 *縮小変換* により、有効値の一部を保持できない可能性のあるデータ型に値が変更されます。 たとえば、小数値を整数型に変換すると丸められ、`Boolean` に変換される数値型は `True` または `False`に縮小されます。  
  
## <a name="widening-conversions"></a>拡大変換  
 次の表は、標準の拡大変換を示しています。  
  
|データ型|データ型への拡大変換<sup>1</sup>|  
|---|---|  
|[SByte](../../../../visual-basic/language-reference/data-types/sbyte-data-type.md)|`SByte`, `Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`|  
|[Byte](../../../../visual-basic/language-reference/data-types/byte-data-type.md)|`Byte`、`Short`、`UShort`、`Integer`、`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`|  
|[Short](../../../../visual-basic/language-reference/data-types/short-data-type.md)|`Short`, `Integer`, `Long`, `Decimal`, `Single`, `Double`|  
|[UShort](../../../../visual-basic/language-reference/data-types/ushort-data-type.md)|`UShort`, `Integer`, `UInteger`, `Long`, `ULong`, `Decimal`, `Single`, `Double`|  
|[Integer](../../../../visual-basic/language-reference/data-types/integer-data-type.md)|`Integer`、`Long`、`Decimal`、`Single`、`Double`<sup>2</sup>|  
|[UInteger](../../../../visual-basic/language-reference/data-types/uinteger-data-type.md)|`UInteger`、`Long`、`ULong`、`Decimal`、`Single`、`Double`<sup>2</sup>|  
|[Long](../../../../visual-basic/language-reference/data-types/long-data-type.md)|`Long`、`Decimal`、`Single`、`Double`<sup>2</sup>|  
|[ULong](../../../../visual-basic/language-reference/data-types/ulong-data-type.md)|`ULong`、`Decimal`、`Single`、`Double`<sup>2</sup>|  
|[Decimal](../../../../visual-basic/language-reference/data-types/decimal-data-type.md)|`Decimal`、`Single`、`Double`<sup>2</sup>|  
|[Single](../../../../visual-basic/language-reference/data-types/single-data-type.md)|`Single`, `Double`|  
|[Double](../../../../visual-basic/language-reference/data-types/double-data-type.md)|`Double`|  
|任意の列挙型 ([列挙](../../../../visual-basic/language-reference/statements/enum-statement.md)型)|基になる整数型と、基になる型が拡大変換される任意の型。|  
|[Char](../../../../visual-basic/language-reference/data-types/char-data-type.md)|`Char`, `String`|  
|`Char` 配列|`Char` 配列、`String`|  
|任意の型|[オブジェクト](../../../../visual-basic/language-reference/data-types/object-data-type.md)|  
|任意の派生型|派生された<sup>3</sup>の基本型。|  
|任意の型|実装する任意のインターフェイス。|  
|[Nothing](../../../../visual-basic/language-reference/nothing.md)|任意のデータ型またはオブジェクト型。|  
  
 <sup>1</sup>定義により、すべてのデータ型がそれ自体に拡大変換されます。  
  
 <sup>2</sup> `Integer`、`UInteger`、`Long`、`ULong`、または `Decimal` から `Single` または `Double` への変換では、精度は失われますが、マグニチュードが失われることはありません。 この意味では、情報の損失は発生しません。  
  
 <sup>3</sup>派生型からその基本型の1つへの変換が拡大しているように思えるかもしれません。 理由は、派生型には基本型のすべてのメンバーが含まれているため、基本型のインスタンスとして修飾されます。 逆方向では、基本型には派生型によって定義された新しいメンバーは含まれません。  
  
 拡大変換は実行時に常に成功し、データ損失は発生しません。 [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)で型チェックスイッチを `On` に設定するか `Off`に設定するかにかかわらず、常に暗黙的に実行できます。  
  
## <a name="narrowing-conversions"></a>縮小変換  
 標準的な縮小変換には、次のようなものがあります。  
  
- 前の表の拡大変換の逆方向 (すべての型がそれ自体に拡大変換される点を除く)  
  
- [Boolean データ型](../../../../visual-basic/language-reference/data-types/boolean-data-type.md)と任意の数値型の間の双方向の変換  
  
- 任意の数値型から任意の列挙型への変換 (`Enum`)  
  
- [文字列](../../../../visual-basic/language-reference/data-types/string-data-type.md)と任意の数値型、`Boolean`、または[日付](../../../../visual-basic/language-reference/data-types/date-data-type.md)の間の双方向の変換  
  
- データ型またはオブジェクト型から派生した型への変換  
  
 縮小変換は実行時に必ず成功するわけではないため、失敗したり、データ損失が発生したりする可能性があります。 変換先のデータ型が変換されている値を受け取ることができない場合、エラーが発生します。 たとえば、数値変換でオーバーフローが発生する場合があります。 [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)で型チェックスイッチが `Off`に設定されていない限り、コンパイラでは、暗黙的に縮小変換を実行することはできません。  
  
> [!NOTE]
> `For Each…Next` コレクション内の要素から loop コントロール変数への変換では、縮小変換エラーが抑制されます。 詳細と例については、「[For Each...Next ステートメント](../../../../visual-basic/language-reference/statements/for-each-next-statement.md)」の「縮小変換」セクションを参照してください。  
  
### <a name="when-to-use-narrowing-conversions"></a>縮小変換を使用する場合  
 変換元の値が変換先のデータ型に変換できることがわかっている場合は、縮小変換を使用します。エラーやデータの損失は発生しません。 たとえば、"True" または "False" のいずれかが含まれていることがわかっている `String` がある場合は、`CBool` キーワードを使用して `Boolean`に変換できます。  
  
## <a name="exceptions-during-conversion"></a>変換中の例外  
 拡大変換は常に成功するため、例外はスローされません。 縮小変換では、エラーが発生した場合、通常、次の例外がスローされます。  
  
- <xref:System.InvalidCastException> —2つの型の間に変換が定義されていない場合  
  
- 変換後の値が対象の型に対して大きすぎる場合の <xref:System.OverflowException> (整数型のみ)  
  
 クラスまたは構造体で、そのクラスまたは構造体への変換演算子として機能する[CType 関数](../../../../visual-basic/language-reference/functions/ctype-function.md)が定義されている場合、その `CType` によって、適切な例外がスローされる可能性があります。 さらに、この `CType` は Visual Basic 関数または .NET Framework メソッドを呼び出す可能性があります。これにより、さまざまな例外がスローされる可能性があります。  
  
## <a name="changes-during-reference-type-conversions"></a>参照型変換中の変更  
 *参照型*からの変換では、ポインターだけが値にコピーされます。 値自体は、どのような方法でもコピーも変更もされません。 変更できるのは、ポインターを保持している変数のデータ型だけです。 次の例では、データ型は派生クラスから基本クラスに変換されますが、両方の変数が参照するオブジェクトは変更されません。  
  
```vb  
' Assume class cSquare inherits from class cShape.  
Dim shape As cShape  
Dim square As cSquare = New cSquare  
' The following statement performs a widening  
' conversion from a derived class to its base class.  
shape = square  
```  
  
## <a name="see-also"></a>参照

- [データの種類](../../../../visual-basic/programming-guide/language-features/data-types/index.md)
- [Visual Basic における型変換](../../../../visual-basic/programming-guide/language-features/data-types/type-conversions.md)
- [暗黙の型変換と明示的な型変換](../../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [文字列とその他の型との変換](../../../../visual-basic/programming-guide/language-features/data-types/conversions-between-strings-and-other-types.md)
- [方法: Visual Basic でオブジェクトを別の型に変換する](../../../../visual-basic/programming-guide/language-features/data-types/how-to-convert-an-object-to-another-type.md)
- [配列変換](../../../../visual-basic/programming-guide/language-features/data-types/array-conversions.md)
- [データの種類](../../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../../visual-basic/language-reference/functions/type-conversion-functions.md)
