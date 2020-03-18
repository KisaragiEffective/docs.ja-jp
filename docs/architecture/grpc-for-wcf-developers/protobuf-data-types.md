---
title: プロトブーフ スカラー データ型 - WCF 開発者向け gRPC
description: NET Core で Protobuf および gRPC がサポートする基本的なデータ型と既知のデータ型について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: a40f51fa32ddb97ba417ec01f31e1f0187f0d544
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148128"
---
# <a name="protobuf-scalar-data-types"></a>Protobuf スカラー データ型

プロトコル バッファ (Protobuf) は、ネイティブスカラー値の種類の範囲をサポートします。 次の表は、それらすべてを同等の C# 型と共に示しています。

| プロトブーフタイプ | C# 型      | Notes |
| ------------- | ------------ | ----- |
| `double`      | `double`     |       |
| `float`       | `float`      |       |
| `int32`       | `int`        | 1     |
| `int64`       | `long`       | 1     |
| `uint32`      | `uint`       |       |
| `uint64`      | `ulong`      |       |
| `sint32`      | `int`        | 1     |
| `sint64`      | `long`       | 1     |
| `fixed32`     | `uint`       | 2     |
| `fixed64`     | `ulong`      | 2     |
| `sfixed32`    | `int`        | 2     |
| `sfixed64`    | `long`       | 2     |
| `bool`        | `bool`       |       |
| `string`      | `string`     | 3     |
| `bytes`       | `ByteString` | 4     |

注:

1. 署名付`int32`き`int64`の値を使用する場合は、標準エンコードは非効率的です。 フィールドに負の数が含まれる可能性がある場合`sint32`は`sint64`、またはを使用します。 これらの型は、それぞれ`int`C#`long`と型にマップされます。
2. フィールド`fixed`は、値が何であるかに関係なく、常に同じバイト数を使用します。 この動作により、シリアル化と逆シリアル化が大きい値に対して高速になります。
3. プロトブフ文字列は UTF-8 (または 7 ビット ASCII) でエンコードされています。 エンコードされた長さは 2<sup>32</sup>より大きくすることはできません。
4. Protobuf ランタイムは、C#`ByteString``byte[]`配列との間で簡単にマップする型を提供します。

## <a name="other-net-primitive-types"></a>その他の .NET プリミティブ型

### <a name="dates-and-times"></a>日付と時刻

ネイティブ スカラー型は、日付<xref:System.DateTimeOffset>と時刻の値<xref:System.DateTime>を提供しません。 <xref:System.TimeSpan> Google の「よく知られているタイプ」の拡張機能を使用して、これらのタイプを指定できます。 これらの拡張機能は、サポートされているプラットフォーム全体で、複雑なフィールド型のコード生成とランタイムサポートを提供します。

次の表は、日付と時刻の種類を示しています。

| C# 型 | プロトブーフの有名なタイプ |
| ------- | ------------------------ |
| `DateTimeOffset` | `google.protobuf.Timestamp` |
| `DateTime` | `google.protobuf.Timestamp` |
| `TimeSpan` | `google.protobuf.Duration` |

```protobuf  
syntax = "proto3"

import "google/protobuf/duration.proto";  
import "google/protobuf/timestamp.proto";

message Meeting {

    string subject = 1;
    google.protobuf.Timestamp time = 2;
    google.protobuf.Duration duration = 3;

}  
```

C# クラスで生成されるプロパティは、.NET の日付と時刻の型ではありません。 プロパティは、名前空間`Timestamp`内`Duration`の クラス`Google.Protobuf.WellKnownTypes`と クラスを使用します。 これらのクラスは、 `DateTimeOffset`、 `DateTime`、および との`TimeSpan`間で変換するためのメソッドを提供します。

```csharp
// Create Timestamp and Duration from .NET DateTimeOffset and TimeSpan
var meeting = new Meeting
{
    Time = Timestamp.FromDateTimeOffset(meetingTime), // also FromDateTime()
    Duration = Duration.FromTimeSpan(meetingLength)
};

// Convert Timestamp and Duration to .NET DateTimeOffset and TimeSpan
DateTimeOffset time = meeting.Time.ToDateTimeOffset();
TimeSpan? duration = meeting.Duration?.ToTimeSpan();
```

> [!NOTE]
> この`Timestamp`型は UTC 時刻で動作します。 `DateTimeOffset`値は常にゼロのオフセットを`DateTime.Kind`持ち、プロパティは`DateTimeKind.Utc`常に です。

### <a name="systemguid"></a>System.Guid

Protobuf は、他のプラットフォーム<xref:System.Guid>と呼ばれる`UUID`型を直接サポートしていません。 よく知られているタイプはありません。

最適な方法は、標準`Guid``string``8-4-4-4-12`の 16 進形式 (たとえば)`45a9fda3-bd01-47a9-8460-c1cd7484b0b3`を使用して、値をフィールドとして処理することです。 すべての言語とプラットフォームは、その形式を解析できます。

`Guid`値にフィールドを`bytes`使用しないでください。 Protobuf が Java などの他のプラットフォームと対話しているとき、*エンディアンネス*([Wikipedia 定義](https://en.wikipedia.org/wiki/Endianness)) の問題は、不安定な動作を引き起こす可能性があります。

### <a name="nullable-types"></a>Null 許容型

C# の Protobuf コード生成では、`int`ネイティブ型を使用`int32`します。 したがって、値は常に含まれ、null にすることはできません。

C# コードでの使用`int?`など、明示的な null を必要とする値の場合、Protobuf の "既知の型" には、null 許容 C# 型にコンパイルされるラッパーが含まれます。 これらを使用するには、次`wrappers.proto`のように`.proto`ファイルにインポートします。

```protobuf  
syntax = "proto3"

import "google/protobuf/wrappers.proto"

message Person {

    ...
    google.protobuf.Int32Value age = 5;

}
```

Protobuf は、生成`T?`されたメッセージ プロパティ`int?`に単純な ( など ) を使用します。

次の表は、同等の C# 型を持つラッパー型の完全な一覧を示しています。

| C# 型   | よく知られている型ラッパー       |
| --------- | ----------------------------- |
| `double?` | `google.protobuf.DoubleValue` |
| `float?`  | `google.protobuf.FloatValue`  |
| `int?`    | `google.protobuf.Int32Value`  |
| `long?`   | `google.protobuf.Int64Value`  |
| `uint?`   | `google.protobuf.UInt32Value` |
| `ulong?`  | `google.protobuf.UInt64Value` |

既知の型`Timestamp`と`Duration`.NET ではクラスとして表現されるため、null 許容バージョンは必要ありません。 しかし、これらの型のプロパティを null に変換する場合は、これらの型のプロパティ`DateTimeOffset`を`TimeSpan`チェックすることが重要です。

## <a name="decimals"></a>10 進数

Protobuf は.NET`decimal`型を`double`ネイティブにサポートしていません。 `float` Protobuf プロジェクトでは、標準型をサポートする言語やフレームワークのプラットフォームサポートを`Decimal`使用して、既知の型に標準型を追加する可能性について、継続的な議論が行われています。 まだ何も実装されていません。

.NET クライアントとサーバー間の安全なシリアル化`decimal`に使用する型を表すメッセージ定義を作成できます。 しかし、他のプラットフォームの開発者は、使用されている形式を理解し、独自の処理を実装する必要があります。

### <a name="creating-a-custom-decimal-type-for-protobuf"></a>Protobuf のカスタム 10 進型の作成

単純な実装は、フィールドを使用しない一`Money`部の Google API が使用する`currency`非標準型に似ている可能性があります。

```protobuf
package CustomTypes;

// Example: 12345.6789 -> { units = 12345, nanos = 678900000 }
message Decimal {

    // Whole units part of the amount
    int64 units = 1;

    // Nano units of the amount (10^-9)
    // Must be same sign as units
    sfixed32 nanos = 2;
}
```

この`nanos`フィールドは、`0.999_999_999`から`-0.999_999_999`から への値を表します。 たとえば、`decimal`値`1.5m`は`{ units = 1, nanos = 500_000_000 }`として表されます。 この例の`nanos`フィールドでは、大きな値よりも`sfixed32``int32`効率的にエンコードされる型を使用するのはこのためです。 フィールドが`units`負の場合、`nanos`フィールドも負の値にする必要があります。

> [!NOTE]
> バイト文字列として値をエンコード`decimal`するためのアルゴリズムは他に複数ありますが、このメッセージはそれらよりも理解しやすいです。 値は、異なるプラットフォームでのエンディアンの影響を受けません。

この型と BCL`decimal`型の間の変換は、C# で次のように実装できます。

```csharp
namespace CustomTypes
{
    public partial class GrpcDecimal
    {
        private const decimal NanoFactor = 1_000_000_000;
        public GrpcDecimal(long units, int nanos)
        {
            Units = units;
            Nanos = nanos;
        }

        public long Units { get; }
        public int Nanos { get; }

        public static implicit operator decimal(CustomTypes.Decimal grpcDecimal)
        {
            return grpcDecimal.Units + grpcDecimal.Nanos / NanoFactor;
        }

        public static implicit operator CustomTypes.Decimal(decimal value)
        {
            var units = decimal.ToInt64(value);
            var nanos = decimal.ToInt32((value - units) * NanoFactor);
            return new CustomTypes.Decimal(units, nanos);
        }
    }
}
```

> [!IMPORTANT]
> このようなカスタム メッセージタイプを*使用する場合*は、常に`.proto`. その後、他の開発者は、対応する型との変換を、独自の言語またはフレームワークで実装できます。

>[!div class="step-by-step"]
>[前次](protobuf-messages.md)
>[Next](protobuf-nested-types.md)
