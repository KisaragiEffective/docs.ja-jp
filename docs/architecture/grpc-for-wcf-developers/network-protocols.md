---
title: ネットワークプロトコル-WCF 開発者向け gRPC
description: GRPC ネットワークプロトコルの概要について説明します。
ms.date: 09/02/2019
ms.openlocfilehash: 1ceb140f7b7ac7e796a87612ebb9d21e28d33968
ms.sourcegitcommit: 44a7cd8687f227fc6db3211ccf4783dc20235e51
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/26/2020
ms.locfileid: "77628489"
---
# <a name="network-protocols"></a>ネットワーク プロトコル

Windows Communication Foundation (WCF) とは異なり、gRPC はネットワークのベースとして HTTP/2 を使用します。 これは、HTTP/1.1 でのみ動作する WCF と SOAP よりも大きな利点を提供します。 GRPC の使用を検討している開発者にとって、HTTP/2 の代替手段がない場合は、HTTP/2 の詳細を確認し、gRPC を使用する利点をさらに特定することが理想的です。

2015の Internet Engineering Task Force によってリリースされた HTTP/2 は、既に Google によって使用されていた実験的な SPDY プロトコルから派生しました。 これは、HTTP/1.1 よりも効率的で高速かつ安全になるように設計されています。

## <a name="key-features-of-http2"></a>HTTP/2 の主な機能

この一覧には、HTTP/2 の主な機能と利点がいくつか示されています。

### <a name="binary-protocol"></a>バイナリプロトコル

要求/応答サイクルでは、テキストコマンドが不要になりました。 これにより、コマンドの実装が簡略化され、高速化されます。 具体的には、データの解析が高速で、使用するメモリが少なくなるため、ネットワーク待機時間が短縮されます。

### <a name="streams"></a>ストリーム

ストリームを使用すると、送信側と受信側の間に有効期間の長い接続を作成し、複数のメッセージやフレームを非同期に送信することができます。 複数のストリームは、1つの HTTP/2 接続で個別に操作できます。

### <a name="request-multiplexing-over-a-single-tcp-connection"></a>1つの TCP 接続で多重化を要求する

この機能は、HTTP/2 の最も重要なイノベーションの1つです。 データに対して複数の並列要求が許可されるため、1台のサーバーから web ファイルを同時にダウンロードできるようになりました。 Web サイトの読み込みが速くなり、最適化の必要性が軽減されます。 先行 (ホル) ブロック。これは、前の要求が完了するまで送信されるのを待機する必要がある場合にも軽減されます (ただし、ホルブロックは TCP トランスポートレベルでも発生します)。

### <a name="nettcp-like-performance-cross-platform"></a>Net.tcp と同様のパフォーマンス、クロスプラットフォーム

基本的に、gRPC と HTTP/2 の組み合わせにより、開発者は、少なくとも WCF の Net.tcp バインドと同等の速度と効率が得られます。また、場合によっては、速度と効率が向上します。 ただし、Net.tcp とは異なり、gRPC over HTTP/2 は .NET アプリケーションに制約されません。

>[!div class="step-by-step"]
>[前へ](interface-definition-language.md)
>[次へ](why-grpc.md)
