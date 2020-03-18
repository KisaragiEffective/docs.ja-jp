---
title: サービス ID サンプル
ms.date: 03/30/2017
ms.assetid: 79fa8c1c-85bb-4b67-bc67-bfaf721303f8
ms.openlocfilehash: 868bd6e0ac7429224462c973c1c48132ec3860ba
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/01/2020
ms.locfileid: "76919374"
---
# <a name="service-identity-sample"></a>サービス ID サンプル
このサービス ID サンプルでは、サービスの ID を設定する方法を示します。 クライアントは、デザイン時にサービスのメタデータを使用して ID を取得し、実行時にそのサービス ID を認証することができます。 サービス ID の概念は、クライアントがサービス操作を呼び出す前にそのサービスを認証できるようにし、それによって認証されていない呼び出しからクライアントを保護することにあります。 セキュリティ保護されている接続では、サービスがクライアントの資格情報を認証した後にクライアントのアクセスを許可できますが、このサンプルではこのことを主眼とはしていません。 「サーバー認証を表示する[クライアント](../../../../docs/framework/wcf/samples/client.md)」のサンプルを参照してください。

> [!NOTE]
> このサンプルのセットアップ手順とビルド手順については、このトピックの最後を参照してください。

 このサンプルでは次の機能を示します。

- サービスのさまざまなエンドポイントにある、さまざまな種類の ID を設定する方法。 ID の種類によって機能も異なります。 使用する ID の種類は、エンドポイントのバインディングで使用するセキュリティ資格情報の種類によって決まります。

- ID は、構成内での宣言によって設定できるほか、コードで強制的に設定することもできます。 通常、クライアントとサービスのどちらの場合も、ID の設定には構成を使用します。

- クライアントのカスタム ID を設定する方法。 カスタム ID は通常、既存の種類の ID をカスタマイズしたもので、クライアントがサービスの資格情報で提供される他のクレーム情報を調べ、サービスを呼び出す前に承認決定を行うようにします。

    > [!NOTE]
    > このサンプルでは、identity.com という特定の証明書の ID と、この証明書内に含まれる RSA キーをチェックします。 クライアントの構成で、証明書と RSA 型の ID を使用する場合、これらの値を取得する簡単な方法は、これらの値がシリアル化されて表されるサービスの WSDL を検査することです。

 次のサンプル コードでは、WSHttpBinding を使用して、証明書のドメイン ネーム サーバー (DNS) によってサービス エンドポイントの ID を構成する方法を示します。

```csharp
//Create a service endpoint and set its identity to the certificate's DNS
WSHttpBinding wsAnonbinding = new WSHttpBinding (SecurityMode.Message);
// Client are Anonymous to the service
wsAnonbinding.Security.Message.ClientCredentialType = MessageCredentialType.None;
WServiceEndpoint ep = serviceHost.AddServiceEndpoint(typeof(ICalculator),wsAnonbinding, String.Empty);
EndpointAddress epa = new EndpointAddress(dnsrelativeAddress,EndpointIdentity.CreateDnsIdentity("identity.com"));
ep.Address = epa;
```

 ID は、App.config ファイルの構成内でも指定できます。 サービス エンドポイントの UPN (ユーザー プリンシパル名) ID を設定する方法を次の例に示します。

```xml
<endpoint address="upnidentity"
        behaviorConfiguration=""
        binding="wsHttpBinding"
        bindingConfiguration="WSHttpBinding_Windows"
        name="WSHttpBinding_ICalculator_Windows"
        contract="Microsoft.ServiceModel.Samples.ICalculator">
  <!-- Set the UPN identity for this endpoint -->
  <identity>
      <userPrincipalName value="host\myservice.com" />
  </identity >
</endpoint>
```

 カスタム ID は、<xref:System.ServiceModel.EndpointIdentity> クラスと <xref:System.ServiceModel.Security.IdentityVerifier> クラスから派生させることによって、クライアント上で設定できます。 概念上、<xref:System.ServiceModel.Security.IdentityVerifier> クラスは、サービスの `AuthorizationManager` クラスと同等のクライアントと見なすことができます。 `OrgEndpointIdentity` の実装のコード例を次に示します。この実装では、サーバーの証明書のサブジェクト名と一致する組織名が格納されます。 この組織名の承認チェックは、`CheckAccess` クラスの `CustomIdentityVerifier` メソッドで発生します。

```csharp
// This custom EndpointIdentity stores an organization name
public class OrgEndpointIdentity : EndpointIdentity
{
    private string orgClaim;
    public OrgEndpointIdentity(string orgName)
    {
        orgClaim = orgName;
    }
    public string OrganizationClaim
    {
        get { return orgClaim; }
        set { orgClaim = value; }
    }
}

//This custom IdentityVerifier uses the supplied OrgEndpointIdentity to
//check that X.509 certificate's distinguished name claim contains
//the organization name e.g. the string value "O=Contoso"
class CustomIdentityVerifier : IdentityVerifier
{
    public override bool CheckAccess(EndpointIdentity identity, AuthorizationContext authContext)
    {
        bool returnvalue = false;
        foreach (ClaimSet claimset in authContext.ClaimSets)
        {
            foreach (Claim claim in claimset)
            {
                if (claim.ClaimType == "http://schemas.microsoft.com/ws/2005/05/identity/claims/x500distinguishedname")
                {
                    X500DistinguishedName name = (X500DistinguishedName)claim.Resource;
                    if (name.Name.Contains(((OrgEndpointIdentity)identity).OrganizationClaim))
                    {
                        Console.WriteLine("Claim Type: {0}",claim.ClaimType);
                        Console.WriteLine("Right: {0}", claim.Right);
                        Console.WriteLine("Resource: {0}",claim.Resource);
                        Console.WriteLine();
                        returnvalue = true;
                    }
                }
            }
        }
        return returnvalue;
    }
}
```

 このサンプルでは identity.com という証明書を使用します。この証明書は、言語固有の ID ソリューション フォルダーにあります。

### <a name="to-set-up-build-and-run-the-sample"></a>サンプルをセットアップ、ビルド、および実行するには

1. [Windows Communication Foundation サンプルの1回限りのセットアップ手順](../../../../docs/framework/wcf/samples/one-time-setup-procedure-for-the-wcf-samples.md)を実行したことを確認します。

2. ソリューションの C# 版または Visual Basic .NET 版をビルドするには、「 [Building the Windows Communication Foundation Samples](../../../../docs/framework/wcf/samples/building-the-samples.md)」の手順に従います。

3. サンプルを単一コンピューター構成または複数コンピューター構成で実行するには、「 [Windows Communication Foundation サンプルの実行](../../../../docs/framework/wcf/samples/running-the-samples.md)」の手順に従います。

### <a name="to-run-the-sample-on-the-same-computer"></a>サンプルを同じコンピューターで実行するには

1. Windows XP または Windows Vista では、MMC スナップインツールを使用して、id ソリューションフォルダー内の Id .pfx 証明書ファイルを LocalMachine/My (Personal) 証明書ストアにインポートします。 このファイルは、パスワードで保護されています。 インポート中に、パスワードの入力を求められます。 [パスワード] ボックスに「`xyz`」と入力します。 詳細については、「[方法: MMC スナップインを使用して証明書を表示する](../../../../docs/framework/wcf/feature-details/how-to-view-certificates-with-the-mmc-snap-in.md)」を参照してください。 この処理が完了したら、管理者特権を使用して Visual Studio の開発者コマンドプロンプトで Setup.exe を実行します。これにより、クライアントで使用するために、この証明書が CurrentUser/Trusted People ストアにコピーされます。

2. Windows Server 2003 で、管理者特権を使用して Visual Studio 2012 コマンドプロンプト内のサンプルのインストールフォルダーから、Setup.exe を実行します。 これにより、サンプルの実行に必要なすべての証明書がインストールされます。

    > [!NOTE]
    > セットアップの .bat バッチファイルは、Visual Studio 2012 のコマンドプロンプトから実行するように設計されています。 Visual Studio 2012 のコマンドプロンプト内で設定された PATH 環境変数は、セットアップの .bat スクリプトで必要な実行可能ファイルが格納されているディレクトリを指します。 サンプルの実行後は、Cleanup.bat を実行して証明書を削除してください。 他のセキュリティ サンプルでも同じ証明書を使用します。  
  
3. Service.exe を \service\bin ディレクトリで起動します。 サービスが準備完了であることを示していることを確認し、> \<を押してサービスを終了するためのプロンプトを表示します。  
  
4. Client.exe を \client\bin ディレクトリで起動するか、または Visual Studio で F5 を押して起動し、ビルドして実行します。 クライアント アクティビティがクライアントのコンソール アプリケーションに表示されます。  
  
5. クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
### <a name="to-run-the-sample-across-computers"></a>サンプルを複数のコンピューターで実行するには  
  
1. サンプルのクライアント部分をビルドする前に、`CallServiceCustomClientIdentity` メソッドで、Client.cs ファイルのサービスのエンドポイント アドレスの値を変更してください。 その後、サンプルをビルドします。  
  
2. サービス コンピューターにディレクトリを作成します。  
  
3. service\bin のサービス プログラム ファイルを、サービス コンピューターのディレクトリにコピーします。 Setup.bat ファイルと Cleanup.bat ファイルもサービス コンピューターにコピーします。  
  
4. クライアント コンピューターにクライアント バイナリ用のディレクトリを作成します。  
  
5. クライアント プログラム ファイルを、クライアント コンピューターに作成したクライアント ディレクトリにコピーします。 Setup.bat、Cleanup.bat、ImportServiceCert.bat の各ファイルもクライアントにコピーします。  
  
6. サービスで、管理者特権で開いた Visual Studio の開発者コマンドプロンプトで `setup.bat service` を実行します。 `service` 引数を指定して `setup.bat` を実行すると、コンピューターの完全修飾ドメイン名を使用してサービス証明書が作成され、service .cer という名前のファイルにエクスポートされます。  
  
7. Service.cer ファイルを、サービス ディレクトリからクライアント コンピューターのクライアント ディレクトリにコピーします。  
  
8. クライアント コンピューターの Client.exe.config ファイルで、エンドポイントのアドレス値をサービスの新しいアドレスに合わせます。 複数のインスタンスを変更する必要があります。  
  
9. クライアントで、管理者特権で開かれた Visual Studio の開発者コマンドプロンプトで Importservicecert.bat を実行します。 これにより、サービス証明書が Service.cer ファイルから CurrentUser - TrustedPeople ストアにインポートされます。  
  
10. サービス コンピューターで、コマンド プロンプトから Service.exe を起動します。  
  
11. クライアント コンピューターで、コマンド プロンプトから Client.exe を起動します。 クライアントとサービスが通信できない場合は、「 [WCF サンプルのトラブルシューティングのヒント](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90))」を参照してください。  
  
### <a name="to-clean-up-after-the-sample"></a>サンプルの実行後にクリーンアップするには  
  
- サンプルの実行が終わったら、サンプル フォルダーにある Cleanup.bat を実行します。  
  
    > [!NOTE]
    > このサンプルを複数のコンピューターで実行している場合、このスクリプトはサービス証明書をクライアントから削除しません。 コンピューター間で証明書を使用する Windows Communication Foundation (WCF) サンプルを実行した場合は、CurrentUser-TrustedPeople ストアにインストールされているサービス証明書を必ずオフにしてください。 削除するには、コマンド `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` を実行します。たとえば、`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` となります。
