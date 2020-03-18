---
title: WS-* プロトコル-WCF 開発者向け gRPC
description: WCF でサポートされている WS-* プロトコルと gRPC で使用可能な代替方法の確認
author: markrendle
ms.date: 09/02/2019
ms.openlocfilehash: c8c06a5e23a4d7859165e1c562032055d63d76f7
ms.sourcegitcommit: f38e527623883b92010cf4760246203073e12898
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/20/2020
ms.locfileid: "77503295"
---
# <a name="ws--protocols"></a>WS-MANAGEMENT プロトコル\*

Windows Communication Foundation (WCF) を使用する利点の1つは、既存の_WS\*_ 標準プロトコルの多くをサポートすることでした。 このセクションでは、gRPC で同じ\* WS-MANAGEMENT プロトコルを管理する方法と、代替手段がない場合に使用できるオプションについて説明します。

## <a name="metadata-exchange-ws-policy-ws-discovery-and-so-on"></a>メタデータ交換: WS-POLICY、WS-I Discovery など

SOAP サービスでは、Web サービス記述言語 (WSDL) スキーマドキュメントが、データ形式、操作、通信オプションなどの情報と共に公開されます。 このスキーマを使用して、クライアントコードを生成できます。

gRPC は、サーバーとクライアントが同じ `.proto` ファイルから生成された場合に最適に機能しますが、サーバーリフレクションのオプションの拡張機能を使用すると、実行中のサーバーから動的な情報を公開することができます。 詳細については、「 [grpc. リフレクション](https://nuget.org/packages/Grpc.Reflection)NuGet パッケージ」と「 [grpc C#サーバーリフレクション](https://github.com/grpc/grpc/blob/master/doc/csharp/server_reflection.md)」を参照してください。

WS-MANAGEMENT プロトコルは、ローカルネットワーク上のサービスを検索するために使用されます。 gRPC サービスは通常、DNS または Consul や飼育係などのサービスレジストリを使用して配置されます。

## <a name="security-ws-security-ws-federation-xml-encryption-and-so-on"></a>セキュリティ: WS-SECURITY、WS-FEDERATION、XML 暗号化、その他

セキュリティ、認証、および承認の詳細については、[第6章](security.md)を参照してください。 ただし、WCF とは異なり、gRPC は WS-SECURITY、WS-FEDERATION、または XML 暗号化をサポートしていないことに注意してください。 それでも、gRPC は優れたセキュリティを提供しています。 すべての gRPC ネットワークトラフィックは、HTTP/2 over TLS を使用しているときに自動的に暗号化されます。 相互クライアント/サーバー認証には X509 証明書を使用できます。

## <a name="ws-reliablemessaging"></a>WS-ReliableMessaging

gRPC では、ws-reliablemessaging に相当するものは提供されません。 再試行のセマンティクスは、コードで処理する必要があります[。たとえば](https://github.com/App-vNext/Polly)、通常のようなライブラリを使用します。 Kubernetes または同様のオーケストレーション環境で実行されている場合、[サービスメッシュ](service-mesh.md)はサービス間の信頼性の高いメッセージングを提供するのにも役立ちます。

## <a name="ws-transaction-ws-coordination"></a>WS-ATOMICTRANSACTION、WS-ATOMICTRANSACTION

WCF による分散トランザクションの実装では、Microsoft 分散トランザクションコーディネーター (MSDTC) を使用します。 これは、SQL Server、MSMQ、Windows ファイルシステムなど、特にサポートされているリソースマネージャーと連携します。 現在のマイクロサービスの世界では、使用されているテクノロジの範囲が広いため、まだ同等のものはありません。 トランザクションの詳細については、「[付録 a](appendix.md)」を参照してください。

>[!div class="step-by-step"]
>[前へ](error-handling.md)
>[次へ](migrate-wcf-to-grpc.md)
