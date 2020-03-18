---
title: WCF セキュリティのプログラミング
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- message security [WCF], programming overview
ms.assetid: 739ec222-4eda-4cc9-a470-67e64a7a3f10
ms.openlocfilehash: e19f858818866f16b8af44abe462ddb826d43b69
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741481"
---
# <a name="programming-wcf-security"></a>WCF セキュリティのプログラミング
このトピックでは、セキュリティで保護された Windows Communication Foundation (WCF) アプリケーションの作成に使用される基本的なプログラミングタスクについて説明します。 このトピックでは、*転送セキュリティ*と総称される認証、機密性、および整合性についてのみ説明します。 このトピックでは、承認 (リソースまたはサービスへのアクセスの制御) については説明しません。承認の詳細については、「[承認](../../../../docs/framework/wcf/feature-details/authorization-in-wcf.md)」を参照してください。  
  
> [!NOTE]
> 特に WCF に関するセキュリティの概念の概要については、MSDN の「 [Web サービス拡張機能 (WSE) 3.0 のシナリオ、パターン、実装ガイダンス](https://docs.microsoft.com/previous-versions/msp-n-p/ff648183(v=pandp.10))」で、一連のパターンとプラクティスに関するチュートリアルを参照してください。  
  
 WCF セキュリティのプログラミングは、セキュリティモード、クライアント資格情報の種類、および資格情報の値を設定する3つの手順に基づいています。 これらの手順は、コードまたは構成を使用して実行できます。  
  
## <a name="setting-the-security-mode"></a>セキュリティ モードの設定  
 ここでは、WCF のセキュリティモードを使用したプログラミングの一般的な手順について説明します。  
  
1. アプリケーション要件を満たす適切な定義済みバインディングを選択します。 バインディングの選択肢の一覧については、「[システム指定のバインディング](../../../../docs/framework/wcf/system-provided-bindings.md)」を参照してください。 既定では、ほとんどのバインディングでセキュリティが有効になっています。 1つの例外は <xref:System.ServiceModel.BasicHttpBinding> クラスです (構成、 [\<basicHttpBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/basichttpbinding.md)を使用)。  
  
     選択するバインディングによってトランスポートが決まります。 たとえば、<xref:System.ServiceModel.WSHttpBinding> はトランスポートとして HTTP を使用し、<xref:System.ServiceModel.NetTcpBinding> は TCP を使用します。  
  
2. バインディングのセキュリティ モードを選択します。 選択したバインディングによって、選択できるモードが決まります。 たとえば、<xref:System.ServiceModel.WSDualHttpBinding> を選択すると、トランスポート セキュリティを使用できません (このオプションはありません)。 同様に、<xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> や <xref:System.ServiceModel.NetNamedPipeBinding> を選択すると、メッセージ セキュリティを使用できません。  
  
     次の 3 つの選択肢があります。  
  
    1. `Transport`  
  
         トランスポート セキュリティは、選択したバインディングが使用する機構に依存します。 たとえば、`WSHttpBinding` を使用している場合、セキュリティ機構は SSL (Secure Sockets Layer) です (これは、HTTPS プロトコルの機構でもあります)。 一般に、トランスポート セキュリティの主な利点は、使用するトランスポートに関係なく、優れたスループットが得られることです。 ただし、制限が 2 つあります。1 つは、ユーザーの認証に使用する資格情報の種類がトランスポート機構によって決まることです。 これは、異なる種類の資格情報を要求する他のサービスと相互運用する必要がある場合には不都合です。 もう 1 つの制限は、メッセージ レベルでセキュリティが適用されないため、エンド ツー エンドではなく、ホップ単位でセキュリティが実装されることです。 この 2 つ目の制限は、クライアントとサービス間のメッセージ パスに中継局が含まれている場合にのみ問題になります。 使用するトランスポートの詳細については、「[トランスポートの選択](../../../../docs/framework/wcf/feature-details/choosing-a-transport.md)」を参照してください。 トランスポートセキュリティの使用方法の詳細については、「[トランスポートセキュリティの概要](../../../../docs/framework/wcf/feature-details/transport-security-overview.md)」を参照してください。  
  
    2. `Message`  
  
         メッセージ セキュリティでは、メッセージの安全性を維持するために必要なヘッダーとデータがすべてのメッセージに含まれます。 ヘッダーの構成は変更可能であるため、任意の数の資格情報を含めることができます。 トランスポート機構で提供できない特殊な資格情報の種類を要求する他のサービスと相互運用している場合や、メッセージを複数のサービスで使用する必要があり、各サービスが異なる資格情報の種類を要求する場合には、これは重要な要素になります。  
  
         詳細については、「[メッセージセキュリティ](../../../../docs/framework/wcf/feature-details/message-security-in-wcf.md)」を参照してください。  
  
    3. `TransportWithMessageCredential`  
  
         このモードを選択すると、トランスポート層を使用してメッセージの転送がセキュリティで保護されると同時に、他のサービスが必要とするさまざまな資格情報がすべてのメッセージに含まれます。 これにより、トランスポート セキュリティのパフォーマンス上の利点とメッセージ セキュリティの豊富な資格情報の利点の両方が提供されます。 このモードは、<xref:System.ServiceModel.BasicHttpBinding>、<xref:System.ServiceModel.WSFederationHttpBinding>、<xref:System.ServiceModel.NetPeerTcpBinding>、および <xref:System.ServiceModel.WSHttpBinding> の各バインディングで使用できます。  
  
3. HTTP でトランスポート セキュリティを使用する (つまり、HTTPS を使用する) 場合は、SSL 証明書を使用してホストを構成し、ポートで SSL を有効にします。 詳細については、「 [HTTP トランスポートセキュリティ](../../../../docs/framework/wcf/feature-details/http-transport-security.md)」を参照してください。  
  
4. <xref:System.ServiceModel.WSHttpBinding> を使用している場合、セキュリティで保護されたセッションを確立する必要がないときは、<xref:System.ServiceModel.NonDualMessageSecurityOverHttp.EstablishSecurityContext%2A> プロパティを `false` に設定します。  
  
     セキュリティで保護されたセッションは、クライアントとサーバーが対称キーを使用してチャネルを作成するときに発生します (メッセージ交換中、やりとりが終了するまで、クライアントとサーバーの両方が同じキーを使用します)。  
  
## <a name="setting-the-client-credential-type"></a>クライアント資格情報の種類の設定  
 適切なクライアント資格情報の種類を選択します。 詳細については、「[資格情報の種類の選択](../../../../docs/framework/wcf/feature-details/selecting-a-credential-type.md)」を参照してください。 使用できるクライアント資格情報の種類は、次のとおりです。  
  
- `Windows`  
  
- `Certificate`  
  
- `Digest`  
  
- `Basic`  
  
- `UserName`  
  
- `NTLM`  
  
- `IssuedToken`  
  
 モードの設定に応じて、資格情報の種類を設定する必要があります。 たとえば、次の構成例に示すように、`wsHttpBinding` を選択しており、モードを "Message" に設定した場合は、Message 要素の `clientCredentialType` 属性の値を `None`、`Windows`、`UserName`、`Certificate`、および `IssuedToken` のいずれかに設定できます。  
  
```xml  
<system.serviceModel>  
<bindings>  
  <wsHttpBinding>  
    <binding name="myBinding">  
      <security mode="Message"/>  
      <message clientCredentialType="Windows"/>  
    </binding>  
</bindings>  
</system.serviceModel>  
```  
  
 または  
  
 [!code-csharp[c_WsHttpService#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_wshttpservice/cs/source.cs#1)]
 [!code-vb[c_WsHttpService#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_wshttpservice/vb/source.vb#1)]  
  
## <a name="setting-service-credential-values"></a>サービス資格情報の値の設定  
 クライアント資格情報の種類を選択したら、サービスとクライアントが使用する実際の資格情報を設定する必要があります。 サービスでは、資格情報は <xref:System.ServiceModel.Description.ServiceCredentials> クラスを使用して設定します。また、資格情報は <xref:System.ServiceModel.ServiceHostBase.Credentials%2A> クラスの <xref:System.ServiceModel.ServiceHostBase> プロパティによって返されます。 使用しているバインディングによって、サービス資格情報の種類、選択されるセキュリティ モード、およびクライアント資格情報の種類が暗黙的に決まります。 サービス資格情報の証明書を設定するコードを次に示します。  
  
 [!code-csharp[c_tcpService#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpservice/cs/source.cs#3)]
 [!code-vb[c_tcpService#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpservice/vb/source.vb#3)]  
  
## <a name="setting-client-credential-values"></a>クライアント資格情報の値の設定  
 クライアントでは、クライアント資格情報の値は <xref:System.ServiceModel.Description.ClientCredentials> クラスを使用して設定します。また、クライアント資格情報の値は <xref:System.ServiceModel.ClientBase%601.ClientCredentials%2A> クラスの <xref:System.ServiceModel.ClientBase%601> プロパティから返されます。 TCP プロトコルを使用するクライアントで、資格情報として証明書を設定するコードを次に示します。  
  
 [!code-csharp[c_TcpClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_tcpclient/cs/source.cs#1)]
 [!code-vb[c_TcpClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_tcpclient/vb/source.vb#1)]  
  
## <a name="see-also"></a>参照

- [基本的な WCF プログラミング](../../../../docs/framework/wcf/basic-wcf-programming.md)
- [一般的なセキュリティ シナリオ](../../../../docs/framework/wcf/feature-details/common-security-scenarios.md)
