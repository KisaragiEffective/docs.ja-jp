---
title: プロトブーフ列挙 - WCF 開発者向け gRPC
description: Protobuf で列挙体を宣言して使用する方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 2177f568a671fa0e651625c6e025ac70c243feb5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79148076"
---
# <a name="protobuf-enumerations"></a>Protobuf 列挙型

Protobuf は列挙型をサポートしています。 前のセクションでは、フィールドの種類を決定するために列挙型を使用したこのサポートを`Oneof`確認しました。 独自の列挙型を定義すると、Protobuf は C# 列挙型にコンパイルします。

Protobuf はさまざまな言語で使用できるため、列挙体の名前付け規則は C# の規則とは異なります。 ただし、コード ジェネレーターは、名前を従来の C# の大文字と小文字に変換します。 フィールド名の Pascal の大文字と小文字の対応が列挙名で始まる場合は、削除されます。

たとえば、次の Protobuf 列挙体では、フィールドの先頭に`ACCOUNT_STATUS`. このプレフィックスは、パスカルケースの列挙名と`AccountStatus`同じです。

```protobuf
enum AccountStatus {
  ACCOUNT_STATUS_UNKNOWN = 0;
  ACCOUNT_STATUS_PENDING = 1;
  ACCOUNT_STATUS_ACTIVE = 2;
  ACCOUNT_STATUS_SUSPENDED = 3;
  ACCOUNT_STATUS_CLOSED = 4;
}
```

ジェネレーターは、次のコードと同等の C# 列挙型を作成します。

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

Protobuf 列挙型の定義は、最初のフィールドとしてゼロ定数を持つ*必要があります*。 C# と同様に、同じ値を持つ複数のフィールドを宣言できます。 ただし、列挙型のオプションを使用して、この`allow_alias`オプションを明示的に有効にする必要があります。

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

列挙型は`.proto`、ファイルの最上位レベルで宣言するか、メッセージ定義内で入れ子にできます。 入れ子になったメッセージなどの入れ子になった列挙型は、生成`.Types`されたメッセージ クラスの静的クラス内で宣言されます。

プロトブーフで生成された列挙型に[[Flags]](xref:System.FlagsAttribute)属性を適用する方法はなく、Protobuf はビットごとの列挙型の組み合わせを理解していません。 次の例を見てください。

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

に`product.AvailableIn``Region.NorthAmerica | Region.SouthAmerica`設定すると、整数値としてシリアル化されます`3`。 クライアントまたはサーバーが値を逆シリアル化しようとすると、`3`の enum 定義に一致するものが見つかりません。 結果は`Region.None`.

Protobuf で複数の列挙値を操作する最善の方法は、`repeated`列挙型のフィールドを使用することです。

>[!div class="step-by-step"]
>[前次](protobuf-any-oneof.md)
>[Next](protobuf-maps.md)
