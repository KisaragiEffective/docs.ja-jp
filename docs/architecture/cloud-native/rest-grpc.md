---
title: REST と gRPC
description: GRPC、クラウドネイティブアプリケーションでのその役割、および HTTP REST との違いについて説明します。
author: robvet
ms.date: 09/08/2019
ms.openlocfilehash: f5725106b251a7e62ef74ea36a63c663e5db36de
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76781064"
---
# <a name="rest-and-grpc"></a>REST と gRPC

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

ここまでは、 [REST ベース](https://docs.microsoft.com/azure/architecture/best-practices/api-design)の通信に重点を置いてきました。 REST は、分散コンピューターシステム間の相互運用性を促進するアーキテクチャスタイルです。 要求/応答モデルを使用します。このモデルでは、サーバーからのすべての応答がクライアントからの要求になります。 広く普及していますが、すべての問題に対して REST が完全に適合するわけではありません。 新しい通信テクノロジである gRPC は、人気を高め、クラウドネイティブ環境に向かっています。

## <a name="grpc"></a>gRPC

gRPC は、Google から発信されたオープンソースの通信です。 これは、分散コンピューティングで広く使われている[リモートプロシージャコール (RPC)](https://en.wikipedia.org/wiki/Remote_procedure_call)モデルに基づいて構築されています。 このモデルに従って、ローカルクライアントプログラムは、操作を実行するためのインプロセスメソッドを公開します。 この呼び出しは、分散ネットワーク経由でアウトプロセスマイクロサービスで呼び出されます。 開発者は、ローカルプロシージャ呼び出しとして操作をコードします。 基になるプラットフォームによって、ポイントツーポイントのネットワーク通信、シリアル化、および実行が抽象化されます。

gRPC は、軽量で高いパフォーマンスを備えた最新の RPC フレームワークです。 トランスポートプロトコルには HTTP/2 を使用します。 Http/2 は HTTP 1.1 と互換性がありますが、多くの高度な機能を備えています。

- HTTP 1.1 はデータをクリアテキストとして送信しますが、HTTP/2 はバイナリプロトコルです。 これにより、メモリの使用量を減らし、ネットワーク待機時間を短縮し、ネットワークリソースをより効率的に管理できるようになります。
- HTTP 1.1 は一度に1回のラウンドトリップ要求/応答の処理に制限されていますが、HTTP/2 では、同じ接続で多重化または複数の並列要求をサポートしています。
- HTTP/2 では、クライアントとサーバーの両方が同時に通信できる、全二重または双方向の通信がサポートされています。 クライアントは、サーバーが応答データを送信しているときと同時に要求データをアップロードできます。
- ストリーミングは HTTP/2 に組み込まれています。これは、要求と応答の両方が、大きなデータセットを非同期にストリーミングできることを意味します。
- GRPC と HTTP/2 を組み合わせると、パフォーマンスが飛躍的に向上します。 [Windows Communication Foundation (WCF)](https://docs.microsoft.com/dotnet/framework/wcf/whats-wcf)用語では、grpc のパフォーマンスは、 [nettcp バインド](https://docs.microsoft.com/dotnet/api/system.servicemodel.nettcpbinding?view=netframework-4.8)の速度と効率を満たしています。 ただし、NetTCP とは異なり、gRPC C#はや Visual Basic などの Microsoft 言語に制約されません。

gRPC は、Java、 C#、Golang、nodejs など、最も一般的なプラットフォームでサポートされています。

## <a name="protocol-buffers"></a>プロトコル バッファー

gRPC は、[プロトコルバッファー](https://developers.google.com/protocol-buffers/docs/overview)または Protobuf メッセージと呼ばれる別のオープンソーステクノロジを利用して、データの送受信を行います。 [WCF データコントラクト](https://docs.microsoft.com/dotnet/framework/wcf/feature-details/using-data-contracts)と同様に、Protobuf はシステム用の構造化データをシリアル化して読み取りと書き込みを行います。 これにより、XML や JSON などの人間が判読できる形式のオーバーヘッドが軽減されます。

多くのオブジェクトシリアル化手法は、実行時にデータオブジェクトの構造全体に反映されます。 Protobuf では、プラットフォームに依存しない言語 (プロトコルバッファー言語) を使用して、構造を事前に定義する必要があります。 各定義は、"proto" ファイルに格納されます。 次に、Protobuf compiler "Proton" を使用して、サポートされているプラットフォームのいずれかについて、クライアントとサーバーのコードを生成します。 生成されたコードは、データの高速なシリアル化/逆シリアル化のために最適化されています。 実行時には、各メッセージが厳密に型指定されたサービスコントラクトにラップされ、標準の Protobuf 表現でシリアル化されます。

## <a name="grpc-support-in-net"></a>.NET での gRPC のサポート

Microsoft .NET Core framework 3.0 には、gRPC のツールとネイティブサポートが含まれています。 図4-20 は、gRPC サービス用に gRPC スケルトンプロジェクトをスキャフォールディングする Visual Studio 2019 テンプレートを示しています。 .NET Core では、Windows、Linux、および macOS プラットフォームがサポートされていることに注意してください。

![Visual Studio 2019 での gRPC のサポート](./media/visual-studio-2019-grpc-template.png)

**図 4-20** Visual Studio 2019 での gRPC のサポート

.NET Core 3.0 は、エンドポイントのルーティング、組み込みの IoC サポート、ログ記録など、gRPC をフレームワークにシームレスに統合します。 オープンソース Kestrel web サーバーは、HTTP/2 接続を完全にサポートしています。

図4-21 は、Visual Studio 2019 における gRPC サービスの構造を示しています。 フォルダー構造には、proto ファイルおよびサービスコードのフォルダーが含まれていることに注意してください。

![Visual Studio 2019 の gRPC プロジェクト](./media/grpc-project.png  )

**図 4-21** Visual Studio 2019 の gRPC プロジェクト

## <a name="grpc-usage"></a>gRPC の使用

gRPC は、次のシナリオに適しています。

- 低待機時間と高スループット通信。 gRPC は、効率性が非常に重要な軽量マイクロサービスに適しています。
- ポイントツーポイントのリアルタイム通信。 gRPC は双方向ストリーミングに対して優れたサポートをしています。 gRPC サービスは、ポーリングせずにリアルタイムでメッセージをプッシュできます。
- 多言語環境– gRPC ツールでは、多くの一般的な開発言語がサポートされているため、多言語環境に適しています。
- ネットワークの制約付き環境– gRPC メッセージは、軽量メッセージ形式である Protobuf を使用してシリアル化されます。 GRPC メッセージは、常に同等の JSON メッセージよりも小さくなります。

この書籍の執筆時点では、ほとんどのブラウザーで gRPC のサポートが制限されています。 gRPC は HTTP/2 機能を多用しており、gRPC クライアントをサポートするために web 要求で必要な制御レベルを提供するブラウザーはありません。 gRPC は、通常、内部マイクロサービスからマイクロサービスへの通信に使用されます。 図4-22 は、単純な一般的な使用パターンを示しています。

![gRPC の使用パターン](./media/grpc-usage.png)

**図 4-22** gRPC の使用パターン

前の図では、バックエンドのマイクロサービスへのマイクロサービスが gRPC を使用しているときに、フロントエンドトラフィックが HTTP で呼び出されるしくみを示しています。

GRPC は、クラウドネイティブシステム向けに REST の支配を dethroning という大きな役割を果たすことができました。 パフォーマンスが向上し、開発が容易になりすぎます。 しかし、間違いはありませんが、残りの時間は長くなります。 パブリックに公開されている Api や旧バージョンとの互換性のためには引き続き威力を持っています。

>[!div class="step-by-step"]
>[前へ](service-to-service-communication.md)
>[次へ](service-mesh-communication-infrastructure.md)
