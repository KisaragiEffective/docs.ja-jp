---
title: クラウドの準備が整っている回復力のあるサービスを構築する。 クラウド内の一時的な障害を受け入れる
description: Azure Cloud と Windows コンテナーを使って既存の .NET アプリケーションを最新化する | クラウドに対応する回復力のあるサービスを構築する。 クラウド内の一時的な障害を受け入れる
ms.date: 04/30/2018
ms.openlocfilehash: e516dc675ceb8def25c6d676bced0ea7f253b2d5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74711260"
---
# <a name="build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud"></a>クラウドの準備が整っている回復力のあるサービスの構築:クラウド内の一時的な障害を受け入れる

回復性は、障害から回復し、引き続き機能する能力です。 回復性は障害を回避することではなく、障害が発生するという事実を受け入れ、ダウンタイムまたはデータの損失を回避するようにそれらに対応することです。 回復性の目的は、障害発生後にアプリケーションを完全に機能する状態に戻すことです。

少なくとも、ハードウェアベースのモデルではなく、ソフトウェアベースの回復性のモデルを実装する場合、アプリケーションはクラウドに対応できます。 クラウド アプリケーションは、確実に発生する部分的な障害を受け入れる必要があります。 アプリケーションを設計または部分的にリファクタリングし、予想される部分的な障害に対する回復力を実現します。 一時的なネットワークの停止、クラウド内のノードまたは VM のクラッシュなどの部分的な障害に対処するように設計する必要があります。 オーケストレーター クラスタ内でコンテナーを別のノードに移動するだけでも、アプリケーション内での断続的な短いエラーが発生する可能性があります。

## <a name="handling-partial-failure"></a>部分的なエラーの処理

クラウドベースのアプリケーションでは、部分的な障害のリスクが常にあります。 たとえば、1 つの Web サイト インスタンスまたはコンテナーに障害が発生する場合や、短時間使用不能になったり、応答しなくなったりする場合があります。 また、1 つの VM またはサーバーがクラッシュする可能性があります。

クライアントとサービスは別のプロセスなので、サービスがクライアントの要求に迅速に応答できないことがあります。 サービスが過負荷になり、要求への応答が遅くなる場合や、ネットワークの問題のために短時間アクセスできなくなる場合があります。

たとえば、Azure SQL Database のデータベースにアクセスするモノリシック .NET アプリケーションについて考えてみます。 Azure SQL Database またはその他のサードパーティ サービスが短時間応答しない場合 (Azure SQL Database が別のノードまたはサーバーに移動し、数秒間応答しない場合)、ユーザーが何らかのアクションを実行しようとしたとき、アプリケーションがクラッシュし、同時に例外が表示される場合があります。

HTTP サービスを使用するアプリでも、同様のシナリオが発生する場合があります。 短時間の一時的な障害の間に、ネットワークまたはサービス自体をクラウドで利用できない場合があります。

図 4-9 に示すような回復性があるアプリケーションでは、"エクスポネンシャル バックオフを伴う再試行" などの手法を実装して、リソースの一時的な障害を処理する機会をアプリケーションに与える必要があります。 また、アプリケーションには "サーキット ブレーカー" も使用することをお勧めします。 サーキット ブレーカーを使うと、実際には長期的な障害の場合に、アプリケーションによるリソースへのアクセスを防ぎます。 アプリケーションにサーキット ブレーカーを使用することで、アプリケーション自体へのサービス拒否攻撃の発生が回避されます。

![エクスポネンシャル バックオフによる再試行によって処理される部分的な障害の図。](./media/retry-partial-failures.png)

**図 4-9** エクスポネンシャル バックオフを使用した再試行によって処理される部分的な障害

これらの手法は、HTTP リソースとデータベース リソースの両方で使用できます。 図 4-9 では、アプリケーションは 3 層アーキテクチャに基づいているため、これらの手法はサービス レベル (HTTP) およびデータ層レベル (TCP) で必要です。 データベースに加えて 1 つのアプリ層のみを使用する (追加のサービスやマイクロサービスは使用しない) モノリシック アプリケーションでは、データベース接続レベルで一時的な障害を処理するだけで十分な場合があります。 このシナリオでは、データベース接続の特定の構成のみが必要です。

データベースにアクセスする回復性がある通信を実装する場合、使用している .NET のバージョンによっては簡単な可能性があります (たとえば、[Entity Framework 6 以降](/ef/ef6/fundamentals/connection-resiliency/retry-logic)。 これは、データベース接続を構成するだけの問題です)。 または、[一時的エラー処理アプリケーション ブロック](https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)) (以前のバージョンの .NET の場合) などの追加のライブラリを使用したり、独自のライブラリを実装したりする必要がある場合があります。

HTTP 再試行とサーキット ブレーカーを実装する場合、.NET の推奨事項は、.NET Framework サポートを含む .NET Framework 4.0、.NET Framework 4.5、および .NET Standard 1.1 を対象とする [Polly](https://github.com/App-vNext/Polly) ライブラリを使用することです。

クラウドで部分的なエラーを処理する戦略を実装する方法については、次の参照情報を参照してください。

### <a name="additional-resources"></a>その他の技術情報

- **部分的なエラーを処理するための回復性がある通信の実装**

    [https://docs.microsoft.com/dotnet/architecture/microservices/implement-resilient-applications/partial-failure-strategies](../../microservices/implement-resilient-applications/partial-failure-strategies.md)

- **接続の回復性と再試行ロジックの Entity Framework (バージョン 6 以降)**

    [https://docs.microsoft.com/ef/ef6/fundamentals/connection-resiliency/retry-logic](/ef/ef6/fundamentals/connection-resiliency/retry-logic)

- **Transient Fault Handling Application Block**

- <https://docs.microsoft.com/previous-versions/msp-n-p/hh680934(v=pandp.50)>

- **回復性の高い HTTP 通信のためのライブラリ**

    https://github.com/App-vNext/Polly

>[!div class="step-by-step"]
>[前へ](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
>[次へ](modernize-your-apps-with-monitoring-and-telemetry.md)
