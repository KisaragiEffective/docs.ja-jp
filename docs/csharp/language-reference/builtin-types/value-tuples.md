---
title: タプル型 - C# リファレンス
description: 'C# のタプルについて学習する: 関連性の低いデータ要素をグループ化するために使用できる軽量データ構造'
ms.date: 07/09/2020
helpviewer_keywords:
- value tuples [C#]
ms.openlocfilehash: 3d79ab19117847e2364b154db33a1521416bb3f4
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174975"
---
# <a name="tuple-types-c-reference"></a>タプル型 (C# リファレンス)

C# 7.0 以降で利用できる "*タプル*" 機能により、軽量データ構造に複数のデータ要素をグループ化するための簡潔な構文が提供されています。 次の例は、タプル変数を宣言して初期化し、そのデータ メンバーにアクセスする方法を示しています。

[!code-csharp-interactive[tuple intro](snippets/ValueTuples.cs#Introduction)]

前の例で示したように、タプル型を定義するには、すべてのデータ メンバーの型と、必要に応じて[フィールド名](#tuple-field-names)を指定します。 タプル型でメソッドを定義することはできませんが、次の例に示すように、.NET によって提供されるメソッドを使用できます。

[!code-csharp-interactive[tuple methods](snippets/ValueTuples.cs#MethodOnTuples)]

C# 7.3 以降では、タプル型で[等値演算子](../operators/equality-operators.md) `==` と `!=` がサポートされます。 詳しくは、「[タプルの等値性](#tuple-equality)」セクションをご覧ください。

タプル型は[値型](value-types.md)であり、タプル要素はパブリック フィールドです。 そのため、タプルは "*変更可能な*" 値の型になります。

> [!NOTE]
> タプルの機能には、<xref:System.ValueTuple?displayProperty=nameWithType> 型と、.NET Core と .NET Framework 4.7 以降で使用できる、関連のジェネリック型 (たとえば、<xref:System.ValueTuple%602?displayProperty=nameWithType>) が必要です。 4\.6.2 以前の .NET Framework を対象とするプロジェクトでタプルを使用するには、NuGet パッケージ [`System.ValueTuple`](https://www.nuget.org/packages/System.ValueTuple/) をプロジェクトに追加します。

任意の多数の要素を持つタプルを定義できます。

[!code-csharp-interactive[large tuple](snippets/ValueTuples.cs#LargeTuple)]

## <a name="use-cases-of-tuples"></a>タプルのユース ケース

特に一般的なタプルのユース ケースの 1 つが、メソッドの戻り値の型です。 つまり、[`out` メソッド パラメーター](../keywords/out-parameter-modifier.md)を定義するのではなく、次の例のようにメソッドの結果をタプルの戻り値の型でグループ化できます。

[!code-csharp-interactive[multiple method outputs](snippets/ValueTuples.cs#MultipleReturns)]

前の例で示したように、返されたタプルのインスタンスを直接操作することも、別の変数に[分解する](#tuple-assignment-and-deconstruction)こともできます。

タプル型は[匿名型](../../programming-guide/classes-and-structs/anonymous-types.md)の代わりに、たとえば LINQ クエリで使用することもできます。 詳細については、「[匿名型またはタプル型の選択](../../../standard/base-types/choosing-between-anonymous-and-tuple.md)」を参照してください。

通常、タプルは関連性の低いデータ要素をグループ化するために使用します。 これは通常、非公開および内部のユーティリティ メソッド内で役に立ちます。 パブリック API の場合は、[クラス](../keywords/class.md)または[構造体](struct.md)型を定義することを検討してください。

## <a name="tuple-field-names"></a>タプルのフィールド名

次の例に示すように、タプルの初期化式またはタプルの型の定義でタプル フィールドの名前を明示的に指定できます。

[!code-csharp-interactive[explicit field names](snippets/ValueTuples.cs#ExplicitFieldNames)]

C# 7.1 以降では、フィールド名を指定しない場合、次の例に示すように、タプルの初期化式で対応する変数の名前からそれが推論される場合があります。

[!code-csharp-interactive[inferred field names](snippets/ValueTuples.cs#InferFieldNames)]

これはタプル プロジェクション初期化子と呼ばれます。 変数の名前は、次の場合、タプル フィールド名に投影されません。

- 候補名が `Item3`、`ToString`、`Rest` などのタプル型のメンバー名である。
- 候補名が、別のタプル フィールド名 (明示的または暗黙的のいずれか) の複製である。

このような場合は、フィールドの名前を明示的に指定するか、既定の名前でフィールドにアクセスします。

タプル フィールドの既定の名前は、`Item1`、`Item2`、`Item3` などです。 次の例に示すように、フィールド名が明示的に指定されていたり推論されていたりする場合でも、フィールドの既定の名前をいつでも使用できます。

[!code-csharp-interactive[default field names](snippets/ValueTuples.cs#DefaultFieldNames)]

[タプルの代入](#tuple-assignment-and-deconstruction)と[タプルの等価比較](#tuple-equality)では、フィールド名が考慮されません。

コンパイル時に、コンパイラは既定以外のフィールド名を、対応する既定の名前に置き換えます。 このため、明示的に指定された、または推論されたフィールド名は実行時に使用できません。

## <a name="tuple-assignment-and-deconstruction"></a>タプルの代入と分解

C# では、次の両方の条件を満たすタプル型間の代入がサポートされます。

- 両方のタプル型に、同じ数の要素がある
- タプルのそれぞれの位置で、右側のタプル要素の型が対応する左側のタプル要素の型と同じか、暗黙的に変換可能である

タプル要素の値は、タプル要素の順序に従って代入されます。 次の例に示すように、タプル フィールドの名前は無視され、代入されません。

[!code-csharp-interactive[tuple assignment](snippets/ValueTuples.cs#Assignment)]

また、代入演算子 `=` を使用して、別の変数でタプル インスタンスを "*分解する*" こともできます。 これは、次の方法のいずれかで実行できます。

- かっこ内の各変数の型を明示的に宣言する。

  [!code-csharp-interactive[specify types of variables](snippets/ValueTuples.cs#DeconstructExplicit)]

- かっこの外にある `var` キーワードを使用して、暗黙的に型指定された変数を宣言し、コンパイラによってその型が推論されるようにする。

  [!code-csharp-interactive[implicitly typed variables](snippets/ValueTuples.cs#DeconstructVar)]

- 既存の変数を使用する。

  [!code-csharp-interactive[existing variables](snippets/ValueTuples.cs#DeconstructExisting)]

タプルとその他の型の分解の詳細については、「[タプルとその他の型の分解](../../deconstruct.md)」を参照してください。

## <a name="tuple-equality"></a>タプルの等価性

C# 7.3 以降では、タプル型で `==` および `!=` 演算子がサポートされます。 これらの演算子により、左側のオペランドのメンバーが、タプル要素の順序に従って、右側のオペランドの対応するメンバーと比較されます。

[!code-csharp-interactive[tuple equality](snippets/ValueTuples.cs#TupleEquality)]

前の例で示したように、`==` と `!=` の演算では、タプルのフィールド名は考慮されません。

2 つのタプルは、次の両方の条件が満たされている場合に比較できます。

- 両方のタプルが、同じ数の要素を保持している。 たとえば、`t1` と `t2` の要素数が異なる場合、`t1 != t2` はコンパイルされません。
- タプルの位置ごとに、左側と右側のタプルのオペランドの対応する要素が、`==` と `!=` の演算子と比較されます。 たとえば、`1` は `(1, 2)` と比較できないため、`(1, (2, 3)) == ((1, 2), 3)` はコンパイルされません。

`==` と `!=` の演算子によって、タプルがショートサーキット方式で比較されます。 つまり、等しくない要素のペアか、タプルの端に到達するとすぐに、演算が停止します。 ただし、比較の前には必ず、次の例に示すように、"*すべての*" タプル要素が評価されます。

[!code-csharp-interactive[tuple element evaluation](snippets/ValueTuples.cs#TupleEvaluationForEquality)]

## <a name="tuples-as-out-parameters"></a>out パラメーターとしてのタプル

通常は、[`out` パラメーター](../keywords/out-parameter-modifier.md)を持つメソッドを、タプルを返すメソッドにリファクタリングします。 ただし、`out` パラメーターがタプル型である場合もあります。 次の例に、`out` パラメーターとしてタプルを操作する方法を示します。

[!code-csharp-interactive[tuple as out parameter](snippets/ValueTuples.cs#TupleAsOutParameter)]

## <a name="tuples-vs-systemtuple"></a>タプルと `System.Tuple`

<xref:System.ValueTuple?displayProperty=nameWithType> 型によってサポートされる C# のタプルは、<xref:System.Tuple?displayProperty=nameWithType> タプルで表されるタプルとは異なります。 主な相違点は、次のとおりです。

- `ValueTuple` 型は[値の型](value-types.md)です。 `Tuple` 型は[参照型](../keywords/reference-types.md)です。
- `ValueTuple` 型は変更可能です。 `Tuple` 型は変更不可能です。
- `ValueTuple` 型のデータ メンバーはフィールドです。 `Tuple` 型のデータ メンバーはプロパティです。

## <a name="c-language-specification"></a>C# 言語仕様

詳しくは、次の機能提案メモをご覧ください。

- [タプル名 (別名タプル プロジェクション初期化子) を推測する](~/_csharplang/proposals/csharp-7.1/infer-tuple-names.md)
- [タプル型での `==` と `!=` のサポート](~/_csharplang/proposals/csharp-7.3/tuple-equality.md)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [値型](value-types.md)
- [匿名型またはタプル型の選択](../../../standard/base-types/choosing-between-anonymous-and-tuple.md)
- <xref:System.ValueTuple?displayProperty=nameWithType>
