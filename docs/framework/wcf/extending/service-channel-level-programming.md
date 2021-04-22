---
description: '詳細情報: サービス チャネル レベルのプログラミング'
title: サービス チャネル レベルのプログラミング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 8d8dcd85-0a05-4c44-8861-4a0b3b90cca9
ms.openlocfilehash: 9d89b073430ccdf80bdcbdfc50fd1e002fc807ff
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644062"
---
# <a name="service-channel-level-programming"></a>サービス チャネル レベルのプログラミング

ここでは、Windows Communication Foundation (WCF) サービス アプリケーションを <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> とこれに関係するオブジェクト モデルを使用しないで作成する方法を説明します。  
  
## <a name="receiving-messages"></a>受信、メッセージ  

 メッセージの受信と処理の準備を整えるには、次の手順に従う必要があります。  
  
1. バインディングを作成します。  
  
2. チャネル リスナーをビルドします。  
  
3. チャネル リスナーを開きます。  
  
4. 要求を読み取り、応答を送信します。  
  
5. すべてのチャネル オブジェクトを閉じます。  
  
#### <a name="creating-a-binding"></a>バインディングの作成  

 メッセージのリッスンと受信の最初の手順として、バインディングを作成します。 WCF には、組み込みバインディングまたはシステム標準のバインディングが複数付属しており、そのうちの 1 個をインスタンス化することによって直接使用できます。 また、CustomBinding クラスをインスタンス化することにより、独自のバインディングを作成することもできます。手順 1. のコードは、この処理を実行します。  
  
 後のコード例は、<xref:System.ServiceModel.Channels.CustomBinding?displayProperty=nameWithType> のインスタンスを作成し、その Elements コレクションに <xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=nameWithType> を追加します。Elements コレクションは、チャネル スタックを作成するために使用されるバインド要素のコレクションです。 この例では、Elements コレクションには <xref:System.ServiceModel.Channels.HttpTransportBindingElement> しか含まれないため、チャネル スタックは HTTP トランスポート チャネルだけを持ちます。  
  
#### <a name="building-a-channellistener"></a>ChannelListener のビルド  

 バインディングを作成したら、<xref:System.ServiceModel.Channels.Binding.BuildChannelListener%2A?displayProperty=nameWithType> を呼び出してチャネル リスナーをビルドします。型パラメーターは、作成するチャネル形状です。 この例では、要求/応答メッセージ交換パターンで受信メッセージをリッスンする必要があるため、<xref:System.ServiceModel.Channels.IReplyChannel?displayProperty=nameWithType> を使用します。  
  
 <xref:System.ServiceModel.Channels.IReplyChannel> は、要求メッセージを受信し、応答メッセージを返信するために使用されます。 <xref:System.ServiceModel.Channels.IReplyChannel.ReceiveRequest%2A?displayProperty=nameWithType> を呼び出すと、要求メッセージの受信と応答メッセージの返信に使用できる <xref:System.ServiceModel.Channels.IRequestChannel?displayProperty=nameWithType> が返されます。  
  
 リスナーを作成する際には、リッスンするネットワーク アドレス (この場合は `http://localhost:8080/channelapp`) を渡します。 一般に、各トランスポート チャネルは 1 つまたは複数のアドレス スキームをサポートします。たとえば、HTTP トランスポートは、http スキームと https スキームの両方をサポートします。  
  
 また、リスナーを作成する際には、空の <xref:System.ServiceModel.Channels.BindingParameterCollection?displayProperty=nameWithType> を渡します。 バインディング パラメーターは、リスナーのビルド方法を制御するパラメーターを渡すしくみです。 この例では、このようなパラメーターは使用しないため、空のコレクションを渡します。  
  
#### <a name="listening-for-incoming-messages"></a>受信メッセージのリッスン  

 次に、ビルドしたリスナーで <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> を呼び出し、チャネルの受け入れを開始します。 <xref:System.ServiceModel.Channels.IChannelListener%601.AcceptChannel%2A?displayProperty=nameWithType> の動作は、トランスポートが接続指向であるか、コネクションレスであるかによって異なります。 接続指向トランスポートの場合、<xref:System.ServiceModel.Channels.IChannelListener%601.AcceptChannel%2A> は新しい接続要求が届くまでブロックし、接続要求が届いた時点で、その新しい接続を表す新しいチャネルを返します。 HTTP などのコネクションレス トランスポートの場合、<xref:System.ServiceModel.Channels.IChannelListener%601.AcceptChannel%2A> は、トランスポート リスナーが作成する唯一のチャネルを直ちに返します。  
  
 この例では、リスナーは、<xref:System.ServiceModel.Channels.IReplyChannel> を実装するチャネルを返します。 このチャネルでメッセージを受信するには、まず、チャネルで <xref:System.ServiceModel.ICommunicationObject.Open%2A?displayProperty=nameWithType> を呼び出して通信できる状態にします。 次に、メッセージが到着するまでブロックする <xref:System.ServiceModel.Channels.IReplyChannel.ReceiveRequest%2A> を呼び出します。  
  
#### <a name="reading-the-request-and-sending-a-reply"></a>要求の読み取りと応答の送信  

 <xref:System.ServiceModel.Channels.IReplyChannel.ReceiveRequest%2A> が <xref:System.ServiceModel.Channels.RequestContext> を返すときに、その <xref:System.ServiceModel.Channels.RequestContext.RequestMessage%2A> プロパティを使用して受信メッセージを取得します。 メッセージのアクションと本文のコンテンツ (文字列であることを前提とします) を書き込みます。  
  
 応答を送信するには、新しい応答メッセージを作成します。この場合は、要求で受信した文字列データを渡します。 次に、<xref:System.ServiceModel.Channels.RequestContext.Reply%2A> を呼び出して、その応答メッセージを送信します。  
  
#### <a name="closing-objects"></a>オブジェクトの終了  

 リソースのリークを避けるには、通信に使用したオブジェクトが不要になったら閉じることが重要です。 この例では、要求メッセージ、要求コンテキスト、チャネル、およびリスナーを閉じます。  
  
 次のコード例は、チャネル リスナーがメッセージを 1 つだけ受け取る基本的なサービスです。 実際のサービスでは、サービスが終了するまでチャネルの受け入れとメッセージの受信を続けます。  
  
 [!code-csharp[ChannelProgrammingBasic#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/channelprogrammingbasic/cs/serviceprogram.cs#1)]
 [!code-vb[ChannelProgrammingBasic#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/channelprogrammingbasic/vb/serviceprogram.vb#1)]
