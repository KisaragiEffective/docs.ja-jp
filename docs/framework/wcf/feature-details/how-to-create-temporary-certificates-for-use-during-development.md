---
title: '方法 : 開発中に使用する一時的な証明書を作成する'
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], creating temporary certificates
- temporary certificates [WCF]
ms.assetid: bc5f6637-5513-4d27-99bb-51aad7741e4a
ms.openlocfilehash: 9e01ccb29ad017a2657ab08b54d7f01ef4564481
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964535"
---
# <a name="how-to-create-temporary-certificates-for-use-during-development"></a>方法 : 開発中に使用する一時的な証明書を作成する

Windows Communication Foundation (WCF) を使用してセキュリティで保護されたサービスまたはクライアントを開発する場合、資格情報として使用される x.509 証明書を指定することが必要になることがよくあります。 証明書は通常、単独ではなく、いくつもの証明書が信頼チェーンとしてつながった形で存在しており、その最上位に位置するルート証明機関の証明書は、各コンピューターの [信頼されたルート証明機関] の証明書ストアに格納されています。 証明書を調べて順に信頼チェーンをたどっていくと、たとえば所属する会社や事業部門が運営する、ルート証明機関に到達します。 開発時にこの過程をエミュレートするためには、セキュリティ要件を満たす 2 種類の証明書を作る必要があります。 1 つは自己署名証明書で、[信頼されたルート証明機関] の証明書ストアに配置します。もう 1 つは、先の自己署名証明書を使って署名を施した証明書で、[ローカル コンピューター] の [個人] ストア、または [現在のユーザー] の [個人] ストアに配置します。 このトピックでは、Powershell の[新しい SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)コマンドレットを使用して、これら2つの証明書を作成する手順について説明します。

> [!IMPORTANT]
> 新しい SelfSignedCertificate コマンドレットによって生成される証明書は、テスト目的でのみ提供されます。 実際にサービスやクライアントを業務に使用する際には、証明機関から取得した、適切な証明書が必要です。 これは、組織内の Windows Server 証明書サーバーまたはサードパーティのいずれかになります。
>
> 既定では、[新しい-SelfSignedCertificate](/powershell/module/pkiclient/new-selfsignedcertificate)コマンドレットは、自己署名の証明書を作成しますが、これらの証明書は安全ではありません。 自己署名証明書を信頼されたルート証明機関ストアに配置すると、展開環境をより厳密にシミュレートする開発環境を作成できます。

 証明書の作成と使用の詳細については、「[証明書の使用](working-with-certificates.md)使用」を参照してください。 証明書を資格情報として使用する方法の詳細については、「[サービスおよびクライアントのセキュリティ保護](securing-services-and-clients.md)」を参照してください。 Microsoft Authenticode テクノロジの使用方法については、「 [Authenticode Overviews and Tutorials (Authenticode の概要とチュートリアル)](https://docs.microsoft.com/previous-versions/windows/internet-explorer/ie-developer/platform-apis/ms537360(v=vs.85))」を参照してください。

## <a name="to-create-a-self-signed-root-authority-certificate-and-export-the-private-key"></a>自己署名ルート証明書を作成して秘密キーをエクスポートするには

次のコマンドは、現在のユーザーの個人用ストアでサブジェクト名が "RootCA" の自己署名証明書を作成します。

```powershell
$rootcert = New-SelfSignedCertificate -CertStoreLocation Cert:\CurrentUser\My -DnsName "RootCA" -TextExtension @("2.5.29.19={text}CA=true") -KeyUsage CertSign,CrlSign,DigitalSignature
```

証明書を PFX ファイルにエクスポートして、後の手順で必要な場所にインポートできるようにする必要があります。 証明書を秘密キーと共にエクスポートする場合は、パスワードを保護するためにパスワードが必要です。 パスワードを `SecureString` に保存し、 [get-pfxcertificate](/powershell/module/pkiclient/export-pfxcertificate)コマンドレットを使用して、関連付けられている秘密キーを持つ証明書を PFX ファイルにエクスポートします。 また、[証明書のエクスポート](/powershell/module/pkiclient/export-certificate)コマンドレットを使用して、公開証明書のみを CRT ファイルに保存します。

```powershell
[System.Security.SecureString]$rootcertPassword = ConvertTo-SecureString -String "password" -Force -AsPlainText
[String]$rootCertPath = Join-Path -Path 'cert:\CurrentUser\My\' -ChildPath "$($rootcert.Thumbprint)"
Export-PfxCertificate -Cert $rootCertPath -FilePath 'RootCA.pfx' -Password $rootcertPassword
Export-Certificate -Cert $rootCertPath -FilePath 'RootCA.crt'
```

## <a name="to-create-a-new-certificate-signed-by-a-root-authority-certificate"></a>ルート証明書によって署名された新しい証明書を作成するには

次のコマンドは、発行者の秘密キーを使用して、サブジェクト名が "SignedByRootCA" である `RootCA` によって署名された証明書を作成します。

```powershell
$testCert = New-SelfSignedCertificate -CertStoreLocation Cert:\LocalMachine\My -DnsName "SignedByRootCA" -KeyExportPolicy Exportable -KeyLength 2048 -KeyUsage DigitalSignature,KeyEncipherment -Signer $rootCert
```

同様に、秘密キーを含む署名入り証明書を PFX ファイルに保存し、公開キーだけを CRT ファイルに保存します。

```powershell
[String]$testCertPath = Join-Path -Path 'cert:\LocalMachine\My\' -ChildPath "$($testCert.Thumbprint)"
Export-PfxCertificate -Cert $testCertPath -FilePath testcert.pfx -Password $rootcertPassword
Export-Certificate -Cert $testCertPath -FilePath testcert.crt
```

## <a name="installing-a-certificate-in-the-trusted-root-certification-authorities-store"></a>信頼されたルート証明機関の証明書ストアに証明書をインストールする

作成された自己署名証明書は、[信頼されたルート証明機関] の証明書ストアにインストールできます。 この証明書で署名された証明書は、この時点で信頼できるものと見なされるようになります。 したがって、この証明書が不要になった場合は、直ちに証明書ストアから削除してください。 この証明書を削除すると、それを使って署名した証明書は認証されなくなります。 ルート証明機関の証明書は、必要に応じて一連の証明書を有効化する手段の 1 つにすぎません。 たとえばピアツーピア アプリケーションの場合、証明書が提示されれば相手の識別情報を信頼できるので、ルート証明機関は必要ないのが普通です。

### <a name="to-install-a-self-signed-certificate-in-the-trusted-root-certification-authorities"></a>自己署名証明書を信頼されたルート証明機関としてインストールするには

1. 証明書スナップインを開きます。 詳細については、「[方法: MMC スナップインを使用して証明書を参照する](how-to-view-certificates-with-the-mmc-snap-in.md)」を参照してください。

2. 証明書の格納先となる、 **[ローカル コンピューター]** または **[現在のユーザー]** のフォルダーを開きます。

3. **[信頼されたルート証明機関]** フォルダーを開きます。

4. **[証明書]** フォルダーを右クリックして、 **[すべてのタスク]** メニューの **[インポート]** を実行します。

5. 画面に表示されるウィザードの指示に従って、RootCA .pfx をストアにインポートします。

## <a name="using-certificates-with-wcf"></a>WCF での証明書の使用

一時的な証明書を設定したら、それを使用して、クライアント資格情報の種類として証明書を指定する WCF ソリューションを開発できます。 たとえば、次の XML 構成では、メッセージ セキュリティを設定して、クライアント資格情報の種類として証明書を指定しています。

### <a name="to-specify-a-certificate-as-the-client-credential-type"></a>クライアント資格情報の種類として証明書を指定するには

1. サービスの構成ファイルで、次の XML を使用して、セキュリティ モードをメッセージに、クライアント資格情報の種類を証明書に設定します。

    ```xml
    <bindings>
      <wsHttpBinding>
        <binding name="CertificateForClient">
          <security>
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    ```

2. クライアントの構成ファイルで、次の XML を使用して、証明書がユーザーのストアに存在することを指定します。また、SubjectName フィールドで値 "CohoWinery" を検索して見つけることができます。

    ```xml
    <behaviors>
      <endpointBehaviors>
        <behavior name="CertForClient">
          <clientCredentials>
            <clientCertificate findValue="CohoWinery" x509FindType="FindBySubjectName" />
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>
    ```

WCF での証明書の使用に関する詳細については、「 [Working with Certificates](working-with-certificates.md)」を参照してください。

## <a name="net-framework-security"></a>.NET Framework のセキュリティ

一時的なルート証明書は、不要になったら **[信頼されたルート証明機関]** フォルダーや **[個人]** フォルダーから確実に削除してください。削除するには、証明書を右クリックし、 **[削除]** をクリックします。

## <a name="see-also"></a>関連項目

- [証明書の使用](working-with-certificates.md)
- [方法 : MMC スナップインを使用して証明書を参照する](how-to-view-certificates-with-the-mmc-snap-in.md)
- [Securing Services and Clients](securing-services-and-clients.md)
