---
title: Property ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.PropertySet
- vb.Property
- vb.PropertyGet
helpviewer_keywords:
- Property statement [Visual Basic]
- default modifier
- property procedures [Visual Basic], Property statements
- Property keyword [Visual Basic]
ms.assetid: 3155edaf-8ebd-45c6-9cef-11d5d2dc8d38
ms.openlocfilehash: 80bce2442d96ecb9c548a88c8e5ee44c6258473b
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346758"
---
# <a name="property-statement"></a>Property ステートメント

プロパティの名前、およびプロパティの値を格納および取得するために使用されるプロパティプロシージャを宣言します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ Default ] [ accessmodifier ]
[ propertymodifiers ] [ Shared ] [ Shadows ] [ ReadOnly | WriteOnly ] [ Iterator ]
Property name ( [ parameterlist ] ) [ As returntype ] [ Implements implementslist ]
    [ <attributelist> ] [ accessmodifier ] Get
        [ statements ]
    End Get
    [ <attributelist> ] [ accessmodifier ] Set ( ByVal value As returntype [, parameterlist ] )
        [ statements ]
    End Set
End Property
- or -
[ <attributelist> ] [ Default ] [ accessmodifier ]
[ propertymodifiers ] [ Shared ] [ Shadows ] [ ReadOnly | WriteOnly ]
Property name ( [ parameterlist ] ) [ As returntype ] [ Implements implementslist ]
```

## <a name="parts"></a>指定項目

- `attributelist`

  任意。 このプロパティまたは `Get` または `Set` プロシージャに適用される属性の一覧。 「[属性リスト](attribute-list.md)」を参照してください。

- `Default`

  任意。 このプロパティが、定義されているクラスまたは構造体の既定のプロパティであることを指定します。 既定のプロパティはパラメーターを受け取る必要があり、プロパティ名を指定せずに設定および取得できます。 プロパティを `Default`として宣言する場合、プロパティまたはプロパティプロシージャのいずれかで `Private` を使用することはできません。

- `accessmodifier`

  `Property` ステートメントでは省略可能で、`Get` ステートメントと `Set` ステートメントのうちの1つでも指定できます。 次のいずれかになります。

  - [Public](../modifiers/public.md)

  - [Protected](../modifiers/protected.md)

  - [Friend](../modifiers/friend.md)

  - [Private](../modifiers/private.md)

  - [Protected Friend](../modifiers/protected-friend.md)

  - [Private Protected](../modifiers/private-protected.md)

  「[Visual Basic でのアクセス レベル](../../programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

- `propertymodifiers`

  任意。 次のいずれかになります。

  - [Overloads](../modifiers/overloads.md)

  - [Overrides](../modifiers/overrides.md)

  - [Overridable](../modifiers/overridable.md)

  - [NotOverridable](../modifiers/notoverridable.md)

  - [MyBase](../modifiers/mustoverride.md)

  - `MustOverride Overrides`

  - `NotOverridable Overrides`

- `Shared`

  任意。 「[Shared](../modifiers/shared.md)」を参照してください。

- `Shadows`

  任意。 「[Shadows](../modifiers/shadows.md)」を参照してください。

- `ReadOnly`

  任意。 「[ReadOnly](../modifiers/readonly.md)」を参照してください。

- `WriteOnly`

  任意。 「[WriteOnly](../modifiers/writeonly.md)」を参照してください。

- `Iterator`

  任意。 「[Iterator](../modifiers/iterator.md)」を参照してください。

- `name`

  必須。 プロパティ名。 「[宣言された要素の名前](../../programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。

- `parameterlist`

  任意。 このプロパティのパラメーターを表すローカル変数名の一覧と、`Set` プロシージャの追加パラメーター。 「[パラメーターリスト](parameter-list.md)」を参照してください。

- `returntype`

  `Option Strict` が `On`の場合は必須です。 このプロパティによって返される値のデータ型。

- `Implements`

  任意。 このプロパティが1つ以上のプロパティを実装することを示します。各プロパティは、このプロパティのクラスまたは構造体を含むインターフェイスで定義されています。 「[Implements ステートメント](implements-statement.md)」を参照してください。

- `implementslist`

  `Implements` を指定する場合は、必ず指定します。 実装されているプロパティの一覧。

  `implementedproperty [ , implementedproperty ... ]`

  `implementedproperty` の構文と指定項目は次のとおりです。

  `interface.definedname`

  |要素|説明|
  |---|---|
  |`interface`|必須。 このプロパティのクラスまたは構造体によって実装されるインターフェイスの名前。|
  |`definedname`|必須。 プロパティが定義されている名前 `interface`です。|

- `Get`

  任意。 プロパティが `ReadOnly`としてマークされている場合は必須。 プロパティの値を返すために使用される `Get` プロパティプロシージャを開始します。  `Get` ステートメントは、[自動実装プロパティ](../../programming-guide/language-features/procedures/auto-implemented-properties.md)では使用されません。

- `statements`

  任意。 `Get` または `Set` プロシージャ内で実行するステートメントのブロック。

- `End Get`

  `Get` プロパティプロシージャを終了します。

- `Set`

  任意。 プロパティが `WriteOnly`としてマークされている場合は必須。 プロパティの値を格納するために使用される `Set` プロパティプロシージャを開始します。  `Set` ステートメントは、[自動実装プロパティ](../../programming-guide/language-features/procedures/auto-implemented-properties.md)では使用されません。

- `End Set`

  `Set` プロパティプロシージャを終了します。

- `End Property`

  このプロパティの定義を終了します。

## <a name="remarks"></a>コメント

`Property` ステートメントでは、プロパティの宣言が導入されています。 プロパティは、`Get` プロシージャ (読み取り専用)、`Set` プロシージャ (書き込み専用)、またはその両方 (読み取り/書き込み) を持つことができます。 自動実装プロパティを使用する場合は、`Get` と `Set` の手順を省略できます。 詳細については、「[自動実装プロパティ](../../programming-guide/language-features/procedures/auto-implemented-properties.md)」を参照してください。

`Property` はクラスレベルでのみ使用できます。 つまり、プロパティの*宣言コンテキスト*はクラス、構造体、モジュール、またはインターフェイスである必要があり、ソースファイル、名前空間、プロシージャ、またはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](declaration-contexts-and-default-access-levels.md)」を参照してください。

既定では、プロパティはパブリックアクセスを使用します。 `Property` ステートメントでアクセス修飾子を使用してプロパティのアクセスレベルを調整できます。また、必要に応じて、プロパティプロシージャの1つをより制限の厳しいアクセスレベルに調整することもできます。

Visual Basic は、プロパティの割り当て時に `Set` プロシージャにパラメーターを渡します。 `Set`のパラメーターを指定しない場合、統合開発環境 (IDE) は `value`という名前の暗黙的なパラメーターを使用します。 このパラメーターは、プロパティに割り当てられる値を保持します。 通常、この値はプライベートローカル変数に格納し、`Get` プロシージャが呼び出されるたびに返されます。

## <a name="rules"></a>ルール

- **混合アクセスレベル。** 読み取り/書き込みプロパティを定義する場合は、必要に応じて、`Get` または `Set` のいずれかのプロシージャに対して異なるアクセスレベルを指定できますが、両方を指定することはできません。 この場合、プロシージャのアクセスレベルは、プロパティのアクセスレベルよりも制限されている必要があります。 たとえば、プロパティが `Friend`として宣言されている場合は、`Set` プロシージャ `Private`を宣言できますが、`Public`は宣言できません。

  `ReadOnly` または `WriteOnly` プロパティを定義する場合、1つのプロパティプロシージャ (それぞれ`Get` または `Set`) が、すべてのプロパティを表します。 このようなプロシージャに対して異なるアクセスレベルを宣言することはできません。これは、プロパティに2つのアクセスレベルが設定されるためです。

- **戻り値の型。** `Property` ステートメントでは、返される値のデータ型を宣言できます。 任意のデータ型を指定することも、列挙型、構造体、クラス、またはインターフェイスの名前を指定することもできます。

  `returntype`を指定しない場合、プロパティは `Object`を返します。

- **ション.** このプロパティで `Implements` キーワードが使用されている場合、その親クラスまたは構造体には、その `Class` または `Structure` ステートメントの直後にある `Implements` ステートメントが必要です。 `Implements` ステートメントには、`implementslist`で指定された各インターフェイスを含める必要があります。 ただし、インターフェイスが `Property` を定義するときに使用する名前 (`definedname`) は、このプロパティの名前と同じである必要はありません (`name`)。

## <a name="behavior"></a>動作

- **プロパティプロシージャからを返します。** `Get` または `Set` プロシージャが呼び出し元のコードに戻ると、そのプロシージャを呼び出したステートメントの後のステートメントで実行が続行されます。

  `Exit Property` ステートメントおよび `Return` ステートメントでは、プロパティプロシージャがすぐに終了します。 プロシージャ内の任意の場所で任意の数の `Exit Property` および `Return` ステートメントを使用できます。また、`Exit Property` と `Return` のステートメントを混在させることができます。

- **戻り値。** `Get` プロシージャから値を返すには、プロパティ名に値を割り当てるか、`Return` ステートメントに値を含めることができます。 次の例では、プロパティ名 `quoteForTheDay` に戻り値を割り当て、`Exit Property` ステートメントを使用してを返します。

  [!code-vb[VbVbalrStatements#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#27)]

  [!code-vb[VbVbalrStatements#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#28)]

  `name`に値を割り当てずに `Exit Property` を使用する場合、`Get` プロシージャはプロパティのデータ型の既定値を返します。

  同時に `Return` ステートメントによって、`Get` プロシージャの戻り値が割り当てられ、プロシージャが終了します。 この例を次に示します。

  [!code-vb[VbVbalrStatements#27](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#27)]

  [!code-vb[VbVbalrStatements#29](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#29)]

## <a name="example"></a>例

次の例では、クラスのプロパティを宣言しています。

[!code-vb[VbVbalrStatements#51](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#51)]

## <a name="see-also"></a>関連項目

- [自動実装プロパティ](../../programming-guide/language-features/procedures/auto-implemented-properties.md)
- [クラスとオブジェクト](../../programming-guide/language-features/objects-and-classes/index.md)
- [Get ステートメント](get-statement.md)
- [Set ステートメント](set-statement.md)
- [パラメーター リスト](parameter-list.md)
- [Shared](../modifiers/default.md)
