---
title: ユーザー名パスワード検証
ms.date: 03/30/2017
ms.assetid: 42f03841-286b-42d8-ba58-18c75422bc8e
ms.openlocfilehash: 5e128c8dde14d67a77eeb6f33a99723ddbdfbd9a
ms.sourcegitcommit: 5fb5b6520b06d7f5e6131ec2ad854da302a28f2e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/03/2019
ms.locfileid: "74715318"
---
# <a name="user-name-password-validator"></a>ユーザー名パスワード検証
このサンプルでは、カスタム UserNamePassword 検証を実装する方法を示します。 これは、アプリケーションの要件に適した組み込みの UserNamePassword 検証モードがない場合に便利です。たとえば、ユーザー名とパスワードの組み合わせがデータベースなどの外部ストアに保存されている場合などです。 このサンプルでは、2 つの特定のユーザー名とパスワードの組み合わせをチェックする、カスタム検証を備えたサービスを示します。 クライアントはそのようなユーザー名とパスワードの組み合わせを使用して、サービスに対する認証を行います。

> [!IMPORTANT]
> サンプルは、既にコンピューターにインストールされている場合があります。 続行する前に、次の (既定の) ディレクトリを確認してください。  
>   
> `<InstallDrive>:\WF_WCF_Samples`  
>   
> このディレクトリが存在しない場合は、 [Windows Communication Foundation (wcf) および Windows Workflow Foundation (WF) のサンプルの .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459)にアクセスして、すべての WINDOWS COMMUNICATION FOUNDATION (wcf) と [!INCLUDE[wf1](../../../../includes/wf1-md.md)] サンプルをダウンロードしてください。 このサンプルは、次のディレクトリに格納されます。  
>   
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Security\UserNamePasswordValidator`  
  
> [!NOTE]
> カスタム検証が許可するユーザー名とパスワードの組み合わせを使用する UserName 資格情報は、どのユーザーでも作成できるので、このサービスは、標準の UserNamePassword 検証によって提供される既定の動作よりも安全性が低くなります。 標準の UserNamePassword 検証は、指定されたユーザー名とパスワードの組み合わせを Windows アカウントにマップするよう試みます。マップできなかった場合は、認証が失敗します。 このサンプルのカスタム UserNamePassword 検証は、製品版のコードでは使用しないでください。このサンプルは、デモンストレーションのためだけに作成されています。

 このサンプルで示す処理の概要は次のとおりです。

- クライアントはユーザー名トークンを使用して認証される。

- サーバーはクライアント資格情報をカスタム UserNamePassword 検証と照合し、カスタム エラーをユーザー名とパスワードの検証ロジックからクライアントに伝達する。

- サーバーがそのサーバーの X.509 証明書を使用して認証される。

 サービスは、構成ファイルの App.config を使用して定義された、サービスと通信するための単一のエンドポイントを公開します。エンドポイントは、アドレス、バインディング、およびコントラクトで構成されます。 バインディングは、既定で WS-SECURITY とユーザー名認証を使用する標準 `wsHttpBinding` で構成されます。 サービス動作では、クライアントのユーザー名とパスワードの組み合わせを検証するための `Custom` モード、および検証クラスの型を指定します。 さらに、`serviceCertificate` 要素を使用しているサーバー証明書も指定します。 サーバー証明書には、 [\<serviceCertificate >](../../../../docs/framework/configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md)の `findValue` と同じ `SubjectName` の値が含まれている必要があります。

```xml
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <!-- use host/baseAddresses to configure base address provided by host -->
      <host>
        <baseAddresses>
          <add baseAddress ="http://localhost:8001/servicemodelsamples/service" />
        </baseAddresses>
      </host>
      <!-- use base address specified above, provide one endpoint -->
      <endpoint address="username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>

  <bindings>
    <wsHttpBinding>
      <!-- username binding -->
      <binding name="Binding">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>

  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceDebug includeExceptionDetailInFaults ="true"/>
        <serviceCredentials>
          <!--
          The serviceCredentials behavior allows one to
          specify a custom validator for username/password
          combinations.
          -->
          <userNameAuthentication userNamePasswordValidationMode="Custom"
                                  customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+MyCustomUserNameValidator, service" />
          <!--
          The serviceCredentials behavior allows one to define a service certificate. A service certificate is used by a client to authenticate the service and provide message protection. You must specify a server certificate when passing username/passwords to encrypt the information as it is sent on the wire. Otherwise the username and password information would be sent as clear text. This configuration references the "localhost" certificate installed during the setup instructions.
          -->
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>

</system.serviceModel>
```

 クライアント エンドポイント構成は、構成名、サービス エンドポイントの絶対アドレス、バインディング、およびコントラクトで構成されます。 クライアント バインディングは、適切なモードおよびメッセージ `clientCredentialType` で構成されます。

```xml
<system.serviceModel>

    <client>
      <!-- Username based endpoint -->
      <endpoint name="Username"
address="http://localhost:8001/servicemodelsamples/service/username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>

    <bindings>
      <wsHttpBinding>
        <!-- Username binding -->
        <binding name="Binding">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <clientCredentials>
            <serviceCertificate>
              <!--
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
            is in the user's Trusted People store, then it will be trusted without performing a
            validation of the certificate's issuer chain. This setting is used here for convenience so that the
            sample can be run without having to have certificates issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust. The security implications of this
            setting should be carefully considered before using PeerOrChainTrust in production code.
            -->
              <authentication certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>

  </system.serviceModel>
```

 クライアント実装では、ユーザー名とパスワードの入力がユーザーに対して求められます。

```csharp
// Get the username and password
Console.WriteLine("Username authentication required.");
Console.WriteLine("Provide a username.");
Console.WriteLine("   Enter username: (test1)");
string username = Console.ReadLine();
Console.WriteLine("   Enter password:");
string password = "";
ConsoleKeyInfo info = Console.ReadKey(true);
while (info.Key != ConsoleKey.Enter)
{
    if (info.Key != ConsoleKey.Backspace)
    {
        if (info.KeyChar != '\0')
        {
            password += info.KeyChar;
        }
        info = Console.ReadKey(true);
    }
    else if (info.Key == ConsoleKey.Backspace)
    {
        if (password != "")
        {
            password = password.Substring(0, password.Length - 1);
        }
        info = Console.ReadKey(true);
    }
}
for (int i = 0; i < password.Length; i++)
{
    Console.Write("*");
}
Console.WriteLine();
// Create a proxy with Certificate endpoint configuration
CalculatorProxy proxy = new CalculatorProxy("Username")
try
{
  proxy.ClientCredentials.Username.Username = username;
  proxy.ClientCredentials.Username.Password = password;
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = proxy.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
  }
  catch (Exception e)
  {
      Console.WriteLine("Call failed:");
      while (e != null)
      {
          Console.WriteLine("\t{0}", e.Message);
          e = e.InnerException;
      }
      proxy.Abort();
  }
}
```

 このサンプルではカスタム UserNamePassword 検証を使用して、ユーザー名とパスワードの組み合わせを検証します。 サンプルは、`CustomUserNamePasswordValidator` から派生する <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> を実装しています。 詳細については、<xref:System.IdentityModel.Selectors.UserNamePasswordValidator> のドキュメントを参照してください。 特定のカスタム検証を使用しているこのサンプルは、2 つの特定のユーザー名とパスワードの組み合わせを許可する `Validate` メソッドを実装しています。次のコードを参照してください。

```csharp
public class CustomUserNameValidator : UserNamePasswordValidator
{
 // This method validates users. It allows in two users,
 // test1 and test2 with passwords 1tset and 2tset respectively.
 // This code is for illustration purposes only and
 // MUST NOT be used in a production environment because it
 // is NOT secure.
 public override void Validate(string userName, string password)
 {
  if (null == userName || null == password)
  {
   throw new ArgumentNullException();
  }

  if (!(userName == "test1" && password == "1tset") && !(userName == "test2" && password == "2tset"))
  {
   throw new FaultException("Unknown Username or Incorrect Password");
   }
  }
 }
```

 サービス コードに検証を実装した場合、使用する検証インスタンスをサービス ホストに通知する必要があります。 これは次のコードで実行されます。

```csharp
serviceHost.Credentials.UserNameAuthentication.UserNamePasswordValidationMode = UserNamePasswordValidationMode.Custom;
serviceHost.Credentials. UserNameAuthentication.CustomUserNamePasswordValidator = new CustomUserNamePasswordValidator();
```

 または、構成で次のように指定することで、同じことを実現できます。

```xml
<behaviors>
 <serviceBehaviors>
  <behavior name="CalculatorServiceBehavior">
  ...
   <serviceCredentials>
    <!--
    The serviceCredentials behavior allows one to specify authentication constraints on username / password combinations.
    -->
    <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+CustomUserNameValidator, service" />
   ...
  </behavior>
 </serviceBehaviors>
</behaviors>
```

 このサンプルを実行すると、操作要求および応答がクライアントのコンソール ウィンドウに表示されます。 クライアントはすべてのメソッドを問題なく呼び出すことができるようになります。 クライアントをシャットダウンするには、クライアント ウィンドウで Enter キーを押します。

## <a name="setup-batch-file"></a>セットアップ バッチ ファイル
 このサンプルに用意されている Setup.bat バッチ ファイルを使用すると、適切な証明書を使用してサーバーを構成し、サーバー証明書ベースのセキュリティを必要とする自己ホスト型アプリケーションを実行できるようになります。 このバッチ ファイルは、別のコンピューターを使用する場合または自己ホスト型の場合に応じて変更する必要があります。

 次に、バッチ ファイルのセクションのうち、該当する構成で実行するために変更が必要となる部分を示します。

- サーバー証明書の作成 :

     Setup.bat バッチ ファイルの次の行は、使用するサーバー証明書を作成します。 %SERVER_NAME% 変数はサーバー名を指定します。 この変数を変更して、使用するサーバー名を指定します。 既定値は、localhost です。

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

#### <a name="to-set-up-and-build-the-sample"></a>サンプルをセットアップしてビルドするには

1. ソリューションをビルドするには、「 [Windows Communication Foundation サンプルのビルド](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

2. サンプルを単一コンピューター構成で実行するか、複数コンピューター構成で実行するかに応じて、次の手順に従います。

#### <a name="to-run-the-sample-on-the-same-machine"></a>サンプルを同じコンピューターで実行するには

1. Visual Studio 2012 のコマンドプロンプトで、サンプルのインストールフォルダーから Setup.exe を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。

    > [!NOTE]
    > セットアップの .bat バッチファイルは、Visual Studio 2012 のコマンドプロンプトから実行するように設計されています。 Visual Studio 2012 のコマンドプロンプト内で設定された PATH 環境変数は、セットアップの .bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指します。  
  
2. Service.exe を service\bin で起動します。  
  
3. Client.exe を \client\bin で起動します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
4. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-run-the-sample-across-machines"></a>サンプルを複数コンピューターで実行するには  
  
1. サービス コンピューターにサービス バイナリ用のディレクトリを作成します。  
  
2. サービス プログラム ファイルを、サービス コンピューターのサービス ディレクトリにコピーします。 Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
3. コンピューターの完全修飾ドメイン名を含むサブジェクト名を持つサーバー証明書が必要です。 新しい証明書名を反映するには、サーバーの構成ファイルを更新する必要があります。  
  
4. サーバー証明書をクライアントの CurrentUser-TrustedPeople ストアにコピーします。 このようにする必要があるのは、サーバー証明書が信頼できる発行元から発行されていない場合のみです。  
  
5. サービス コンピューターの App.config ファイルで、ベース アドレスの値を localhost から完全修飾コンピューター名に変更します。  
  
6. サービス コンピューターで、コマンド プロンプト ウィンドウから Service.exe を起動します。  
  
7. クライアント プログラム ファイルを、言語固有のフォルダーにある \client\bin\ フォルダーからクライアント コンピューターにコピーします。  
  
8. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。  
  
9. クライアント コンピューターで、コマンド プロンプト ウィンドウから Client.exe を起動します。  
  
10. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
#### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
1. サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。 これにより、証明書ストアからサーバー証明書が削除されます。  
