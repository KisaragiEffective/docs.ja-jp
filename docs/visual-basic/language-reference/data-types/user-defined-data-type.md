---
title: ユーザー定義型
ms.date: 07/20/2015
f1_keywords:
- UserDefined
- UDT
- vb.UDT
- User-Defined
- vb.UserDefined
- vb.User-Defined
helpviewer_keywords:
- user-defined data types [Visual Basic], Visual Basic
- user-defined types
- structures [Visual Basic], as user-defined data types
- user-defined types [Visual Basic], Visual Basic
- user-defined types [Visual Basic], structure declaration
- user-defined types [Visual Basic], structures in Visual Basic
- user-defined data types [Visual Basic], structure declaration
- data types [Visual Basic], assigning
- Structure statement [Visual Basic]
- data types [Visual Basic], user-defined
- user-defined data types [Visual Basic], structures in Visual Basic
- user-defined data types
- types [Visual Basic], user-defined
ms.assetid: be913dca-a364-4a51-96a1-549a1b390b0a
ms.openlocfilehash: 99eeb4b619f6bb23d00f8e449de953d41843f714
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343865"
---
# <a name="user-defined-data-type"></a>ユーザー定義型

では、定義した形式でデータを保持します。 `Structure` ステートメントは、形式を定義します。

以前のバージョンの Visual Basic では、ユーザー定義型 (UDT) がサポートされています。 現在のバージョンは、UDT を*構造体*に拡張します。 構造体は、さまざまなデータ型の1つ以上の*メンバー*を連結したものです。 Visual Basic は、構造体を1つの単位として扱いますが、そのメンバーに個別にアクセスすることもできます。

## <a name="remarks"></a>コメント

さまざまなデータ型を1つの単位に結合する必要がある場合、または基本データ型のいずれも必要でない場合は、構造体のデータ型を定義して使用します。

構造体のデータ型の既定値は、各メンバーの既定値の組み合わせで構成されます。

## <a name="declaration-format"></a>宣言の形式

構造体の宣言は、 [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)で始まり、`End Structure` ステートメントで終了します。 `Structure` ステートメントは、構造体の名前を指定します。これは、構造体が定義しているデータ型の識別子でもあります。 コードの他の部分では、この識別子を使用して、この構造体のデータ型の変数、パラメーター、および関数の戻り値を宣言できます。

`Structure` ステートメントと `End Structure` ステートメントの間の宣言では、構造体のメンバーを定義します。

## <a name="member-access-levels"></a>メンバーアクセスレベル

すべてのメンバーは、 [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)を使用するか、 [Public](../../../visual-basic/language-reference/modifiers/public.md)、 [Friend](../../../visual-basic/language-reference/modifiers/friend.md)、 [Private](../../../visual-basic/language-reference/modifiers/private.md)などのアクセスレベルを指定するステートメントを使用して宣言する必要があります。 `Dim` ステートメントを使用する場合、アクセスレベルの既定値は public です。

## <a name="programming-tips"></a>プログラミングのヒント

- **メモリ使用量。** 他のすべての複合データ型と同様に、構造体の総メモリ使用量を計算する場合、各メンバーのストレージ割り当ての公称サイズを単に合計しただけでは安全ではありません。 さらに、メモリ内に格納される順序が宣言の順序と同じであると仮定するのも安全ではありません。 構造体のストレージ レイアウトを制御する必要がある場合は、<xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を `Structure` ステートメントに適用します。

- **相互運用に関する考慮事項。** オートメーションまたは COM オブジェクトなどの .NET Framework 用に作成されていないコンポーネントをやり取りする場合、他の環境でのユーザー定義型は Visual Basic 構造型と互換性がないことに注意してください。

- **広げ.** 任意の構造体のデータ型との間で自動変換が行われることはありません。 [Operator ステートメント](../../../visual-basic/language-reference/statements/operator-statement.md)を使用して構造体に変換演算子を定義できます。また、各変換演算子を `Widening` または `Narrowing`として宣言できます。

- **文字を入力します。** 構造体のデータ型には、リテラルの型文字または識別子の型文字がありません。

- **フレームワークの種類。** .NET Framework に対応する型がありません。 すべての構造体は .NET Framework クラス <xref:System.ValueType?displayProperty=nameWithType>から継承されますが、<xref:System.ValueType?displayProperty=nameWithType>に対応する個々の構造はありません。

## <a name="example"></a>例

次のパラダイムは、構造体の宣言の概要を示しています。

```vb
[Public | Protected | Friend | Protected Friend | Private] Structure structname
    {Dim | Public | Friend | Private} member1 As datatype1
    ' ...
    {Dim | Public | Friend | Private} memberN As datatypeN
End Structure
```

## <a name="see-also"></a>参照

- <xref:System.ValueType>
- <xref:System.Runtime.InteropServices.StructLayoutAttribute>
- [データの種類](../../../visual-basic/language-reference/data-types/index.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [変換の概要](../../../visual-basic/language-reference/keywords/conversion-summary.md)
- [Structure ステートメント](../../../visual-basic/language-reference/statements/structure-statement.md)
- [Widening](../../../visual-basic/language-reference/modifiers/widening.md)
- [Narrowing](../../../visual-basic/language-reference/modifiers/narrowing.md)
- [構造体](../../../visual-basic/programming-guide/language-features/data-types/structures.md)
- [データ型の有効な使用方法](../../../visual-basic/programming-guide/language-features/data-types/efficient-use-of-data-types.md)
