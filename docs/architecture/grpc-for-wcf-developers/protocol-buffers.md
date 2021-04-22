---
title: プロトコル バッファー - WCF 開発者向け gRPC
description: gRPC ネットワークに使用されるプロトコル バッファー ワイヤ形式の概要。
ms.date: 09/09/2019
ms.openlocfilehash: 35319d299a8bc2866a87954b3e54bfda9314ffe8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147933"
---
# <a name="protocol-buffers"></a>プロトコル バッファー

Windows Communication Foundation (WCF) のデータ コントラクトと同様に、gRPC サービスでは、データが "*プロトコル バッファー (Protobuf) メッセージ*" として送受信されます。 Protobuf は、コンピューターの読み書き用の構造化データをシリアル化する効率的な方法であり、XML や JSON のような人間が判読できる形式で発生するオーバーヘッドはありません。

この章では、Protobuf のしくみと、独自の Protobuf メッセージを定義する方法について説明します。

## <a name="how-protobuf-works"></a>Protobuf のしくみ

WCF のデータ コントラクトを含むほとんどの .NET オブジェクトのシリアル化手法は、リフレクションを使用して実行時にオブジェクトの構造を分析することで機能します。 これに対し、ほとんどの Protobuf ライブラリでは、専用の言語 ("*プロトコル バッファー言語*") を使用して、`.proto` ファイルで構造を事前に定義する必要があります。 その後、コンパイラにより、このファイルを使用して、サポートされている任意のプラットフォームのコードが生成されます。 サポートされているプラットフォームには、.NET、Java、C/C++、JavaScript などがあります。

Protobuf のコンパイラ `protoc` は Google によって管理されていますが、代替の実装も利用できます。 生成されたコードは効率的で、データの高速のシリアル化と逆シリアル化のために最適化されています。

Protobuf のワイヤ形式は、バイナリ エンコードです。 いくつかの巧妙なテクニックを使用して、メッセージを表すために使用されるバイト数が最小限に抑えられます。 Protobuf を使用するために、バイナリ エンコード形式の知識は必要ありません。 ただし、興味がある場合は、[プロトコル バッファーの Web サイト](https://developers.google.com/protocol-buffers/docs/encoding)で詳しく学習できます。

>[!div class="step-by-step"]
>[前へ](why-grpc.md)
>[次へ](protobuf-messages.md)
