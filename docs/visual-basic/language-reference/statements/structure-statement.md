---
title: Structure ステートメント
ms.date: 05/12/2018
f1_keywords:
- vb.Structure
- Structure
helpviewer_keywords:
- user-defined types [Visual Basic], Structure statement
- compound data types [Visual Basic]
- Structure keyword [Visual Basic]
- Structure statement [Visual Basic]
- UDT (user-defined types)
- types [Visual Basic], user-defined
ms.assetid: 9bd1deea-2a89-4cdc-812c-6dcbb947c391
ms.openlocfilehash: 120f836b9d49c00e9c53af0d1fc832e22c8cbbb8
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346458"
---
# <a name="structure-statement"></a>Structure ステートメント

構造体の名前を宣言し、構造体を構成する変数、プロパティ、イベント、およびプロシージャの定義を提供します。

## <a name="syntax"></a>構文

```vb
[ <attributelist> ] [ accessmodifier ] [ Shadows ] [ Partial ] _
Structure name [ ( Of typelist ) ]
    [ Implements interfacenames ]
    [ datamemberdeclarations ]
    [ methodmemberdeclarations ]
End Structure
```

## <a name="parts"></a>指定項目

|用語|定義|
|---|---|
|`attributelist`|任意。 「[属性リスト](attribute-list.md)」を参照してください。|
|`accessmodifier`|任意。 次のいずれかになります。<br /><br /> -   [Public](../modifiers/public.md)<br />-     [Protected](../modifiers/protected.md)<br />-   [Friend](../modifiers/friend.md)<br />-   [Private](../modifiers/private.md)<br />- [Protected Friend](../modifiers/protected-friend.md)<br/>- [Private Protected](../modifiers/private-protected.md) <br /><br /> 「[Visual Basic でのアクセス レベル](../../programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。|
|`Shadows`|任意。 「[Shadows](../modifiers/shadows.md)」を参照してください。|
|`Partial`|任意。 構造体の部分定義を示します。 「[Partial](../modifiers/partial.md)」を参照してください。|
|`name`|必須。 この構造体の名前です。 「[宣言された要素の名前](../../programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。|
|`Of`|任意。 これがジェネリックな構造体であることを指定します。|
|`typelist`|[Of](of-clause.md)キーワードを使用する場合は必須です。 この構造体の型パラメーター リストを指定します。 [型リスト](type-list.md)を参照してください。|
|`Implements`|任意。 この構造体が、複数のインターフェイスのメンバーを実装していることを示します。 「[Implements ステートメント](implements-statement.md)」を参照してください。|
|`interfacenames`|`Implements` ステートメントを使用する場合は必ず指定します。 この構造体が実装するインターフェイスの名前を指定します。|
|`datamemberdeclarations`|必須。 構造体の*データメンバー*を宣言する、0個以上の `Const`、`Dim`、`Enum`、または `Event` ステートメント。|
|`methodmemberdeclarations`|任意。 構造体の*メソッドメンバー*として機能する、`Function`、`Operator`、`Property`、または `Sub` の各プロシージャの、0個以上の宣言。|
|`End Structure`|必須。 `Structure` の定義を終了します。|

## <a name="remarks"></a>コメント

`Structure` ステートメントは、カスタマイズできる複合値型を定義します。 *構造体*は、以前のバージョンの Visual Basic のユーザー定義型 (UDT) を一般化したものです。 詳細については、「[構造体](../../programming-guide/language-features/data-types/structures.md)」を参照してください。

構造体は、クラスと同じ機能の多くをサポートします。 たとえば、構造体は、プロパティやプロシージャを持つことができ、インターフェイスを実装でき、パラメーター化されたコンストラクターを持つことができます。 ただし、継承、宣言、および使用方法に関しては、構造体とクラスの間には大きな違いがあります。 また、クラスは参照型ですが、構造体は値型です。 詳細については、「[構造体とクラス](../../programming-guide/language-features/data-types/structures-and-classes.md)」を参照してください。

`Structure` は、名前空間またはモジュール レベルでのみ使用できます。 つまり、構造体の*宣言コンテキスト*は、ソースファイル、名前空間、クラス、構造体、モジュール、またはインターフェイスである必要があり、プロシージャまたはブロックにすることはできません。 詳細については、「[宣言コンテキストと既定のアクセス レベル](declaration-contexts-and-default-access-levels.md)」を参照してください。

構造体は、既定で [Friend](../modifiers/friend.md) アクセスに設定されます。 アクセス修飾子を使用してこれらのアクセス レベルを調整できます。 詳細については、「[Visual Basic でのアクセス レベル](../../programming-guide/language-features/declared-elements/access-levels.md)」を参照してください。

## <a name="rules"></a>ルール

- **入れ子。** 構造体の内部に別の構造体を定義できます。 外側の構造体は、*包含構造体*と呼ばれ、内側の構造体は*入れ子構造*体と呼ばれます。 ただし、包含構造体をとおして入れ子構造体のメンバーにアクセスすることはできません。 入れ子構造体のメンバーにアクセスするには、入れ子構造体のデータ型の変数を宣言する必要があります。

- **メンバー宣言。** 構造体のすべてのメンバーを宣言する必要があります。 構造体を継承することはできないため、構造体メンバーを [Protected](../modifiers/protected.md)または `Protected Friend` できません。 ただし、構造体そのものを`Protected` または `Protected Friend` にすることはできます。
  
     0 個または 1 つ以上の非共有変数または非共有の非カスタム イベントを構造体内で宣言することができます。 非共有ではあっても、定数、プロパティ、およびプロシージャだけを宣言することはできません。

- **初期化。** 構造体の非共有データ メンバーの値を宣言の一部として初期化することはできません。 構造体のパラメーター化されたコンストラクターを使ってそのようなデータ メンバーを初期化するか、または構造体のインスタンスを作成した後にメンバーに値を割り当てる必要があります。

- **継承。** 構造体は、<xref:System.ValueType> 以外の型を継承できません。すべての構造体が、この型を継承します。 特に、構造体は別の構造体を継承できません。

     <xref:System.ValueType>を指定する場合でも、構造体の定義で[Inherits ステートメント](inherits-statement.md)を使用することはできません。

- **実装。** 構造体が [Implements ステートメント](implements-statement.md)を使用する場合は、`interfacenames`で指定するすべてのインターフェイスで定義されているすべてのメンバーを実装する必要があります。

- **既定のプロパティ。** 構造体では、[Default](../modifiers/default.md)の修飾子を使用して、最大で1つのプロパティを*既定のプロパティ*として指定できます。 詳細については、「[Default](../modifiers/default.md)」を参照してください。

## <a name="behavior"></a>動作

- **アクセスレベル。** 構造体の内部では、各メンバーを独自のアクセス レベルで宣言できます。 すべての構造体メンバーは、既定で [Public](../modifiers/public.md)アクセスになります。 構造体そのものにこれより厳しいアクセス レベルを指定した場合は、たとえアクセス修飾子を使ってメンバーのアクセス レベルを調整していても、メンバーへのアクセスが自動的に制限されることに注意してください。

- **スコープ。** 構造体は、そこに含まれる名前空間、クラス、構造体、またはモジュールをスコープとします。

     すべての構造体メンバーのスコープは、構造体全体になります。

- **有効期間。** 構造体に有効期間はありません。 ただし、構造体の各インスタンスには、他のインスタンスに依存しない独自の有効期間があります。

     インスタンスの有効期間は、[New 演算子](../operators/new-operator.md) 句によって作成されると開始されます。 インスタンスに含まれる変数の有効期間が終わった時点で、そのインスタンスの有効期間は終わります。

     構造体インスタンスの有効期間を延長することはできません。 静的構造体に相当する機能は、モジュールに用意されています。 詳細については、「[Module ステートメント](module-statement.md)」を参照してください。

     構造体メンバーの有効期間は、それを宣言する方法と場所で決まります。 詳細については、「[Class ステートメント](class-statement.md)」の「有効期間」を参照してください。

- **修飾**。 構造体の外部にあるコードでは、メンバーの名前をその構造体の名前で修飾する必要があります。

     入れ子構造体の内部のコードでプログラミング要素を修飾なしで参照した場合、Visual Basic はその要素をまず入れ子構造体の内部で探し、その次にコンテナー構造体の内部で探します。この手順が、最も外側のコンテナー要素にまで繰り返されます。 詳細については、「[宣言された要素の参照](../../programming-guide/language-features/declared-elements/references-to-declared-elements.md)」を参照してください。

- **メモリ使用量。** 他のすべての複合データ型と同様に、構造体の総メモリ使用量を計算する場合、各メンバーのストレージ割り当ての公称サイズを単に合計しただけでは安全ではありません。 さらに、メモリ内に格納される順序が宣言の順序と同じであると仮定するのも安全ではありません。 構造体のストレージ レイアウトを制御する必要がある場合は、<xref:System.Runtime.InteropServices.StructLayoutAttribute> 属性を `Structure` ステートメントに適用します。

## <a name="example"></a>例

次の例では、`Structure` ステートメントを使って、従業員に関連のある複数のデータを定義しています。 データ項目の機密性に応じて `Public`、`Friend`、`Private` の各メンバーを使用する方法が示されています。 プロシージャ、プロパティ、およびイベント メンバーも示されています。

[!code-vb[VbVbalrStatements#57](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#57)]

`Structure`s の使用方法の詳細については、「[Structure Variable](../../programming-guide/language-features/data-types/structure-variables.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [Class ステートメント](class-statement.md)
- [Interface ステートメント](interface-statement.md)
- [Module ステートメント](module-statement.md)
- [Dim ステートメント](dim-statement.md)
- [Const ステートメント](const-statement.md)
- [Enum ステートメント](enum-statement.md)
- [Event ステートメント](event-statement.md)
- [Operator ステートメント](operator-statement.md)
- [Property ステートメント](property-statement.md)
- [構造体とクラス](../../programming-guide/language-features/data-types/structures-and-classes.md)
