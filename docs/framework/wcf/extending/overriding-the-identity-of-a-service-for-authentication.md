---
description: '詳細情報: 認証のためのサービスの ID をオーバーライドする'
title: 認証のためのサービスの ID のオーバーライド
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d613a22b-07d7-41a4-bada-1adc653b9b5d
ms.openlocfilehash: f83c10d75c4ea71170a76a7fd10efb33958f5b52
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644165"
---
# <a name="override-the-identity-of-a-service-for-authentication"></a>認証のためのサービスの ID をオーバーライドする

クライアント資格情報の種類を選択すると、サービス メタデータで公開される ID の種類が指定されるため、通常、サービスで ID を設定する必要はありません。 たとえば、次の構成コードでは、[\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) 要素を使用し、`clientCredentialType` 属性を Windows に設定しています。  

 次の Web サービス記述言語 (WSDL) コードは、定義済みのエンドポイントの ID を示しています。 この例では、サービスは、特定のユーザー アカウント (username@contoso.com) で自己ホスト型サービスとして実行されているため、ユーザー プリンシパル名 (UPN) ID にこのアカウント名が含まれています。 UPN は、Windows ドメインではユーザー サインイン名とも呼ばれます。  

 ID の設定を示すサンプル アプリケーションについては、「[サービス ID のサンプル](../samples/service-identity-sample.md)」を参照してください。 サービス ID の詳細については、「[サービス ID と認証](../feature-details/service-identity-and-authentication.md)」を参照してください。  
  
## <a name="kerberos-authentication-and-identity"></a>Kerberos 認証と ID  

 Windows 資格情報を使用するようにサービスを構成した場合、既定では、[\<userPrincipalName>](../../configure-apps/file-schema/wcf/userprincipalname.md) 要素または [\<servicePrincipalName>](../../configure-apps/file-schema/wcf/serviceprincipalname.md) 要素を含む [\<identity>](../../configure-apps/file-schema/wcf/identity.md) 要素が WSDL で生成されます。 サービスが `LocalSystem`、`LocalService`、`NetworkService` のいずれかのアカウントで実行されている場合、これらのアカウントはコンピューターのサービス プリンシパル名 (SPN) データにアクセスできるため、`host/`\<*hostname*> という形式のサービス プリンシパル名 (SPN) が既定で生成されます。 サービスが別のアカウントで実行されている場合、Windows Communication Foundation (WCF) は、\<*username*>@<*domainName*`>` という形式の UPN を生成します。 これらが生成されるのは、Kerberos 認証では、サービスを認証するために UPN または SPN をクライアントに提供する必要があるからです。  
  
 [Setspn](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731241(v=ws.10)) ツールを使用して、他の SPN をドメイン内のサービスのアカウントに登録することもできます。 登録した SPN は、そのサービスの ID として使用できます。 ツールの詳細については、「[Setspn の概要](/previous-versions/windows/it-pro/windows-server-2003/cc773257(v=ws.10))」を参照してください。  
  
> [!NOTE]
> ネゴシエーションを行わずに Windows 資格情報を使用するには、サービスのユーザー アカウントが Active Directory ドメインに登録された SPN にアクセスできる必要があります。 これは、次の方法で行うことができます。  
  
- サービスを実行するには、NetworkService アカウントまたは LocalSystem アカウントを使用します。 これらのアカウントは、コンピューターが Active Directory ドメインに参加したときに確立されたコンピューターの SPN にアクセスできるため、WCF は、適切な SPN 要素を、サービスのメタデータ (WSDL) にあるサービスのエンドポイント内部に自動的に生成します。  
  
- 任意の Active Directory ドメイン アカウントを使用してサービスを実行します。 この場合、そのドメイン アカウント用の SPN を確立します。これには、Setspn.exe ユーティリティ ツールを使用できます。 サービスのアカウント用の SPN を作成したら、SPN をそのメタデータ (WSDL) を通じてサービスのクライアントに公開するように WCF を構成します。 これを行うには、アプリケーション構成ファイルまたはコードを使用して、公開されるエンドポイントのエンドポイント ID を設定します。  
  
 SPN、Kerberos プロトコル、および Active Directory の詳細については、「[Windows に対する Kerberos のテクニカル サポート](/previous-versions/msp-n-p/ff649429(v=pandp.10))」を参照してください。  
  
### <a name="when-spn-or-upn-equals-the-empty-string"></a>SPN または UPN が空の文字列の場合  

 空の文字列の SPN または UPN を設定した場合は、使用しているセキュリティ レベルと認証モードに応じて、次のようになります。  
  
- トランスポート レベルのセキュリティを使用している場合は、NTLM (NT LanMan) 認証が選択されます。  
  
- メッセージ レベルのセキュリティを使用している場合は、認証モードによっては認証に失敗する可能性があります。  
  
- `spnego` モードを使用し、`AllowNtlm` 属性を `false` に設定している場合は、認証に失敗します。  
  
- `spnego` モードを使用し、`AllowNtlm` 属性を `true` に設定している場合、UPN が空の場合は認証に失敗しますが、SPN が空の場合は認証に成功します。  
  
- Kerberos ダイレクト ("ワンショット" とも呼ばれます) を使用している場合は、認証に失敗します。  
  
### <a name="use-the-identity-element-in-configuration"></a>構成で \<identity> 要素を使用する  

 上記の例で示したバインディングのクライアント資格情報の種類を `Certificate` に変更すると、次のコードに示すように、生成される WSDL には、Base64 でシリアル化された、ID 値用の X.509 証明書が含まれます。 これは、Windows 以外のすべてのクライアント資格情報の種類の既定値です。  

 構成で <`identity`> 要素を使用するか、コードで ID を設定することにより、既定のサービス ID の値を変更するか、ID の種類を変更することができます。 値 `contoso.com` を使用してドメイン ネーム システム (DNS) ID を設定する構成コードを次に示します。  

### <a name="set-identity-programmatically"></a>プログラムによって ID を設定する  

 ID は、WCF によって自動的に決定されるため、サービスで ID を明示的に指定する必要はありません。 ただし、WCF では、必要に応じてエンドポイントの ID を指定できます。 特定の DNS ID を持つ新しいサービス エンドポイントを追加するコードを次に示します。  
  
 [!code-csharp[C_Identity#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#5)]
 [!code-vb[C_Identity#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#5)]  
  
## <a name="see-also"></a>関連項目

- [方法: カスタム クライアント ID 検証機能を作成する](how-to-create-a-custom-client-identity-verifier.md)
- [サービス ID と認証](../feature-details/service-identity-and-authentication.md)
