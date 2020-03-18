---
title: WS 2007 フェデレーション HTTP バインディング
ms.date: 03/30/2017
ms.assetid: 91c1b477-a96e-4bf5-9330-5e9312113371
ms.openlocfilehash: 74f21494c08f33a61eb1e1b0a102638cff59a970
ms.sourcegitcommit: 42ed59871db1f29a32b3d8e7abeb20e6eceeda7c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/10/2019
ms.locfileid: "74960195"
---
# <a name="ws-2007-federation-http-binding"></a>WS 2007 フェデレーション HTTP バインディング

このサンプルでは、<xref:System.ServiceModel.WS2007FederationHttpBinding> の使用例を示します。これは、WS-Trust 仕様のバージョン 1.3 に対応したフェデレーション シナリオを構築するための標準のバインディングです。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

このサンプルは、コンソールベースのクライアントプログラム (*client.exe*)、コンソールベースの Security Token Service プログラム (*ccmsetup.exe)、* およびコンソールベースのサービスプログラム (*services.exe*) で構成されています。 サービスは、要求/応答通信パターンを定義するコントラクトを実装します。 このコントラクトは `ICalculator` インターフェイスによって定義されており、算術演算 (`Add`、`Subtract`、`Multiply`、`Divide`) を公開しています。 クライアントは、セキュリティ トークンをセキュリティ トークン サービス (STS) から取得し、指定された算術演算を実行する同期要求をサービスに対して行います。 サービスが応答し、結果を返します。 クライアント アクティビティは、コンソール ウィンドウに表示されます。

このサンプルは、`ICalculator` 要素を使用して `ws2007FederationHttpBinding` コントラクトを利用できるようにします。 クライアントでのこのバインディングの構成を次のコードに示します。

```xml
<bindings>
  <ws2007FederationHttpBinding>
    <binding name="ServiceFed" >
      <security mode ="Message">
        <message issuedKeyType ="SymmetricKey"
                 issuedTokenType ="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" >
          <!-- Endpoint address and binding for Security Token Service -->
          <issuer address ="http://localhost:8000/sts/windows"
                  binding ="ws2007HttpBinding" />
        </message>
      </security>
    </binding>
  </ws2007FederationHttpBinding>
</bindings>
```

[\<セキュリティ >](../../configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)で、`security` の値によって、使用するセキュリティモードを指定します。 このサンプルでは、`message` セキュリティを使用します。このため、 [\<メッセージ >](../../configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)は[\<セキュリティ >](../../configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)内で指定されます。 [\<メッセージ >](../../configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)内の[\<issuer >](../../configure-apps/file-schema/wcf/issuer.md)要素は、クライアントが `ICalculator` サービスに対して認証できるように、セキュリティトークンをクライアントに発行する STS のアドレスとバインディングを指定します。
  
サービスでのこのバインディングの構成を次のコードに示します。

```xml
<bindings>
  <ws2007FederationHttpBinding>
    <binding name="ServiceFed" >
      <security mode ="Message">
        <message issuedKeyType ="SymmetricKey"
                 issuedTokenType ="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" >
          <!-- Metadata address for Security Token Service -->
          <issuerMetadata address ="http://localhost:8000/sts/mex" >
            <identity>
              <certificateReference storeLocation ="CurrentUser"
                                    storeName="TrustedPeople"
                                    x509FindType ="FindBySubjectDistinguishedName"
                                    findValue ="CN=STS" />
            </identity>
          </issuerMetadata>
        </message>
      </security>
    </binding>
  </ws2007FederationHttpBinding>
</bindings>
```

[\<セキュリティ >](../../configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)で、`security` の値によって、使用するセキュリティモードを指定します。 このサンプルでは、`message` セキュリティを使用します。このため、 [\<メッセージ >](../../configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)は[\<セキュリティ >](../../configure-apps/file-schema/wcf/security-element-of-ws2007federationhttpbinding.md)内で指定されます。 [\<> メッセージ](../../configure-apps/file-schema/wcf/message-element-of-ws2007federationhttpbinding.md)内の `ws2007FederationHttpBinding` の[\<issuermetadata >](../../configure-apps/file-schema/wcf/issuermetadata.md)要素は、STS のメタデータを取得するために使用できるエンドポイントのアドレスと id を指定します。

サービスの動作を次のコードに示します。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name ="ServiceBehaviour" >
      <serviceDebug includeExceptionDetailInFaults ="true"/>
      <serviceMetadata httpGetEnabled ="true"/>
      <serviceCredentials>
        <issuedTokenAuthentication>
          <knownCertificates>
            <add storeLocation ="LocalMachine"
                 storeName="TrustedPeople"
                 x509FindType="FindBySubjectDistinguishedName"
                 findValue="CN=STS" />
          </knownCertificates>
        </issuedTokenAuthentication>
        <serviceCertificate storeLocation ="LocalMachine"
                            storeName ="My"
                            x509FindType ="FindBySubjectDistinguishedName"
                            findValue ="CN=localhost"/>
      </serviceCredentials>
    </behavior>
  </serviceBehaviors>
</behaviors>
```
  
[\<issuedTokenAuthentication >](../../configure-apps/file-schema/wcf/issuedtokenauthentication-of-servicecredentials.md)> を使用すると、サービスは、認証時にクライアントが提示できるトークンに対して制約を指定できます。 この構成の指定では、サブジェクト名が CN=STS である証明書によって署名されたトークンがサービスによって受け入れられます。

STS は、標準の <xref:System.ServiceModel.WS2007HttpBinding> を使用して、単一のエンドポイントを利用できるようにします。 このサービスは、クライアントからのトークンの要求に応答し、 クライアントが Windows アカウントを使用して認証されている場合は、クライアントのユーザー名がクレームとして含まれているトークンを発行します。 STS は、トークン作成の一環として、CN=STS 証明書に関連付けられている秘密キーを使用して、トークンに署名します。 また、対称キーを作成し、CN=localhost 証明書に関連付けられている秘密キーを使用して暗号化します。 STS は、トークンをクライアントに返すときに、対称キーも返します。 クライアントは、発行されたトークンを `ICalculator` サービスに提示し、対称キーを使用してメッセージに署名することで対称キーを認識していることを証明します。

サンプルを実行すると、セキュリティ トークン要求が STS のコンソール ウィンドウに表示されます。 操作要求と応答は、クライアントとサービスのコンソール ウィンドウに表示されます。 いずれかのコンソール ウィンドウで Enter キーを押すと、アプリケーションがシャットダウンします。

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714
Press <ENTER> to terminate client.
```

このサンプルに含まれている*セットアップ .bat*ファイルを使用すると、適切な証明書を使用してサーバーと STS を構成し、自己ホスト型アプリケーションを実行できます。 このバッチ ファイルにより、LocalMachine/TrustedPeople 証明書ストアに 2 つの証明書が作成されます。 片方の証明書は CN=STS のサブジェクト名を持ち、クライアントに発行するセキュリティ トークンを署名するために STS が使用します。 もう片方の証明書は CN=localhost のサブジェクト名を持ち、サービスが暗号化を解除できるようにシークレットを暗号化するために STS が使用します。

## <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. 管理者特権で Visual Studio の開発者コマンドプロンプトを開き、Setup.exe ファイルを実行して必要な証明書を作成します。

 このバッチファイルは、Windows SDK と共に配布される*certmgr.exe*と Makecert を使用します。 ただし、スクリプトでこれらのツールを検索できるようにするには、Visual Studio コマンドプロンプト内から*setup.exe*を実行する必要があります。

1. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](building-the-samples.md)」の手順に従います。

2. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](running-the-samples.md)」の手順に従います。 Windows Vista を使用している場合は、*サービス* *を管理*者特権で実行する必要があります (ファイルを右クリックし、[**管理者として実行** *] をクリック*します)。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。
> 
> `<InstallDrive>:\WF_WCF_Samples`
> 
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。
> 
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\WS2007FederationHttp`
