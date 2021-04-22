---
title: Protobuf 入れ子型 - WCF 開発者向け gRPC
description: Protobuf と gRPC での入れ子になったメッセージ型と、それらを C# で生成する方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 7b9a331336ebe1ca7bc75fdd164b7b88ae4f9db2
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77542847"
---
# <a name="protobuf-nested-types"></a>Protobuf 入れ子型

C# で他のクラス内にクラスを宣言できるのと同様に、プロトコル バッファー (Protobuf) でも、メッセージの定義を他のメッセージ内に入れ子にすることができます。 次の例では、入れ子になったメッセージ型を作成する方法を示します。

```protobuf
message Outer {
    message Inner {
        string text = 1;
    }
    Inner inner = 1;
}
```

生成された C# コードで、`Inner` 型は、`HelloRequest` クラス内の入れ子になった静的 `Types` クラスで宣言されています。

```csharp
var inner = new Outer.Types.Inner { Text = "Hello" };
```

>[!div class="step-by-step"]
>[前へ](protobuf-data-types.md)
>[次へ](protobuf-repeated.md)
