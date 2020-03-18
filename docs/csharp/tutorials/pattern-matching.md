---
title: パターン マッチング機能を使用してデータ型を拡張する
description: この高度なチュートリアルでは、パターン マッチング手法を使用して、別々に作成されたデータとアルゴリズムを使用して機能を作成する方法を示します。
ms.date: 03/13/2019
ms-technology: csharp-whats-new
ms.custom: mvc
ms.openlocfilehash: df1054d8e0ec2b2539e6a1d00bf353d8ca927397
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156533"
---
# <a name="tutorial-using-pattern-matching-features-to-extend-data-types"></a>チュートリアル: パターン マッチング機能を使用してデータ型を拡張する

C# 7 で、基本的なパターン マッチング機能が導入されました。 C# 8 では、これらの機能が新しい式とパターンで拡張されています。 他のライブラリ内に存在する可能性がある型を拡張したかのように動作する機能を記述できます。 パターンの別の用途は、アプリケーションで必要な、拡張される型の基本機能ではない機能を作成することです。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> - パターン マッチングを使用する必要がある状況を認識する。
> - パターン マッチング式を使用して、型とプロパティの値に基づく動作を実装する。
> - パターン マッチングと他の手法を組み合わせて、完全なアルゴリズムを作成する。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 コンパイラも実行されるようにします。 C# 8 コンパイラは [Visual Studio 2019 バージョン 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) または [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 以降で使用できます。

このチュートリアルでは、.NET と、C# と Visual Studio または .NET Core CLI のいずれかに精通していることを前提としています。

## <a name="scenarios-for-pattern-matching"></a>パターン マッチングのシナリオ

多くの場合、最新の開発には、1 つのまとまりのあるアプリケーションの中で、複数のソースから受信するデータを統合し、それらのデータに基づいて情報と分析情報を提示することが含まれます。 開発者やチームは、受信データを表す型のすべてをコントロールしたり、アクセスしたりすることはできません。

従来のオブジェクト指向設計では、複数のデータ ソースから受信する各データ型を表す型をアプリケーション内で作成する必要があります。 その後、アプリケーションで、それらの新しい型を操作し、継承階層を構築し、仮想メソッドを作成し、抽象化を実装します。 これらの手法は有効であり、ときには最善のツールです。 少ないコードを記述できる場合があります。 データと、そのデータを操作する操作を分離する手法を使用して、もっと読みやすいコードを記述できます。

このチュートリアルでは、1 つのシナリオでさまざまな外部ソースから受信データを受け取るアプリケーションを作成して探索します。 **パターン マッチング**を使用して、元のシステムには含まれていない効率的な方法で、データを使用して処理するしくみを見ていきます。

通行料金とピーク時間帯料金を使用して交通量を管理する大都市圏について検討します。 車種に基づいて通行料金を計算するアプリケーションを記述します。 機能強化として、車両の乗員数に基づく料金を組み込みます。 さらなる機能強化として、時間帯と曜日に基づく料金を追加します。

この簡単な説明から、このシステムをモデル化するオブジェクト階層をすぐに思い描くことができます。 ただし、データは、他の車両登録管理システムなどの複数のソースから送信されます。 これらのシステムでは異なるクラスを使用してデータがモデル化されているため、使用できる単一のオブジェクト モデルはありません。 このチュートリアルでは、次のコードに示すように、簡略化されたクラスを使用して外部システムからの車両データをモデル化します。

[!code-csharp[ExternalSystems](~/samples/snippets/csharp/tutorials/patterns/start/toll-calculator/ExternalSystems.cs)]

GitHub の [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/start) リポジトリから、スタート コードをダウンロードすることができます。 さまざまなシステムからの車両クラスがあり、異なる名前空間に存在していることがわかります。 `System.Object` を除いて、利用できる共通基底クラスはありません。

## <a name="pattern-matching-designs"></a>パターン マッチング設計

このチュートリアルで使用するシナリオでは、パターン マッチングを使用して解決するのに適した種類の問題に焦点を当てています。

- 操作する必要があるオブジェクトは、目標と一致するオブジェクト階層内にはありません。 別個のシステムの一部であるクラスを操作する可能性があります。
- 追加する機能は、これらのクラスのコア抽象化の一部ではありません。 車両が支払う通行料金は、車両の種類によって "*変化*" しますが、通行料金は車両の中心的な関数ではありません。

データの "*形状*" とデータに対する "*操作*" が一緒に記述されていない場合は、C# のパターン マッチング機能によって、作業が容易になります。

## <a name="implement-the-basic-toll-calculations"></a>基本的な通行料金計算を実装する

最も基本的な通行料金計算は、車種にのみ依存します。

- `Car` は $2.00。
- `Taxi` は $3.50。
- `Bus` は $5.00。
- `DeliveryTruck` は $10.00。

新しい `TollCalculator` クラスを作成し、車種に対するパターン マッチングを実装して、通行料金の金額を取得します。 `TollCalculator` の最初の実装を次のコードに示します。

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    public class TollCalculator
    {
        public decimal CalculateToll(object vehicle) =>
            vehicle switch
        {
            Car c           => 2.00m,
            Taxi t          => 3.50m,
            Bus b           => 5.00m,
            DeliveryTruck t => 10.00m,
            { }             => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
            null            => throw new ArgumentNullException(nameof(vehicle))
        };
    }
}
```

上記のコードでは、**型パターン**をテストする **switch 式** ([`switch`](../language-reference/keywords/switch.md) ステートメントとは異なります) が使用されています。 上記のコードでは、**switch 式**は変数 `vehicle` で始まり、`switch` キーワードが続きます。 次に、すべての **switch アーム**が中かっこ内に指定されます。 `switch` 式は、`switch` ステートメントを囲む構文に対して、その他の絞り込みを行います。 `case` キーワードは省略され、各アームの結果が式になります。 最後の 2 つのアームは、新しい言語機能を示しています。 `{ }` case は、前のアームと一致しなかった null 以外のオブジェクトと一致します。 このアームは、このメソッドに渡された正しくない型をキャッチします。  `{ }` case は、車種ごとの case に従う必要があります。 順序が逆になった場合は、`{ }` case が優先されます。 最後に、`null` パターンで、このメソッドに `null` が渡されたときを検出します。 他の型パターンが null 以外の正しい車種のオブジェクトのみと一致するため、`null` パターンを最後にすることができます。

このコードを `Program.cs` の次のコードを使用してテストできます。

```csharp
using System;
using CommercialRegistration;
using ConsumerVehicleRegistration;
using LiveryRegistration;

namespace toll_calculator
{
    class Program
    {
        static void Main(string[] args)
        {
            var tollCalc = new TollCalculator();

            var car = new Car();
            var taxi = new Taxi();
            var bus = new Bus();
            var truck = new DeliveryTruck();

            Console.WriteLine($"The toll for a car is {tollCalc.CalculateToll(car)}");
            Console.WriteLine($"The toll for a taxi is {tollCalc.CalculateToll(taxi)}");
            Console.WriteLine($"The toll for a bus is {tollCalc.CalculateToll(bus)}");
            Console.WriteLine($"The toll for a truck is {tollCalc.CalculateToll(truck)}");

            try
            {
                tollCalc.CalculateToll("this will fail");
            }
            catch (ArgumentException e)
            {
                Console.WriteLine("Caught an argument exception when using the wrong type");
            }
            try
            {
                tollCalc.CalculateToll(null);
            }
            catch (ArgumentNullException e)
            {
                Console.WriteLine("Caught an argument exception when using null");
            }
        }
    }
}
```

このコードはスタート プロジェクトに含まれていますが、コメント アウトされています。コメントを削除することで、自分で記述したコードをテストできます。

コードとデータが分離されるアルゴリズムの作成にパターンがどのように役立つかを示す最初の例を見てきました。 `switch` 式によって型がテストされ、結果に基づいて別の値が生成されます。 これはほんの序の口に過ぎません。

## <a name="add-occupancy-pricing"></a>乗員料金を追加する

料金徴収機関は、車両が乗車定員を乗せて走行することを推奨したいと思っています。 車両の乗員数が少ない場合は多く請求し、乗車定員の場合は料金を下げることによって、定員いっぱいでの走行を推奨することを決定しています。

- 乗客がいない自動車とタクシーは、$0.50 余分に支払う。
- 2 名の乗客がいる自動車とタクシーは、$0.50 割引される。
- 3 名以上の乗客がいる自動車とタクシーは、$1.00 割引される。
- 乗車率が 50% 未満のバスは、$2.00 余分に支払う。
- 乗車率が 90% を超えるバスは、$1.00 割引される。

このルールは、同じ switch 式で**プロパティ パターン**を使用して実装できます。 プロパティ パターンでは、型が決定した後、オブジェクトのプロパティが調べられます。 `Car` に対する 1 つの case が、4 つの異なる case に拡張されます。

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1 }       => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c                       => 2.00m - 1.0m,

    // ...
};
```

最初の 3 つの case では、型を `Car` としてテストした後、`Passengers` プロパティの値がチェックされます。 両方が一致した場合は、その式が評価され、結果が返されます。

タクシーに対する case も、同様の方法で拡張します。

```csharp
vehicle switch
{
    // ...

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    // ...
};
```

上記の例では、最後の case の `when` 句は省略されています。

次に、次の例に示すように、バスに対する case を拡張して、乗車率ルールを実装します。

```csharp
vehicle switch
{
    // ...

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    // ...
};
```

料金徴収機関は、配送トラックの乗客数には関心がありません。 代わりに、次のようにトラックの重量クラスに基づいて通行料金の金額を調整します。

- 5,000 lbs を超えるトラックは、$5.00 余分に請求される。
- 3,000 lbs を下回る軽トラックは、$2.00 割引される。

これらのルールは、次のコードで実装されます。

```csharp
vehicle switch
{
    // ...

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,
};
```

前のコードは、switch アームの `when` 句を示しています。 `when` 句を使用して、プロパティの等価以外の条件をテストできます。 完了すると、以下によく似たメソッドが作成されます。

```csharp
vehicle switch
{
    Car { Passengers: 0}        => 2.00m + 0.50m,
    Car { Passengers: 1}        => 2.0m,
    Car { Passengers: 2}        => 2.0m - 0.50m,
    Car c                       => 2.00m - 1.0m,

    Taxi { Fares: 0}  => 3.50m + 1.00m,
    Taxi { Fares: 1 } => 3.50m,
    Taxi { Fares: 2}  => 3.50m - 0.50m,
    Taxi t            => 3.50m - 1.00m,

    Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
    Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
    Bus b => 5.00m,

    DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
    DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
    DeliveryTruck t => 10.00m,

    { }     => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
    null    => throw new ArgumentNullException(nameof(vehicle))
};
```

これらの switch アームの多くは、**再帰パターン**の例です。 たとえば、`Car { Passengers: 1}` は、プロパティ パターンの内部の定数パターンを示しています。

switch を入れ子にして使用することで、このコードの反復を少なくすることができます。 `Car` と `Taxi` は、どちらにも上記の例で示した 4 つの異なるアームがあります。 両方に対して、プロパティ パターンに供給される型パターンを作成できます。 この手法を次のコードに示します。

```csharp
public decimal CalculateToll(object vehicle) =>
    vehicle switch
    {
        Car c => c.Passengers switch
        {
            0 => 2.00m + 0.5m,
            1 => 2.0m,
            2 => 2.0m - 0.5m,
            _ => 2.00m - 1.0m
        },

        Taxi t => t.Fares switch
        {
            0 => 3.50m + 1.00m,
            1 => 3.50m,
            2 => 3.50m - 0.50m,
            _ => 3.50m - 1.00m
        },

        Bus b when ((double)b.Riders / (double)b.Capacity) < 0.50 => 5.00m + 2.00m,
        Bus b when ((double)b.Riders / (double)b.Capacity) > 0.90 => 5.00m - 1.00m,
        Bus b => 5.00m,

        DeliveryTruck t when (t.GrossWeightClass > 5000) => 10.00m + 5.00m,
        DeliveryTruck t when (t.GrossWeightClass < 3000) => 10.00m - 2.00m,
        DeliveryTruck t => 10.00m,

        { }  => throw new ArgumentException(message: "Not a known vehicle type", paramName: nameof(vehicle)),
        null => throw new ArgumentNullException(nameof(vehicle))
    };
```

上記の例では、再帰式の使用は、プロパティの値をテストする子アームを含む `Car` と `Taxi` のアームを繰り返さないことを意味しています。 この手法は、プロパティの個別の値ではなくその範囲をテストする `Bus` と `DeliveryTruck` のアームでは使用されません。

## <a name="add-peak-pricing"></a>ピーク料金を追加する

最後の機能として、料金徴収機関は、時間に依存するピーク料金を追加することを望んでいます。 朝と夕方のラッシュ アワーの間は、通行料金を倍にします。 このルールは、片方向の通行のみに影響し、朝のラッシュ時は市内に入る (インバウンド) 通行が、夕方のラッシュ時は市外に出てゆく (アウトバウンド) 通行が対象になります。 平日のその他の時間帯では、通行料金は 50% 増額される。 深夜と早朝の通行料金は、25% 減額される。 週末は、時間に関係なく、通常料金になる。

この機能のためにパターン マッチングを使用しますが、それは他の手法と統合します。 方向、曜日、および時間帯のすべてを組み合わせた単一のパターン マッチ式を作成できますが、 結果は、複雑な式になるでしょう。 読みにくく、理解しにくくなるでしょう。 それは、正確さを保証することを難しくします。 代わりに、メソッドを組み合わせて、すべての状態を簡潔に記述する値のタプルを作成します。 その後、パターン マッチングを使用して、通行料金の乗数を計算できます。 タプルには、3 つの個別の条件が含まれています。

- その日が平日または週末のどちらであるか。
- 通行料金が収集される時間帯。
- 方向が市内に入るほうか市外に出るほうか。

次の表に、入力値の組み合わせとピーク時の乗数を示します。

| Day        | 時刻         | 方向 | 割り増し |
| ---------- | ------------ | --------- |--------:|
| 平日    | 朝のラッシュ時 | インバウンド   | x 2.00  |
| 平日    | 朝のラッシュ時 | アウトバウンド  | x 1.00  |
| 平日    | 日中      | インバウンド   | x 1.50  |
| 平日    | 日中      | アウトバウンド  | x 1.50  |
| 平日    | 夕方のラッシュ時 | インバウンド   | x 1.00  |
| 平日    | 夕方のラッシュ時 | アウトバウンド  | x 2.00  |
| 平日    | 夜間    | インバウンド   | x 0.75  |
| 平日    | 夜間    | アウトバウンド  | x 0.75  |
| 週末    | 朝のラッシュ時 | インバウンド   | x 1.00  |
| 週末    | 朝のラッシュ時 | アウトバウンド  | x 1.00  |
| 週末    | 日中      | インバウンド   | x 1.00  |
| 週末    | 日中      | アウトバウンド  | x 1.00  |
| 週末    | 夕方のラッシュ時 | インバウンド   | x 1.00  |
| 週末    | 夕方のラッシュ時 | アウトバウンド  | x 1.00  |
| 週末    | 夜間    | インバウンド   | x 1.00  |
| 週末    | 夜間    | アウトバウンド  | x 1.00  |

3 つの変数の 16 組の異なる組み合わせがあります。 いくつかの条件を組み合わせることで、最終的な switch 式を簡略化します。

通行料金を収集するシステムでは、通行料金が徴収される時刻に対して <xref:System.DateTime> 構造体を使用しています。 上記の表から、変数を作成するメンバー メソッドを作成します。 次の関数では、パターン マッチングの switch 式を使用して、<xref:System.DateTime> が週末または平日のどちらを表しているかを示しています。

```csharp
private static bool IsWeekDay(DateTime timeOfToll) =>
    timeOfToll.DayOfWeek switch
    {
        DayOfWeek.Monday    => true,
        DayOfWeek.Tuesday   => true,
        DayOfWeek.Wednesday => true,
        DayOfWeek.Thursday  => true,
        DayOfWeek.Friday    => true,
        DayOfWeek.Saturday  => false,
        DayOfWeek.Sunday    => false
    };
```

このメソッドは機能しますが、繰り返しが含まれています。 次のコードに示すように簡略化できます。

[!code-csharp[IsWeekDay](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#IsWeekDay)]

次に、時間をブロックに分類する同様の関数を追加します。

[!code-csharp[GetTimeBand](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#GetTimeBand)]

上記のメソッドでは、パターン マッチングは使用されていません。 使い慣れた `if` ステートメントの連鎖を使用するほうがわかりやすくなります。 プライベート `enum` を追加して、時間の各範囲を個別の値に変換します。

これらのメソッドを作成したら、別の `switch` 式と**タプル パターン**を使用して、割増料金を計算できます。 全部で 16 のアームがある `switch` 式を作成できます。

[!code-csharp[FullTuplePattern](~/samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#TuplePatternOne)]

上記のコードは機能しますが、簡略化できます。 週末の 8 つの組み合わせは、同じ通行料金になります。 8 つのすべてを、次の行に置き換えることができます。

```csharp
(false, _, _) => 1.0m,
```

平日の日中と夜間のインバウンドとアウトバウンドの通行では、同じ乗数が使用されます。 これら 4 つの switch アームは、次の 2 行に置換できます。

```csharp
(true, TimeBand.Overnight, _) => 0.75m,
(true, TimeBand.Daytime, _)   => 1.5m,
```

これら 2 つの変更を行った後のコードは、次のようになります。

```csharp
public decimal PeakTimePremium(DateTime timeOfToll, bool inbound) =>
    (IsWeekDay(timeOfToll), GetTimeBand(timeOfToll), inbound) switch
    {
        (true, TimeBand.MorningRush, true)  => 2.00m,
        (true, TimeBand.MorningRush, false) => 1.00m,
        (true, TimeBand.Daytime,     _)     => 1.50m,
        (true, TimeBand.EveningRush, true)  => 1.00m,
        (true, TimeBand.EveningRush, false) => 2.00m,
        (true, TimeBand.Overnight,   _)     => 0.75m,
        (false, _,                   _)     => 1.00m,
    };
```

最後に、通常料金を支払う 2 つのラッシュの時間帯は削除できます。 これらのアームを削除した後、最後の switch アーム内の `false` を破棄 (`_`)  に置き換えることができます。 次のメソッドが完成します。

[!code-csharp[SimplifiedTuplePattern](../../../samples/snippets/csharp/tutorials/patterns/finished/toll-calculator/TollCalculator.cs#FinalTuplePattern)]

この例では、パターン マッチングの利点の 1 つに注目しています。パターンの分岐は、順序正しく評価されます。 前のほうの分岐で後ろにあるいずれかの case が処理されるようにパターンを並べ替えると、コンパイラによって到達できないコードに関する警告が表示されます。 これらの言語ルールによって、コードが変化しないという自信を持って、前述した簡略化を簡単に実行できます。

パターン マッチングによって、ある種のコードが読みやすくなり、クラスにコードを追加できない場合はオブジェクト指向の手法の代替として機能します。 クラウドによって、データと機能は分離されています。 データの "*形状*" とデータに対する "*操作*" は、必ずしも一緒に記述されるわけではありません。 このチュートリアルでは、既存のデータを、元の関数とは完全に異なる方法で使用しています。 パターン マッチングでは、型を拡張できない場合でも、それらをオーバーライドする機能を記述できます。

## <a name="next-steps"></a>次の手順

GitHub リポジトリの [dotnet/samples](https://github.com/dotnet/samples/tree/master/csharp/tutorials/patterns/finished) から、完成したコードをダウンロードできます。 自分のパターンを調査し、通常のコーディング アクティビティにこの手法を追加してください。 これらの手法を習得することで、別の方法で問題にアプローチし、新しい機能を作成できます。
