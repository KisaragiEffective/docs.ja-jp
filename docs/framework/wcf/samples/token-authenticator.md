---
title: トークン認証システム
ms.date: 03/30/2017
ms.assetid: 84382f2c-f6b1-4c32-82fa-aebc8f6064db
ms.openlocfilehash: 835a158ba668a3aef749602c73fd9157e8d83a40
ms.sourcegitcommit: 14ad34f7c4564ee0f009acb8bfc0ea7af3bc9541
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425030"
---
# <a name="token-authenticator"></a>トークン認証システム
このサンプルでは、カスタム トークンの認証システムを実装する方法を示します。 Windows Communication Foundation (WCF) のトークン認証システムは、メッセージで使用されるトークンを検証し、それが自己整合していることを確認し、トークンに関連付けられている id を認証するために使用されます。

 カスタム トークン認証システムは次のようなさまざまな場合に役立ちます。

- トークンに関連付けられた既定の認証機構をオーバーライドする場合。

- カスタム トークンを構築している場合。

 このサンプルでは、次の方法を示します。

- クライアントがユーザー名/パスワードの組み合わせを使用して認証する方法。

- サーバーがカスタム トークン認証システムを使用してクライアント資格情報を検証する方法。

- WCF サービスコードがカスタムトークン認証システムと連携する方法。

- サーバーの X.509 証明書を使用してそのサーバーを認証する方法。

 このサンプルでは、カスタムトークン認証プロセスの後に、WCF から呼び出し元の id にアクセスする方法も示しています。

 サービスは、そのサービスとの通信に使用する単一エンドポイントを公開します。エンドポイントは App.config 構成ファイルで定義します。 エンドポイントは、アドレス、バインディング、およびコントラクトがそれぞれ 1 つずつで構成されます。 バインディングの構成には、標準の `wsHttpBinding` が使用されます。このセキュリティ モードは、`wsHttpBinding` の既定モードであるメッセージに設定されます。 このサンプルは、クライアント ユーザー名認証を使用するように標準の `wsHttpBinding`を設定します。 また、サービスは `serviceCredentials` 動作を使用してサービス証明書の構成も行います。 `securityCredentials` 動作を使用すると、サービス証明書を指定できます。 クライアントはサービス証明書を使用して、サービスを認証し、メッセージを保護します。 次の構成では、次のセットアップ手順で説明しているサンプル セットアップでインストールされる localhost 証明書を参照しています。

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service" />
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
....        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

 クライアント エンドポイント構成は、構成名、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。 クライアント バインディングは、適切な `Mode` と `clientCredentialType` で構成されます。

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

 クライアント実装では、使用するユーザー名とパスワードが設定されます。

```csharp
static void Main()
{
     ...
     client.ClientCredentials.UserNamePassword.UserName = username;
     client.ClientCredentials.UserNamePassword.Password = password;
     ...
}
```

## <a name="custom-token-authenticator"></a>カスタム トークン認証システム
 カスタム トークン認証システムを作成するには、次の手順に従います。

1. カスタム トークン認証システムを作成します。

     このサンプルは、ユーザー名が有効な電子メール形式であることを検証するカスタム トークン認証システムを実装しています。 この実装は <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator> から派生します。 このクラスで最も重要なメソッドは <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator.ValidateUserNamePasswordCore%28System.String%2CSystem.String%29> です。 このメソッドで、認証システムはユーザー名の形式を検証し、さらに、ホスト名が非承認のドメインのものでないことを検証します。 どちらの条件も満たされる場合は、<xref:System.IdentityModel.Policy.IAuthorizationPolicy> インスタンスの読み取り専用のコレクションを返します。次にこれを使用して、ユーザー名トークン内に格納されている情報を表すクレームを指定します。

    ```csharp
    protected override ReadOnlyCollection<IAuthorizationPolicy> ValidateUserNamePasswordCore(string userName, string password)
    {
        if (!ValidateUserNameFormat(userName))
            throw new SecurityTokenValidationException("Incorrect UserName format");

        ClaimSet claimSet = new DefaultClaimSet(ClaimSet.System, new Claim(ClaimTypes.Name, userName, Rights.PossessProperty));
        List<IIdentity> identities = new List<IIdentity>(1);
        identities.Add(new GenericIdentity(userName));
        List<IAuthorizationPolicy> policies = new List<IAuthorizationPolicy>(1);
        policies.Add(new UnconditionalPolicy(ClaimSet.System, claimSet, DateTime.MaxValue.ToUniversalTime(), identities));
        return policies.AsReadOnly();
    }
    ```

2. カスタム トークン認証システムによって返された承認ポリシーを指定します。

     このサンプルでは、<xref:System.IdentityModel.Policy.IAuthorizationPolicy> という名前の `UnconditionalPolicy` の独自の実装を示します。これにより、コンストラクター内でこのポリシーに渡されたクレームと ID のセットが返されます。

    ```csharp
    class UnconditionalPolicy : IAuthorizationPolicy
    {
        String id = Guid.NewGuid().ToString();
        ClaimSet issuer;
        ClaimSet issuance;
        DateTime expirationTime;
        IList<IIdentity> identities;

        public UnconditionalPolicy(ClaimSet issuer, ClaimSet issuance, DateTime expirationTime, IList<IIdentity> identities)
        {
            if (issuer == null)
                throw new ArgumentNullException("issuer");
            if (issuance == null)
                throw new ArgumentNullException("issuance");

            this.issuer = issuer;
            this.issuance = issuance;
            this.identities = identities;
            this.expirationTime = expirationTime;
        }

        public string Id
        {
            get { return this.id; }
        }

        public ClaimSet Issuer
        {
            get { return this.issuer; }
        }

        public DateTime ExpirationTime
        {
            get { return this.expirationTime; }
        }

        public bool Evaluate(EvaluationContext evaluationContext, ref object state)
        {
            evaluationContext.AddToTarget(this, this.issuance);

            if (this.identities != null)
            {
                object value;
                IList<IIdentity> contextIdentities;
                if (!evaluationContext.Properties.TryGetValue("Identities", out value))
                {
                    contextIdentities = new List<IIdentity>(this.identities.Count);
                    evaluationContext.Properties.Add("Identities", contextIdentities);
                }
                else
                {
                    contextIdentities = value as IList<IIdentity>;
                }
                foreach (IIdentity identity in this.identities)
                {
                    contextIdentities.Add(identity);
                }
            }

            evaluationContext.RecordExpirationTime(this.expirationTime);
            return true;
        }
    }
    ```

3. カスタム セキュリティ トークン マネージャーを作成します。

     <xref:System.IdentityModel.Selectors.SecurityTokenManager> は、<xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> メソッド内でカスタム セキュリティ トークン マネージャーに渡される特定の <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> オブジェクトを対象とした `CreateSecurityTokenAuthenticator` の作成に使用されます。 セキュリティ トークン マネージャーは、トークン プロバイダーとトークン シリアライザーの作成にも使用されますが、このサンプルでは扱っていません。 このサンプルでは、カスタム セキュリティ トークン マネージャーは <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> クラスを継承し、渡されたトークンの要件でユーザー名認証システムが必要であることが示されている場合に、`CreateSecurityTokenAuthenticator` メソッドをオーバーライドしてユーザー名トークンのカスタム認証システムを返します。

    ```csharp
    public class MySecurityTokenManager : ServiceCredentialsSecurityTokenManager
    {
        MyUserNameCredential myUserNameCredential;

        public MySecurityTokenManager(MyUserNameCredential myUserNameCredential)
            : base(myUserNameCredential)
        {
            this.myUserNameCredential = myUserNameCredential;
        }

        public override SecurityTokenAuthenticator CreateSecurityTokenAuthenticator(SecurityTokenRequirement tokenRequirement, out SecurityTokenResolver outOfBandTokenResolver)
        {
            if (tokenRequirement.TokenType ==  SecurityTokenTypes.UserName)
            {
                outOfBandTokenResolver = null;
                return new MyTokenAuthenticator();
            }
            else
            {
                return base.CreateSecurityTokenAuthenticator(tokenRequirement, out outOfBandTokenResolver);
            }
        }
    }
    ```

4. カスタム サービス資格情報を作成します。

     サービス資格情報クラスは、サービス用に構成された資格情報を表すために使用され、トークン認証システム、トークン プロバイダー、およびトークン シリアライザーの取得に使用されるセキュリティ トークン マネージャーを作成します。

    ```csharp
    public class MyUserNameCredential : ServiceCredentials
    {

        public MyUserNameCredential()
            : base()
        {
        }

        protected override ServiceCredentials CloneCore()
        {
            return new MyUserNameCredential();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            return new MySecurityTokenManager(this);
        }

    }
    ```

5. カスタム サービス資格情報を使用するようサービスを構成します。

     サービスでカスタム サービス資格情報を使用するには、既定のサービス資格情報に事前構成されているサービス証明書をキャプチャした後で既定のサービス資格情報クラスを削除し、事前構成されているサービス証明書を使用するよう新しいサービス資格情報のインスタンスを構成します。さらに、この新しいサービス資格情報のインスタンスをサービス動作に追加します。

    ```csharp
    ServiceCredentials sc = serviceHost.Credentials;
    X509Certificate2 cert = sc.ServiceCertificate.Certificate;
    MyUserNameCredential serviceCredential = new MyUserNameCredential();
    serviceCredential.ServiceCertificate.Certificate = cert;
    serviceHost.Description.Behaviors.Remove((typeof(ServiceCredentials)));
    serviceHost.Description.Behaviors.Add(serviceCredential);
    ```

 呼び出し元の情報を表示するには、次のコードに示すように <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> を使用できます。 <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> には、現在のユーザーに関する情報が保持されています。

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
            ServiceSecurityContext.Current.PrimaryIdentity.Name);
     return;
}
```

 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

## <a name="setup-batch-file"></a>セットアップ バッチ ファイル
 このサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、サーバー証明書ベースのセキュリティを必要とする自己ホスト型アプリケーションを実行できるようになります。 このバッチ ファイルは、複数のコンピューターを使用する場合またはホストなしの場合に応じて変更する必要があります。

 次に、バッチ ファイルのセクションのうち、適切な構成で実行するために変更が必要となる部分を示します。

- サーバー証明書の作成。

     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。 `%SERVER_NAME%` 変数はサーバー名です。 この変数を変更して、使用するサーバー名を指定します。 このバッチ ファイルでの既定は localhost です。

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- サーバー証明書のクライアントの信頼された証明書ストアへのインストール。

     Setup.bat バッチ ファイルの次の行は、サーバー証明書をクライアントの信頼されたユーザーのストアにコピーします。 この手順が必要なのは、Makecert.exe によって生成される証明書がクライアント システムにより暗黙には信頼されないからです。 マイクロソフト発行の証明書など、クライアントの信頼されたルート証明書に基づいた証明書が既にある場合は、クライアント証明書ストアにサーバー証明書を配置するこの手順は不要です。

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

    > [!NOTE]
    > セットアップ バッチ ファイルは、Windows SDK コマンド プロンプトから実行します。 MSSDK 環境変数が SDK のインストール ディレクトリを指している必要があります。 この環境変数は、Windows SDK コマンド プロンプトで自動設定されます。

#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

#### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには

1. 管理者特権で開いた Visual Studio 2012 コマンドプロンプト内のサンプルのインストールフォルダーから、Setup.exe を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。

    > [!NOTE]
    > セットアップの .bat バッチファイルは、Visual Studio 2012 のコマンドプロンプトから実行するように設計されています。 Visual Studio 2012 のコマンドプロンプト内で設定された PATH 環境変数は、セットアップの .bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指します。  
  
2. service.exe を \service\bin で起動します。  
  
3. client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
4. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1. サービス コンピューターにサービス バイナリ用のディレクトリを作成します。  
  
2. サービス プログラム ファイルを、サービス コンピューターのサービス ディレクトリにコピーします。 Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
3. コンピューターの完全修飾ドメイン名を含むサブジェクト名を持つサーバー証明書が必要です。 新しい証明書名を反映するには、サービスの App.config ファイルを更新する必要があります。 `%SERVER_NAME%` 変数を、サービスを実行するコンピューターの完全修飾ホスト名に設定している場合は、Setup.bat を使用してこの証明書を作成できます。 セットアップの .bat ファイルは、管理者特権で開かれた Visual Studio の開発者コマンドプロンプトから実行する必要があることに注意してください。  
  
4. サーバー証明書をクライアントの CurrentUser-TrustedPeople ストアにコピーします。 サーバー証明書の発行元をクライアントが信頼できる場合を除き、この操作は不要です。  
  
5. サービス コンピューターの App.config ファイルで、ベース アドレスの値を localhost から完全修飾コンピューター名に変更します。  
  
6. サービス コンピューターで、コマンド プロンプトから service.exe を実行します。  
  
7. クライアント プログラム ファイルを、言語固有のフォルダーにある \client\bin\ フォルダーからクライアント コンピューターにコピーします。  
  
8. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。  
  
9. クライアント コンピューターで、コマンド プロンプトから Client.exe を起動します。  
  
10. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
1. サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
