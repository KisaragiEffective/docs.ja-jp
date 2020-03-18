---
title: Protobuf maps for dictionary-gRPC for WCF 開発者
description: Protobuf maps を使用して .NET で辞書型を表す方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: bf848bbc7e3618f6d78e280fcd85d5eb88d5cfae
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77543132"
---
# <a name="protobuf-maps-for-dictionaries"></a>Protobuf のディクショナリのマップ

メッセージ内の名前付きの値の任意のコレクションを表すことができることが重要です。 .NET では、通常、これはディクショナリ型によって処理されます。 プロトコルバッファー (Protobuf) の .NET <xref:System.Collections.Generic.IDictionary%602> 型に相当するものは、`map<key_type, value_type>` の種類です。 このセクションでは、Protobuf で `map` 型を宣言する方法と、生成されたコードを使用する方法について説明します。

```protobuf
message StockPrices {
    map<string, double> prices = 1;
}
```

生成されたコードでは、`map` フィールドは `Google.Protobuf.Collections.MapField<TKey, TValue>` クラスを使用します。 このクラスは、<xref:System.Collections.Generic.IDictionary%602>を含む標準の .NET コレクションインターフェイスを実装します。

マップフィールドは、メッセージ定義内で直接繰り返すことはできません。 ただし、次の例のように、マップを含む入れ子になったメッセージを作成し、メッセージの種類に `repeated` を使用することができます。

```protobuf
message Order {
    message Attributes {
        map<string, string> values = 1;
    }
    repeated Attributes attributes = 1;
}
```

## <a name="using-mapfield-properties-in-code"></a>コードでの MapField プロパティの使用

`map` フィールドから生成された `MapField` プロパティは読み取り専用であり、`null`されることはありません。 マッププロパティを設定するには、空の `MapField` プロパティの `Add(IDictionary<TKey,TValue> values)` メソッドを使用して、任意の .NET ディクショナリから値をコピーします。

```csharp
public Order CreateOrder(Dictionary<string, string> attributes)
{
    var order = new Order();
    order.Attributes.Add(attributes);
    return order;
}
```

## <a name="further-reading"></a>関連項目

Protobuf の詳細については、公式の[Protobuf のドキュメント](https://developers.google.com/protocol-buffers/docs/overview)を参照してください。

>[!div class="step-by-step"]
>[前へ](protobuf-enums.md)
>[次へ](wcf-services-to-grpc-comparison.md)
