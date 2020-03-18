---
title: DateTimeOffset オブジェクトのインスタンス化
ms.date: 04/10/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- instantiating time zone objects
- time zone objects [.NET Framework], instantiation
- DateTimeOffset structure, converting to DateTime
- DateTimeOffset structure, instantiating
ms.assetid: 9648375f-d368-4373-a976-3332ece00c0a
ms.openlocfilehash: 1b1178623258393eab28a7087eb04c47b55a9176
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73122312"
---
# <a name="instantiating-a-datetimeoffset-object"></a>DateTimeOffset オブジェクトのインスタンス化

<xref:System.DateTimeOffset> 構造体には、新しい <xref:System.DateTimeOffset> 値を作成するさまざまな方法が用意されています。 これらの多くは、新しい <xref:System.DateTime> 値をインスタンス化するために使用できるメソッドに直接対応しています。拡張機能を使用すると、世界協定時刻 (UTC) からの日付と時刻の値のオフセットを指定できます。 具体的には、次の方法で <xref:System.DateTimeOffset> 値をインスタンス化できます。

- 日付と時刻のリテラルを使用する。

- <xref:System.DateTimeOffset> コンストラクターを呼び出す。

- 値を <xref:System.DateTimeOffset> 値に暗黙的に変換する。

- 日付と時刻の文字列形式を解析する。

このトピックでは、新しい <xref:System.DateTimeOffset> 値をインスタンス化するこれらの方法を示す、より詳細なコード例を示します。

## <a name="date-and-time-literals"></a>日付と時刻のリテラル

それをサポートする言語の場合、<xref:System.DateTime> 値をインスタンス化する最も一般的な方法の1つは、ハードコーディングされたリテラル値として日付と時刻を指定することです。 たとえば、次の Visual Basic コードでは、10:00 AM で2008年1月1日の値を持つ <xref:System.DateTime> オブジェクトを作成します。

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#1)]

<xref:System.DateTimeOffset> 値は、<xref:System.DateTime> リテラルをサポートする言語を使用するときに、日付と時刻のリテラルを使用して初期化することもできます。 たとえば、次の Visual Basic コードでは、<xref:System.DateTimeOffset> オブジェクトを作成します。

[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#2)]

コンソール出力に示すように、この方法で作成された <xref:System.DateTimeOffset> 値には、ローカルタイムゾーンのオフセットが割り当てられます。 つまり、コードが別のコンピューターで実行されている場合、文字リテラルを使用して割り当てられた <xref:System.DateTimeOffset> 値は、単一の時点を識別しません。

## <a name="datetimeoffset-constructors"></a>DateTimeOffset コンストラクター

<xref:System.DateTimeOffset> 型は、6つのコンストラクターを定義します。 これらの4つは <xref:System.DateTime> コンストラクターに直接対応しており、UTC からの日付と時刻のオフセットを定義する <xref:System.TimeSpan> 型の追加パラメーターがあります。 これにより、個々の日付と時刻のコンポーネントの値に基づいて、<xref:System.DateTimeOffset> 値を定義できます。 たとえば、次のコードでは、これらの4つのコンストラクターを使用して、7/1/2008 12:05 AM + 01:00 と同じ値を持つ <xref:System.DateTimeOffset> オブジェクトをインスタンス化しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#3)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#3)]

コンストラクターへの引数の1つとして <xref:System.Globalization.PersianCalendar> オブジェクトを使用してインスタンス化された <xref:System.DateTimeOffset> オブジェクトの値がコンソールに表示される場合は、ペルシャ暦ではなく、グレゴリオ暦の日付として表されることに注意してください。 ペルシャ暦を使用して日付を出力するには、<xref:System.Globalization.PersianCalendar> のトピックの例を参照してください。

他の2つのコンストラクターは、<xref:System.DateTime> 値から <xref:System.DateTimeOffset> オブジェクトを作成します。 1つ目のパラメーターには、<xref:System.DateTimeOffset> 値に変換する1つのパラメーター、<xref:System.DateTime> 値があります。 結果の <xref:System.DateTimeOffset> 値のオフセットは、コンストラクターの単一のパラメーターの <xref:System.DateTime.Kind%2A> プロパティによって異なります。 値が <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>の場合、オフセットは <xref:System.TimeSpan.Zero?displayProperty=nameWithType>と同じに設定されます。 それ以外の場合、オフセットはローカル タイム ゾーンのオフセットと等しい値に設定されます。 次の例は、このコンストラクターを使用して、UTC とローカルタイムゾーンを表す <xref:System.DateTimeOffset> オブジェクトをインスタンス化する方法を示しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#4)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#4)]

> [!NOTE]
> 1つの <xref:System.DateTime> パラメーターを持つ <xref:System.DateTimeOffset> コンストラクターのオーバーロードを呼び出すことは、<xref:System.DateTime> 値から <xref:System.DateTimeOffset> 値への暗黙的な変換を実行することと同じです。

<xref:System.DateTime> 値から <xref:System.DateTimeOffset> オブジェクトを作成する2つ目のコンストラクターには、変換する <xref:System.DateTime> 値と UTC からの日付と時刻のオフセットを表す <xref:System.TimeSpan> 値の2つのパラメーターがあります。 このオフセット値は、コンストラクターの最初のパラメーターの <xref:System.DateTime.Kind%2A> プロパティに対応している必要があります。一致しない場合、<xref:System.ArgumentException> がスローされます。 最初のパラメーターの <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>の場合は、2番目のパラメーターの値を <xref:System.TimeSpan.Zero?displayProperty=nameWithType>する必要があります。 最初のパラメーターの <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Local?displayProperty=nameWithType>の場合、2番目のパラメーターの値は、ローカルシステムのタイムゾーンのオフセットである必要があります。 最初のパラメーターの <xref:System.DateTime.Kind%2A> プロパティが <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>の場合、オフセットには任意の有効な値を指定できます。 次のコードは、<xref:System.DateTime> を <xref:System.DateTimeOffset> 値に変換するこのコンストラクターの呼び出しを示しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#5)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#5)]

## <a name="implicit-type-conversion"></a>暗黙の型変換

<xref:System.DateTimeOffset> 型は、<xref:System.DateTime> 値から <xref:System.DateTimeOffset> 値への暗黙的な型変換の1つをサポートします。 (暗黙的な型変換とは、明示的なキャスト (C# の場合) または変換 (Visual Basic の場合) を必要とせず、情報を失わない 1 つの型から別の型への変換です)。 これにより、次のようなコードが可能となります。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#6)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#6)]

結果の <xref:System.DateTimeOffset> 値のオフセットは、<xref:System.DateTime.Kind%2A?displayProperty=nameWithType> プロパティ値によって異なります。 値が <xref:System.DateTimeKind.Utc?displayProperty=nameWithType>の場合、オフセットは <xref:System.TimeSpan.Zero?displayProperty=nameWithType>と同じに設定されます。 値が <xref:System.DateTimeKind.Local?displayProperty=nameWithType> または <xref:System.DateTimeKind.Unspecified?displayProperty=nameWithType>の場合、オフセットはローカルタイムゾーンのオフセットと同じに設定されます。

## <a name="parsing-the-string-representation-of-a-date-and-time"></a>日付と時刻の文字列形式の解析

<xref:System.DateTimeOffset> 型では、日付と時刻の文字列形式を <xref:System.DateTimeOffset> の値に変換できる4つのメソッドがサポートされています。

- <xref:System.DateTimeOffset.Parse%2A>、日付と時刻の文字列形式を <xref:System.DateTimeOffset> 値に変換しようとし、変換に失敗した場合は例外をスローします。

- <xref:System.DateTimeOffset.TryParse%2A>、日付と時刻の文字列形式を <xref:System.DateTimeOffset> 値に変換しようとし、変換に失敗した場合は `false` を返します。

- <xref:System.DateTimeOffset.ParseExact%2A>、指定した形式の日付と時刻の文字列形式を <xref:System.DateTimeOffset> 値に変換しようとします。 変換が失敗すると、メソッドは例外をスローします。

- <xref:System.DateTimeOffset.TryParseExact%2A>、指定した形式の日付と時刻の文字列形式を <xref:System.DateTimeOffset> 値に変換しようとします。 変換が失敗すると、メソッドは `false` を返します。

次の例は、これらの4つの文字列変換メソッドを呼び出して、<xref:System.DateTimeOffset> 値をインスタンス化する方法を示しています。

[!code-csharp[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/cs/Instantiate.cs#7)]
[!code-vb[System.DateTimeOffset.Conceptual.Instantiate#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.DateTimeOffset.Conceptual.Instantiate/vb/Instantiate.vb#7)]

## <a name="see-also"></a>関連項目

- [日付、時刻およびタイム ゾーン](../../../docs/standard/datetime/index.md)
