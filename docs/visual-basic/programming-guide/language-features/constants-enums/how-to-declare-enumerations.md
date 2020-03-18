---
title: '方法: 列挙型を宣言する'
ms.date: 07/20/2015
helpviewer_keywords:
- declarations [Visual Basic], enumerations
- enumerations [Visual Basic], declaring
- declaring enumerations [Visual Basic]
ms.assetid: db4ca1c3-f429-4c81-ae81-29e0157b29fd
ms.openlocfilehash: 042aea045313bcaf3832274acf1000f87a084b72
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354043"
---
# <a name="how-to-declare-enumerations-visual-basic"></a>方法: 列挙型を宣言する (Visual Basic)
列挙体を作成するには、クラスまたはモジュールの宣言セクションにある `Enum` ステートメントを使用します。 メソッド内で列挙を宣言することはできません。 適切なアクセスレベルを指定するには、`Private`、`Protected`、`Friend`、または `Public`を使用します。  
  
 `Enum` 型には、名前、基になる型、および一連のフィールドがあり、それぞれが定数を表します。 名前は、有効な Visual Basic .NET 修飾子である必要があります。 基になる型は、整数型 (`Byte`、`Short`、`Long`、または `Integer`のいずれかである必要があります。 既定値は `Integer` です。 列挙は常に厳密に型指定され、整数の数値型とは交換できません。  
  
 列挙体に浮動小数点値を指定することはできません。 列挙に `Option Strict On`を持つ浮動小数点値が割り当てられている場合、コンパイラエラーが発生します。 `Option Strict` が `Off`場合、値は `Enum` の型に自動的に変換されます。  
  
 名前について、および `Imports` ステートメントを使用して名前の修飾を不要にする方法については、「[列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)」を参照してください。  
  
### <a name="to-declare-an-enumeration"></a>列挙体を宣言するには  
  
1. 次の例に示すように、コードアクセスレベル、`Enum` キーワード、有効な名前を含む宣言を記述します。それぞれが異なる `Enum`を宣言します。  
  
     [!code-vb[VbEnumsTask#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#3)]  
  
2. 列挙体の定数を定義します。 既定では、列挙体の最初の定数は `0`に初期化され、後続の定数は前の定数より1つ大きい値に初期化されます。 たとえば、次の列挙型の `Days`には、値 `0`を持つ `Sunday` という定数と、値 `1`を持つ `Monday` という名前の定数が含まれています。また、`Tuesday` という名前の定数を `2`の値と共に使用することもできます。  
  
     [!code-vb[VbEnumsTask#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#4)]  
  
3. 代入ステートメントを使用して、列挙体の定数に値を明示的に割り当てることができます。 負の数値を含む任意の整数値を割り当てることができます。 たとえば、0未満の値を持つ定数を使用して、エラー条件を表すことができます。 次の列挙体では、定数 `Invalid` に `–1`値が明示的に割り当てられており、定数 `Sunday` に `0`値が割り当てられています。 列挙体の最初の定数であるため、`Saturday` も `0`値に初期化されます。 `Monday` の値が `1` (`Sunday`の値を超えています)。`Tuesday` の値は `2`のようになります。  
  
     [!code-vb[VbEnumsTask#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#5)]  
  
### <a name="to-declare-an-enumeration-as-an-explicit-type"></a>列挙型を明示的な型として宣言するには  
  
- 次の例に示すように、`As` 句を使用して列挙型の型を指定します。  
  
     [!code-vb[VbEnumsTask#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#6)]  
  
## <a name="see-also"></a>参照

- [列挙型と名前の修飾](../../../../visual-basic/programming-guide/language-features/constants-enums/enumerations-and-name-qualification.md)
- [方法 : 列挙型のメンバーを参照する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-refer-to-an-enumeration-member.md)
- [方法: Visual Basic 内の列挙体を反復処理する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-iterate-through-an-enumeration.md)
- [方法 : 列挙値に関連付けられている文字列を確認する](../../../../visual-basic/programming-guide/language-features/constants-enums/how-to-determine-the-string-associated-with-an-enumeration-value.md)
- [列挙型を使用する状況](../../../../visual-basic/programming-guide/language-features/constants-enums/when-to-use-an-enumeration.md)
- [定数の概要](../../../../visual-basic/programming-guide/language-features/constants-enums/constants-overview.md)
- [定数とリテラルのデータ型](../../../../visual-basic/programming-guide/language-features/constants-enums/constant-and-literal-data-types.md)
- [定数と列挙体](../../../../visual-basic/language-reference/constants-and-enumerations.md)
