---
title: リストと配列の繰り返しフィールド - WCF 開発者向け gRPC
description: Protobuf がコレクションを処理する方法と、コレクションと .NET コレクションとの関連について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 63d99532d14deea7800673dd5a6350dd9362ad54
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147972"
---
# <a name="repeated-fields-for-lists-and-arrays"></a>リストと配列の繰り返しフィールド

プレフィックス キーワードを使用して、プロトコル バッファー (Protobuf) でリストを`repeated`指定します。 次の例は、リストを作成する方法を示しています。

```protobuf
message Person {
    // Other fields elided
    repeated string aliases = 8;
}
```

生成されたコードでは、`repeated`フィールドは、組み`Google.Protobuf.Collections.RepeatedField<T>`込みの .NET コレクション型ではなく、ジェネリック型で表されます。

型`RepeatedField<T>`には、リストをシリアル化し、バイナリ ワイヤ形式に逆シリアル化するために必要なコードが含まれています。 標準の .NET コレクション インターフェイス<xref:System.Collections.Generic.IList%601>を実装します。 <xref:System.Collections.Generic.IEnumerable%601> そのため、LINQ クエリを使用したり、簡単に配列またはリストに変換したりできます。

>[!div class="step-by-step"]
>[前次](protobuf-nested-types.md)
>[Next](protobuf-reserved.md)
