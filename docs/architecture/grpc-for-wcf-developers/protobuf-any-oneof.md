---
title: Protobuf のバリアント型の Any フィールドと OneOf フィールド - WCF 開発者向け gRPC
description: Any 型と Oneof キーワードを使用して、メッセージ内のバリアント オブジェクト型を表す方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 6fe7acbd1ec35289f7ad6f3acee8509ab934619d
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543197"
---
# <a name="protobuf-any-and-oneof-fields-for-variant-types"></a>Protobuf のバリアント型の Any フィールドと OneOf フィールド

Windows Communication Foundation (WCF) で動的プロパティ型 (つまり、`object` 型のプロパティ) を処理することは複雑です。 たとえば、シリアライザーを指定し、[KnownType](xref:System.Runtime.Serialization.KnownTypeAttribute) 属性を指定する必要があります。

プロトコル バッファー (Protobuf) には、複数の型の値を処理するための 2 つの簡単なオプションが用意されています。 `Any` 型では、任意の既知の Protobuf メッセージ型を表すことができます。 また、`oneof` キーワードを使用すると、ある範囲のフィールドのうちの 1 つだけをメッセージで設定できることを指定できます。

## <a name="any"></a>Any

`Any` は、Protobuf の "既知の型"、つまりサポートされているすべての言語の実装での、便利で再利用可能なメッセージ型のコレクションの 1 つです。 `Any` 型を使用するには、`google/protobuf/any.proto` 定義をインポートする必要があります。

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

C# コードでは、`Any` クラスに、フィールドの設定、メッセージの抽出、型のチェックを行うためのメソッドが用意されています。

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

Protobuf の内部リフレクション コードでは、生成された各型の `Descriptor` 静的フィールドを使用して、`Any` フィールドの型が解決されます。 また、`TryUnpack<T>` メソッドもありますが、それを使用すると、失敗した場合でも、`T` の初期化されていないインスタンスが作成されます。 前に示したように、`Is` メソッドを使用することをお勧めします。

## <a name="oneof"></a>Oneof

Oneof フィールドは言語機能です。コンパイラにより、メッセージ クラスを生成するときに `oneof` キーワードが処理されます。 `oneof` を使用した `ChangeNotification` メッセージの指定は、次のようになります。

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

`oneof` セット内のフィールドには、メッセージの宣言全体で一意のフィールド番号が含まれている必要があります。

`oneof` を使用する場合、生成される C# コードには、どのフィールドが設定されているかを示す列挙型が含まれます。 その列挙型をテストして、どのフィールドが設定されているかを確認することができます。 設定されていないフィールドによって、例外がスローされるのではなく、`null` または既定値が返されます。

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

`oneof` セットの一部である任意のフィールドを設定すると、セット内の他のすべてのフィールドは自動的にクリアされます。 `oneof` で `repeated` を使用することはできません。 代わりに、repeated フィールドまたは `oneof` セットを使用して入れ子になったメッセージを作成し、この制限を回避することができます。

>[!div class="step-by-step"]
>[前へ](protobuf-reserved.md)
>[次へ](protobuf-enums.md)
