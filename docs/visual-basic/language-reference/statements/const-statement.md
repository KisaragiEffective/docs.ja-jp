---
title: Const ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.Const
helpviewer_keywords:
- Const statement [Visual Basic]
ms.assetid: 495b318d-b7c5-4198-94f8-0790a541b07a
ms.openlocfilehash: 1411e019058e7aac8249b7a50ecd295885a74177
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74354120"
---
# <a name="const-statement-visual-basic"></a>Const ステートメント (Visual Basic)

1つ以上の定数を宣言して定義します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ]
Const constantlist
```

## <a name="parts"></a>指定項目

`attributelist`  
省略可。 このステートメントで宣言されているすべての定数に適用される属性のリスト。 山かっこ ("`<`" と "`>`") の[属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)を参照してください。

`accessmodifier`  
省略可。 これらの定数にアクセスできるコードを指定するには、これを使用します。 [Public](../../../visual-basic/language-reference/modifiers/public.md)、 [Protected](../../../visual-basic/language-reference/modifiers/protected.md)、 [Friend](../../../visual-basic/language-reference/modifiers/friend.md)、 [Protected Friend](../modifiers/protected-friend.md)、 [private](../../../visual-basic/language-reference/modifiers/private.md)、または[Private](../../language-reference/modifiers/private-protected.md)を指定できます。

`Shadows`  
省略可。 基底クラスのプログラミング要素を再宣言および非表示にするには、これを使用します。 「[Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)」を参照してください。

`constantlist`  
必須。 このステートメントで宣言されている定数の一覧。

`constant` `[ ,` `constant` `... ]`

`constant` の構文と指定項目は次のとおりです。

`constantname` `[ As` `datatype` `] =` `initializer`

|要素|説明|
|----------|-----------------|
|`constantname`|必須。 定数の名前。 「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
|`datatype`|`Option Strict` が `On`の場合は必須です。 定数のデータ型。|
|`initializer`|必須。 コンパイル時に評価され、定数に割り当てられる式。|

## <a name="remarks"></a>コメント

アプリケーションで変更されない値がある場合は、名前付き定数を定義し、リテラル値の代わりに使用することができます。 名前は、値よりも覚えやすくなります。 定数は一度だけ定義し、コード内の多くの場所で使用できます。 後のバージョンで値を再定義する必要がある場合は、`Const` ステートメントだけを変更する必要があります。

`Const` は、モジュールレベルまたはプロシージャレベルでのみ使用できます。 つまり、変数の*宣言コンテキスト*はクラス、構造体、モジュール、プロシージャ、またはブロックである必要があり、ソースファイル、名前空間、またはインターフェイスにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。

ローカル定数 (プロシージャ内) は、既定でパブリックアクセスに設定され、アクセス修飾子を使用することはできません。 クラスとモジュールのメンバー定数 (プロシージャ以外) では、既定でプライベートアクセスが使用され、構造体メンバー定数は既定でパブリックアクセスになります。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。

## <a name="rules"></a>ルール

- **宣言コンテキスト。** モジュールレベルで、プロシージャの外部で宣言された定数は、*メンバー定数*です。これは、それを宣言するクラス、構造体、またはモジュールのメンバーです。

  プロシージャレベルで宣言された定数は*ローカル定数*です。これは、それを宣言するプロシージャまたはブロックに対してローカルです。

- **属性。 ** 属性は、ローカル定数ではなく、メンバー定数にのみ適用できます。 属性は、アセンブリのメタデータに情報を提供します。これは、ローカル定数などの一時的なストレージには意味がありません。

- **修飾子。 ** 既定では、すべての定数は `Shared`、`Static`、および `ReadOnly`です。 定数を宣言するときにこれらのキーワードを使用することはできません。

  プロシージャレベルでは、`Shadows` または任意のアクセス修飾子を使用してローカル定数を宣言することはできません。

- **複数の定数。** 同じ宣言ステートメントで複数の定数を宣言し、それぞれに対して `constantname` の部分を指定できます。 複数の定数は、コンマで区切られます。

## <a name="data-type-rules"></a>データ型ルール

- **データ型。** `Const` ステートメントでは、変数のデータ型を宣言できます。 任意のデータ型または列挙型の名前を指定できます。

- **既定の型。** `datatype`を指定しない場合、定数は `initializer`のデータ型を受け取ります。 `datatype` と `initializer`の両方を指定する場合、`initializer` のデータ型は `datatype`に変換可能である必要があります。 `datatype` も `initializer` も存在しない場合、データ型は既定で `Object`に設定されます。

- **異なる型。** 宣言する変数ごとに個別の `As` 句を使用して、異なる定数に異なるデータ型を指定できます。 ただし、共通の `As` 句を使用して、同じ型の複数の定数を宣言することはできません。

- **初期化。  **   `constantlist`内のすべての定数の値を初期化する必要があります。 `initializer` を使用して、定数に割り当てられる式を指定します。 式には、リテラルの任意の組み合わせ、既に定義されている他の定数、および既に定義されている列挙メンバーを使用できます。 算術演算子と論理演算子を使用すると、このような要素を組み合わせることができます。

  `initializer`では、変数または関数を使用できません。 ただし、`CByte` や `CShort`などの変換キーワードを使用することもできます。 定数 `String` または `Char` 引数を使用して呼び出す場合は、コンパイル時に評価できるため、`AscW` を使用することもできます。

## <a name="behavior"></a>動作

- **スコープ。 ** ローカル定数は、プロシージャまたはブロック内からのみアクセスできます。 メンバー定数は、クラス、構造体、またはモジュール内のどこからでもアクセスできます。

- **修飾。 ** クラス、構造体、またはモジュールの外部のコードでは、メンバー定数の名前を、そのクラス、構造体、またはモジュールの名前で修飾する必要があります。 プロシージャまたはブロックの外側のコードは、そのプロシージャまたはブロック内のローカル定数を参照できません。

## <a name="example"></a>例

次の例では、`Const` ステートメントを使用して、リテラル値の代わりに使用する定数を宣言します。

[!code-vb[VbVbalrStatements#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#13)]

## <a name="example"></a>例

`Object`データ型を持つ定数を定義すると、Visual Basic コンパイラによって `Object`ではなく `initializer`の種類が指定されます。 次の例では、定数 `naturalLogBase` にランタイム型 `Decimal`が含まれています。

[!code-vb[VbVbalrStatements#87](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#87)]

前の例では、 [GetType 演算子](../../../visual-basic/language-reference/operators/gettype-operator.md)によって返された <xref:System.Type> オブジェクトの <xref:System.Type.ToString%2A> メソッドを使用しています。これは <xref:System.Type> を `CStr`を使用して `String` に変換できないためです。

## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Strings.Asc%2A>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- [Enum ステートメント](../../../visual-basic/language-reference/statements/enum-statement.md)
- [#Const ディレクティブ](../../../visual-basic/language-reference/directives/const-directive.md)
- [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)
- [ReDim ステートメント](../../../visual-basic/language-reference/statements/redim-statement.md)
- [暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [定数と列挙体](../../../visual-basic/programming-guide/language-features/constants-enums/index.md)
- [定数と列挙体](../../../visual-basic/language-reference/constants-and-enumerations.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
