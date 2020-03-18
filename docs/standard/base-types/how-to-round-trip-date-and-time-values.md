---
title: '方法: 日付と時刻の値をラウンドトリップさせる'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- round-trip date and time values
- dates [.NET Framework], round-trip values
- time zones [.NET Framework], round-trip date and time values
- time [.NET Framework], round-trip values
- formatting strings [.NET Framework], round-trip values
ms.assetid: b609b277-edc6-4c74-b03e-ea73324ecbdb
ms.openlocfilehash: 2e3a58ffe8332e0afec62461f6897d673e1da09f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73132004"
---
# <a name="how-to-round-trip-date-and-time-values"></a>方法: 日付と時刻の値をラウンドトリップさせる

ある特定の時点を明確に表すように日付と時刻の値を保つことは、多くのアプリケーションに共通する要件です。 このトピックでは、<xref:System.DateTime> 値、<xref:System.DateTimeOffset> 値、日時値と時間帯の情報を保存し、復元する方法について説明します。復元した値によって、保存した値と同じ時刻が識別されるようにします。

### <a name="to-round-trip-a-datetime-value"></a>DateTime 値をラウンドトリップさせるには

1. <xref:System.DateTime.ToString%28System.String%29?displayProperty=nameWithType> メソッドを "o" 書式指定子と共に呼び出して、<xref:System.DateTime> 値を対応する文字列形式に変換します。

2. <xref:System.DateTime> 値の文字列形式をファイルに保存するか、プロセス、アプリケーション ドメイン、またはマシン境界を通過させます。

3. <xref:System.DateTime> 値を表す文字列を取得します。

4. <xref:System.DateTime.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> メソッドを呼び出し、`styles` パラメーターの値として <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> を渡します。

<xref:System.DateTime> 値をラウンドトリップさせる方法を次の例に示します。

[!code-csharp[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#1)]
[!code-vb[Formatting.HowTo.RoundTrip#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#1)]

<xref:System.DateTime> 値をラウンドトリップさせる場合、この方法により、あらゆる現地時刻と協定世界時刻を適切に保存できます。 たとえば、現地時刻の <xref:System.DateTime> 値が米国太平洋標準時タイム ゾーンのシステムで保存され、米国中部標準時タイム ゾーンのシステムで復元された場合、復元された日付と時刻は 2 つのタイム ゾーンの時差を反映し、元の時刻より 2 時間後になります。 ただし、タイム ゾーンが指定されていない時刻の場合、この方法では必ずしも正確な結果を得られるわけではありません。 <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Unspecified> であるすべての <xref:System.DateTime> 値がローカル時間のように扱われます。 それ以外では、<xref:System.DateTime> は間違った時点を示すことになります。 この制限を回避するには、日付と時刻の値と該当するタイム ゾーンを確実に関連付けてから保存操作と復元操作を行います。

### <a name="to-round-trip-a-datetimeoffset-value"></a>DateTimeOffset 値をラウンドトリップさせるには

1. <xref:System.DateTimeOffset.ToString%28System.String%29?displayProperty=nameWithType> メソッドを "o" 書式指定子と共に呼び出して、<xref:System.DateTimeOffset> 値を対応する文字列形式に変換します。

2. <xref:System.DateTimeOffset> 値の文字列形式をファイルに保存するか、プロセス、アプリケーション ドメイン、またはマシン境界を通過させます。

3. <xref:System.DateTimeOffset> 値を表す文字列を取得します。

4. <xref:System.DateTimeOffset.Parse%28System.String%2CSystem.IFormatProvider%2CSystem.Globalization.DateTimeStyles%29?displayProperty=nameWithType> メソッドを呼び出し、`styles` パラメーターの値として <xref:System.Globalization.DateTimeStyles.RoundtripKind?displayProperty=nameWithType> を渡します。

<xref:System.DateTimeOffset> 値をラウンドトリップさせる方法を次の例に示します。

[!code-csharp[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#2)]
[!code-vb[Formatting.HowTo.RoundTrip#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#2)]

この方法により、<xref:System.DateTimeOffset> 値は常にある特定の時点として明確に識別されます。 識別されたら、<xref:System.DateTimeOffset.ToUniversalTime%2A?displayProperty=nameWithType> メソッドを呼び出すことで、この値を協定世界時 (UTC) に変換できます。あるいは、<xref:System.DateTimeOffset.ToOffset%2A?displayProperty=nameWithType> または <xref:System.TimeZoneInfo.ConvertTime%28System.DateTimeOffset%2CSystem.TimeZoneInfo%29?displayProperty=nameWithType> メソッドを呼び出すことで、特定のタイム ゾーンの時刻に変換できます。 この方法には、特定のタイム ゾーンの時刻を表す <xref:System.DateTimeOffset> 値に対して日付と時刻の演算を行ったときに、そのタイム ゾーンにおける正確な結果を得ることができない場合があるという重要な制限があります。 これは、<xref:System.DateTimeOffset> 値をインスタンス化すると、対応するタイム ゾーンとの関連付けが解除されるためです。 したがって、日付と時刻の計算を実行した場合、タイム ゾーンの調整規則は適用されなくなります。 この問題を回避するには、日付と時刻の値、および付随するタイム ゾーンの両方を保持するカスタム型を定義します。

### <a name="to-round-trip-a-date-and-time-value-with-its-time-zone"></a>日時値とそのタイム ゾーンをラウンドトリップするには

1. クラスまたは構造と 2 つのフィールドを定義します。 最初のフィールドは <xref:System.DateTime> または <xref:System.DateTimeOffset> オブジェクトです。2 つ目のフィールドは <xref:System.TimeZoneInfo> オブジェクトです。 次の例は、このような型を簡易にしたものです。

    [!code-csharp[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#3)]
    [!code-vb[Formatting.HowTo.RoundTrip#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#3)]

2. クラスに <xref:System.SerializableAttribute> 属性を付けます。

3. <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Serialize%2A?displayProperty=nameWithType> メソッドを使用し、オブジェクトをシリアル化します。

4. <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A> メソッドを使用し、オブジェクトを復元します。

5. 逆シリアル化されたオブジェクトを適切な型のオブジェクトにキャスト (C# の場合) するか、変換 (Visual Basic の場合) します。

次の例では、日時とタイム ゾーン情報の両方を保存するオブジェクトをラウンドトリップする方法を確認できます。

[!code-csharp[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/cs/RoundTrip.cs#4)]
[!code-vb[Formatting.HowTo.RoundTrip#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.RoundTrip/vb/RoundTrip.vb#4)]

この手法では、日時とタイム ゾーン オブジェクトを組み合わせるとき、日付値とタイム ゾーン値の同期が外れてはならないとき、保存と復元の前後で必ず、常に正しい時点を不明瞭なところなく反映する必要があります。

## <a name="compiling-the-code"></a>コードのコンパイル

これらの例には次の項目が必要です。

- 次の名前空間を C# `using` ステートメントまたは Visual Basic `Imports` ステートメントでインポートすること:

  - <xref:System> (C# のみ)。

  - <xref:System.Globalization?displayProperty=nameWithType>。

  - <xref:System.IO?displayProperty=nameWithType>.

  - <xref:System.Runtime.Serialization?displayProperty=nameWithType>.

  - <xref:System.Runtime.Serialization.Formatters.Binary?displayProperty=nameWithType>。

- `DateInTimeZone` クラス以外、各コード例はクラスまたは Visual Basic モジュールに含め、メソッドでラップし、`Main` メソッドから呼び出します。

## <a name="see-also"></a>関連項目

- [書式設定操作の実行](../../../docs/standard/base-types/performing-formatting-operations.md)
- [DateTime、DateTimeOffset、TimeSpan、および TimeZoneInfo の使い分け](../../../docs/standard/datetime/choosing-between-datetime.md)
- [標準の日時形式文字列](../../../docs/standard/base-types/standard-date-and-time-format-strings.md)
