---
title: WCF の単純化機能
ms.date: 03/30/2017
ms.assetid: 4535a511-6064-4da0-b361-80262a891663
ms.openlocfilehash: 28a05053fda8380b55a1a9eee20119b8c4cfccfe
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452657"
---
# <a name="wcf-simplification-features"></a>WCF の単純化機能

ここでは、WCF アプリケーションの作成を容易にする新機能について説明します。

## <a name="simplified-generated-configuration-files"></a>生成された構成ファイルの簡略化

Visual Studio でサービス参照を追加したり、SvcUtil.exe ツールを使用したりすると、クライアント構成ファイルが生成されます。 以前のバージョンの WCF では、このような構成ファイルには、各バインド プロパティの値が既定値であっても、その値が含まれていました。 WCF 4.5 では、生成された構成ファイルには、既定値以外の値に設定されているバインド プロパティのみが含まれます。

WCF 3.0 で生成された構成ファイルの例を次に示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.serviceModel>
        <bindings>
            <basicHttpBinding>
                <binding name="BasicHttpBinding_IService1" closeTimeout="00:01:00"
                    openTimeout="00:01:00" receiveTimeout="00:10:00" sendTimeout="00:01:00"
                    allowCookies="false" bypassProxyOnLocal="false"
                    hostNameComparisonMode="StrongWildcard" maxBufferSize="65536"
                    maxBufferPoolSize="524288" maxReceivedMessageSize="65536"
                    messageEncoding="Text" textEncoding="utf-8" transferMode="Buffered"
                    useDefaultWebProxy="true">
                    <readerQuotas maxDepth="32" maxStringContentLength="8192"
                        maxArrayLength="16384" maxBytesPerRead="4096"
                        maxNameTableCharCount="16384" />
                    <security mode="None">
                        <transport clientCredentialType="None" proxyCredentialType="None"
                            realm="" />
                        <message clientCredentialType="UserName" algorithmSuite="Default" />
                    </security>
                </binding>
            </basicHttpBinding>
        </bindings>
        <client>
            <endpoint address="http://localhost:36906/Service1.svc" binding="basicHttpBinding"
                bindingConfiguration="BasicHttpBinding_IService1" contract="IService1"
                name="BasicHttpBinding_IService1" />
        </client>
    </system.serviceModel>
</configuration>
```

WCF 4.5 で生成された同じ構成ファイルの例を次に示します。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
    <system.serviceModel>
        <bindings>
            <basicHttpBinding>
                <binding name="BasicHttpBinding_IService1" />
            </basicHttpBinding>
        </bindings>
        <client>
            <endpoint address="http://localhost:36906/Service1.svc" binding="basicHttpBinding"
                bindingConfiguration="BasicHttpBinding_IService1" contract="IService1"
                name="BasicHttpBinding_IService1" />
        </client>
    </system.serviceModel>
</configuration>
```

## <a name="contract-first-development"></a>コントラクト優先の開発

WCF では、コントラクト優先の開発がサポートされるようになりました。 Svcutil.exe ツールには、WSDL ドキュメントからサービスコントラクトとデータコントラクトを生成するための/serviceContract スイッチがあります。

## <a name="add-service-reference-from-a-portable-subset-project"></a>ポータブル サブセット プロジェクトからのサービス参照の追加

ポータブルサブセットプロジェクトを使用すると、.NET アセンブリプログラマは、1つのソースツリーとビルドシステムを維持しながら、複数の .NET 実装 (デスクトップ、Silverlight、Windows Phone、Xbox) を引き続きサポートできます。 ポータブルサブセットプロジェクトは、任意の .NET 実装で使用できるアセンブリである .NET ポータブルライブラリのみを参照します。 開発者から見れば、他の WCF クライアント アプリケーション内でサービス参照を追加するのと同じです。 詳細については、「[ポータブルサブセットプロジェクトでのサービス参照の追加](add-service-reference-in-a-portable-subset-project.md)」を参照してください。

## <a name="aspnet-compatibility-mode-default-changed"></a>ASP.NET 互換性モードの既定値の変更

WCF には ASP.NET 互換性モードが用意されています。これにより、開発者は WCF サービスを作成する際に ASP.NET HTTP パイプラインの機能へのフル アクセスが付与されます。 このモードを使用するには、web.config の[\<serviceHostingEnvironment >](../configure-apps/file-schema/wcf/servicehostingenvironment.md)セクションで `aspNetCompatibilityEnabled` 属性を true に設定する必要があります。また、この appDomain のすべてのサービスは、その <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> の `RequirementsMode` プロパティを <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> または <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Required>に設定する必要があります。 既定では <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> が <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsMode.Allowed> に設定され、既定の WCF サービスアプリケーションテンプレートによって `aspNetCompatibilityEnabled` 属性が `true`に設定されます。 詳細については、「Windows Communication Foundation 4.5 および[WCF サービスと ASP.NET](./feature-details/wcf-services-and-aspnet.md)[の新機能](whats-new.md)」を参照してください。

## <a name="streaming-improvements"></a>ストリーミングの強化

- 非同期ストリーミングに対する新しいサポートが WCF に追加されました。 非同期ストリーミングを有効にするには、エンドポイントの <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior> 動作をサービス ホストに追加し、その <xref:System.ServiceModel.Description.DispatcherSynchronizationBehavior.AsynchronousSendEnabled%2A> プロパティを `true` に設定します。 これにより、読み取りに時間のかかる複数のクライアントに対してサービスがストリーム メッセージを送信するときにスケーラビリティが得られます。 WCF では、クライアントごとに 1 つのスレッドがブロックされなくなり、別のクライアントにサービスを提供するためにスレッドを解放します。

- サービスが IIS でホストされるときのメッセージのバッファー処理に関する制限がなくなりました。 以前のバージョンの WCF では、ストリーム メッセージ転送を使用する IIS でホストされるサービスに対するメッセージを受信すると、ASP.NET はメッセージ全体を WCF に送信する前にバッファー処理しました。 これにより、メモリの大量消費が発生しました。 このバッファー処理は .NET 4.5 で削除されました。現在は、IIS でホストされる WCF サービスが、メッセージ全体が受信される前に着信ストリームの処理を開始できるため、実際のストリーミングが可能になります。 これにより、WCF は、メッセージにすぐに応答できるようになり、パフォーマンスは向上します。 さらに、着信要求に対する ASP.NET のサイズ制限である `maxRequestLength` の値を指定する必要がなくなりました。 このプロパティを設定した場合、このプロパティは無視されます。 `maxRequestLength` の詳細については、「 [\<httpRuntime > configuration 要素](https://docs.microsoft.com/previous-versions/dotnet/netframework-1.1/e1f13641(v=vs.71))」を参照してください。 MaxAllowedContentLength を構成する必要があります。詳細については、「 [IIS 要求の制限](https://docs.microsoft.com/previous-versions/iis/settings-schema/ms689462(v=vs.90))」を参照してください。

## <a name="new-transport-default-values"></a>トランスポートの新しい既定値

次の表は、変更された設定と追加情報の場所を示しています。

|プロパティ|On|新しい既定値|詳細|
|--------------|--------|-----------------|----------------------|
|channelInitializationTimeout|<xref:System.ServiceModel.NetTcpBinding>|30 秒|このプロパティは、TCP 接続が .NET フレーミングプロトコルを使用して自身を認証するために実行できる時間を決定します。 クライアントは、サーバーが認証を実行するための十分な情報を得る前に初期データを送信する必要があります。 このタイムアウトは意図的に ReceiveTimeout (10 分) よりも小さい値に設定されます。これにより、悪意のある認証されていないクライアントは、長時間にわたってサーバーへの接続を保持できません。 既定値は 30 秒です。 詳細については <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.ChannelInitializationTimeout%2A> を参照してください。|
|listenBacklog|<xref:System.ServiceModel.NetTcpBinding>|16 * プロセッサの数|このソケット レベルのプロパティは、キューに入れられる "受入保留中の" 要求の数を示します。 リッスン バックログ キューがいっぱいになると、新しいソケット要求は拒否されます。 詳細については <xref:System.ServiceModel.NetTcpBinding.ListenBacklog%2A> を参照してください。|
|maxPendingAccepts|ConnectionOrientedTransportBindingElement<br /><br /> SMSvcHost.exe|2 * トランスポート用のプロセッサの数<br /><br /> 4 \* SMSvcHost のプロセッサ数|このプロパティは、サーバーがリスナーで待機できるチャネルの数を制限します。 MaxPendingAccepts が小さすぎると、待機しているすべてのチャネルが接続のサービスを開始する間隔が小さくなりますが、新しいチャネルがリッスンを開始できなくなります。 接続がこの間に到着した場合、サーバー上でこの接続を待機しているものがないため、接続は失敗します。 このプロパティは、<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A> プロパティを大きな値に設定することで構成できます。 詳細については、「 [Net.tcp ポート共有サービスの](./feature-details/configuring-the-net-tcp-port-sharing-service.md)<xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingAccepts%2A> と構成」を参照してください。|
|maxPendingConnections|ConnectionOrientedTransportBindingElement|12 * プロセッサの数|このプロパティは、トランスポートが受け入れたにもかかわらず ServiceModel ディスパッチャーによって取得されていない接続の数を制御します。 この値を設定するには、バインドの `MaxConnections` を使用するか、またはバインド要素の `maxOutboundConnectionsPerEndpoint` を使用してください。 詳細については <xref:System.ServiceModel.Channels.ConnectionOrientedTransportBindingElement.MaxPendingConnections%2A> を参照してください。|
|receiveTimeout|SMSvcHost.exe|30 秒|このプロパティは、TCP フレーム データを読み取り、基になる接続からディスパッチする接続を実行するためのタイムアウトを指定します。 これは、SMSvcHost.exe サービスで受信接続からの前文データの読み取り操作を行う時間に制限を設定するために使用されます。 詳細については、「 [Net.tcp ポート共有サービスの構成](./feature-details/configuring-the-net-tcp-port-sharing-service.md)」を参照してください。|

> [!NOTE]
> これらの新しい既定値は、.NET Framework 4.5 がインストールされているコンピューターに WCF サービスを配置する場合のみ使用されます。 .NET Framework 4.0 がインストールされているコンピューターに同じサービスを配置すると、.NET Framework 4.0 の既定値が使用されます。 このような場合は、これらの設定を明示的に構成することをお勧めします。

## <a name="xmldictionaryreaderquotas"></a>XmlDictionaryReaderQuotas

<xref:System.Xml.XmlDictionaryReaderQuotas> には、メッセージの作成中にエンコーダーで使用されるメモリの量を制限する XML ディクショナリ リーダーの構成可能なクォータ値が格納されます。 これらのクォータは構成可能ですが、開発者がこのクォータを明示的に設定する必要性を低くするために既定値が変更されました。 `MaxReceivedMessageSize` クォータは変更されていないため、メモリ使用量を引き続き制限して、<xref:System.Xml.XmlDictionaryReaderQuotas> の複雑さを処理する必要性を回避できます。 次の表に、クォータ、クォータの新しい既定値、および各クォータの用途の簡単な説明を示します。

|クォータ名|Default value|説明|
|----------------|-------------------|-----------------|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>|Int32.MaxValue|許される最大配列長を取得または設定します。 このクォータは、XML リーダーが返すプリミティブ配列 (バイト配列など) の最大サイズを制限します。 このクォータは、XML リーダー自体のメモリ消費は制限しませんが、このリーダーを使用するコンポーネントのメモリ消費を制限します。 たとえば、 <xref:System.Runtime.Serialization.DataContractSerializer> が <xref:System.Xml.XmlDictionaryReaderQuotas.MaxArrayLength%2A>でセキュリティ保護されたリーダーを使用するときは、このクォータを超えるバイト配列を逆シリアル化することはありません。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A>|Int32.MaxValue|1 回の読み取りで返すことができる最大バイト数を取得または設定します。 このクォータは、要素の開始タグとその属性を読み取るときに、1 回の読み取り操作で読み取るバイト数を制限します (ストリーミングを使用しない場合、要素名自体がクォータに照らし合わせてカウントされることはありません)。 XML 属性が多すぎると、属性名は一意かどうかを確認する必要があるため、処理時間が大幅に増加する可能性があります。 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A> によってこの脅威を軽減できます。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A>|128 ノードの深さ|このクォータは、XML 要素の入れ子の深さの最大値を制限します。 <xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> は <xref:System.Xml.XmlDictionaryReaderQuotas.MaxBytesPerRead%2A>と相互に関係しています。リーダーは常に、現在の要素とそのすべての先祖に関するデータをメモリ内に維持します。このため、リーダーのメモリ消費の最大値は、この 2 つの設定値の積に比例します。 深く入れ子になったオブジェクト グラフを逆シリアル化する場合、デシリアライザーはスタック全体にアクセスし、回復不可能な <xref:System.StackOverflowException>をスローするしかありません。 <xref:System.Runtime.Serialization.DataContractSerializer> の場合も <xref:System.Xml.Serialization.XmlSerializer>の場合も、XML の入れ子構造とオブジェクトの入れ子構造との間に直接的な相関関係が存在します。 この脅威を軽減するには、<xref:System.Xml.XmlDictionaryReaderQuotas.MaxDepth%2A> を使用します。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxNameTableCharCount%2A>|Int32.MaxValue|このクォータは、nametable で使用できる最大文字数を制限します。 nametable には、XML ドキュメントを処理するときに出現する特定の文字列 (名前空間やプレフィックスなど) が入っています。 これらの文字列はメモリ内でバッファー化されるので、ストリーミングが予想されるときは、このクォータを使用して過度のバッファーを防止します。|
|<xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>|Int32.MaxValue|このクォータは、XML リーダーが返す文字列の最大サイズを制限します。 このクォータは、XML リーダー自体のメモリ消費は制限しませんが、このリーダーを使用するコンポーネントのメモリ消費を制限します。 たとえば、 <xref:System.Runtime.Serialization.DataContractSerializer> が <xref:System.Xml.XmlDictionaryReaderQuotas.MaxStringContentLength%2A>でセキュリティ保護されたリーダーを使用するときは、このクォータを超える文字列を逆シリアル化することはありません。|

> [!IMPORTANT]
> データのセキュリティ保護の詳細については、「[データのセキュリティに関する考慮事項](./feature-details/security-considerations-for-data.md)」の「XML の安全な使用」を参照してください。

> [!NOTE]
> これらの新しい既定値は、.NET Framework 4.5 がインストールされているコンピューターに WCF サービスを配置する場合のみ使用されます。 .NET Framework 4.0 がインストールされているコンピューターに同じサービスを配置すると、.NET Framework 4.0 の既定値が使用されます。 このような場合は、これらの設定を明示的に構成することをお勧めします。

## <a name="wcf-configuration-validation"></a>WCF 構成検証

Visual Studio 内でのビルド プロセスの一環として、WCF 構成ファイルが検証されるようになりました。 検証に失敗すると、Visual Studio では検証エラーと警告の一覧が表示されます。

## <a name="xml-editor-tooltips"></a>XML エディターのツールヒント

新規および既存の WCF サービスの開発者がサービスを構成するうえで役立つよう、Visual Studio の XML エディターでは、サービス構成ファイルに含まれる各構成要素とそのプロパティにツールヒントが表示されるようになりました。

## <a name="basichttpbinding-improvements"></a>BasicHttpBinding の機能強化

1. 単一の WCF エンドポイントでさまざまな認証モードに応答できます。

2. WCF サービスのセキュリティ設定を IIS で制御できます。
