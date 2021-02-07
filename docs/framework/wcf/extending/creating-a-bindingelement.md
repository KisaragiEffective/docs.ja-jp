---
description: '詳細情報: BindingElement の作成'
title: BindingElement の作成
ms.date: 03/30/2017
ms.assetid: 01a35307-a41f-4ef6-a3db-322af40afc99
ms.openlocfilehash: de5ef045f2e83985cabd36c53652d46536889fa2
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99735427"
---
# <a name="creating-a-bindingelement"></a>BindingElement の作成

バインドとバインド要素 (それぞれ、およびを拡張するオブジェクト <xref:System.ServiceModel.Channels.Binding?displayProperty=nameWithType> <xref:System.ServiceModel.Channels.BindingElement?displayProperty=nameWithType> ) は、WINDOWS COMMUNICATION FOUNDATION (WCF) アプリケーションモデルがチャネルファクトリとチャネルリスナーに関連付けられている場所です。 バインディングを使用しない場合、カスタムチャネルを使用するには、「 [サービス Channel-Level プログラミング](service-channel-level-programming.md) と [クライアント Channel-Level プログラミング](client-channel-level-programming.md)」で説明されているように、チャネルレベルでのプログラミングが必要です。 このトピックでは、WCF でのチャネルの使用を有効にするための最小要件と、チャネルのの開発について説明し <xref:System.ServiceModel.Channels.BindingElement> ます。また、 [「チャネルの開発](developing-channels.md)」の手順 4. で説明したように、アプリケーションからの使用を有効にします。  
  
## <a name="overview"></a>概要  

 チャネルのを作成すると、 <xref:System.ServiceModel.Channels.BindingElement> 開発者はこれを WCF アプリケーションで使用できるようになります。 <xref:System.ServiceModel.Channels.BindingElement> オブジェクトをクラスから使用して、 <xref:System.ServiceModel.ServiceHost?displayProperty=nameWithType> チャネルの正確な型情報を必要とせずに、WCF アプリケーションをチャネルに接続できます。  
  
 を作成したら <xref:System.ServiceModel.Channels.BindingElement> 、「 [チャネルの開発](developing-channels.md)」で説明されている残りのチャネルの開発手順に従って、要件に応じてより多くの機能を有効にすることができます。  
  
## <a name="adding-a-binding-element"></a>バインド要素の追加  

 カスタムの <xref:System.ServiceModel.Channels.BindingElement> を実装するには、<xref:System.ServiceModel.Channels.BindingElement> を継承するクラスを記述します。 たとえば、サイズの大きいメッセージをチャンクに分割し、接続先で元のメッセージを再構築できる `ChunkingChannel` を開発している場合、<xref:System.ServiceModel.Channels.BindingElement> を実装してこれを使用するようにバインディングを構成することによって、任意のバインディングでこのチャネルを使用できるようになります。 このトピックの残りの部分では、`ChunkingChannel` を例にして、バインド要素を実装する際の要件を示します。  
  
 `ChunkingBindingElement` は、`ChunkingChannelFactory` および `ChunkingChannelListener` を作成します。 このバインド要素は、<xref:System.ServiceModel.Channels.BindingElement.CanBuildChannelFactory%2A> 実装および <xref:System.ServiceModel.Channels.BindingElement.CanBuildChannelListener%2A> 実装をオーバーライドし、型パラメーターが <xref:System.ServiceModel.Channels.IDuplexSessionChannel> (この例では、これが `ChunkingChannel` でサポートされる唯一のチャネル形状です) であることと、バインディングの他のバインド要素がこのチャネル形状をサポートすることを確認します。  
  
 <xref:System.ServiceModel.Channels.BindingElement.BuildChannelFactory%2A> は、要求されたチャネル形状を構築できることをまず確認し、次にチャンク対象のメッセージ アクションのリストを取得します。 さらに、新しい `ChunkingChannelFactory` を作成してこれに内部チャネル ファクトリを渡します  (トランスポート バインド要素を作成する場合、これはバインディング スタック内の最後の要素となるため、チャネル リスナーとチャネル ファクトリを作成する必要があります)。  
  
 <xref:System.ServiceModel.Channels.BindingElement.BuildChannelListener%2A> には、`ChunkingChannelListener` を作成してこれに内部チャネル リスナーを渡す同様の実装があります。  
  
 トランスポートチャネルを使用したもう1つの例として、 [transport: UDP](../samples/transport-udp.md) サンプルには次のオーバーライドが用意されています。  
  
 このサンプルでは、バインド要素は `UdpTransportBindingElement` で、<xref:System.ServiceModel.Channels.TransportBindingElement> から派生しています。 このバインド要素は、チャネルに関連付けられているファクトリを作成する、次のメソッドをオーバーライドします。  
  
```csharp  
public IChannelFactory<TChannel> BuildChannelFactory<TChannel>(BindingContext context)  
{  
    return (IChannelFactory<TChannel>)(object)new UdpChannelFactory(this, context);  
}  
  
public IChannelListener<TChannel> BuildChannelListener<TChannel>(BindingContext context)  
{  
    return (IChannelListener<TChannel>)(object)new UdpChannelListener(this, context);  
}  
```  
  
 また、この要素には、`BindingElement` を複製したり、スキーム (soap.udp) を返したりするためのメンバーも含まれます。  
  
#### <a name="protocol-binding-elements"></a>プロトコル バインド要素  

 含まれているバインド要素を新しいバインド要素で置き換えたり補足したりすることにより、新しいトランスポート、エンコーディング、または高レベルのプロトコルを追加できます。 新しいプロトコル バインド要素を作成するには、まず <xref:System.ServiceModel.Channels.BindingElement> クラスを拡張します。 少なくとも、を実装し、を使用してを実装する必要があり <xref:System.ServiceModel.Channels.BindingElement.Clone%2A?displayProperty=nameWithType> `ChannelProtectionRequirements` <xref:System.ServiceModel.Channels.IChannel.GetProperty%2A?displayProperty=nameWithType> ます。 これにより、このバインド要素の <xref:System.ServiceModel.Security.ChannelProtectionRequirements> が返されます。  詳細については、「<xref:System.ServiceModel.Security.ChannelProtectionRequirements>」を参照してください。  
  
 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> は、このバインド要素の新しいコピーを返します。 最善の方法としては、バインド要素の作成者は基本の copy コンストラクターを呼び出す copy コンストラクターを使用して、<xref:System.ServiceModel.Channels.BindingElement.Clone%2A> を実装し、このクラスに含まれるすべての追加フィールドを複製することをお勧めします。  
  
#### <a name="transport-binding-elements"></a>トランスポート バインド要素  

 新しいトランスポート バインド要素を作成するには、<xref:System.ServiceModel.Channels.TransportBindingElement> インターフェイスを拡張します。 次に、少なくとも <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> メソッドと <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A?displayProperty=nameWithType> プロパティを実装する必要があります。  
  
 <xref:System.ServiceModel.Channels.BindingElement.Clone%2A> - このバインド要素の新しいコピーを返します。  最善の方法としては、バインド要素の作成者は基本の copy コンストラクターを呼び出す copy コンストラクターを使用して、複製を実装し、このクラスに含まれるすべての追加フィールドを複製することをお勧めします。  
  
 <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> – <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> get プロパティは、バインド要素によって表されるトランスポート プロトコルの URI スキームを返します。 たとえば、とは、 <xref:System.ServiceModel.Channels.HttpTransportBindingElement?displayProperty=nameWithType> <xref:System.ServiceModel.Channels.TcpTransportBindingElement?displayProperty=nameWithType> それぞれのプロパティから "http" と "net.tcp" を返し <xref:System.ServiceModel.Channels.TransportBindingElement.Scheme%2A> ます。  
  
#### <a name="encoding-binding-elements"></a>エンコーディング バインド要素  

 新しいエンコーディング バインド要素を作成するには、まず <xref:System.ServiceModel.Channels.BindingElement> クラスを拡張し、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement?displayProperty=nameWithType> クラスを実装します。 次に、少なくとも、<xref:System.ServiceModel.Channels.BindingElement.Clone%2A> メソッド、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A?displayProperty=nameWithType> メソッド、および <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.MessageVersion%2A?displayProperty=nameWithType> プロパティを実装する必要があります。  
  
- <xref:System.ServiceModel.Channels.BindingElement.Clone%2A>. このバインド要素の新しいコピーを返します。 最善の方法としては、バインド要素の作成者は基本の copy コンストラクターを呼び出す copy コンストラクターを使用して、<xref:System.ServiceModel.Channels.BindingElement.Clone%2A> を実装し、このクラスに含まれるすべての追加フィールドを複製することをお勧めします。  
  
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.CreateMessageEncoderFactory%2A>. <xref:System.ServiceModel.Channels.MessageEncoderFactory> を返します。これは、新しいエンコーダーを実装する実際のクラスを識別するハンドルを提供し、<xref:System.ServiceModel.Channels.MessageEncoder> を拡張します。 詳細については、次のトピックを参照してください。 <xref:System.ServiceModel.Channels.MessageEncoderFactory> および <xref:System.ServiceModel.Channels.MessageEncoder>  
  
- <xref:System.ServiceModel.Channels.MessageEncodingBindingElement.MessageVersion%2A>. このエンコーディングで使用する <xref:System.ServiceModel.Channels.MessageVersion> を返します。これは、使用する SOAP および WS-Addressing のバージョンを表します。  
  
 ユーザー定義エンコーディング バインド要素のオプション メソッドとプロパティの完全な一覧については、<xref:System.ServiceModel.Channels.MessageEncodingBindingElement> を参照してください。  
  
 新しいバインド要素の作成の詳細については、「 [User-Defined バインドの作成](creating-user-defined-bindings.md)」を参照してください。  
  
 チャネルのバインド要素を作成したら、「 [チャネルの開発](developing-channels.md) 」トピックに戻り、構成ファイルのサポートをバインド要素に追加するかどうか、メタデータの公開サポートを追加するかどうか、およびバインド要素を使用するユーザー定義のバインディングを構築するかどうか、および作成する方法を確認します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.BindingElement>
- [チャネルの開発](developing-channels.md)
- [トランスポート:UDP](../samples/transport-udp.md)
