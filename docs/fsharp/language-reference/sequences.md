---
title: シーケンス
description: シーケンスを使用F#する方法について説明します。データの大きな順序で並べ替えられたコレクションがあり、必ずしもすべての要素を使用するとは限りません。
ms.date: 11/04/2019
ms.openlocfilehash: 34e03f1cead0a9f678f637afcb6c8397ef7572bc
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73971442"
---
# <a name="sequences"></a>シーケンス

> [!NOTE]
> この記事の API リファレンスのリンクをクリックすると MSDN に移動します。  docs.microsoft.com API リファレンスは完全ではありません。

*シーケンス*は、すべて1つの型からなる論理的な一連の要素です。 シーケンスは、大量の順序付けられたデータのコレクションがあり、すべての要素を使用するとは限らない場合に特に便利です。 個々のシーケンス要素は必要に応じてのみ計算されるので、すべての要素が使用されるわけではありません。 シーケンスは、<xref:System.Collections.Generic.IEnumerable%601>のエイリアスである `seq<'T>` 型によって表されます。 したがって、<xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装するすべての .NET 型をシーケンスとして使用できます。 [Seq モジュール](https://msdn.microsoft.com/library/54e8f059-ca52-4632-9ae9-49685ee9b684)は、シーケンスに関連する操作のサポートを提供します。

## <a name="sequence-expressions"></a>シーケンス式

*シーケンス式*は、シーケンスに評価される式です。 シーケンス式は、さまざまな形式を取ることができます。 最も単純な形式は範囲を指定します。 たとえば、`seq { 1 .. 5 }` は、エンドポイント1と5を含む5つの要素を含むシーケンスを作成します。 2つの2つの期間の間にインクリメント (またはデクリメント) を指定することもできます。 たとえば、次のコードでは、10の倍数のシーケンスが作成されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1502.fs)]

シーケンス式は、シーケンスのF#値を生成する式で構成されます。 プログラムで値を生成することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1503.fs)]

前のサンプルでは、`->` 演算子を使用しています。これにより、値がシーケンスの一部になる式を指定できます。 `->` は、その後に続くコードのすべての部分が値を返す場合にのみ使用できます。

または、次に示すオプションの `yield` を使用して、`do` キーワードを指定することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1504.fs)]

次のコードでは、グリッドを表す配列へのインデックスと共に、座標ペアのリストが生成されます。 最初の `for` 式では、`do` を指定する必要があることに注意してください。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1505.fs)]

シーケンスで使用される `if` 式は、フィルターです。 たとえば、`int -> bool`型の関数 `isprime` があると仮定して、素数のみのシーケンスを生成するには、次のようにシーケンスを構築します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1506.fs)]

前に説明したように、ここでは `do` が必要です。 `if`を持つ `else` 分岐が存在しないためです。 `->`を使用しようとすると、すべてのブランチが値を返すわけではないというエラーが表示されます。

## <a name="the-yield-keyword"></a>`yield!` キーワード

場合によっては、要素のシーケンスを別のシーケンスに含めることが必要になることがあります。 シーケンスを別のシーケンス内に含めるには、`yield!` キーワードを使用する必要があります。

```fsharp
// Repeats '1 2 3 4 5' ten times
seq {
    for _ in 1..10 do
        yield! seq { 1; 2; 3; 4; 5}
}
```

`yield!` を検討するもう1つの方法は、内部シーケンスを平坦化し、それを含んでいるシーケンスに含めることです。

`yield!` が式で使用されている場合、他のすべての単一の値では `yield` キーワードを使用する必要があります。

```fsharp
// Combine repeated values with their values
seq {
    for x in 1..10 do
        yield x
        yield! seq { for i in 1..x -> i}
}
```

前の例で `x` だけを指定すると、シーケンスに値が生成されなくなります。

## <a name="examples"></a>使用例

最初の例では、反復処理、フィルター、および yield を含むシーケンス式を使用して、配列を生成します。 このコードは、1から100までの一連の素数をコンソールに出力します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1507.fs)]

次の例では、3つの要素の組で構成される乗算テーブルを作成します。各要素は2つの要素と製品で構成されています。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1508.fs)]

次の例では、`yield!` を使用して、個々のシーケンスを1つの最終シーケンスに結合する方法を示します。 この場合、バイナリツリー内の各サブツリーのシーケンスは、最後のシーケンスを生成するために再帰関数で連結されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1509.fs)]

## <a name="using-sequences"></a>シーケンスの使用

シーケンスは、[リスト](lists.md)と同じ機能の多くをサポートします。 シーケンスは、キー生成関数を使用したグループ化やカウントなどの操作もサポートします。 シーケンスは、サブシーケンスを抽出するためのさまざまな関数もサポートします。

リスト、配列、セット、マップなどの多くのデータ型は、列挙可能なコレクションであるため、暗黙的にシーケンスされます。 引数としてシーケンスを受け取る関数は、`System.Collections.Generic.IEnumerable<'T>`を実装するすべてF#の .net データ型に加えて、共通のデータ型のいずれかで動作します。 リストを引数として受け取る関数と比較します。この関数はリストを受け取ることしかできません。 型 `seq<'T>` は `IEnumerable<'T>`の型略称です。 これは、のF#配列、リスト、セット、マップを含むジェネリック `System.Collections.Generic.IEnumerable<'T>`を実装する型、およびほとんどの .net コレクション型は、`seq` の型と互換性があり、シーケンスが必要な場合に使用できることを意味します。

## <a name="module-functions"></a>モジュール関数

[Fsharp.core 名前空間](https://msdn.microsoft.com/library/24f64e5f-5030-47d0-9759-8d3e398ed13f)の[Seq モジュール](https://msdn.microsoft.com/library/54e8f059-ca52-4632-9ae9-49685ee9b684)には、シーケンスを操作するための関数が含まれています。 これらの関数は、リスト、配列、マップ、およびセットとも連動します。これらの型はすべて列挙可能であるため、シーケンスとして扱うことができます。

## <a name="creating-sequences"></a>シーケンスの作成

シーケンス式を使用してシーケンスを作成するには、前に説明したように、または特定の関数を使用します。

空のシーケンスは、 [seq](https://msdn.microsoft.com/library/3c7f1c69-6117-4782-b2da-0e04d6854f59)を使用して作成できます。また、 [seq. シングルトン](https://msdn.microsoft.com/library/9b8cc460-a282-4ec5-b29a-630ab17e9de7)を使用して、指定した要素のシーケンスを1つだけ作成することもできます。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet9.fs)]

[Seq. init](https://msdn.microsoft.com/library/059de69d-812c-4f8e-be86-88aa72101576)を使用して、指定した関数を使用して要素が作成されるシーケンスを作成できます。 シーケンスのサイズも指定します。 この関数は、シーケンスを反復処理するまで要素が作成されない点を除いて、 [List. init](https://msdn.microsoft.com/library/dd38c096-0ea8-4858-be6b-794b90418b83)と同じです。 次のコードは、`Seq.init` の使用例です。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet10.fs)]

出力は次のようになります。

```console
0 10 20 30 40
```

[Seq. ofArray](https://msdn.microsoft.com/library/299cd4d9-be72-4511-aac8-089e1ddaac99)と[Seq. ofarray&#60;&#62; t 関数](https://msdn.microsoft.com/visualfsharpdocs/conceptual/seq.oflist%5b%27t%5d-function-%5bfsharp%5d)を使用すると、配列とリストからシーケンスを作成できます。 ただし、キャスト演算子を使用して、配列とリストをシーケンスに変換することもできます。 次のコードは、両方の手法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet11.fs)]

[Seq](https://msdn.microsoft.com/library/1d087db3-a8b2-41dd-8ddc-227544529334)を使用することにより、`System.Collections`で定義されているような、弱く型指定されたコレクションからシーケンスを作成できます。 このように弱い型指定のコレクションには `System.Object` 要素型があり、非ジェネリック `System.Collections.Generic.IEnumerable&#96;1` 型を使用して列挙されます。 次のコードは、`Seq.cast` を使用して `System.Collections.ArrayList` をシーケンスに変換する方法を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet12.fs)]

無限のシーケンスを定義するには、 [Seq. initinfinite](https://msdn.microsoft.com/library/d1804e53-da92-48ec-8d6e-57eaf4c62bef)関数を使用します。 このようなシーケンスでは、要素のインデックスから各要素を生成する関数を指定します。 無限のシーケンスは、レイジー評価のために可能です。要素は、必要に応じて、指定した関数を呼び出すことによって作成されます。 次のコード例では、浮動小数点数の無限のシーケンスが生成されます。この例では、連続した整数の2乗の reciprocals が連続して作成されています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet13.fs)]

[Seq](https://msdn.microsoft.com/library/7d9232fc-742e-42bc-bdf7-6f130f0eff21)は、状態を受け取るコンピュテーション関数からシーケンスを生成し、シーケンス内の後続の各要素を生成するように変換します。 状態は、各要素を計算するために使用される値であり、各要素が計算されるたびに変更できます。 `Seq.unfold` の2番目の引数は、シーケンスを開始するために使用される初期値です。 `Seq.unfold` では、状態にオプションの種類を使用します。これにより、`None` 値を返すことによってシーケンスを終了できます。 次のコードは、`unfold` 操作によって生成される、`seq1` と `fib`のシーケンスの2つの例を示しています。 1つ目の `seq1`は、数が20までの単純なシーケンスです。 2番目の `fib`では、`unfold` を使用してフィボナッチシーケンスを計算します。 フィボナッチシーケンス内の各要素は前の2つのフィボナッチ数の合計であるため、状態の値は、シーケンス内の前の2つの数値で構成される組になります。 初期値は `(1,1)`、シーケンスの最初の2つの数値です。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet14.fs)]

出力は次のとおりです。

```console
The sequence seq1 contains numbers from 0 to 20.

0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19 20

The sequence fib contains Fibonacci numbers.

2 3 5 8 13 21 34 55 89 144 233 377 610 987 1597
```

次のコードは、ここで説明するシーケンスモジュール関数の多くを使用して無限シーケンスの値を生成および計算する例です。 コードの実行には数分かかる場合があります。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet15.fs)]

## <a name="searching-and-finding-elements"></a>要素の検索と検索

シーケンスは、リストで使用できる機能をサポートし[ています](https://msdn.microsoft.com/library/428c97bf-599d-4c39-a5b9-f8717c198ad1)。 [list.exists2](https://msdn.microsoft.com/library/efdf14a4-27f7-4dc1-9281-52639e66d565)、 [seq、Seq](https://msdn.microsoft.com/library/02c21ecd-97e5-4e99-a4c1-b4d0b730b7d8)、seq。 [findindex](https://msdn.microsoft.com/library/96dfe86b-df15-4d92-8316-7cd6055e09f3) [、Seq](https://msdn.microsoft.com/library/a87bc771-55f7-43f9-94f9-33d8f9bf325d). [tryfind](https://msdn.microsoft.com/library/ac43c6f5-4dc7-4e9a-a222-00b5736aee47)、および[seq. tryfindindex](https://msdn.microsoft.com/library/c357b221-edf6-4f68-bf40-82a3156d945a)。 シーケンスで使用できるこれらの関数のバージョンによって、シーケンスは検索対象の要素までのみ評価されます。 例については、「[リスト](https://msdn.microsoft.com/library/83102799-f251-42e1-93ef-64232e8c5b1d)」を参照してください。

## <a name="obtaining-subsequences"></a>サブシーケンスの取得

[Seq. filter](https://msdn.microsoft.com/library/7f2e9850-a660-460c-9831-3bbff5613770)と[seq. choose](https://msdn.microsoft.com/library/63b83b06-4b24-4239-bf69-a2c12d891395)は、リストで使用できる対応する関数と似ています。ただし、フィルター処理と選択は、シーケンス要素が評価されるまで発生しません。

[Seq. truncate](https://msdn.microsoft.com/library/1892dfeb-308e-45e2-857a-3c3405d02244)は、別のシーケンスからシーケンスを作成しますが、シーケンスは指定した数の要素に制限されます。 [Seq. take](https://msdn.microsoft.com/library/6e75f701-640b-4c4a-9d63-4313fc090596)は、シーケンスの先頭から指定された数の要素のみを含む新しいシーケンスを作成します。 指定した数よりも多くの要素がシーケンス内にある場合、`Seq.take` は `System.InvalidOperationException`をスローします。 `Seq.take` と `Seq.truncate` の違いは、要素の数が指定した数よりも小さい場合、`Seq.truncate` によってエラーが生成されないことです。

次のコードは、`Seq.truncate` と `Seq.take`の動作を示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet16.fs)]

エラーが発生する前の出力は次のとおりです。

```console
1 4 9 16 25
1 4 9 16 25 36 49 64 81 100
1 4 9 16 25
1 4 9 16 25 36 49 64 81 100
```

Seq を使用することにより、述語関数 (ブール関数) を指定し、述語を `true`元のシーケンスの要素で構成される別のシーケンスからシーケンスを作成し、の最初の要素の前に停止し[ます。](https://msdn.microsoft.com/library/19eea4ce-66e0-4353-b015-72eb03421d92)述語から `false`が返されます。 [Seq. skip](https://msdn.microsoft.com/library/b4eb3f08-8594-4d17-8180-852c6c688bf1)は、指定された数の別のシーケンスの最初の要素をスキップし、残りの要素を返すシーケンスを返します。 [Seq. skipWhile](https://msdn.microsoft.com/library/fb729021-2a3c-430f-83c3-0b37526f1a16)は、述語が `true`返す限り、別のシーケンスの最初の要素をスキップし、残りの要素を返すシーケンスを返します。これは、述語が `false`返す最初の要素から始まります。

次のコード例は、の動作と、`Seq.takeWhile`、`Seq.skip`、および `Seq.skipWhile`の違いを示しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet17.fs)]

出力は次のとおりです。

```console
1 4 9
36 49 64 81 100
16 25 36 49 64 81 100
```

## <a name="transforming-sequences"></a>シーケンスの変換

[Seq (ペア](https://msdn.microsoft.com/library/210dcf26-4e24-4d83-af6d-a8288b2ae4b1)) は、入力シーケンスの連続する要素を組にグループ化する新しいシーケンスを作成します。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet18.fs)]

[Seq。ウィンドウ](https://msdn.microsoft.com/library/8b565b8f-d645-4dba-be22-099075fe4744)は `Seq.pairwise`に似ていますが、組のシーケンスを生成する代わりに、シーケンスから隣接する要素 (*ウィンドウ*) のコピーを格納する配列のシーケンスが生成されます。 各配列で隣接する要素の数を指定します。

次のコード例は、`Seq.windowed` の使用方法を示します。 この場合、ウィンドウ内の要素の数は3です。 この例では、前のコード例で定義されている `printSeq`を使用します。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet180.fs)]

出力は次のとおりです。

初期シーケンス:

```console
1.0 1.5 2.0 1.5 1.0 1.5

Windows of length 3:
[|1.0; 1.5; 2.0|] [|1.5; 2.0; 1.5|] [|2.0; 1.5; 1.0|] [|1.5; 1.0; 1.5|]

Moving average:
1.5 1.666666667 1.5 1.333333333
```

## <a name="operations-with-multiple-sequences"></a>複数のシーケンスを持つ操作

[Seq](https://msdn.microsoft.com/library/0a5df8bf-0d48-44ce-bff4-e8ef1df5bca4)と[array.zip3](https://msdn.microsoft.com/library/ef13bebb-22ae-4eb9-873b-87dd29154d16)は、2つまたは3つのシーケンスを受け取り、組のシーケンスを生成します。 これらの関数は、[リスト](https://msdn.microsoft.com/library/83102799-f251-42e1-93ef-64232e8c5b1d)で使用できる対応する関数に似ています。 1つのシーケンスを2つ以上のシーケンスに分割する対応する機能はありません。 シーケンスにこの機能が必要な場合は、シーケンスをリストに変換し、 [unzip](https://msdn.microsoft.com/library/639db80c-41b5-45bb-a6b4-1eaa04d61d21)を使用します。

## <a name="sorting-comparing-and-grouping"></a>並べ替え、比較、およびグループ化

リストでサポートされている並べ替え関数は、シーケンスでも使用できます。 これに[は、](https://msdn.microsoft.com/library/327ea595-e77c-4529-b61e-8c6cbf5ec92e) SortBy および[seq](https://msdn.microsoft.com/library/4f8b4fb9-bf20-49d9-b4ee-dcc906c8208f)が含まれます。 これらの関数は、シーケンス全体を反復処理します。

2つのシーケンスを比較するには、 [Seq. compareWith](https://msdn.microsoft.com/library/5a740135-0b3a-4545-816f-8f91cc31290f)関数を使用します。 関数は、連続する要素を順番に比較し、最初の等しくないペアが検出されたときに停止します。 その他の要素は、比較には関与しません。

`Seq.compareWith` の使用方法を次のコードに示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet19.fs)]

前のコードでは、最初の要素のみが計算され、検査され、結果は-1 になります。

[CountBy](https://msdn.microsoft.com/library/721702a5-150e-4fe8-81cd-ffbf8476cc1f)は、各要素の*キー*と呼ばれる値を生成する関数を受け取ります。 各要素に対してこの関数を呼び出すことにより、各要素に対してキーが生成されます。 次に、キー値を含むシーケンスと、キーの各値を生成した要素の数を返します。 `Seq.countBy`

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet201.fs)]

出力は次のとおりです。

```console
(1, 34) (2, 33) (0, 33)
```

前の出力は、キー1、キー2を生成した33値、およびキー0を生成した33値を生成した、元のシーケンスの34要素があることを示しています。

シーケンスの要素をグループ化するには、 [Seq. groupBy](https://msdn.microsoft.com/library/d46a04df-1a42-40cc-a368-058c9c5806fd)を呼び出します。 `Seq.groupBy` は、要素からキーを生成するシーケンスと関数を受け取ります。 関数は、シーケンスの各要素に対して実行されます。 `Seq.groupBy` は組のシーケンスを返します。各組の最初の要素はキーで、2番目の要素はそのキーを生成する要素のシーケンスです。

次のコード例では、`Seq.groupBy` を使用して、0、1、および2の個別のキー値を持つ3つのグループに、1 ~ 100 の一連の数値を分割しています。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet202.fs)]

出力は次のとおりです。

```console
(1, seq [1; 4; 7; 10; ...]) (2, seq [2; 5; 8; 11; ...]) (0, seq [3; 6; 9; 12; ...])
```

[Seq](https://msdn.microsoft.com/library/99d01014-7e0e-4e7b-9d0a-41a61d93f401)を呼び出すことで、重複する要素を排除するシーケンスを作成できます。 または、 [seq.distinctby](https://msdn.microsoft.com/library/9293293b-9420-49c8-848f-401a9cd49b75)を使用することもできます。この場合、各要素に対して呼び出されるキー生成関数を受け取ります。 結果のシーケンスには、一意のキーを持つ元のシーケンスの要素が含まれます。それより前の要素に対して重複するキーを生成する要素は破棄されます。

`Seq.distinct` の使用方法を次のコード例に示します。 `Seq.distinct` は、バイナリ数値を表すシーケンスを生成し、個別の要素だけが0と1であることを示すことによって示されます。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet22.fs)]

次のコードは、負の数値と正の数値を格納し、キー生成関数として絶対値関数を使用するシーケンスから開始することによって `Seq.distinctBy` を示しています。 結果のシーケンスには、シーケンスの負の数値に対応する正の数値がすべて不足しています。負の数値はシーケンスの前に表示されるため、絶対値が同じ正の数値の代わりに選択されるためです。値、またはキー。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet23.fs)]

## <a name="readonly-and-cached-sequences"></a>読み取り専用とキャッシュされたシーケンス

[Seq. readonly](https://msdn.microsoft.com/library/88059cb4-3bb0-4126-9448-fbcd48fe13a7)は、シーケンスの読み取り専用コピーを作成します。 `Seq.readonly` は、配列などの読み取り/書き込みコレクションがあり、元のコレクションを変更したくない場合に便利です。 この関数は、データのカプセル化を維持するために使用できます。 次のコード例では、配列を含む型が作成されます。 プロパティは配列を公開しますが、配列を返す代わりに、`Seq.readonly`を使用して配列から作成されたシーケンスを返します。

[!code-fsharp[Main](~/samples/snippets/fsharp/fssequences/snippet24.fs)]

[Seq. cache](https://msdn.microsoft.com/library/d197f9cc-08bf-4986-9869-246e72ca73f0)は、シーケンスの格納されているバージョンを作成します。 シーケンスの再評価を避けるため、またはシーケンスを使用する複数のスレッドがある場合は、`Seq.cache` を使用します。ただし、各要素が1回だけ実行されるようにする必要があります。 複数のスレッドで使用されているシーケンスがある場合は、元のシーケンスの値を列挙して計算するスレッドを1つ持つことができ、残りのスレッドはキャッシュされたシーケンスを使用できます。

## <a name="performing-computations-on-sequences"></a>シーケンスに対する計算の実行

単純な算術演算は、 [seq. average](https://msdn.microsoft.com/library/609d793b-c70f-4e36-9ab4-d928056d65b8)、seq. [sum](https://msdn.microsoft.com/library/01208515-4880-4358-91f5-af34f66dc77a)、seq. [AverageBy](https://msdn.microsoft.com/library/47c855c1-2dbd-415a-885e-b909d9d3e4f8)、 [seq. sumby](https://msdn.microsoft.com/library/68cca78c-94ed-4a45-9b8d-34d2c5f2b1b1)などのリストのようなものです。

[Seq](https://msdn.microsoft.com/library/30c4c95a-9563-4c96-bbe1-f7aacfd026e3)、 [Seq](https://msdn.microsoft.com/library/a2ad4f64-ac69-47d2-92f0-7173d9dfeae9)、および[seq. scan](https://msdn.microsoft.com/library/7e2d23e9-f153-4411-a884-b6d415ff627e)は、リストで使用できる対応する関数に似ています。 シーケンスは、サポートを一覧表示するこれらの関数の全バリエーションのサブセットをサポートします。 詳細と例については、「[リスト](lists.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [F# 言語リファレンス](index.md)
- [F# の型](fsharp-types.md)
