---
description: '詳細情報: 信頼できるサブシステム'
title: 信頼できるサブシステム
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1f5ce46b-e259-4bc9-a0b9-89d06fc9341c
ms.openlocfilehash: 41c2943c7794206dba06ef8b5bbee762931ce0c0
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99733048"
---
# <a name="trusted-subsystem"></a>信頼できるサブシステム

クライアントは、ネットワーク全体に分散している 1 つ以上の Web サービスにアクセスします。 Web サービスは、追加のリソース (データベースや他の Web サービスなど) に対するアクセスが、Web サービスのビジネス ロジック内にカプセル化されるように設計されています。 これらのリソースは、非承認のアクセスに対して保護する必要があります。 信頼できるサブシステムの処理を次の図に示します。  
  
 ![信頼できるサブシステム](media/wcfc-trustedsubsystemc.gif "wcfc_TrustedSubsystemc")  
  
 上図に示した信頼できるサブシステムの処理について、以下の手順で説明します。  
  
1. クライアントが、信頼できるサブシステムに、資格情報と共に要求を送信します。  
  
2. 信頼できるサブシステムが、ユーザーの認証と承認を行います。  
  
3. 信頼できるサブシステムは、リモート リソースに要求メッセージを送信します。 この要求には、信頼できるサブシステムの資格情報 (または信頼できるサブシステムの処理を実行しているサービス アカウント) が付属しています。  
  
4. バックエンド リソースが、信頼できるサブシステムの認証と承認を行います。 次にバックエンド リソースは要求を処理し、信頼できるサブシステムへの応答を発行します。  
  
5. 信頼できるサブシステムはこの応答を処理し、自身の応答をクライアントに発行します。  
  
|特徴|説明|  
|--------------------|-----------------|  
|セキュリティ モード|メッセージ|  
|相互運用性|Windows Communication Foundation (WCF) のみ。|  
|認証 (サービス)|セキュリティ トークン サービスはクライアントの認証と承認を行います。|  
|認証 (クライアント)|信頼できるサブシステムがクライアントを認証し、リソースが信頼できるサブシステム サービスを認証します。|  
|整合性|はい|  
|機密性|はい|  
|トランスポート|クライアントと信頼できるサブシステム サービス間にある HTTP<br /><br /> 信頼できるサブシステム サービスとリソース (バックエンド サービス) の間にある NET.TCP|  
|バインド|<xref:System.ServiceModel.WSHttpBinding> と <xref:System.ServiceModel.NetTcpBinding>[\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md)|  
  
## <a name="resource-back-end-service"></a>リソース (バックエンド サービス)  
  
### <a name="code"></a>コード  

 次のコードでは、TCP トランスポート プロトコル上でトランスポート セキュリティを使用するリソースのサービス エンドポイントを作成する方法を示します。  
  
 [!code-csharp[TrustedSubSystemsResource#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/trustedsubsystemsresource/cs/source.cs#1)]
 [!code-vb[TrustedSubSystemsResource#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/trustedsubsystemsresource/vb/source.vb#1)]  
  
### <a name="configuration"></a>構成  

 次の構成では、構成を使用して同一のエンドポイントをセットアップします。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.BackendService"  
               behaviorConfiguration="BackendServiceBehavior">  
        <endpoint address="net.tcp://localhost.com:8001/BackendService"  
                  binding="customBinding"  
                  bindingConfiguration="Binding1"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator"/>  
      </service>  
    </services>  
    <bindings>  
      <customBinding>  
        <binding name="Binding1">  
          <security authenticationMode="UserNameOverTransport"/>  
          <windowsStreamSecurity/>  
          <tcpTransport/>  
        </binding>  
      </customBinding>  
    </bindings>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="BackendServiceBehavior">  
          <serviceCredentials>  
            <userNameAuthentication userNamePasswordValidationMode="Custom"  
                                    customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator, BackendService"/>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="trusted-subsystem"></a>信頼できるサブシステム  
  
### <a name="code"></a>コード  

 次のコードでは、HTTP プロトコル上のメッセージ セキュリティで認証にユーザー名とパスワードを使用する、信頼できるサブシステムのサービス エンドポイントを作成する方法を示します。  
  
 [!code-csharp[TrustedSubSystems#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/trustedsubsystems/cs/source.cs#1)]
 [!code-vb[TrustedSubSystems#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/trustedsubsystems/vb/source.vb#1)]  
  
 次のコードでは、TCP トランスポート プロトコル上でトランスポート セキュリティを使用してバックエンド サービスと通信を行う、信頼できるサブシステムのサービスを示します。  
  
 [!code-csharp[TrustedSubSystems#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/trustedsubsystems/cs/source.cs#2)]
 [!code-vb[TrustedSubSystems#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/trustedsubsystems/vb/source.vb#2)]  
  
### <a name="configuration"></a>構成  

 次の構成では、構成を使用して同一のエンドポイントをセットアップします。 2 つのバインディングがあることに注意してください。1 つは、信頼できるサブシステムでホストされるサービスをセキュリティで保護するバインディングで、もう 1 つは、信頼できるサブシステムとバックエンド サービスの間の通信のためのバインディングです。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Microsoft.ServiceModel.Samples.FacadeService"  
               behaviorConfiguration="FacadeServiceBehavior">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost:8000/FacadeService"/>  
          </baseAddresses>  
        </host>  
        <endpoint address="http://localhost:8000/FacadeService"  
                  binding="wsHttpBinding"  
                  bindingConfiguration="Binding1"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator"/>  
      </service>  
    </services>  
    <client>  
      <endpoint name=""
                address="net.tcp://contoso.com:8001/BackendService"  
                binding="customBinding"  
                bindingConfiguration="ClientBinding"  
                contract="Microsoft.ServiceModel.Samples.ICalculator"/>  
    </client>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="UserName"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
      <customBinding>  
        <binding name="ClientBinding">  
          <security authenticationMode="UserNameOverTransport"/>  
          <windowsStreamSecurity/>  
          <tcpTransport/>  
        </binding>  
      </customBinding>  
    </bindings>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="FacadeServiceBehavior">  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceCredentials>  
            <serviceCertificate findValue="Contoso.com"  
                                storeLocation="LocalMachine"  
                                storeName="My"  
                                x509FindType="FindBySubjectName" />  
            <userNameAuthentication userNamePasswordValidationMode="Custom"  
                                    customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator, FacadeService"/>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="client"></a>Client  
  
### <a name="code"></a>コード  

 次のコードでは、HTTP プロトコル上のメッセージ セキュリティで認証にユーザー名とパスワードを使用することで、信頼できるサブシステムと通信を行うクライアントを作成する方法を示します。  
  
 [!code-csharp[TrustedSubSystemsClient#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/trustedsubsystemsclient/cs/source.cs#1)]
 [!code-vb[TrustedSubSystemsClient#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/trustedsubsystemsclient/vb/source.vb#1)]  
  
### <a name="configuration"></a>構成  

 次のコードでは、HTTP プロトコル上のメッセージ セキュリティ、および認証用のユーザー名とパスワードを使用するようにクライアントを構成します。 ユーザー名とパスワードの指定はコードを使用する場合に限られます (構成可能ではありません)。  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<configuration>  
  <system.serviceModel>  
    <client>  
        <endpoint name=""
                  address="http://www.cohowinery.com:8000/FacadeService"  
                  binding="wsHttpBinding"  
                  bindingConfiguration="Binding1"  
                  behaviorConfiguration="ClientUserNameBehavior"  
                  contract="Microsoft.ServiceModel.Samples.ICalculator"/>  
    </client>  
    <bindings>  
      <wsHttpBinding>  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="UserName"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <behaviors>  
      <endpointBehaviors>  
        <behavior name="ClientUserNameBehavior">  
          <clientCredentials>  
            <serviceCertificate>  
              <authentication certificateValidationMode="PeerOrChainTrust"/>  
            </serviceCertificate>  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [セキュリティの概要](security-overview.md)
- [Windows Server AppFabric のセキュリティ モデル](/previous-versions/appfabric/ee677202(v=azure.10))
