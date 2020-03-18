---
title: プロトコル バッファー - WCF 開発者向けの gRPC
description: gRPC ネットワーキングに使用されるプロトコル バッファの配線形式の概要。
ms.date: 09/09/2019
ms.openlocfilehash: 35319d299a8bc2866a87954b3e54bfda9314ffe8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2020
ms.locfileid: "79147933"
---
# <a name="protocol-buffers"></a>プロトコル バッファ

gRPC サービスは、プロトコル*バッファー (Protobuf) メッセージ*としてデータを送受信する Windows 通信基盤 (WCF) のデータ コントラクトと同様です。 Protobuf は、XML や JSON などの人間が読み取り可能な形式で発生するオーバーヘッドを伴わずに、コンピューターが読み書きできるように構造化データを効率的にシリアル化する方法です。

この章では、Protobuf のしくみと、独自の Protobuf メッセージを定義する方法について説明します。

## <a name="how-protobuf-works"></a>プロトブーフの仕組み

WCF のデータ コントラクトを含むほとんどの .NET オブジェクトシリアル化手法は、実行時にオブジェクト構造を分析するリフレクションを使用して動作します。 一方、Protobuf ライブラリの多くは、ファイル内の専用言語 ( プロトコル*バッファ言語*) を`.proto`使用して、事前に構造を定義する必要があります。 コンパイラは、このファイルを使用して、サポートされているプラットフォームのコードを生成します。 サポートされるプラットフォームには、.NET、Java、C/C++、JavaScriptなどがあります。

Protobuf コンパイラは`protoc`、別の実装が利用可能ですが、Google によって保守されています。 生成されたコードは効率的で、データの高速なシリアル化と逆シリアル化に最適化されています。

Protobuf ワイヤー形式はバイナリエンコーディングです。 メッセージを表すために使用されるバイト数を最小限に抑えるために、巧妙なトリックを使用します。 バイナリエンコーディング形式の知識は、Protobufを使用する必要はありません。 しかし、興味がある場合は、[プロトコルバッファのウェブサイト](https://developers.google.com/protocol-buffers/docs/encoding)で詳細を学ぶことができます。

>[!div class="step-by-step"]
>[前次](why-grpc.md)
>[Next](protobuf-messages.md)
