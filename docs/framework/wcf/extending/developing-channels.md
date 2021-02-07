---
description: 詳細については、「チャネルの開発」をご覧ください。
title: チャネルの開発
ms.date: 03/30/2017
ms.assetid: 0513af9f-a0c2-457b-9a50-5b6bfee48513
ms.openlocfilehash: 7fd6dc68a72d5167868c3ccee0f53c1ba3090322
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99743747"
---
# <a name="developing-channels"></a>チャネルの開発

Windows Communication Foundation (WCF) アプリケーション層で使用できるプロトコルまたはトランスポートチャネルを開発するには、いくつかの手順が必要です。 このトピックでは、これらの手順について説明し、詳細情報の参照先となる特定のトピックを示します。 チャネルモデルと、このトピックで説明されているさまざまな型については、「 [チャネルモデルの概要](channel-model-overview.md)」を参照してください。 トランスポートチャネルの完全なサンプルについては、「 [transport: UDP](../samples/transport-udp.md)」を参照してください。  
  
## <a name="the-channel-development-task-list"></a>チャネル開発タスクの一覧  

 ユーザー定義チャネルを作成する手順は、次のとおりです。 すべてのチャネルで、次の手順が必要です。  
  
1. <xref:System.ServiceModel.Channels.IOutputChannel> と <xref:System.ServiceModel.Channels.IInputChannel> で、チャネルのメッセージ交換パターン (<xref:System.ServiceModel.Channels.IDuplexChannel>、<xref:System.ServiceModel.Channels.IRequestChannel>、<xref:System.ServiceModel.Channels.IReplyChannel>、<xref:System.ServiceModel.Channels.IChannelFactory>、または <xref:System.ServiceModel.Channels.IChannelListener>) のうちのどれをサポートするか、また、選択したパターンでこれらのインターフェイスのセッションの多いバリエーションをサポートするかどうかを決定します。 詳細については、「 [メッセージ交換パターンの選択](choosing-a-message-exchange-pattern.md)」を参照してください。  
  
2. 選択したメッセージ交換パターンをサポートするチャネル ファクトリおよびリスナー (<xref:System.ServiceModel.Channels.IChannelFactory> および <xref:System.ServiceModel.Channels.IChannelListener>) を作成します。 ファクトリの開発の詳細については、「 [クライアント: チャネルファクトリと](client-channel-factories-and-channels.md)チャネル」を参照してください。 リスナーの開発の詳細については、「 [サービス: チャネルリスナーと](service-channel-listeners-and-channels.md)チャネル」を参照してください。  
  
3. ネットワーク固有の例外が、<xref:System.TimeoutException?displayProperty=nameWithType> の適切な派生クラスまたは <xref:System.ServiceModel.CommunicationException> に標準化されていることを確認します。 詳細については、「 [例外とエラーの処理](handling-exceptions-and-faults.md)」を参照してください。  
  
4. アプリケーション レイヤーから使用できるようにするには、カスタム チャネルを追加する <xref:System.ServiceModel.Channels.BindingElement> をチャネル スタックに追加します。 詳細については、「 [BindingElement の作成](creating-a-bindingelement.md)」を参照してください。  
  
 アプリケーション レイヤーでより完全なサポートを実現するには、次の追加手順が必要です。  
  
1. バインド要素拡張セクションを追加して、新しいバインド要素を構成システムに公開します。 詳細については、「 [構成とメタデータのサポート](configuration-and-metadata-support.md)」を参照してください。  
  
2. 他のエンドポイントに機能を伝達するメタデータ拡張を追加します。 詳細については、「 [構成とメタデータのサポート](configuration-and-metadata-support.md)」を参照してください。  
  
3. 適切に定義されたプロファイルに従って、バインド要素のスタックを事前構成するバインディングを追加します。 詳細については、「 [User-Defined バインドの作成](creating-user-defined-bindings.md)」を参照してください。  
  
4. 構成システムにバインディングを開示する、バインディング セクションおよびバインド構成要素を追加します。 詳細については、「 [構成とメタデータのサポート](configuration-and-metadata-support.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [バインディングの拡張](extending-bindings.md)
