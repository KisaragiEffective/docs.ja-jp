---
title: Enum ステートメント
ms.date: 07/20/2015
f1_keywords:
- vb.Enum
helpviewer_keywords:
- enumerated constants [Visual Basic]
- Enum statement [Visual Basic]
- Private keyword [Visual Basic], Enum statements
- Public keyword [Visual Basic], in Enum statement
- variables [Visual Basic], enumeration
- constants [Visual Basic], enumerated
ms.assetid: a45e51f1-65ff-48e1-bf32-79130f137377
ms.openlocfilehash: 48220fd1e88cf38e67db5dd3a2ad90638eb6b6df
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74343710"
---
# <a name="enum-statement-visual-basic"></a>Enum ステートメント (Visual Basic)

列挙体を宣言し、そのメンバーの値を定義します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ]  [ Shadows ]
Enum enumerationname [ As datatype ]
   memberlist
End Enum
```

## <a name="parts"></a>指定項目

- `attributelist`

  省略可。 この列挙に適用される属性のリスト。 [属性リスト](../../../visual-basic/language-reference/statements/attribute-list.md)は山かっこ ("`<`" と "`>`") で囲む必要があります。

  <xref:System.FlagsAttribute> 属性は、列挙体のインスタンスの値に複数の列挙体メンバーを含めることができ、各メンバーが列挙値のビットフィールドを表すことができることを示します。

- `accessmodifier`

  省略可。 この列挙体にアクセスできるコードを指定します。 次のいずれかになります。

  - [Public](../../../visual-basic/language-reference/modifiers/public.md)

  - [Protected](../../../visual-basic/language-reference/modifiers/protected.md)

  - [Friend](../../../visual-basic/language-reference/modifiers/friend.md)

  - [Private](../../../visual-basic/language-reference/modifiers/private.md)

  - [Protected Friend](../../language-reference/modifiers/protected-friend.md)

  - [Private Protected](../../language-reference/modifiers/private-protected.md)

- `Shadows`

  省略可。 この列挙体が、基底クラスで同じ名前を持つプログラミング要素、またはオーバーロードされた要素のセットを直しして非表示にすることを指定します。 [Shadows](../../../visual-basic/language-reference/modifiers/shadows.md)は、そのメンバーではなく、列挙自体にのみ指定できます。

- `enumerationname`

  必須。 列挙体の名前。 有効な名前の詳細については、「[宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

- `datatype`

  省略可。 列挙型とそのすべてのメンバーのデータ型。

- `memberlist`

  必須。 このステートメントで宣言されているメンバー定数の一覧。 個々のソースコード行に複数のメンバーが表示されます。

  各 `member` には、次の構文と部分があります。 `[<attribute list>] member name [ = initializer ]`

  |要素|説明|
  |---|---|
  |`membername`|必須。 このメンバーの名前。|
  |`initializer`|省略可。 コンパイル時に評価され、このメンバーに割り当てられる式。|

- `End` `Enum`

  `Enum` ブロックを終了します。

## <a name="remarks"></a>コメント

論理的に相互に関連付けられている変化しない値のセットがある場合は、それらを列挙体でまとめて定義できます。 これにより、列挙体とそのメンバーにわかりやすい名前が提供され、その値よりも覚えやすくなります。 その後、コード内のさまざまな場所で列挙メンバーを使用できます。

列挙を使用する利点は、次のとおりです。

- 数値の転置またはミスによって発生するエラーを減らします。

- では、将来の値の変更が簡単になります。

- コードを読みやすくするため、エラーが発生する可能性は低くなります。

- 上位互換性を確保します。 列挙型を使用する場合、後でメンバー名に対応する値を変更すると、コードが失敗する可能性は低くなります。

列挙体には、名前、基になるデータ型、およびメンバーのセットが含まれます。 各メンバーは定数を表します。

プロシージャ以外のクラス、構造体、モジュール、またはインターフェイスレベルで宣言された列挙型は、*メンバー列挙*型です。 これは、それを宣言するクラス、構造体、モジュール、またはインターフェイスのメンバーです。

メンバー列挙体には、クラス、構造体、モジュール、またはインターフェイス内のどこからでもアクセスできます。 クラス、構造体、またはモジュールの外部のコードでは、メンバーの列挙体の名前を、そのクラス、構造体、またはモジュールの名前で修飾する必要があります。 [Imports ステートメント](../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)をソースファイルに追加することで、完全修飾名を使用する必要がないようにすることができます。

名前空間レベルで、任意のクラス、構造体、モジュール、またはインターフェイスの外側で宣言された列挙型は、それが出現する名前空間のメンバーになります。

列挙体の*宣言コンテキスト*は、ソースファイル、名前空間、クラス、構造体、モジュール、またはインターフェイスである必要があり、プロシージャにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](../../../visual-basic/language-reference/statements/declaration-contexts-and-default-access-levels.md)」を参照してください。

列挙体全体に属性を適用することはできますが、メンバーには個別には適用できません。 属性は、アセンブリのメタデータに情報を提供します。

## <a name="data-type"></a>データ型

`Enum` ステートメントでは、列挙体のデータ型を宣言できます。 各メンバーは、列挙体のデータ型を受け取ります。 `Byte`、`Integer`、`Long`、`SByte`、`Short`、`UInteger`、`ULong`、`UShort`を指定できます。

列挙に `datatype` を指定しない場合、各メンバーはその `initializer`のデータ型を受け取ります。 `datatype` と `initializer`の両方を指定する場合、`initializer` のデータ型は `datatype`に変換可能である必要があります。 `datatype` も `initializer` も存在しない場合、データ型は既定で `Integer`に設定されます。

## <a name="initializing-members"></a>メンバーの初期化

`Enum` ステートメントで `memberlist`で選択したメンバーの内容を初期化できます。 `initializer` を使用して、メンバーに割り当てられる式を指定します。

メンバーに `initializer` を指定しなかった場合は、Visual Basic 0 (`memberlist`の最初の `member`)、または直前の `member`の値より大きい値に初期化されます。

各 `initializer` に用意されている式には、リテラル、既に定義されている他の定数、およびこの列挙の前のメンバーを含む定義済みの列挙型メンバーの任意の組み合わせを指定できます。 算術演算子と論理演算子を使用すると、このような要素を組み合わせることができます。

`initializer`では、変数または関数を使用できません。 ただし、`CByte` や `CShort`などの変換キーワードを使用することもできます。 定数 `String` または `Char` 引数を使用して呼び出す場合は、コンパイル時に評価できるため、`AscW` を使用することもできます。

列挙体に浮動小数点値を指定することはできません。 メンバーに浮動小数点値が割り当てられていて `Option Strict` が on に設定されている場合、コンパイラエラーが発生します。 `Option Strict` がオフの場合、値は自動的に `Enum` の種類に変換されます。

メンバーの値が、基になるデータ型で許容される範囲を超えている場合、またはメンバーが基になるデータ型で許容される最大値に初期化されている場合、コンパイラはエラーを報告します。

## <a name="modifiers"></a>修飾子

クラス、構造体、モジュール、およびインターフェイス メンバーの列挙型は、既定でパブリック アクセスに設定されています。アクセス修飾子を使用してこれらのアクセス レベルを調整できます。名前空間メンバーの列挙は、既定でフレンド アクセスに設定されています。アクセス レベルは public に調整できますが、private または protected にすることはできません。詳細については、「[Visual Basic でのアクセス レベル](../../../visual-basic/programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。


すべての列挙メンバーにはパブリックアクセスがあり、それらのメンバーに対してアクセス修飾子を使用することはできません。 ただし、列挙自体のアクセスレベルがより制限されている場合は、指定された列挙アクセスレベルが優先されます。

既定では、すべての列挙型とそのフィールドは定数です。 したがって、列挙型またはそのメンバーを宣言するときに、`Shared`、`Static`、および `ReadOnly` キーワードを使用することはできません。

## <a name="assigning-multiple-values"></a>複数の値の割り当て

列挙型は、通常、相互に排他的な値を表します。 `Enum` 宣言に <xref:System.FlagsAttribute> 属性を含めることにより、複数の値を列挙型のインスタンスに割り当てることができます。 <xref:System.FlagsAttribute> 属性は、列挙型をビットフィールド、つまりフラグのセットとして扱うことを指定します。 これらは*ビットごと*の列挙体と呼ばれます。

<xref:System.FlagsAttribute> 属性を使用して列挙体を宣言する場合は、値に対して2の累乗 (1、2、4、8、16など) を使用することをお勧めします。 また、"None" は、値が0のメンバーの名前にすることをお勧めします。 その他のガイドラインについては、「<xref:System.FlagsAttribute>」および「<xref:System.Enum>」を参照してください。

## <a name="example"></a>例

`Enum` ステートメントを使用する方法の例を次に示します。 メンバーは、`Medium`ではなく `EggSizeEnum.Medium`と呼ばれることに注意してください。

[!code-vb[VbEnumsTask#41](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#41)]

## <a name="example"></a>例

次の例のメソッドは、`Egg` クラスの外側にあります。 したがって、`EggSizeEnum` は完全に `Egg.EggSizeEnum`として修飾されます。

[!code-vb[VbEnumsTask#42](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#42)]

## <a name="example"></a>例

次の例では、`Enum` ステートメントを使用して、関連する一連の名前付き定数値を定義します。 この場合、値は、データベースのデータ入力フォームをデザインするために選択できる色です。

[!code-vb[VbEnumsTask#30](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#30)]

## <a name="example"></a>例

次の例は、正と負の両方の値を含む値を示しています。

[!code-vb[VbEnumsTask#31](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#31)]

## <a name="example"></a>例

次の例では、`As` 句を使用して、列挙型の `datatype` を指定しています。

[!code-vb[VbEnumsTask#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class2.vb#6)]

## <a name="example"></a>例

ビットごとの列挙体を使用する方法を次の例に示します。 ビットごとの列挙体のインスタンスに複数の値を割り当てることができます。 `Enum` 宣言には <xref:System.FlagsAttribute> 属性が含まれています。これは、列挙体をフラグのセットとして処理できることを示します。

[!code-vb[VbEnumsTask#61](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#61)]

## <a name="example"></a>例

次の例では、列挙体を反復処理します。 <xref:System.Enum.GetNames%2A> メソッドを使用して、列挙からメンバー名の配列を取得し、<xref:System.Enum.GetValues%2A> してメンバー値の配列を取得します。

[!code-vb[VbEnumsTask#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbEnumsTask/VB/Class1.vb#51)]

## <a name="see-also"></a>参照

- <xref:System.Enum>
- <xref:Microsoft.VisualBasic.Strings.AscW%2A>
- [Const ステートメント](../../../visual-basic/language-reference/statements/const-statement.md)
- [Dim ステートメント](../../../visual-basic/language-reference/statements/dim-statement.md)
- [暗黙の型変換と明示的な型変換](../../../visual-basic/programming-guide/language-features/data-types/implicit-and-explicit-conversions.md)
- [CString](../../../visual-basic/language-reference/functions/type-conversion-functions.md)
- [定数と列挙体](../../../visual-basic/language-reference/constants-and-enumerations.md)
