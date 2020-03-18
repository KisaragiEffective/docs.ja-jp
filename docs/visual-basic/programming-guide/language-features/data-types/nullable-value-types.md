---
title: null 許容値型
ms.date: 07/20/2015
f1_keywords:
- vb.Nullable
helpviewer_keywords:
- nullable types [Visual Basic]
- '? [Visual Basic]'
- types [Visual Basic], nullable
- nullable types [Visual Basic]
- data types [Visual Basic], nullable
ms.assetid: 9ac3b602-6f96-4e6d-96f7-cd4e81c468a6
ms.openlocfilehash: 0d259e11a969f4eb7e64626a4adf498db06ece06
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347835"
---
# <a name="nullable-value-types-visual-basic"></a>null 許容値型 (Visual Basic)

場合によっては、特定の状況で値が定義されていない値型を使用することがあります。 たとえば、データベースのフィールドは、値が割り当てられていない、意味のある値が割り当てられているかを区別する必要がある場合があります。 値型は、通常の値または null 値を受け取るように拡張できます。 このような拡張機能は、 *null 許容型*と呼ばれます。

各 null 許容型は、ジェネリック <xref:System.Nullable%601> 構造体から構築されます。 作業に関連するアクティビティを追跡するデータベースについて考えてみましょう。 次の例では、null 許容の `Boolean` 型を構築し、その型の変数を宣言します。 宣言は、次の3つの方法で記述できます。

[!code-vb[VbVbalrNullableValue#1](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#1)]

変数 `ridesBusToWork` には `True`の値、`False`の値、または値を指定できません。 初期の既定値はまったく値ではありません。この場合は、このユーザーに対して情報がまだ取得されていないことを意味します。 これに対して、`False` 情報が取得されていて、ユーザーがバスを使用できないことを意味する可能性があります。

Null 許容型を使用して変数とプロパティを宣言できます。また、null 許容型の要素を使用して配列を宣言できます。 Null 許容型を持つプロシージャをパラメーターとして宣言し、`Function` プロシージャから null 許容型を返すことができます。

配列、`String`、クラスなどの参照型に対して null 許容型を構築することはできません。 基になる型は、値型である必要があります。 詳細については、「 [Value Types and Reference Types](value-types-and-reference-types.md)」を参照してください。

## <a name="using-a-nullable-type-variable"></a>Null 許容型変数の使用

Null 許容型の最も重要なメンバーは、その <xref:System.Nullable%601.HasValue%2A> と <xref:System.Nullable%601.Value%2A> のプロパティです。 Null 許容型の変数の場合、<xref:System.Nullable%601.HasValue%2A> は、変数に定義済みの値が含まれているかどうかを示します。 <xref:System.Nullable%601.HasValue%2A> が `True`場合は、<xref:System.Nullable%601.Value%2A>から値を読み取ることができます。 <xref:System.Nullable%601.HasValue%2A> と <xref:System.Nullable%601.Value%2A> はどちらも `ReadOnly` プロパティであることに注意してください。

### <a name="default-values"></a>既定値

Null 許容型の変数を宣言すると、その <xref:System.Nullable%601.HasValue%2A> プロパティの既定値 `False`が使用されます。 これは、既定では、基になる値の型の既定値ではなく、変数に定義された値がないことを意味します。 次の例では、`Integer` 型の既定値が0の場合でも、変数 `numberOfChildren` 最初は定義済みの値を持ちません。

[!code-vb[VbVbalrNullableValue#2](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#2)]

Null 値は、未定義または不明な値を示すために役立ちます。 `numberOfChildren` が `Integer`として宣言されている場合、情報が現在使用できないことを示す値は存在しません。

### <a name="storing-values"></a>格納 (値を)

値は、通常の方法で null 許容型の変数またはプロパティに格納します。 次の例では、前の例で宣言した `numberOfChildren` 変数に値を代入します。

[!code-vb[VbVbalrNullableValue#3](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#3)]

Null 許容型の変数またはプロパティに定義済みの値が含まれている場合は、値が割り当てられていない初期状態に戻すことができます。 これを行うには、次の例に示すように、変数またはプロパティを `Nothing`に設定します。

[!code-vb[VbVbalrNullableValue#4](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#4)]

> [!NOTE]
> `Nothing` を null 許容型の変数に割り当てることはできますが、等号を使用して `Nothing` をテストすることはできません。 等号 (`someVar = Nothing`) を使用する比較では、常に `Nothing`として評価されます。 `False`に対して変数の <xref:System.Nullable%601.HasValue%2A> プロパティをテストしたり、`Is` または `IsNot` 演算子を使用してテストしたりできます。

### <a name="retrieving-values"></a>取得 (値を)

Null 許容型の変数の値を取得するには、まずその <xref:System.Nullable%601.HasValue%2A> プロパティをテストして、値があることを確認する必要があります。 <xref:System.Nullable%601.HasValue%2A> が `False`ときに値を読み込んだ場合、Visual Basic は <xref:System.InvalidOperationException> 例外をスローします。 次の例は、前の例の変数 `numberOfChildren` を読み取るための推奨される方法を示しています。

[!code-vb[VbVbalrNullableValue#5](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#5)]

## <a name="comparing-nullable-types"></a>Null 許容型の比較

Null 値を許容する `Boolean` 変数がブール式で使用される場合、結果は `True`、`False`、または `Nothing`になります。 `And` と `Or`の真理テーブルを次に示します。 `b1` と `b2` は3つの値を持つことができるため、評価するのは9つの組み合わせです。

|b|b2|b1 と b2|b1 または b2|
|--------|--------|---------------|--------------|
|`Nothing`|`Nothing`|`Nothing`|`Nothing`|
|`Nothing`|`True`|`Nothing`|`True`|
|`Nothing`|`False`|`False`|`Nothing`|
|`True`|`Nothing`|`Nothing`|`True`|
|`True`|`True`|`True`|`True`|
|`True`|`False`|`False`|`True`|
|`False`|`Nothing`|`False`|`Nothing`|
|`False`|`True`|`False`|`True`|
|`False`|`False`|`False`|`False`|

ブール型の変数または式の値が `Nothing`場合、`true` も `false`もありません。 例を次に示します。

[!code-vb[VbVbalrNullableValue#6](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#6)]

この例では、`b1 And b2` が `Nothing`に評価されます。 その結果、各 `If` ステートメントで `Else` 句が実行され、出力は次のようになります。

`Expression is not true`

`Expression is not false`

> [!NOTE]
> ショートサーキット評価を使用する `AndAlso` および `OrElse`では、最初のが `Nothing`に評価されるときに、2番目のオペランドを評価する必要があります。

## <a name="propagation"></a>伝達

算術演算、比較演算子、シフト演算、または型演算のオペランドの一方または両方が null 値を許容する場合、演算の結果も null 値を許容します。 両方のオペランドの値が `Nothing`でない場合は、オペランドの基になる値に対して演算が実行されますが、どちらも null 許容型ではありません。 次の例では、変数 `compare1` および `sum1` が暗黙的に型指定されています。 これらの上にマウスポインターを置くと、その両方の null 許容型がコンパイラによって推論されます。

[!code-vb[VbVbalrNullableValue#7](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#7)]

一方または両方のオペランドの値が `Nothing`の場合、結果は `Nothing`されます。

[!code-vb[VbVbalrNullableValue#8](../../../../../samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrNullableValue/VB/Class1.vb#8)]

## <a name="using-nullable-types-with-data"></a>データでの Null 許容型の使用

データベースは、null 許容型を使用する最も重要な場所の1つです。 現在、すべてのデータベースオブジェクトが null 許容型をサポートしているわけではありませんが、デザイナーによって生成されたテーブルアダプターでは、 「 [TableAdapter による null 許容型のサポート」を](/visualstudio/data-tools/fill-datasets-by-using-tableadapters#tableadapter-support-for-nullable-types)参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.InvalidOperationException>
- <xref:System.Nullable%601.HasValue%2A>
- [データの種類](index.md)
- [Value Types and Reference Types](value-types-and-reference-types.md)
- [トラブルシューティング (データ型)](troubleshooting-data-types.md)
- [TableAdapters を使用してデータセットを入力する](/visualstudio/data-tools/fill-datasets-by-using-tableadapters)
- [If 演算子](../../../language-reference/operators/if-operator.md)
- [ローカル型の推論](../variables/local-type-inference.md)
- [Is 演算子](../../../language-reference/operators/is-operator.md)
- [IsNot 演算子](../../../language-reference/operators/isnot-operator.md)
- [Null 許容値型C#()](../../../../csharp/language-reference/builtin-types/nullable-value-types.md)
