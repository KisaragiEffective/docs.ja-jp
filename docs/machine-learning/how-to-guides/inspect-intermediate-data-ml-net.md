---
title: ML.NET 処理中に中間データを検査する
description: ML.NET 機械学習パイプラインの読み込み中、処理中に中間データを検査する方法と、ML.NET のモデル トレーニング手順について説明します。
ms.date: 06/25/2019
author: luisquintanilla
ms.author: luquinta
ms.custom: mvc, how-to, title-hack-0625
ms.openlocfilehash: 11df1d5caaa7b7974360d863f85afbff18985e47
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73977097"
---
# <a name="inspect-intermediate-data-during-processing"></a>処理中に中間データを検査する

読み込み中、処理中に中間データを検査する方法と、ML.NET のモデル トレーニング手順について説明します。 中間データは機械学習パイプラインの各ステージの出力です。

[`IDataView`](xref:Microsoft.ML.IDataView) に読み込まれる以下に示すような中間データは、ML.NET においてさまざまな方法で検査できます。

```csharp
HousingData[] housingData = new HousingData[]
{
    new HousingData
    {
        Size = 600f,
        HistoricalPrices = new float[] { 100000f ,125000f ,122000f },
        CurrentPrice = 170000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 200000f, 250000f, 230000f },
        CurrentPrice = 225000f
    },
    new HousingData
    {
        Size = 1000f,
        HistoricalPrices = new float[] { 126000f, 130000f, 200000f },
        CurrentPrice = 195000f
    },
    new HousingData
    {
        Size = 850f,
        HistoricalPrices = new float[] { 150000f,175000f,210000f },
        CurrentPrice = 205000f
    },
    new HousingData
    {
        Size = 900f,
        HistoricalPrices = new float[] { 155000f, 190000f, 220000f },
        CurrentPrice = 210000f
    },
    new HousingData
    {
        Size = 550f,
        HistoricalPrices = new float[] { 99000f, 98000f, 130000f },
        CurrentPrice = 180000f
    }
};
```

## <a name="convert-idataview-to-ienumerable"></a>IDataView を IEnumerable に変換する

[`IDataView`](xref:Microsoft.ML.IDataView) を検査する最も簡単な方法の 1 つは、[`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) に変換することです。 [`IDataView`](xref:Microsoft.ML.IDataView) を [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) に変換するには、[`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) メソッドを使用します。

パフォーマンスを最適化するには、`reuseRowObject` を `true` に設定します。 これにより、同一オブジェクトには現在の行のデータが遅れて設定され、データセット内の行ごとに新しいオブジェクトを作成する場合とは対照的に評価されます。

```csharp
// Create an IEnumerable of HousingData objects from IDataView
IEnumerable<HousingData> housingDataEnumerable =
    mlContext.Data.CreateEnumerable<HousingData>(data, reuseRowObject: true);

// Iterate over each row
foreach (HousingData row in housingDataEnumerable)
{
    // Do something (print out Size property) with current Housing Data object being evaluated
    Console.WriteLine(row.Size);
}
```

## <a name="accessing-specific-indices-with-ienumerable"></a>IEnumerable を使用して特定のインデックスにアクセスする

データの一部や特定のインデックスにしかアクセスする必要がない場合は、データセット内の要求された各行に対して新しいオブジェクトが作成されるように、[`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) を使用すると共に `reuseRowObject` パラメータ―の値を `false` に設定します。 その後、[`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) を配列またはリストに変換します。

> [!WARNING]
> [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable*) の結果を配列またはリストに変換すると、要求されたすべての [`IDataView`](xref:Microsoft.ML.IDataView) 行がメモリに読み込まれ、パフォーマンスに影響を及ぼす可能性があります。

コレクションが作成されたら、データに対する操作を実行できます。 以下のコード スニペットでは、データセット内の最初の 3 つの行を取得して、現在の平均価格を計算します。

```csharp
// Create an Array of HousingData objects from IDataView
HousingData[] housingDataArray =
    mlContext.Data.CreateEnumerable<HousingData>(data, reuseRowObject: false)
        .Take(3)
        .ToArray();

// Calculate Average CurrentPrice of First Three Elements
HousingData firstRow = housingDataArray[0];
HousingData secondRow = housingDataArray[1];
HousingData thirdRow = housingDataArray[2];
float averageCurrentPrice = (firstRow.CurrentPrice + secondRow.CurrentPrice + thirdRow.CurrentPrice) / 3;
```

## <a name="inspect-values-in-a-single-column"></a>単一列内の値を検査する

モデル構築プロセスの任意の時点で、[`IDataView`](xref:Microsoft.ML.IDataView) の単一列内の値には [`GetColumn`](xref:Microsoft.ML.Data.ColumnCursorExtensions.GetColumn*) メソッドを使用してアクセスできます。 [`GetColumn`](xref:Microsoft.ML.Data.ColumnCursorExtensions.GetColumn*) メソッドは、単一列内のすべての値を [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) として返します。

```csharp
IEnumerable<float> sizeColumn = data.GetColumn<float>("Size").ToList();
```

## <a name="inspect-idataview-values-one-row-at-a-time"></a>IDataView の値を 1 行ずつ検査する

[`IDataView`](xref:Microsoft.ML.IDataView) は遅れて評価されます。 このドキュメントの前のセクションで示したように、[`IDataView`](xref:Microsoft.ML.IDataView) に変換せずに [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) の複数の行にわたって反復処理を行うには、[`DataViewRowCursor`](xref:Microsoft.ML.DataViewRowCursor) メソッドを使用して [`GetRowCursor`](xref:Microsoft.ML.IDataView.GetRowCursor*) の [DataViewSchema](xref:Microsoft.ML.DataViewSchema) をパラメーターとして渡すことで、[`IDataView`](xref:Microsoft.ML.IDataView) を作成します。 その後、複数の行にわたって反復処理を行うために、[`MoveNext`](xref:Microsoft.ML.DataViewRowCursor.MoveNext*) カーソル メソッドを [`ValueGetter`](xref:Microsoft.ML.ValueGetter%601) デリゲートと共に使用して、各列からそれぞれの値を抽出します。

> [!IMPORTANT]
> パフォーマンスのために、ML.NET 内のベクターには、ネイティブなコレクション型 (つまり、[、`VBuffer`) ではなく、](xref:Microsoft.ML.Data.VBuffer%601)`Vector``float[]` を使用します。

```csharp
// Get DataViewSchema of IDataView
DataViewSchema columns = data.Schema;

// Create DataViewCursor
using (DataViewRowCursor cursor = data.GetRowCursor(columns))
{
    // Define variables where extracted values will be stored to
    float size = default;
    VBuffer<float> historicalPrices = default;
    float currentPrice = default;

    // Define delegates for extracting values from columns
    ValueGetter<float> sizeDelegate = cursor.GetGetter<float>(columns[0]);
    ValueGetter<VBuffer<float>> historicalPriceDelegate = cursor.GetGetter<VBuffer<float>>(columns[1]);
    ValueGetter<float> currentPriceDelegate = cursor.GetGetter<float>(columns[2]);

    // Iterate over each row
    while (cursor.MoveNext())
    {
        //Get values from respective columns
        sizeDelegate.Invoke(ref size);
        historicalPriceDelegate.Invoke(ref historicalPrices);
        currentPriceDelegate.Invoke(ref currentPrice);
    }
}
```

## <a name="preview-result-of-pre-processing-or-training-on-a-subset-of-the-data"></a>データのサブセットに対する前処理またはトレーニングの結果をプレビューする

> [!WARNING]
> `Preview` はデバッグを目的としており、パフォーマンスを低下させる可能性があるため、実稼働環境のコードでは使用しないでください。

モデルのビルド プロセスは実験的であり、反復されます。 データのサブセットに対して機械学習モデルの前処理またはトレーニングを行った後にデータがどうなってるかをプレビューするには、[`Preview`](xref:Microsoft.ML.DebuggerExtensions.Preview*) を返す [`DataDebuggerPreview`](xref:Microsoft.ML.Data.DataDebuggerPreview) メソッドを使用します。 結果のオブジェクトは `ColumnView` および `RowView` プロパティを含み、どちらも [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) になっていて、特定の列または行の値が格納されています。 `maxRows` パラメータ―を使って、変換を適用する行数を指定します。

![データ デバッガーによるオブジェクトのプレビュー](./media/inspect-intermediate-data-ml-net/data-debugger-preview-01.png)

[`IDataView`](xref:Microsoft.ML.IDataView) を検査した結果は、次のようになります。

![データ デバッガーによる行のプレビューの表示](./media/inspect-intermediate-data-ml-net/data-debugger-preview-02.png)
