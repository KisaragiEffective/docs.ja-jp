---
description: '詳細情報: サービス: チャネル リスナーとチャネル'
title: 'サービス: チャネル リスナーとチャネル'
ms.date: 03/30/2017
ms.assetid: 8ccbe0e8-7e55-441d-80de-5765f67542fa
ms.openlocfilehash: 2a092813faaa6f8964158adb55d11f21bf3ee60a
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643996"
---
# <a name="service-channel-listeners-and-channels"></a>サービス: チャネル リスナーとチャネル

チャネル オブジェクトには、チャネル、チャネル リスナー、およびチャネル ファクトリという 3 つのカテゴリがあります。 チャネルはアプリケーションおよびチャネル スタックとのインターフェイスです。 チャネル リスナーは受信 (またはリッスン) する側のチャネルを作成する役割を果たします。通常は、新しい受信メッセージまたは接続への応答を行います。 チャネル ファクトリは送信側のチャネルを作成し、エンドポイントとの通信を開始する役割を果たします。

## <a name="channel-listeners-and-channels"></a>チャネル リスナーとチャネル

チャネル リスナーには、チャネルを作成し、下の層またはネットワークからメッセージを受信する役割があります。 受信されたメッセージは、チャネル リスナーによって作成されるチャネルを使用して、上の層に配信されます。

メッセージを受信して上の層に配信するプロセスを次の図に示します。

![チャネル リスナーとチャネル](./media/wcfc-wcfchannelsigure1highlevelc.gif "wcfc_WCFChannelsigure1HighLevelc")

メッセージを受信してチャネル経由で上の層に配信するチャネル リスナー。

このプロセスは、概念上、各チャネル内のキューとしてモデル化できますが、実際には実装がキューを使用しない場合もあります。 チャネル リスナーには、下の層またはネットワークからメッセージを受信し、キューに配置する役割があります。 また、チャネルには、キューからメッセージを取得し、上の層から (たとえば、チャネル上で `Receive` を呼び出すことによって) メッセージが要求されたときにそのメッセージを渡す役割があります。

WCF には、このプロセスに対する基本クラス ヘルパーが用意されています。 この記事で説明するチャネル ヘルパー クラスの図については、「[チャネル モデルの概要](channel-model-overview.md)」を参照してください。

- <xref:System.ServiceModel.Channels.CommunicationObject> クラスには <xref:System.ServiceModel.ICommunicationObject> が実装され、「[チャネルの開発](developing-channels.md)」の手順 2. で説明されているステート マシンが強制実行されます。

- <xref:System.ServiceModel.Channels.ChannelManagerBase> クラスには <xref:System.ServiceModel.Channels.CommunicationObject> が実装され、<xref:System.ServiceModel.Channels.ChannelFactoryBase> と <xref:System.ServiceModel.Channels.ChannelListenerBase> の統合基本クラスが提供されます。 <xref:System.ServiceModel.Channels.ChannelManagerBase> クラスは、<xref:System.ServiceModel.Channels.ChannelBase> を実装する基本クラスである <xref:System.ServiceModel.Channels.IChannel> との組み合わせによって動作します。

- <xref:System.ServiceModel.Channels.ChannelFactoryBase> クラスでは、<xref:System.ServiceModel.Channels.ChannelManagerBase> および <xref:System.ServiceModel.Channels.IChannelFactory> が実装され、`CreateChannel` オーバーロードが 1 つの `OnCreateChannel` 抽象メソッドに統合されます。

- <xref:System.ServiceModel.Channels.ChannelListenerBase> クラスは、<xref:System.ServiceModel.Channels.IChannelListener> を実装しています。 基本状態管理を行います。

次の説明は、「[トランスポート: UDP](../samples/transport-udp.md)」のサンプルに基づいています。

## <a name="creating-a-channel-listener"></a>チャネル リスナーの作成

サンプルで実装される `UdpChannelListener` は、<xref:System.ServiceModel.Channels.ChannelListenerBase> クラスから派生したものです。 単一の UDP ソケットを使用して、データグラムを受信します。 `OnOpen` メソッドは、非同期ループ内で UDP ソケットを使用してデータを受信します。 その後、メッセージ エンコーディング システムを使用して、データを次のようにメッセージに変換します。

```csharp
message = UdpConstants.MessageEncoder.ReadMessage(
  new ArraySegment<byte>(buffer, 0, count),
  bufferManager
);
```

複数のソースから到着するメッセージが同じデータグラム チャネルで表されるので、`UdpChannelListener` はシングルトン リスナーです。 このリスナーに同時に関連付けられるアクティブな <xref:System.ServiceModel.Channels.IChannel> は、最大 1 つです。 このサンプルでは、<xref:System.ServiceModel.Channels.ChannelListenerBase%601.AcceptChannel%2A> メソッドによって返されるチャネルがその後破棄される場合のみ、もう 1 つ生成されます。 メッセージが受信されると、このシングルトン チャネルのキューに置かれます。

### <a name="udpinputchannel"></a>UdpInputChannel

`UdpInputChannel` クラスは、<xref:System.ServiceModel.Channels.IInputChannel> を実装しています。 このクラスは `UdpChannelListener` のソケットによって設定される受信メッセージのキューで構成されています。 これらのメッセージは、<xref:System.ServiceModel.Channels.IInputChannel.Receive%2A> メソッドによってキューから削除されます。
