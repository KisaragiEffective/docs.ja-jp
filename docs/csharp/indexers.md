---
title: インデクサー
description: C# のインデクサーとインデックス付きプロパティの実装方法を説明します。これらのプロパは、1 つまたは複数の引数を使用して参照されます。
ms.date: 06/20/2016
ms.technology: csharp-fundamentals
ms.assetid: 0e9496da-e766-45a9-b92b-91820d4a350e
ms.openlocfilehash: 8e583b8a7cedab61ea6fdd56587608907610b6b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79145685"
---
# <a name="indexers"></a>インデクサー

"*インデクサー*" はプロパティに似ています。 さまざまな面で、インデクサーは[プロパティ](properties.md)と同じ言語機能を基にして構築されています。 インデクサーにより、"*インデックス付き*" プロパティを使用できるようになります。これは、1 つ以上の引数を使用して参照されるプロパティです。 これらの引数は、値のいくつかのコレクションにインデックスを提供します。

## <a name="indexer-syntax"></a>インデクサーの構文

インデクサーにアクセスするには、変数名と角かっこを使用します。 インデクサーの引数は、角かっこで囲みます。

```csharp
var item = someObject["key"];
someObject["AnotherKey"] = item;
```

インデクサーを宣言するには、`this` キーワードをプロパティ名として使用し、引数を各かっこの中で宣言します。 この宣言は、前の段落で説明した使用方法と一致します。

```csharp
public int this[string key]
{
    get { return storage.Find(key); }
    set { storage.SetAt(key, value); }
}
```

この最初の例から、プロパティの構文とインデクサーの構文の関係がわかります。 この類似点は、インデクサーの構文規則のほとんどで見られます。 インデクサーは、有効なアクセス修飾子 (public、protected internal、protected、internal、private、private protected) を持つことができます。 sealed、virtual、または abstract を指定することもできます。 プロパティと同様に、インデクサーの get アクセサーと set アクセサーには異なるアクセス修飾子を指定できます。
また、読み取り専用インデクサー (set アクセサーを省略) または書き込み専用インデクサー (get アクセサーを省略) を指定することもできます。

プロパティの操作方法から習得したほとんどすべてのことをインデクサーにも適用できます。 その唯一の例外は、"*自動実装プロパティ*" です。 コンパイラは、必ずしもインデクサーに適切なストレージを生成できるとは限りません。

項目のセット内の項目を参照するための引数が存在することも、インデクサーとプロパティの違いです。 インデクサーごとの引数リストが一意である場合は、1 つの型に複数のインデクサーを定義できます。 それでは、クラス定義で 1 つ以上のインデクサーを使用するさまざまなシナリオを見ていきましょう。

## <a name="scenarios"></a>シナリオ

型の API がコレクションをモデル化していて、そのコレクションへの引数が定義されている場合に、型に "*インデクサー*" を定義します。 インデクサーは、.NET Core Framework に含まれているコレクション型に直接マップされることもあれば、されないこともあります。 型は、コレクションのモデル化だけでなく、他の役割がある場合もあります。
インデクサーを使用すると、型の抽象化に適した API を提供することができ、抽象化された値の格納や計算の方法の内部の詳細を公開しなくて済みます。

"*インデクサー*" を使用する一般的なシナリオをいくつか見ていきましょう。 [インデクサーのサンプル フォルダー](https://github.com/dotnet/samples/tree/master/csharp/indexers)にアクセスできます。 ダウンロード方法については、「[サンプルおよびチュートリアル](../samples-and-tutorials/index.md#viewing-and-downloading-samples)」を参照してください。

### <a name="arrays-and-vectors"></a>配列とベクター

インデクサーを作成する最も一般的なシナリオの 1 つは、型が配列またはベクターをモデル化する場合です。 データの番号付きリストをモデル化するためのインデクサーを作成できます。

独自のインデクサーを作成する利点は、ニーズに合わせてそのコレクションのストレージを定義できることです。 一度でメモリに読み込むには大きすぎる履歴データを型でモデル化するシナリオを考えてみましょう。 使用状況に基づいて、コレクションのセクションを読み込んだりアンロードしたりする必要があります。 次の例では、この動作をモデル化します。 存在するデータ ポイント数が報告されます。 必要に応じて、データのセクションを保持するページを作成します。 より新しい要求によって必要とされるページのための領域を確保するために、メモリからページを削除します。

```csharp
public class DataSamples
{
    private class Page
    {
        private readonly List<Measurements> pageData = new List<Measurements>();
        private readonly int startingIndex;
        private readonly int length;
        private bool dirty;
        private DateTime lastAccess;

        public Page(int startingIndex, int length)
        {
            this.startingIndex = startingIndex;
            this.length = length;
            lastAccess = DateTime.Now;

            // This stays as random stuff:
            var generator = new Random();
            for(int i=0; i < length; i++)
            {
                var m = new Measurements
                {
                    HiTemp = generator.Next(50, 95),
                    LoTemp = generator.Next(12, 49),
                    AirPressure = 28.0 + generator.NextDouble() * 4
                };
                pageData.Add(m);
            }
        }
        public bool HasItem(int index) =>
            ((index >= startingIndex) &&
            (index < startingIndex + length));

        public Measurements this[int index]
        {
            get
            {
                lastAccess = DateTime.Now;
                return pageData[index - startingIndex];
            }
            set
            {
                pageData[index - startingIndex] = value;
                dirty = true;
                lastAccess = DateTime.Now;
            }
        }

        public bool Dirty => dirty;
        public DateTime LastAccess => lastAccess;
    }

    private readonly int totalSize;
    private readonly List<Page> pagesInMemory = new List<Page>();

    public DataSamples(int totalSize)
    {
        this.totalSize = totalSize;
    }

    public Measurements this[int index]
    {
        get
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");

            var page = updateCachedPagesForAccess(index);
            return page[index];
        }
        set
        {
            if (index < 0)
                throw new IndexOutOfRangeException("Cannot index less than 0");
            if (index >= totalSize)
                throw new IndexOutOfRangeException("Cannot index past the end of storage");
            var page = updateCachedPagesForAccess(index);

            page[index] = value;
        }
    }

    private Page updateCachedPagesForAccess(int index)
    {
        foreach (var p in pagesInMemory)
        {
            if (p.HasItem(index))
            {
                return p;
            }
        }
        var startingIndex = (index / 1000) * 1000;
        var newPage = new Page(startingIndex, 1000);
        addPageToCache(newPage);
        return newPage;
    }

    private void addPageToCache(Page p)
    {
        if (pagesInMemory.Count > 4)
        {
            // remove oldest non-dirty page:
            var oldest = pagesInMemory
                .Where(page => !page.Dirty)
                .OrderBy(page => page.LastAccess)
                .FirstOrDefault();
            // Note that this may keep more than 5 pages in memory
            // if too much is dirty
            if (oldest != null)
                pagesInMemory.Remove(oldest);
        }
        pagesInMemory.Add(p);
    }
}
```

この設計手法に従って、メモリ内コレクションにデータのセット全体を読み込まないことが妥当であるような種類のコレクションをモデル化できます。 `Page` クラスが、パブリック インターフェイスの一部ではない、入れ子になったプライベート クラスであることに注意してください。 これらの詳細は、このクラスのユーザーには表示されません。

### <a name="dictionaries"></a>ディクショナリ

もう 1 つの一般的なシナリオは、ディクショナリまたはマップをモデル化する必要がある場合です。 このシナリオでは、キー (通常はテキスト キー) に基づいて型が値を格納します。 この例では、これらのオプションを管理する[ラムダ式](delegates-overview.md)にコマンド ライン引数をマップするディクショナリを作成します。 次の例では、2 つのクラスを示します。`ArgsActions` クラスは、コマンド ライン オプションを `Action` デリゲートにマップします。`ArgsProcessor` クラスは、そのオプションが検出されたときに、`ArgsActions` を使用して各 `Action` を実行します。

```csharp
public class ArgsProcessor
{
    private readonly ArgsActions actions;

    public ArgsProcessor(ArgsActions actions)
    {
        this.actions = actions;
    }

    public void Process(string[] args)
    {
        foreach(var arg in args)
        {
            actions[arg]?.Invoke();
        }
    }

}
public class ArgsActions
{
    readonly private Dictionary<string, Action> argsActions = new Dictionary<string, Action>();

    public Action this[string s]
    {
        get
        {
            Action action;
            Action defaultAction = () => {} ;
            return argsActions.TryGetValue(s, out action) ? action : defaultAction;
        }
    }

    public void SetOption(string s, Action a)
    {
        argsActions[s] = a;
    }
}
```

この例では、`ArgsAction` コレクションが、基になるコレクションと密接に対応しています。
`get` は、特定のオプションが構成されているかどうかを判定します。 構成されている場合は、そのオプションに関連付けられている `Action` が返されます。 構成されていない場合は、何も実行しない `Action` が返されます。 パブリック アクセサーには、`set` アクセサーは含まれていません。 代わりに、オプションの設定にパブリック メソッドを使用する設計になっています。

### <a name="multi-dimensional-maps"></a>多次元マップ

複数の引数を使用するインデクサーを作成できます。 さらに、これらの引数は、同じ型でなくてもかまいません。 2 つの例を見てみましょう。

最初の例は、マンデルブロ集合の値を生成するクラスを示しています。 集合の数学的な背景情報の詳細については、[こちらの記事](https://en.wikipedia.org/wiki/Mandelbrot_set)を参照してください。
インデクサーは、2 つの倍精度浮動小数点数を使用して、X および Y 平面上の点を定義します。
get アクセサーは、点が集合内に存在しないと判断されるまで、イテレーションの回数を計算します。 イテレーションの最大数に達した場合、点は集合内に存在し、クラスの maxIterations 値が返されます。 マンデルブロ集合でよく知られている、コンピューターで生成される画像では、点が集合外に存在すると判断するために必要なイテレーションの数に対して色を定義します。

```csharp
public class Mandelbrot
{
    readonly private int maxIterations;

    public Mandelbrot(int maxIterations)
    {
        this.maxIterations = maxIterations;
    }

    public int this [double x, double y]
    {
        get
        {
            var iterations = 0;
            var x0 = x;
            var y0 = y;

            while ((x*x + y * y < 4) &&
                (iterations < maxIterations))
            {
                var newX = x * x - y * y + x0;
                y = 2 * x * y + y0;
                x = newX;
                iterations++;
            }
            return iterations;
        }
    }
}
```

マンデルブロ集合では、実数値の各 (x,y) 座標の値を定義します。
これによって、無限の数の値を格納できるディクショナリが定義されます。 そのため、集合の背後にストレージはありません。 代わりに、このクラスはコードが `get` アクセサーを呼び出すときに各点の値を計算します。 使用される、基になるストレージはありません。

最後の使用例を調べてみましょう。この例では、インデクサーが異なる型の複数の引数を受け取ります。 気温の履歴データを管理するプログラムがあるとします。 このインデクサーは、市区町村と日付を使用して、その場所の最高気温と最低気温を設定または取得します。

```csharp
using DateMeasurements =
    System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements =
    System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;

public class HistoricalWeatherData
{
    readonly CityDataMeasurements storage = new CityDataMeasurements();

    public Measurements this[string city, DateTime date]
    {
        get
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
                throw new ArgumentOutOfRangeException(nameof(city), "City not found");

            // strip out any time portion:
            var index = date.Date;
            var measure = default(Measurements);
            if (cityData.TryGetValue(index, out measure))
                return measure;
            throw new ArgumentOutOfRangeException(nameof(date), "Date not found");
        }
        set
        {
            var cityData = default(DateMeasurements);

            if (!storage.TryGetValue(city, out cityData))
            {
                cityData = new DateMeasurements();
                storage.Add(city, cityData);
            }

            // Strip out any time portion:
            var index = date.Date;
            cityData[index] = value;
        }
    }
}
```

この例では、気象データを 2 つの異なる引数にマップするインデクサーを作成します。その引数は、city (`string` で表されます) と date (`DateTime` で表されます) です。 内部ストレージでは、2 つの `Dictionary` クラスを使用して、2 次元ディクショナリを表します。 パブリック API は、基になるストレージを表さなくなります。 代わりに、インデクサーの言語機能を使用すると、基になるストレージで別の主要コレクション型を使用する必要があっても、抽象化を表すパブリック インターフェイスを作成できます。

このコードには、一部の開発者にはなじみのない部分が 2 つあります。 次の 2 つの `using` ステートメントです。

```csharp
using DateMeasurements = System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>;
using CityDataMeasurements = System.Collections.Generic.Dictionary<string, System.Collections.Generic.Dictionary<System.DateTime, IndexersSamples.Common.Measurements>>;
```

構築ジェネリック型の "*エイリアス*" を作成しています。 これらのステートメントにより、後のコードでは、`DateMeasurements` と `CityDateMeasurements` のジェネリック コンストラクションではなく、よりわかりやすい `Dictionary<DateTime, Measurements>` と `Dictionary<string, Dictionary<DateTime, Measurements> >` という名前を使用できます。
このコンストラクトでは、`=` 記号の右辺で完全修飾型名を使用する必要はありません。

2 つ目の手法では、コレクションへのインデックス作成に使用された任意の `DateTime` オブジェクトから時間部分を取り除きます。 .NET には、日付のみの型は含まれません。
開発者は `DateTime` 型を使用しますが、`Date` プロパティを使用して、その日のすべての `DateTime` オブジェクトが等しくなるようにしてください。

## <a name="summing-up"></a>まとめ

クラスにプロパティのような要素があり、そのプロパティが単一の値ではなく値のコレクションを表していて、各項目が引数のセットによって識別される場合は、インデクサーを作成する必要があります。 これらの引数では、参照する必要があるコレクションの項目を一意に特定できます。
インデクサーにより、[プロパティ](properties.md)の概念は拡張されます。メンバーはクラスの外部からデータ項目のように扱われますが、内部ではメソッドに似ています。 インデクサーを使用すると、引数は、項目のセットを表すプロパティ内で単一の項目を見つけることができます。
