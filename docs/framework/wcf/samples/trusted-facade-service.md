---
title: 信頼されたファサード サービス
ms.date: 03/30/2017
ms.assetid: c34d1a8f-e45e-440b-a201-d143abdbac38
ms.openlocfilehash: 40264ee018d3c09d86e1bcd0b8cc96c0610219f9
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74711890"
---
# <a name="trusted-facade-service"></a>信頼されたファサード サービス
このシナリオのサンプルでは、Windows Communication Foundation (WCF) セキュリティインフラストラクチャを使用して、呼び出し元の id 情報をあるサービスから別のサービスにフローする方法を示します。  
  
 ファサード サービスを使用してサービスから提供される機能をパブリック ネットワークに公開するのは、一般的なデザイン パターンです。 ファサード サービスは、通常は境界ネットワーク (DMZ、非武装地帯、およびスクリーン サブネットとも呼ばれます) 内に存在し、ビジネス ロジックを実装して内部データにアクセスする、バックエンド サービスと通信します。 ファサード サービスとバックエンド サービス間の通信チャネルはファイアウォールを通過し、通常は 1 つの目的のみに限定されます。  
  
 このサンプルは、次のコンポーネントで構成されています。  
  
- 電卓クライアント  
  
- 電卓ファサード サービス  
  
- 電卓バックエンド サービス  
  
 ファサード サービスは、要求を検証して呼び出し元を認証します。 認証と検証が正常に完了すると、ファサード サービスは、境界ネットワークから内部ネットワークへの制御された通信チャネルを使用して、バックエンド サービスに要求を転送します。 ファサード サービスは、転送される要求の一部として呼び出し元の ID に関する情報を格納し、バックエンド サービスがプロセスでこの情報を使用できるようにします。 呼び出し元の ID は、メッセージの `Username` ヘッダー内部にある `Security` セキュリティ トークンを使用して送信されます。 このサンプルでは、WCF セキュリティインフラストラクチャを使用して、この情報を `Security` ヘッダーから送信および抽出します。  
  
> [!IMPORTANT]
> バックエンド サービスは、ファサード サービスを信頼して呼び出し元を認証します。 このため、バックエンド サービスは呼び出し元の再認証を行いません。つまり、ファサード サービスから転送された要求内の ID 情報を使用します。 こうした信頼関係があるので、バックエンド サービスでは、転送メッセージが信頼されたソース (この場合はファサード サービス) から送信されことを確認するために、ファサード サービスを認証する必要があります。  
  
## <a name="implementation"></a>実装  
 このサンプルには、通信パスが 2 つあります。 1 つはクライアントとファサード サービス間のパスで、もう 1 つはファサード サービスとバックエンド サービス間のパスです。  
  
### <a name="communication-path-between-client-and-faade-service"></a>クライアントとファサード サービス間の通信パス  
 クライアントとファサード サービスとの通信パスでは、 `wsHttpBinding` 型のクライアント資格情報が設定された `UserName` を使用します。 つまり、クライアントはユーザー名とパスワードを使用してファサード サービスに対する認証を行い、ファサード サービスは X.509 証明書を使用してクライアントに対する認証を行います。 バインディング構成は次の例のようになります。  
  
```xml  
<bindings>  
  <wsHttpBinding>  
    <binding name="Binding1">  
      <security mode="Message">  
        <message clientCredentialType="UserName"/>  
      </security>  
    </binding>  
  </wsHttpBinding>  
</bindings>  
```  
  
 ファサード サービスはカスタム `UserNamePasswordValidator` 実装を使用して、呼び出し元を認証します。 この認証では、デモンストレーション用に、呼び出し元のユーザー名と指定されたパスワードが一致していることだけを確認します。 実環境では多くの場合、Active Directory またはカスタムの ASP.NET メンバシップ プロバイダを使用して認証が行われます。 検証の実装は `FacadeService.cs` ファイルにあります。  
  
```csharp  
public class MyUserNamePasswordValidator : UserNamePasswordValidator  
{  
    public override void Validate(string userName, string password)  
    {  
        // check that username matches password  
        if (null == userName || userName != password)  
        {  
            Console.WriteLine("Invalid username or password");  
            throw new SecurityTokenValidationException(  
                       "Invalid username or password");  
        }  
    }  
}  
```  
  
 カスタム検証が、ファサード サービス構成ファイルの `serviceCredentials` 動作の内側で使用されるよう構成されます。 この動作は、サービスの X.509 証明書の構成にも使用されます。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="FacadeServiceBehavior">  
      <!--The serviceCredentials behavior allows you to define -->  
      <!--a service certificate. -->  
      <!--A service certificate is used by the service to  -->  
      <!--authenticate itself to its clients and to provide  -->  
      <!--message protection. -->  
      <!--This configuration references the "localhost"  -->  
      <!--certificate installed during the setup instructions. -->  
      <serviceCredentials>  
        <serviceCertificate   
               findValue="localhost"   
               storeLocation="LocalMachine"   
               storeName="My"   
               x509FindType="FindBySubjectName" />  
        <userNameAuthentication userNamePasswordValidationMode="Custom"  
            customUserNamePasswordValidatorType=  
           "Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator,  
            FacadeService"/>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
### <a name="communication-path-between-faade-service-and-backend-service"></a>ファサード サービスと バックエンド サービス間の通信パス  
 ファサード サービスとバックエンド サービスとの通信パスでは、複数のバインディング要素で構成される `customBinding` を使用します。 このバインディングにより、次の 2 つが実現されます。 まず、ファサード サービスとバックエンド サービスが認証され、通信がセキュリティで保護されていることと、信頼されたソースからのものであることが確認されます。 次に、 `Username` セキュリティ トークン内にある、最初の呼び出し元の ID が送信されます。 この場合、最初の呼び出し元のユーザー名だけがバックエンド サービスに送信されます。パスワードはメッセージには含まれません。 これは、バックエンド サービスがファサード サービスに要求を転送する前に、ファサード サービスを信頼して呼び出し元を認証するためです。 ファサード サービスはバックエンド サービスに対して自己認証を行うので、バックエンド サービスは転送された要求に含まれる情報を信頼できます。  
  
 この通信パスのバインディング構成は次のようになります。  
  
```xml  
<bindings>  
  <customBinding>  
    <binding name="ClientBinding">  
      <security authenticationMode="UserNameOverTransport"/>  
      <windowsStreamSecurity/>  
      <tcpTransport/>  
    </binding>  
  </customBinding>  
</bindings>  
```  
  
 [\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md) binding 要素は、最初の呼び出し元のユーザー名の転送と抽出を行います。 [\<windowsStreamSecurity >](../../../../docs/framework/configure-apps/file-schema/wcf/windowsstreamsecurity.md)と[\<tcptransport >](../../../../docs/framework/configure-apps/file-schema/wcf/tcptransport.md)は、ファサードとバックエンドサービスおよびメッセージ保護の認証を行います。  
  
 要求を転送するには、ファサードサービスの実装で、最初の呼び出し元のユーザー名を指定する必要があります。これにより、WCF セキュリティインフラストラクチャでは、転送されたメッセージにこれを配置できます。 最初の呼び出し元のユーザー名をファサード サービスの実装で提供するには、ファサード サービスがバックエンド サービスとの通信に使用するクライアント プロキシ インスタンスの `ClientCredentials` プロパティに、このユーザー名を設定します。  
  
 `GetCallerIdentity` メソッドをファサード サービスに実装する方法を次のコードに示します。 他のメソッドも同じパターンを使用します。  
  
```csharp  
public string GetCallerIdentity()  
{  
    CalculatorClient client = new CalculatorClient();  
    client.ClientCredentials.UserName.UserName = ServiceSecurityContext.Current.PrimaryIdentity.Name;  
    string result = client.GetCallerIdentity();  
    client.Close();  
    return result;  
}  
```  
  
 前のコードに示すように、 `ClientCredentials` プロパティではパスワードは設定されず、ユーザー名のみが設定されています。 この場合、WCF セキュリティインフラストラクチャでは、パスワードを使用せずにユーザー名セキュリティトークンが作成されます。この場合、このシナリオではまさにこれが必要です。  
  
 バックエンド サービスでは、ユーザー名セキュリティ トークンに含まれる情報が認証される必要があります。 既定では、WCF セキュリティは、指定されたパスワードを使用して、ユーザーを Windows アカウントにマップしようとします。 この場合、パスワードは指定されず、バックエンド サービスはユーザー名を認証する必要がありません。ファサード サービスで既に認証が行われているからです。 WCF でこの機能を実装するには、ユーザー名がトークンで指定され、追加の認証を実行しないことを強制するカスタム `UserNamePasswordValidator` が用意されています。  
  
```csharp  
public class MyUserNamePasswordValidator : UserNamePasswordValidator  
{  
    public override void Validate(string userName, string password)  
    {  
        // Ignore the password because it is empty,   
        // we trust the facade service to authenticate the client.  
        // Accept the username information here so that the   
        // application gets access to it.  
        if (null == userName)  
        {  
            Console.WriteLine("Invalid username");  
            throw new   
             SecurityTokenValidationException("Invalid username");  
        }  
    }  
}  
```  
  
 カスタム検証が、ファサード サービス構成ファイルの `serviceCredentials` 動作の内側で使用されるよう構成されます。  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="BackendServiceBehavior">  
      <serviceCredentials>  
        <userNameAuthentication userNamePasswordValidationMode="Custom"  
           customUserNamePasswordValidatorType=  
          "Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator,   
           BackendService"/>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 ユーザー名の情報と信頼されたファサード サービス アカウントに関する情報を抽出するには、バックエンド サービスの実装で `ServiceSecurityContext` クラスを使用します。 `GetCallerIdentity` メソッドを実装する方法を次のコードに示します。  
  
```csharp  
public string GetCallerIdentity()  
{  
    // Facade service is authenticated using Windows authentication.  
    //Its identity is accessible.  
    // On ServiceSecurityContext.Current.WindowsIdentity.  
    string facadeServiceIdentityName =   
          ServiceSecurityContext.Current.WindowsIdentity.Name;  
  
    // The client name is transmitted using Username authentication on   
    //the message level without the password  
    // using a supporting encrypted UserNameToken.  
    // Claims extracted from this supporting token are available in   
    // ServiceSecurityContext.Current.AuthorizationContext.ClaimSets   
    // collection.  
    string clientName = null;  
    foreach (ClaimSet claimSet in   
        ServiceSecurityContext.Current.AuthorizationContext.ClaimSets)  
    {  
        foreach (Claim claim in claimSet)  
        {  
            if (claim.ClaimType == ClaimTypes.Name &&   
                                   claim.Right == Rights.Identity)  
            {  
                clientName = (string)claim.Resource;  
                break;  
            }  
        }  
    }  
    if (clientName == null)  
    {  
        // In case there was no UserNameToken attached to the request.  
        // In the real world implementation the service should reject   
        // this request.  
        return "Anonymous caller via " + facadeServiceIdentityName;  
    }  
  
    return clientName + " via " + facadeServiceIdentityName;  
}  
```  
  
 ファサード サービス アカウント情報は、 `ServiceSecurityContext.Current.WindowsIdentity` プロパティを使用して抽出されます。 最初の呼び出し元に関する情報にアクセスするには、バックエンド サービスで `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets` プロパティを使用します。 そして、型 `Identity` を含む `Name`クレームを検索します。 この要求は、WCF セキュリティインフラストラクチャによって、`Username` セキュリティトークンに含まれる情報から自動的に生成されます。  
  
## <a name="running-the-sample"></a>サンプルの実行  
 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。 ファサード サービスまたはバックエンド サービスのコンソール ウィンドウで Enter キーを押すと、サービスがシャットダウンされます。  
  
```console  
Username authentication required.  
Provide a valid machine or domain ac  
   Enter username:  
user  
   Enter password:  
****  
user via MyMachine\testaccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 信頼されたファサード シナリオのサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーが構成され、クライアントに対して自己認証を行う証明書ベースのセキュリティが必要なファサード サービスを実行できるようになります。 詳細については、このトピック末尾のセットアップ手順を参照してください。  
  
 次に、バッチ ファイルの各セクションの概要を簡単に説明します。  
  
- サーバー証明書の作成。  
  
     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。  
  
    ```console  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     `%SERVER_NAME%` 変数はサーバー名を指定します。既定値は localhost です。 証明書は LocalMachine ストアに保存されます。  
  
- クライアントの信頼された証明書ストアへのファサード サービスの証明書のインストール。  
  
     次の行は、ファサード サービスの証明書をクライアントの信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。  
  
    ```console  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには  
  
1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。  
  
2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。  
  
#### <a name="to-run-the-sample-on-the-same-machine"></a>サンプルを同じコンピューターで実行するには  
  
1. Makecert.exe が存在するフォルダがパスに含まれていることを確認します。  
  
2. Setup.bat をサンプルのインストール フォルダーで実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。  
  
3. 別のコンソール ウィンドウで、\BackendService\bin ディレクトリの BackendService.exe を起動します。  
  
4. 別のコンソール ウィンドウで、\FacadeService\bin ディレクトリの FacadeService.exe を起動します。  
  
5. Client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
6. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
1. サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
  
> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\TrustedFacade`  
