---
title: メッセージ セキュリティ匿名
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c321cbf9-8c05-4cce-b5a5-4bf7b230ee03
ms.openlocfilehash: 0665ce331492a5322fdfde9e91fc1dae5b8e7ea8
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73424110"
---
# <a name="message-security-anonymous"></a>メッセージ セキュリティ匿名
Message Security Anonymous サンプルでは、クライアント認証なしでメッセージレベルのセキュリティを使用するが、サーバーの x.509 を使用するサーバー認証を必要とする Windows Communication Foundation (WCF) アプリケーションを実装する方法を示します。証明. クライアント/サーバー間のすべてのアプリケーション メッセージは署名され、暗号化されます。 このサンプルは、 [WSHttpBinding](../../../../docs/framework/wcf/samples/wshttpbinding.md)サンプルを基にしています。 このサンプルは、クライアント コンソール プログラム (.exe) と、インターネット インフォメーション サービス (IIS) によってホストされるサービス ライブラリ (.dll) で構成されています。 サービスは、要求/応答通信パターンを定義するコントラクトを実装します。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

 このサンプルでは、電卓インターフェイスに、クライアントが認証されなかった場合に `True` を返す新しい操作を追加します。

```csharp
public class CalculatorService : ICalculator
{
    public bool IsCallerAnonymous()
    {
        // ServiceSecurityContext.IsAnonymous returns true if the caller is not authenticated.
        return ServiceSecurityContext.Current.IsAnonymous;
    }
    ...
}
```

 サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは構成ファイル (Web.config) で定義します。 エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。 このバインディングの構成には `wsHttpBinding` バインディングを使用します。 `wsHttpBinding` バインディングの既定のセキュリティ モードは `Message` です。 `clientCredentialType` 属性が `None` に設定されています。

```xml
<system.serviceModel>

  <protocolMapping>
    <add scheme="http" binding="wsHttpBinding" />
  </protocolMapping>

  <bindings>
    <wsHttpBinding>
     <!-- This configuration defines the security mode as Message and -->
     <!-- the clientCredentialType as None. This mode provides -->
     <!-- server authentication only using the service certificate. -->

     <binding>
       <security mode="Message">
         <message clientCredentialType="None" />
       </security>
     </binding>
    </wsHttpBinding>
  </bindings>
  ...
</system.serviceModel>
```

 サービス認証に使用される資格情報は、 [\<の動作](../../../../docs/framework/configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md)で指定されます。 サーバー証明書の `SubjectName` には、`findValue` 属性に指定されている値と同じ値が指定されている必要があります。次のサンプル コードを参照してください。

```xml
<behaviors>
  <serviceBehaviors>
    <behavior>
      <!--
    The serviceCredentials behavior allows you to define a service certificate.
    A service certificate is used by a client to authenticate the service and provide message protection.
    This configuration references the "localhost" certificate installed during the setup instructions.
    -->
      <serviceCredentials>
        <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
      </serviceCredentials>
      <serviceMetadata httpGetEnabled="True"/>
      <serviceDebug includeExceptionDetailInFaults="False" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```

 クライアント エンドポイント構成は、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。 `wsHttpBinding` バインディングのクライアント セキュリティ モードは `Message` です。 `clientCredentialType` 属性が `None` に設定されています。

```xml
<system.serviceModel>
  <client>
    <endpoint name=""
             address="http://localhost/servicemodelsamples/service.svc"
             binding="wsHttpBinding"
             behaviorConfiguration="ClientCredentialsBehavior"
             bindingConfiguration="Binding1"
             contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </client>

  <bindings>
    <wsHttpBinding>
      <!--This configuration defines the security mode as -->
      <!--Message and the clientCredentialType as None. -->
      <binding name="Binding1">
        <security mode = "Message">
          <message clientCredentialType="None"/>
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>
  ...
</system.serviceModel>
```

 このサンプルでは、サービスの証明書を認証するために、<xref:System.ServiceModel.Security.X509ServiceCertificateAuthentication.CertificateValidationMode%2A> を <xref:System.ServiceModel.Security.X509CertificateValidationMode.PeerOrChainTrust> に設定します。 これは、`behaviors` セクションのクライアントの App.config ファイルで行われます。 つまり、証明書がユーザーの Trusted People ストア内に存在している場合、その証明書は発行者のチェーンが検証されることなく信頼されます。 証明機関 (CA) から発行された証明書を要求しなくともサンプルを実行できるようにするため、ここでは便宜上この設定が使用されます。 この設定は、既定の ChainTrust よりも安全性が低くなります。 `PeerOrChainTrust` を製品版のコードで使用する前に、この設定のセキュリティへの影響について慎重に考慮する必要があります。

 クライアント実装は `IsCallerAnonymous` メソッドへの呼び出しを追加します。それ以外の場合、 [WSHttpBinding](../../../../docs/framework/wcf/samples/wshttpbinding.md)サンプルとは異なります。

```csharp
// Create a client with a client endpoint configuration.
CalculatorClient client = new CalculatorClient();

// Call the GetCallerIdentity operation.
Console.WriteLine("IsCallerAnonymous returned: {0}", client.IsCallerAnonymous());

// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = client.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);

...

//Closing the client gracefully closes the connection and cleans up resources.
client.Close();

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

```console
IsCallerAnonymous returned: True
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714
Press <ENTER> to terminate client.
```

 メッセージ セキュリティ匿名サンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、証明書ベースのセキュリティを必要とするホスト アプリケーションを実行できるようになります。 バッチ ファイルの実行には 2 つのモードを使用できます。 バッチ ファイルを単一コンピューター モードで実行するには、コマンド ラインに「`setup.bat`」と入力します。 サービス モードで実行するには、「`setup.bat service`」と入力します。 このサンプルを複数のコンピューターで実行している場合は、このモードを使用します。 詳細については、このトピック末尾のセットアップ手順を参照してください。

 次に、バッチ ファイルの各セクションの概要を簡単に説明します。

- サーバー証明書の作成。

     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。

    ```bat
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

     %SERVER_NAME% 変数はサーバー名を指定します。 証明書は LocalMachine ストアに保存されます。 セットアップ バッチ ファイルの実行にサービスの引数 (`setup.bat service` など) が使用された場合、%SERVER_NAME% にはコンピューターの完全修飾ドメイン名が含まれます。 それ以外の場合、既定値は localhost です。

- クライアントの信頼された証明書ストアへのサーバー証明書のインストール。

     次の行は、サーバー証明書をクライアントの信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- 証明書の秘密キーに関する権限の付与。

     セットアップのバッチファイルの次の行を使用して、LocalMachine ストアに格納されているサーバー証明書を ASP.NET ワーカープロセスアカウントにアクセスできるようにします。

    ```bat
    echo ************
    echo setting privileges on server certificates
    echo ************
    for /F "delims=" %%i in ('"%MSSDK%\bin\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE
    (ver | findstr "5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R
    iisreset
    ```

> [!NOTE]
> 米国英語版以外の Windows を使用している場合は、セットアップの .bat ファイルを編集し、`NT AUTHORITY\NETWORK SERVICE` アカウント名を実際の地域に合わせて変更する必要があります。

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには

1. Makecert.exe と FindPrivateKey.exe が含まれているフォルダーがパスに含まれていることを確認します。

2. Visual Studio の開発者コマンドプロンプトのサンプルのインストールフォルダーから、管理者特権で実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。

    > [!NOTE]
    > セットアップバッチファイルは、Visual Studio の開発者コマンドプロンプトから実行するように設計されています。 path 環境変数が SDK のインストール ディレクトリを指している必要があります。 この環境変数は、Visual Studio の開発者コマンドプロンプト内で自動的に設定されます。  
  
3. `http://localhost/servicemodelsamples/service.svc`アドレスを入力して、ブラウザーを使用してサービスへのアクセスを確認します。  
  
4. Client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
5. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1. サービス コンピューターにディレクトリを作成します。 インターネット インフォメーション サービス (IIS) 管理ツールを使用して、このディレクトリ用に servicemodelsamples という仮想アプリケーションを作成します。  
  
2. サービス プログラム ファイルを \inetpub\wwwroot\servicemodelsamples からサービス コンピューターの仮想ディレクトリにコピーします。 ファイルのコピー先が \bin サブディレクトリであることを確認します。 Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
3. クライアント コンピューターにクライアント バイナリ用のディレクトリを作成します。  
  
4. クライアント プログラム ファイルを、クライアント コンピューターに作成したクライアント ディレクトリにコピーします。 Setup.bat、Cleanup.bat、ImportServiceCert.bat の各ファイルもクライアントにコピーします。  
  
5. サーバーで、管理者特権で開いた Visual Studio の開発者コマンドプロンプトで `setup.bat service` を実行します。 `service` 引数を指定して `setup.bat` を実行すると、コンピューターの完全修飾ドメイン名を使用してサービス証明書が作成され、service .cer という名前のファイルにエクスポートされます。  
  
6. Web.config を編集して、新しい証明書名 ( [\<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)の `findValue` 属性) を反映します。これは、コンピューターの完全修飾ドメイン名と同じです。  
  
7. Service.cer ファイルを、サービス ディレクトリからクライアント コンピューターのクライアント ディレクトリにコピーします。  
  
8. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。  
  
9. クライアントで、管理者特権で開かれた Visual Studio の開発者コマンドプロンプトで Importservicecert.bat を実行します。 これにより、サービス証明書が Service.cer ファイルから CurrentUser - TrustedPeople ストアにインポートされます。  
  
10. クライアント コンピューターで、コマンド プロンプトから Client.exe を起動します。 クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
- サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
  
> [!NOTE]
> このサンプルを複数のコンピューターで実行している場合、このスクリプトはサービス証明書をクライアントから削除しません。 コンピューター間で証明書を使用する Windows Communication Foundation (WCF) サンプルを実行した場合は、CurrentUser-TrustedPeople ストアにインストールされているサービス証明書を必ずオフにしてください。 削除するには、コマンド `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` を実行します。たとえば、`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com.` となります。
