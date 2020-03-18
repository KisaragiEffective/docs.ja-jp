---
title: Protobuf 入れ子になった型-gRPC (WCF 開発者向け)
description: Protobuf と gRPC での入れ子になったメッセージの種類、およびでC#の生成方法について説明します。
ms.date: 09/09/2019
ms.openlocfilehash: 7b9a331336ebe1ca7bc75fdd164b7b88ae4f9db2
ms.sourcegitcommit: 771c554c84ba38cbd4ac0578324ec4cfc979cf2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/21/2020
ms.locfileid: "77542847"
---
# <a name="protobuf-nested-types"></a>Protobuf 入れ子型

と同様C#に、他のクラス内でクラスを宣言できるのと同じように、プロトコルバッファー (Protobuf) を使用すると、メッセージ定義を他のメッセージ内に入れ子にすることができます。 次の例は、入れ子になったメッセージ型を作成する方法を示しています。

```protobuf
message Outer {
    message Inner {
        string text = 1;
    }
    Inner inner = 1;
}
```

生成C#されたコードでは、`Inner` 型は `HelloRequest` クラス内の入れ子になった静的 `Types` クラスで宣言されます。

```csharp
var inner = new Outer.Types.Inner { Text = "Hello" };
```

>[!div class="step-by-step"]
>[前へ](protobuf-data-types.md)
>[次へ](protobuf-repeated.md)
