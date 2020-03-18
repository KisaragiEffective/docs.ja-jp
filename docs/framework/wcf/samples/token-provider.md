---
title: トークン プロバイダー
ms.date: 03/30/2017
ms.assetid: 947986cf-9946-4987-84e5-a14678d96edb
ms.openlocfilehash: 6971a70e633f7768c165ee6171fd83f0eefc4183
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425123"
---
# <a name="token-provider"></a>トークン プロバイダー
このサンプルでは、カスタム トークン プロバイダーを実装する方法を示します。 Windows Communication Foundation (WCF) のトークンプロバイダーは、セキュリティインフラストラクチャに資格情報を提供するために使用されます。 一般的に、トークン プロバイダーは、ターゲットをチェックし、適切な証明書を発行して、セキュリティ インフラストラクチャがメッセージのセキュリティを保護できるようにします。 WCF には、既定の資格情報マネージャートークンプロバイダーが付属しています。 また、WCF には、CardSpace トークンプロバイダーも付属しています。 カスタム トークン プロバイダーは、次の場合に便利です。

- トークン プロバイダーが連携動作できない資格情報ストアがある場合。

- ユーザーが資格情報を使用するときに、ユーザーが詳細情報を提供した時点から資格情報を変換するための独自のカスタムメカニズムを提供する場合。

- カスタム トークンを構築している場合。

 このサンプルでは、ユーザーの入力を別の形式に変換するカスタム トークン プロバイダーを構築する方法を示します。

 このサンプルに示されている手順の概要は次のとおりです。

- クライアントがユーザー名/パスワードの組み合わせを使用して認証する方法。

- クライアントをカスタム トークン プロバイダーを使用して構成する手順。

- サーバーが、ユーザー名とパスワードが一致していることを検証するカスタム <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> とパスワードを使用して、クライアント資格情報を検証する手順。

- サーバーがクライアントによってサーバーの X.509 証明書を使用して認証される手順。

 さらにこのサンプルでは、カスタム トークン認証システムの処理後に、呼び出し元の ID にアクセスするための方法も示します。

 サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは App.config 構成ファイルで定義します。 エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。 バインディングの構成には、標準の `wsHttpBinding` が使用されます。これは既定でメッセージ セキュリティを使用します。 このサンプルは、クライアント ユーザー名認証を使用するように標準の `wsHttpBinding`を設定します。 また、サービスは serviceCredentials 動作を使用してサービス証明書の構成も行います。 serviceCredentials 動作を使用すると、サービス証明書を構成できます。 クライアントはサービス証明書を使用して、サービスを認証し、メッセージを保護します。 次の構成では、次のセットアップ手順で説明しているサンプル セットアップでインストールされる localhost 証明書を参照しています。

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service"/>
          </baseAddresses>
        </host>
        <!-- use base address provided by host -->
        <endpoint address=""
                  binding="wsHttpBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
      </service>
    </services>

    <bindings>
      <wsHttpBinding>
        <binding name="Binding1">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>

    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceDebug includeExceptionDetailInFaults="False" />
          <!--
        The serviceCredentials behavior allows one to define a service certificate.
        A service certificate is used by a client to authenticate the service and provide message protection.
        This configuration references the "localhost" certificate installed during the setup instructions.
        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
```

 クライアント エンドポイント構成は、構成名、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。 クライアント バインディングは、適切な `Mode` およびメッセージ `clientCredentialType` で構成されます。

```xml
<system.serviceModel>
  <client>
    <endpoint name=""
              address="http://localhost:8000/servicemodelsamples/service"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              contract="Microsoft.ServiceModel.Samples.ICalculator">
    </endpoint>
  </client>

  <bindings>
    <wsHttpBinding>
      <binding name="Binding1">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>
</system.serviceModel>
```

 次の手順は、カスタムトークンプロバイダーを開発し、それを WCF セキュリティフレームワークと統合する方法を示しています。

1. カスタム トークン プロバイダーを作成します。

     サンプルでは、ユーザー名とパスワードを取得するカスタム トークン プロバイダーを実装します。 パスワードは、このユーザー名と一致する必要があります。 このカスタム トークン プロバイダーは、デモンストレーション用にのみ作成されたものです。実環境に展開することは推奨されません。

     このタスクを実行するため、カスタム トークン プロバイダーは <xref:System.IdentityModel.Selectors.SecurityTokenProvider> クラスを派生し、<xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%28System.TimeSpan%29> メソッドをオーバーライドします。 このメソッドは、新しい `UserNameSecurityToken` を作成して返します。

    ```csharp
    protected override SecurityToken GetTokenCore(TimeSpan timeout)
    {
        // obtain username and password from the user using console window
        string username = GetUserName();
        string password = GetPassword();
        Console.WriteLine("username: {0}", username);

        // return new UserNameSecurityToken containing information obtained from user
        return new UserNameSecurityToken(username, password);
    }
    ```

2. カスタム セキュリティ トークン マネージャーを作成します。

     <xref:System.IdentityModel.Selectors.SecurityTokenManager> は、特定の <xref:System.IdentityModel.Selectors.SecurityTokenProvider> (<xref:System.IdentityModel.Selectors.SecurityTokenRequirement> メソッドで渡されます) に対する `CreateSecurityTokenProvider` を作成するために使用されます。 セキュリティ トークン マネージャーは、トークン認証システムとトークン シリアライザーの作成にも使用されますが、このサンプルでは扱っていません。 このサンプルでは、カスタム セキュリティ トークン マネージャーは <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> クラスを継承し、渡されたトークンの要件でユーザー名プロバイダーが必要であることが示されている場合に、`CreateSecurityTokenProvider` メソッドをオーバーライドして、カスタムのユーザー名トークン プロバイダーを返します。

    ```csharp
    public class MyUserNameSecurityTokenManager : ClientCredentialsSecurityTokenManager
    {
        MyUserNameClientCredentials myUserNameClientCredentials;

        public MyUserNameSecurityTokenManager(MyUserNameClientCredentials myUserNameClientCredentials)
            : base(myUserNameClientCredentials)
        {
            this.myUserNameClientCredentials = myUserNameClientCredentials;
        }

        public override SecurityTokenProvider CreateSecurityTokenProvider(SecurityTokenRequirement tokenRequirement)
        {
            // if token requirement matches username token return custom username token provider
            // otherwise use base implementation
            if (tokenRequirement.TokenType == SecurityTokenTypes.UserName)
            {
                return new MyUserNameTokenProvider();
            }
            else
            {
                return base.CreateSecurityTokenProvider(tokenRequirement);
            }
        }
    }
    ```

3. カスタム クライアント資格情報を作成します。

     クライアント資格情報クラスは、クライアント プロキシ用に構成された資格情報を表すために使用され、トークン認証システム、トークン プロバイダー、およびトークン シリアライザーの取得に使用されるセキュリティ トークン マネージャーを作成します。

    ```csharp
    public class MyUserNameClientCredentials : ClientCredentials
    {
        public MyUserNameClientCredentials()
            : base()
        {
        }

        protected override ClientCredentials CloneCore()
        {
            return new MyUserNameClientCredentials();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            // return custom security token manager
            return new MyUserNameSecurityTokenManager(this);
        }
    }
    ```

4. カスタム クライアント資格情報を使用するようクライアントを構成します。

     クライアントがカスタム クライアント資格情報を使用するため、サンプルでは既定のクライアント資格情報を削除し、新しいクライアント資格情報クラスを提供しています。

    ```csharp
    static void Main()
    {
        // ...
           // Create a client with given client endpoint configuration
          CalculatorClient client = new CalculatorClient();

          // set new credentials
           client.ChannelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));
         client.ChannelFactory.Endpoint.Behaviors.Add(new MyUserNameClientCredentials());
       // ...
    }
    ```

 サービス側で呼び出し元の情報を表示するには、次のコード例に示すように <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> を使用します。 <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> には、現在のユーザーに関する情報が保持されています。

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
        ServiceSecurityContext.Current.PrimaryIdentity.Name);
}
```

 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

## <a name="setup-batch-file"></a>セットアップ バッチ ファイル
 このサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、サーバー証明書ベースのセキュリティを必要とする自己ホスト型アプリケーションを実行できるようになります。 このバッチ ファイルは、複数のコンピューターを使用する場合またはホストなしの場合に応じて変更する必要があります。

 次に、バッチ ファイルのセクションのうち、該当する構成で実行するために変更が必要となる部分を示します。

- サーバー証明書の作成。

     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。 `%SERVER_NAME%` 変数はサーバー名です。 この変数を変更して、使用するサーバー名を指定します。 このバッチ ファイルでの既定値は localhost です。

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- クライアントの信頼された証明書ストアへのサーバー証明書のインストール。

     Setup.bat バッチ ファイルの次の行は、サーバー証明書をクライアントの信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

> [!NOTE]
> Setup.bat バッチ ファイルは、Windows SDK コマンド プロンプトから実行します。 MSSDK 環境変数が SDK のインストール ディレクトリを指している必要があります。 この環境変数は、Windows SDK コマンド プロンプトで自動設定されます。

#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

#### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには

1. 管理者特権で開いた Visual Studio 2012 コマンドプロンプト内のサンプルのインストールフォルダーから、Setup.exe を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。

    > [!NOTE]
    > セットアップの .bat バッチファイルは、Visual Studio 2012 のコマンドプロンプトから実行するように設計されています。 Visual Studio 2012 のコマンドプロンプト内で設定された PATH 環境変数は、セットアップの .bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指します。  
  
2. service.exe を \service\bin で起動します。  
  
3. Client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
4. username プロンプトに対してユーザー名を入力します。  
  
5. password プロンプトに対して、username プロンプトで入力したものと同じ文字列を入力します。  
  
6. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1. サービス コンピューターにサービス バイナリ用のディレクトリを作成します。  
  
2. サービス プログラム ファイルを、サービス コンピューターのサービス ディレクトリにコピーします。 Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
3. コンピューターの完全修飾ドメイン名を含むサブジェクト名を持つサーバー証明書が必要です。 新しい証明書名を反映するには、Service.exe.config ファイルを更新する必要があります。 サーバー証明書を作成するには、Setup.bat バッチ ファイルを変更します。 セットアップの .bat ファイルは、管理者特権で開かれた Visual Studio の開発者コマンドプロンプトから実行する必要があることに注意してください。 `%SERVER_NAME%` 変数には、サービスをホストするコンピューターの完全修飾ホスト名を設定する必要があります。  
  
4. サーバー証明書をクライアントの CurrentUser-TrustedPeople ストアにコピーします。 この操作は、サーバー証明書の発行元をクライアントが信頼できる場合は不要です。  
  
5. サービス コンピューターの Service.exe.config ファイルで、ベース アドレスの値を localhost から完全修飾コンピューター名に変更します。  
  
6. サービス コンピューターで、コマンド プロンプトから service.exe を実行します。  
  
7. クライアント プログラム ファイルを、言語固有のフォルダーにある \client\bin\ フォルダーからクライアント コンピューターにコピーします。  
  
8. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。  
  
9. クライアント コンピューターで、コマンド プロンプト ウィンドウから `Client.exe` を起動します。  
  
10. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
1. サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
