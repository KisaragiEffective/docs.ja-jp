---
title: バインディングとセキュリティ
ms.date: 03/30/2017
helpviewer_keywords:
- bindings [WCF], security
- WCF security
- Windows Communication Foundation, security
- bindings [WCF]
ms.assetid: 4de03dd3-968a-4e65-af43-516e903d7f95
ms.openlocfilehash: 63d3888df364d033b17972a5fd3ba3b851e00c42
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964432"
---
# <a name="bindings-and-security"></a>バインディングとセキュリティ

Windows Communication Foundation (WCF) に含まれるシステム指定のバインディングを使用すると、WCF アプリケーションをすばやくプログラミングできます。 1 つの例外を除き、すべてのバインディングにはセキュリティ スキームが含まれており、既定で有効になっています。 ここでは、セキュリティ ニーズに適した正しいバインディングの選択方法について説明します。

WCF セキュリティの概要については、「[セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)」を参照してください。 バインディングを使用した WCF のプログラミングの詳細については、「 [Wcf セキュリティのプログラミング](../../../../docs/framework/wcf/feature-details/programming-wcf-security.md)」を参照してください。

バインディングを既に選択している場合は、セキュリティ[動作](../../../../docs/framework/wcf/feature-details/security-behaviors-in-wcf.md)のセキュリティに関連付けられている実行時の動作の詳細を確認できます。

セキュリティ機能のなかには、システム指定のバインディングを使用してプログラミングできないものがあります。 カスタムバインドを使用した詳細な制御については、「[カスタムバインドを使用したセキュリティ機能](../../../../docs/framework/wcf/feature-details/security-capabilities-with-custom-bindings.md)」を参照してください。

## <a name="security-functions-of-bindings"></a>バインディングのセキュリティ機能

WCF には、ほとんどのニーズを満たすシステム指定のバインディングが多数用意されています。 特定のバインディングでは不十分な場合は、カスタム バインディングを作成することもできます。 システム指定のバインディングの一覧については、「[システム指定のバインド](../../../../docs/framework/wcf/system-provided-bindings.md)」を参照してください。 カスタムバインディングの詳細については、「[カスタムバインド](../../../../docs/framework/wcf/extending/custom-bindings.md)」を参照してください。

WCF のすべてのバインドには、API として、構成ファイルで使用される XML 要素として、という2つの形式があります。 たとえば、`WSHttpBinding` (API) には、 [\<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)に対応するがあります。

以下のセクションでは、各バインディングについて両方の形式を示し、セキュリティ機能の概要を説明します。

### <a name="basichttp"></a>BasicHttp

コードでは、<xref:System.ServiceModel.BasicHttpBinding> クラスを使用します。[構成] で、 [\<basicHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)を使用します。

このバインディングは、次のような既存のさまざまなテクノロジと共に使用できるようにデザインされています。

- ASP.NET Web サービス (ASMX) Version 1

- Web サービス拡張 (WSE) アプリケーション

- Web Services 相互運用性 (WS-I) 仕様 (<https://go.microsoft.com/fwlink/?LinkId=38955>) で定義されている基本プロファイル。

- WS-I で定義されている基本セキュリティ プロファイル

既定では、このバインディングはセキュリティで保護されません。 ASMX サービスと相互運用するように設計されています。 セキュリティを有効にした場合、このバインディングは、インターネット インフォメーション サービス (IIS: Internet Information Services) のセキュリティ機構 (基本認証、ダイジェスト、Windows 統合セキュリティなど) とシームレスに相互運用できるように設計されています。 詳細については、「[トランスポートセキュリティの概要](../../../../docs/framework/wcf/feature-details/transport-security-overview.md)」を参照してください。 このバインディングでは、以下をサポートしています。

- HTTPS トランスポート セキュリティ

- HTTP 基本認証

- WS-Security。

詳細については、「<xref:System.ServiceModel.BasicHttpSecurity>」、「<xref:System.ServiceModel.BasicHttpMessageSecurity>」、「<xref:System.ServiceModel.BasicHttpMessageCredentialType>」、および「<xref:System.ServiceModel.BasicHttpSecurityMode>」を参照してください。

### <a name="wshttpbinding"></a>WSHttpBinding

コードでは、<xref:System.ServiceModel.WSHttpBinding> クラスを使用します。[構成] で、 [\<wsHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wshttpbinding.md)を使用します。

既定では、このバインディングは WS-Security 仕様を実装しており、WS-* 仕様を実装するサービスとの相互運用性があります。 次のセキュリティをサポートします。

- HTTPS トランスポート セキュリティ

- WS-Security。

- SOAP メッセージ資格情報セキュリティを使用した、HTTPS トランスポート保護による呼び出し元の認証。

詳細については、「<xref:System.ServiceModel.WSHttpSecurity>、<xref:System.ServiceModel.MessageSecurityOverHttp>、<xref:System.ServiceModel.MessageCredentialType>、<xref:System.ServiceModel.SecurityMode>、<xref:System.ServiceModel.HttpTransportSecurity>、<xref:System.ServiceModel.HttpClientCredentialType>、および <xref:System.ServiceModel.HttpProxyCredentialType>」を参照してください。

### <a name="wsdualhttpbinding"></a>WSDualHttpBinding

コードでは、<xref:System.ServiceModel.WSDualHttpBinding> クラスを使用します。[構成] で、 [\<wsDualHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wsdualhttpbinding.md)を使用します。

このバインディングは、双方向サービス アプリケーションを有効にするために設計されています。 このバインディングは、メッセージ ベースの転送セキュリティ用に WS-Security 仕様を実装しています。 トランスポート セキュリティは使用できません。 既定では、次の機能を提供します。

- WS-ReliableMessaging を実装して信頼性を確保します。

- WS-Security を実装して転送セキュリティおよび認証を実現します。

- HTTP を使用してメッセージを配信します。

- テキスト/XML メッセージ エンコーディングを使用します。

 バインディングで WS-Security (メッセージ層セキュリティ) を使用すると、次のパラメーターを構成できるようになります。

- 暗号アルゴリズムを決定するためのセキュリティ アルゴリズム スイート

- 以下を行うためのバインディング オプション

  - クライアントで帯域外で使用可能なサービス資格情報の提供

  - チャネル セットアップの一部としてサービスからネゴシエートされるサービス資格情報の提供

詳細については、「<xref:System.ServiceModel.WSDualHttpSecurity>」および「<xref:System.ServiceModel.WSDualHttpSecurityMode>」を参照してください。

### <a name="nettcpbinding"></a>NetTcpBinding

コードでは、<xref:System.ServiceModel.NetTcpBinding> クラスを使用します。[構成] で、 [\<netTcpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/nettcpbinding.md)を使用します。

このバインディングは複数のコンピューター間での通信に最適化されています。 既定では、次の特性があります。

- トランスポート層セキュリティを実装します。

- Windows セキュリティを利用して、転送セキュリティと認証を確保します。

- トランスポートに TCP を使用します。

- バイナリ メッセージのエンコードを実装します。

- WS-ReliableMessaging を実装します。

次のようなオプションがあります。

- メッセージ層セキュリティ (WS-Security を使用)

- メッセージ資格情報を使用するトランスポート セキュリティ (TLS (Transport Layer Security) over TCP によって実現される機密性と整合性、および WS-Security によって提供される承認に使用する資格情報)

詳細については、「<xref:System.ServiceModel.NetTcpSecurity>、<xref:System.ServiceModel.TcpTransportSecurity>、<xref:System.ServiceModel.TcpClientCredentialType>、<xref:System.ServiceModel.MessageSecurityOverTcp>、および <xref:System.ServiceModel.MessageCredentialType>」を参照してください。

### <a name="netnamedpipebinding"></a>NetNamedPipeBinding

コードでは、<xref:System.ServiceModel.NetNamedPipeBinding> クラスを使用します。[構成] で、 [\<netNamedPipeBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/netnamedpipebinding.md)を使用します。

このバインディングは、(通常では同じコンピューター上の) 複数プロセス間の通信に最適化されています。 既定では、このバインディングには次の特性があります。

- トランスポート セキュリティを使用して、メッセージ転送と認証を実現します。

- 名前付きパイプを使用してメッセージを配信します。

- バイナリ メッセージのエンコードを実装します。

- 暗号化とメッセージの署名を使用します。

次のようなオプションがあります。

- Windows セキュリティを使用した認証

詳細については、「<xref:System.ServiceModel.NetNamedPipeSecurity>」、「<xref:System.ServiceModel.NetNamedPipeSecurityMode>」、および「<xref:System.ServiceModel.NamedPipeTransportSecurity>」を参照してください。

### <a name="msmqintegrationbinding"></a>MsmqIntegrationBinding

コードでは、<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> クラスを使用します。[構成] で、 [\<msmqIntegrationBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/msmqintegrationbinding.md)を使用します。

このバインディングは、wcf 以外の Microsoft Message Queuing (MSMQ) エンドポイントと相互運用する WCF クライアントおよびサービスを作成するために最適化されています。

既定では、このバインディングはトランスポート セキュリティを使用し、次のセキュリティ特性があります。

- セキュリティは無効 (なし) にできます。

- MSMQ トランスポート セキュリティ (トランスポート)。

詳細については、「<xref:System.ServiceModel.NetMsmqSecurity>」および「<xref:System.ServiceModel.NetMsmqSecurityMode>」を参照してください。

### <a name="netmsmqbinding"></a>NetMsmqBinding

コードでは、<xref:System.ServiceModel.NetMsmqBinding> クラスを使用します。[構成] で、 [\<netMsmqBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/netmsmqbinding.md)を使用します。

このバインディングは、MSMQ のキューに置かれたメッセージのサポートを必要とする WCF サービスを作成するときに使用することを意図しています。

既定では、このバインディングはトランスポート セキュリティを使用し、次のセキュリティ特性があります。

- セキュリティは無効 (なし) にできます。

- MSMQ トランスポート セキュリティ (トランスポート)。

- SOAP に基づくメッセージ セキュリティ (メッセージ)。

- トランスポート セキュリティとメッセージ セキュリティ (両方)。

- サポートされるクライアント資格情報の種類 : なし、Windows、UserName、証明書、IssuedToken。

<xref:System.ServiceModel.MessageCredentialType.Certificate> 資格情報は、セキュリティ モードが <xref:System.ServiceModel.NetMsmqSecurityMode.Both> または <xref:System.ServiceModel.NetMsmqSecurityMode.Message> に設定されている場合にのみサポートされます。

詳細については、「<xref:System.ServiceModel.MessageSecurityOverMsmq>」および「<xref:System.ServiceModel.MsmqTransportSecurity>」を参照してください。

### <a name="wsfederationhttpbinding"></a>WSFederationHttpBinding

コードでは、<xref:System.ServiceModel.WSFederationHttpBinding> クラスを使用します。[構成] で、 [\<wsFederationHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/wsfederationhttpbinding.md)を使用します。

既定では、このバインディングは WS-Security (メッセージ層セキュリティ) を使用します。

詳細については、「 [Federation](../../../../docs/framework/wcf/feature-details/federation.md)」、「<xref:System.ServiceModel.WSFederationHttpSecurity>」、および「<xref:System.ServiceModel.WSFederationHttpSecurityMode>」を参照してください。

## <a name="custom-bindings"></a>カスタム バインディング

システム指定のバインディングがいずれも要件を満たさない場合は、カスタム セキュリティ バインド要素を使用してカスタム バインディングを作成できます。 詳細については、「[カスタムバインドを使用したセキュリティ機能](../../../../docs/framework/wcf/feature-details/security-capabilities-with-custom-bindings.md)」を参照してください。

## <a name="binding-choices"></a>バインディングの選択肢

次の表は、セキュリティ モード設定で提供される機能をまとめたものです。つまり、セキュリティ モードを `Transport`、`Message`、または `TransportWithMessageCredential` に設定したときに使用できる機能の一覧です。 アプリケーションで必要なセキュリティ機能を決定するときに、この表を参考にしてください。

|設定|フィーチャー|
|-------------|--------------|
|Transport|サーバーの認証<br /><br /> クライアント認証<br /><br /> Point-to-Point のセキュリティ<br /><br /> 相互運用性<br /><br /> ハードウェアの高速化<br /><br /> 高いスループット<br /><br /> セキュリティで保護されたファイアウォール<br /><br /> 待ち時間の長いアプリケーション<br /><br /> 複数のホップでの再暗号化|
|[メッセージ]|サーバーの認証<br /><br /> クライアント認証<br /><br /> エンド ツー エンドのセキュリティ<br /><br /> 相互運用性<br /><br /> 多様なクレーム<br /><br /> フェデレーション<br /><br /> 複数要因の認証<br /><br /> カスタム トークン<br /><br /> Notary/Timestamp サービス<br /><br /> 待ち時間の長いアプリケーション<br /><br /> メッセージ署名の永続化|
|[TransportWithMessageCredential]|サーバーの認証<br /><br /> クライアント認証<br /><br /> Point-to-Point のセキュリティ<br /><br /> 相互運用性<br /><br /> ハードウェアの高速化<br /><br /> 高いスループット<br /><br /> 多様なクライアント クレーム<br /><br /> フェデレーション<br /><br /> 複数要因の認証<br /><br /> カスタム トークン<br /><br /> セキュリティで保護されたファイアウォール<br /><br /> 待ち時間の長いアプリケーション<br /><br /> 複数のホップでの再暗号化|

さまざまなモード設定をサポートするバインディングを次の表に示します。 サービス エンドポイントを作成するときは、使用するバインディングをこの表から選択してください。

|バインディング|トランスポート モードのサポート|メッセージ モードのサポート|TransportWithMessageCredential のサポート|
|-------------|----------------------------|--------------------------|--------------------------------------------|
|`BasicHttpBinding`|○|○|○|
|`WSHttpBinding`|○|○|○|
|`WSDualHttpBinding`|いいえ|○|いいえ|
|`NetTcpBinding`|○|○|○|
|`NetNamedPipeBinding`|○|いいえ|いいえ|
|`NetMsmqBinding`|○|○|いいえ|
|`MsmqIntegrationBinding`|○|いいえ|いいえ|
|`wsFederationHttpBinding`|いいえ|○|○|

## <a name="transport-credentials-in-bindings"></a>バインディングにおけるトランスポート資格情報

トランスポート セキュリティ モードで `BasicHttpBinding` または `WSHttpBinding` を使用するときに使用できるクライアント資格情報の種類を、次の表に示します。

|の型|説明|
|----------|-----------------|
|[なし]|クライアントが資格情報を提示する必要がないことを指定します。 匿名クライアントであると解釈されます。|
|Basic|基本認証。 詳細については、「RFC 2617 – HTTP Authentication: <https://go.microsoft.com/fwlink/?LinkId=84023>で利用可能な基本認証とダイジェスト認証」を参照してください。|
|Digest|ダイジェスト認証です。 詳細については、「RFC 2617 – HTTP Authentication: <https://go.microsoft.com/fwlink/?LinkId=84023>で利用可能な基本認証とダイジェスト認証」を参照してください。|
|NTLM|NTLM (NT LAN Manager) 認証です。|
|Windows|Windows 認証です。|
|Certificate|証明書を使用して実行される認証です。|
|IssuedToken|サービスが、Security Token Service または CardSpace によって発行されたトークンを使用して、クライアントの認証を要求できるようにします。 詳細については、「[フェデレーションと発行済みトークン](../../../../docs/framework/wcf/feature-details/federation-and-issued-tokens.md)」を参照してください。|

### <a name="message-client-credentials-in-bindings"></a>バインディングにおけるメッセージ クライアント資格情報

メッセージ セキュリティ モードでバインディングを使用するときに使用できるクライアント資格情報の種類を次の表に示します。

|の型|説明|
|----------|-----------------|
|[なし]|サービスが匿名クライアントとやり取りを行うことが可能になります。|
|Windows|Windows 資格情報の認証済みコンテキストの制御下で SOAP メッセージ交換を行うことができます。|
|UserName|サービスが、ユーザー名資格情報を使用したクライアントの認証を要求できるようにします。 セキュリティモードが `TransportWithMessageCredential`に設定されている場合、WCF では、パスワードダイジェストの送信や、パスワードを使用したキーの派生、およびメッセージモードセキュリティのためのこのようなキーの使用はサポートされていません。 そのため、ユーザー名の資格情報を使用する場合、WCF はトランスポートがセキュリティで保護されることを強制します。|
|Certificate|証明書を使用したクライアントの認証を、サービスで要求することが可能になります。|
|IssuedToken|サービスは、セキュリティ トークン サービスを使用してカスタム トークンを提供できます。|

## <a name="see-also"></a>関連項目

- [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [Securing Services and Clients](../../../../docs/framework/wcf/feature-details/securing-services-and-clients.md)
- [資格情報の種類の選択](../../../../docs/framework/wcf/feature-details/selecting-a-credential-type.md)
- [カスタム バインドを使用したセキュリティ機能](../../../../docs/framework/wcf/feature-details/security-capabilities-with-custom-bindings.md)
- [セキュリティ動作](../../../../docs/framework/wcf/feature-details/security-behaviors-in-wcf.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
