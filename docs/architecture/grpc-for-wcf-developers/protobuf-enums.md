---
title: Protobuf の列挙型 - WCF 開発者向け gRPC
description: Protobuf で列挙型を宣言して使用する方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 2177f568a671fa0e651625c6e025ac70c243feb5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148076"
---
# <a name="protobuf-enumerations"></a>Protobuf 列挙型

Protobuf では列挙型がサポートされています。 このサポートについては前のセクションで見ました。そこでは、列挙型を使用して `Oneof` フィールドの型を決定しました。 独自の列挙型を定義することができ、それらは Protobuf によって C# の列挙型にコンパイルされます。

さまざまな言語で Protobuf を使用できるため、列挙の名前付け規則は C# の規則とは異なります。 ただし、コード ジェネレーターにより、名前は従来の C# の形式に変換されます。 フィールド名に対応する Pascal 形式が列挙名で始まる場合、それは削除されます。

たとえば、次の Protobuf 列挙型では、フィールドの先頭に `ACCOUNT_STATUS` が付いています。 このプレフィックスは、Pascal 形式の列挙型名 `AccountStatus` に相当します。

```protobuf
enum AccountStatus {
  ACCOUNT_STATUS_UNKNOWN = 0;
  ACCOUNT_STATUS_PENDING = 1;
  ACCOUNT_STATUS_ACTIVE = 2;
  ACCOUNT_STATUS_SUSPENDED = 3;
  ACCOUNT_STATUS_CLOSED = 4;
}
```

ジェネレーターにより、次のコードに相当する C# の列挙型が作成されます。

```csharp
public enum AccountStatus
{
    Unknown = 0,
    Pending = 1,
    Active = 2,
    Suspended = 3,
    Closed = 4
}
```

Protobuf の列挙型の定義には、最初のフィールドとして定数 0 が含まれている "*必要があります*"。 C# と同様に、同じ値を持つ複数のフィールドを宣言できます。 ただし、列挙型で `allow_alias` オプションを使用することで、このオプションを明示的に有効にする必要があります。

```protobuf
enum AccountStatus {
  option allow_alias = true;
  ACCOUNT_STATUS_UNKNOWN = 0;
  ACCOUNT_STATUS_PENDING = 1;
  ACCOUNT_STATUS_ACTIVE = 2;
  ACCOUNT_STATUS_SUSPENDED = 3;
  ACCOUNT_STATUS_CLOSED = 4;
  ACCOUNT_STATUS_CANCELLED = 4;
}
```

列挙型は、`.proto` ファイルの最上位レベルで宣言することも、メッセージ定義内に入れ子にすることもできます。 入れ子になった列挙型は、入れ子になったメッセージのように、生成されるメッセージ クラスの `.Types` 静的クラス内で宣言されます。

Protobuf で生成される列挙型に [[Flags]](xref:System.FlagsAttribute) 属性を適用する方法はなく、ビットごとの列挙値の組み合わせは Protobuf で認識されません。 次の例を見てください。

```protobuf
enum Region {
  REGION_NONE = 0;
  REGION_NORTH_AMERICA = 1;
  REGION_SOUTH_AMERICA = 2;
  REGION_EMEA = 4;
  REGION_APAC = 8;
}

message Product {
  Region available_in = 1;
}
```

`product.AvailableIn` を `Region.NorthAmerica | Region.SouthAmerica` に設定すると、整数値 `3` としてシリアル化されます。 クライアントまたはサーバーでその値を逆シリアル化しようとすると、列挙型の定義に `3` と一致するものが見つかりません。 結果は `Region.None` となります。

Protobuf で複数の列挙値を操作する最善の方法は、列挙型の `repeated` フィールドを使用することです。

>[!div class="step-by-step"]
>[前へ](protobuf-any-oneof.md)
>[次へ](protobuf-maps.md)
