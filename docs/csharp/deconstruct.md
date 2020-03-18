---
title: タプルとその他の型の分解
description: タプルとその他の型を分解する方法について説明します。
ms.technology: csharp-fundamentals
ms.date: 07/18/2016
ms.assetid: 0b0c4b0f-4a47-4f66-9b8e-f5c63b195960
ms.openlocfilehash: 23d193faf9702628698fe558f6667aeb130e8916
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73100661"
---
# <a name="deconstructing-tuples-and-other-types"></a>タプルとその他の型の分解

タプルでは、軽量な処理でメソッドの呼び出しから複数の値を取得することができます。 ただし、タプルを取得した場合は、その個々の要素を処理する必要があります。 このような処理を要素ごとに行うことは手間がかかります。次に例を示します。 `QueryCityData` メソッドは 3 つのタプルを返し、その各要素は別の操作の変数に割り当てられます。

[!code-csharp[WithoutDeconstruction](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple1.cs)]

1 つのオブジェクトから複数のフィールドとプロパティの値を取得する処理も同様に煩雑です。メンバーごとにフィールドまたはプロパティの値を変数に割り当てる必要があります。

C# 7.0 以降、単一の*分解*操作で、タプルから複数の要素を取得したり、オブジェクトから複数のフィールド、プロパティ、および計算値を取得したりできるようになりました。 タプルを分解するときに、その要素を個々の変数に割り当てます。 オブジェクトを分解するときに、選択した値を個々の変数に割り当てます。

## <a name="deconstructing-a-tuple"></a>タプルの分解

C# には、タプルの分解を組み込みでサポートしているという特長があり、単一操作でタプル内のすべての項目を展開することができます。 タプルを分解する一般的な構文は、タプルを定義する構文と似ています。代入ステートメントの左側で変数をかっこで囲み、その各要素に割り当てられます。 たとえば、次のステートメントは、4 タプルの要素を 4 つの別の変数に割り当てます。

```csharp
var (name, address, city, zip) = contact.GetAddressInfo();
```

タプルの分解には 3 つの方法があります。

- かっこ内の各フィールドの型を明示的に宣言することができます。 次の例では、この方法を使用して、`QueryCityData` メソッドから返される 3 タプルを分解します。

    [!code-csharp[Deconstruction-Explicit](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple2.cs#1)]

- C# で各変数の型を推定するには、`var` キーワードを使用できます。 `var` キーワードはかっこの外に配置します。 次の例では、`QueryCityData` メソッドから返される 3 タプルを分解するときに型の推定を使用します。

    [!code-csharp[Deconstruction-Infer](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple3.cs#1)]

    また、かっこ内の変数宣言のいずれかまたはすべてについて、個々に `var` キーワードを使用することもできます。

    [!code-csharp[Deconstruction-Infer-Some](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple4.cs#1)]

    ただし、この処理は煩雑なため推奨されません。

- 最後に、既に宣言されている変数にタプルを分解することができます。

    [!code-csharp[Deconstruction-Declared](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-tuple5.cs#1)]

タプル内のフィールドすべての型が同じでも、かっこ外では指定できない型があることに注意してください。 この場合、コンパイル エラー CS8136 "分解 `変数 (...)` フォームは特定の種類の '変数' を許可しません" が生成されます。

また、タプルの各要素も変数に割り当てる必要があります。 いずれかの要素を省略すると、コンパイラでエラー CS8132 " 'x' 要素のタプルを 'y' 変数に分解することはできません" が生成されます。

分解の左側にある既存の変数に宣言と割り当てを混在させることはできません。 メンバーに新しく宣言された変数と既存の変数が含まれる場合、コンパイラでエラー CS8184 "分解の左側で宣言と式を混用できません" が生成されます。

## <a name="deconstructing-tuple-elements-with-discards"></a>破棄によるタプル要素の分解

タプルを分解する場合、一部の要素の値のみが必要なことがよくあります。 C# 7.0 以降、C# の*破棄*のサポートを利用できるようになりました。破棄は、無視対象と選択した値が指定された書き込み専用の変数です。 破棄を指定するには、割り当て内でアンダースコア文字 ("\_") を使用します。 必要に応じて任意の数の値を破棄できます。そのため、すべての値を 1 つの破棄 `_` で表すことができます。

破棄を含むタプルの使用例を次に示します。 `QueryCityDataForYears` メソッドは、市区町村名、その地域、年、市区町村のその年の人口、2 つ目の年、市区町村のその 2 つ目の年の人口という 6 つのタプルを返します。 この例は、2 つの年の間に変化した人口数を示しています。 タプルから使用できるデータのうち、市区町村の地域は使用しません。また、指定時に市区町村名と 2 つの日付はわかっています。 そのため、タプルに格納されている 2 つの人口値のみが必要であり、残りの値は破棄対象として処理できます。  

[!code-csharp[Tuple-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

## <a name="deconstructing-user-defined-types"></a>ユーザー定義型の分解

C# には、非タプル型を分解するためのサポートが組み込まれていません。 ただし、クラス、構造体、またはインターフェイスの作成者であれば、1 つまたは複数の `Deconstruct` メソッドを実装することで、型のインスタンスを分解することができます。 このメソッドからは void が返され、分解される各値はメソッド シグネチャの [out](language-reference/keywords/out-parameter-modifier.md) パラメーターで示されます。 たとえば、`Person` クラスの次の `Deconstruct` メソッドは、名、ミドル ネーム、姓を返します。

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#1)]

この場合、`p` という `Person` クラスのインスタンスは、次のような割り当てで分解できます。

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class1.cs#2)]

次の例では、`Deconstruct` メソッドをオーバーロードし、`Person` オブジェクトの多様な組み合わせのプロパティを返します。 各オーバーロードから以下が返されます。

- 名と姓。
- 名、姓、ミドル ネーム。
- 名、姓、市区町村名、州名。

[!code-csharp[Class-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-class2.cs)]

`Deconstruct` メソッドをオーバーロードすると、共通して 1 つのオブジェクトから抽出されたデータのグループを反映できるので、明確であいまいさのないシグネチャを使用して `Deconstruct` メソッドを定義するように気を付けてください。 異なる順序で同数の `out` パラメーター、または数と型が同じ `out` パラメーターを持つ `Deconstruct` メソッドが複数あると、混同する可能性があります。

次のオーバーロードされた `Deconstruct` メソッドは、混同が生じる原因になりうる例を示しています。 1 つ目のオーバーロードは、`Person` オブジェクトの名、ミドル ネーム、姓、年齢の順に返します。 2 つ目のオーバーロードは、名前のみの情報と年収を返しますが、名、ミドル ネーム、姓は異なる順序です。 この場合、`Person` インスタンスを分解するときに引数の順序を間違えやすくなります。

[!code-csharp[Deconstruct-ambiguity](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-ambiguous.cs)]

## <a name="deconstructing-a-user-defined-type-with-discards"></a>破棄によるユーザー定義型の分解

[タプル](#deconstructing-tuple-elements-with-discards)の場合と同様に、破棄を使用して、`Deconstruct` メソッドから返される項目のうち、選択した項目を無視できます。 各破棄は "\_" という変数で定義されます。また、1 つの破棄操作に複数の破棄を含めることができます。

`Person` オブジェクトを 4 つの文字列 (名、姓、市区町村、州) に分解し、姓と州を破棄する例を次に示します。

[!code-csharp[Class-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/class-discard1.cs#1)]

## <a name="deconstructing-a-user-defined-type-with-an-extension-method"></a>拡張メソッドによるユーザー定義型の分解

クラス、構造体、またはインターフェイスを作成していない場合でも、目的の値を返す `Deconstruct` [拡張メソッド](programming-guide/classes-and-structs/extension-methods.md)を 1 つまたは複数実装することで、このようなオブジェクトを分解することができます。

<xref:System.Reflection.PropertyInfo?displayProperty=nameWithType> クラスの `Deconstruct` 拡張メソッドを 2 つ定義する例を次に示します。 1 つ目の拡張メソッドは、型、静的かインスタンスか、読み取り専用かどうか、インデックスが作成されているかどうかなど、プロパティの特徴を示す値のセットを返します。 2 つ目の拡張メソッドは、プロパティのアクセシビリティを示します。 get アクセサーと set アクセサーのアクセシビリティは異なる可能性があるため、ブール値は、プロパティの get アクセサーと set アクセサーが異なるかどうか、また異なる場合はアクセシビリティが同じかどうかを示します。 アクセサーが 1 つのみの場合、または get アクセサーと set アクセサーのアクセシビリティが同じ場合、`access` 変数は、全体としてそのアクセシビリティのプロパティを示します。 それ以外の場合、get アクセサーと set アクセサーのアクセシビリティは `getAccess` 変数と `setAccess` 変数で示されます。

[!code-csharp[Extension-deconstruct](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/deconstruct-extension1.cs)]

## <a name="see-also"></a>関連項目

- [破棄](discards.md)
- [タプル](tuples.md)
