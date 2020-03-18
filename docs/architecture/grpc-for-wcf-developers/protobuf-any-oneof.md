---
title: Protobuf variant 型のフィールドと、WCF 開発者向け gRPC の組み合わせ
description: Any 型および Oneof キーワードを使用して、メッセージ内のバリアントオブジェクト型を表す方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 6fe7acbd1ec35289f7ad6f3acee8509ab934619d
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543197"
---
# <a name="protobuf-any-and-oneof-fields-for-variant-types"></a>Protobuf variant 型のフィールドをすべて使用します。

Windows Communication Foundation (WCF) での動的プロパティ型 (`object`型のプロパティ) の処理は複雑になります。 たとえば、シリアライザーを指定し、 [Knowntype](xref:System.Runtime.Serialization.KnownTypeAttribute)属性を指定する必要があります。

プロトコルバッファー (Protobuf) には、複数の型の値を処理するための2つの簡単なオプションが用意されています。 `Any` 型は、既知の Protobuf メッセージ型を表すことができます。 また、`oneof` キーワードを使用して、任意のメッセージで設定できるフィールドの範囲が1つだけであることを指定できます。

## <a name="any"></a>Any

`Any` は、Protobuf の "既知の型" の1つです。サポートされているすべての言語の実装で、便利で再利用可能なメッセージ型のコレクションです。 `Any` の種類を使用するには、`google/protobuf/any.proto` 定義をインポートする必要があります。

```protobuf
syntax "proto3"

import "google/protobuf/any.proto"

message Stock {
    // Stock-specific data
}

message Currency {
    // Currency-specific data
}

message ChangeNotification {
    int32 id = 1;
    google.protobuf.Any instrument = 2;
}
```

C#コードでは、`Any` クラスに、フィールドの設定、メッセージの抽出、および型のチェックを行うメソッドが用意されています。

```csharp
public void FormatChangeNotification(ChangeNotification change)
{
    if (change.Instrument.Is(Stock.Descriptor))
    {
        FormatStock(change.Instrument.Unpack<Stock>());
    }
    else if (change.Instrument.Is(Currency.Descriptor))
    {
        FormatCurrency(change.Instrument.Unpack<Currency>());
    }
    else
    {
        throw new ArgumentException("Unknown instrument type");
    }
}
```

Protobuf の内部リフレクションコードでは、生成された各型の `Descriptor` 静的フィールドを使用して `Any` フィールド型を解決します。 `TryUnpack<T>` メソッドもありますが、`T` の初期化されていないインスタンスは失敗した場合でも作成されます。 前述のように `Is` メソッドを使用することをお勧めします。

## <a name="oneof"></a>うち

Oneof フィールドは言語機能です。コンパイラは、message クラスを生成するときに、`oneof` キーワードを処理します。 `oneof` を使用して `ChangeNotification` メッセージを指定する場合は、次のようになります。

```protobuf
message Stock {
    // Stock-specific data
}

message Currency {
    // Currency-specific data
}

message ChangeNotification {
  int32 id = 1;
  oneof instrument {
    Stock stock = 2;
    Currency currency = 3;
  }
}
```

`oneof` セット内のフィールドには、メッセージの宣言全体で一意のフィールド番号を指定する必要があります。

`oneof`を使用すると、生成C#されるコードには、どのフィールドが設定されているかを指定する列挙が含まれます。 列挙をテストして、どのフィールドが設定されているかを調べることができます。 設定されていないフィールドは、例外をスローするのではなく、`null` または既定値を返します。

```csharp
public void FormatChangeNotification(ChangeNotification change)
{
    switch (change.InstrumentCase)
    {
        case ChangeNotification.InstrumentOneofCase.None:
            return;
        case ChangeNotification.InstrumentOneofCase.Stock:
            FormatStock(change.Stock);
            break;
        case ChangeNotification.InstrumentOneofCase.Currency:
            FormatCurrency(change.Currency);
            break;
        default:
            throw new ArgumentException("Unknown instrument type");
    }
}
```

`oneof` セットの一部であるフィールドを設定すると、セット内の他のすべてのフィールドが自動的にクリアされます。 `oneof`で `repeated` を使用することはできません。 代わりに、この制限を回避するために、繰り返しフィールドまたは `oneof` 設定を使用して、入れ子になったメッセージを作成できます。

>[!div class="step-by-step"]
>[前へ](protobuf-reserved.md)
>[次へ](protobuf-enums.md)
