---
title: 認証のためのサービスの ID のオーバーライド
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: d613a22b-07d7-41a4-bada-1adc653b9b5d
ms.openlocfilehash: d1c0cc487d8deea7045ee9653d1eae9b73ec864f
ms.sourcegitcommit: 7e2128d4a4c45b4274bea3b8e5760d4694569ca1
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/14/2020
ms.locfileid: "75938013"
---
# <a name="overriding-the-identity-of-a-service-for-authentication"></a>認証のためのサービスの ID のオーバーライド
クライアント資格情報の種類を選択すると、サービス メタデータで公開される ID の種類が指定されるため、通常、サービスで ID を設定する必要はありません。 たとえば、次の構成コードでは、 [\<wsHttpBinding >](../../configure-apps/file-schema/wcf/wshttpbinding.md)要素を使用して、`clientCredentialType` 属性を Windows に設定しています。  

 次の Web サービス記述言語 (WSDL) コードは、定義済みのエンドポイントの ID を示しています。 この例では、サービスが特定のユーザーアカウント (username@contoso.com) で自己ホスト型サービスとして実行されているため、ユーザープリンシパル名 (UPN) id にアカウント名が含まれています。 UPN は、Windows ドメインではユーザー ログオン名とも呼ばれます。  

 Id 設定を示すサンプルアプリケーションについては、「[サービス id のサンプル](../samples/service-identity-sample.md)」を参照してください。 サービス id の詳細については、「[サービス id と認証](../feature-details/service-identity-and-authentication.md)」を参照してください。  
  
## <a name="kerberos-authentication-and-identity"></a>Kerberos 認証と ID  
 既定では、サービスが Windows 資格情報を使用するように構成されている場合、 [\<userPrincipalName >](../../configure-apps/file-schema/wcf/userprincipalname.md)または[\<servicePrincipalName >](../../configure-apps/file-schema/wcf/serviceprincipalname.md)要素を含む[\<id >](../../configure-apps/file-schema/wcf/identity.md)要素が WSDL で生成されます。 サービスが `LocalSystem`、`LocalService`、または `NetworkService` アカウントで実行されている場合、これらのアカウントにはコンピューターの SPN データへのアクセス権があるため、既定では、サービスプリンシパル名 (SPN) が `host/`\<*hostname*> の形式で生成されます。 サービスが別のアカウントで実行されている場合、Windows Communication Foundation (WCF) は \<*username*>@<*domainName*`>`の形式で UPN を生成します。 これらが生成されるのは、Kerberos 認証では、サービスを認証するために UPN または SPN をクライアントに提供する必要があるからです。  
  
 Setspn.exe ツールを使用して、他の SPN をドメイン内のサービスのアカウントに登録することもできます。 登録した SPN は、そのサービスの ID として使用できます。 このツールをダウンロードするには、「 [Windows 2000 リソースキットツール: Setspn](https://go.microsoft.com/fwlink/?LinkId=91752)」を参照してください。 ツールの詳細については、「 [Setspn の概要](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc773257(v=ws.10))」を参照してください。  
  
> [!NOTE]
> ネゴシエーションを行わずに Windows 資格情報を使用するには、サービスのユーザー アカウントが Active Directory ドメインに登録された SPN にアクセスできる必要があります。 これは、次の方法で行うことができます。  
  
- サービスを実行するには、NetworkService アカウントまたは LocalSystem アカウントを使用します。 これらのアカウントは、コンピューターが Active Directory ドメインに参加したときに確立されたコンピューターの SPN にアクセスできるため、WCF はサービスのメタデータ (WSDL) でサービスのエンドポイント内の適切な SPN 要素を自動的に生成します。  
  
- 任意の Active Directory ドメイン アカウントを使用してサービスを実行します。 この場合、そのドメイン アカウント用の SPN を確立します。これには、Setspn.exe ユーティリティ ツールを使用できます。 サービスのアカウントの SPN を作成したら、その SPN をメタデータ (WSDL) を通じてサービスのクライアントに発行するように WCF を構成します。 これを行うには、アプリケーション構成ファイルまたはコードを使用して、公開されるエンドポイントのエンドポイント ID を設定します。  
  
 Spn、Kerberos プロトコル、および Active Directory の詳細については、「 [Windows 用の Kerberos テクニカル補完](https://docs.microsoft.com/previous-versions/msp-n-p/ff649429(v=pandp.10))」を参照してください。  
  
### <a name="when-spn-or-upn-equals-the-empty-string"></a>SPN または UPN が空の文字列の場合  
 空の文字列の SPN または UPN を設定した場合は、使用しているセキュリティ レベルと認証モードに応じて、次のようになります。  
  
- トランスポート レベルのセキュリティを使用している場合は、NTLM (NT LanMan) 認証が選択されます。  
  
- メッセージ レベルのセキュリティを使用している場合は、認証モードによっては認証に失敗する可能性があります。  
  
- `spnego` モードを使用し、`AllowNtlm` 属性を `false` に設定している場合は、認証に失敗します。  
  
- `spnego` モードを使用し、`AllowNtlm` 属性を `true` に設定している場合、UPN が空の場合は認証に失敗しますが、SPN が空の場合は認証に成功します。  
  
- Kerberos ダイレクト ("ワンショット" とも呼ばれます) を使用している場合は、認証に失敗します。  
  
### <a name="using-the-identity-element-in-configuration"></a>構成での \<identity > 要素の使用  
 前の例で示したバインディングのクライアント資格情報の種類を Certificate に変更すると、生成される WSDL には、Base64 でシリアル化された ID 値用の X.509 証明書が含まれます。コードを次に示します`,`。 これは、Windows 以外のすべてのクライアント資格情報の種類の既定値です。  

 既定のサービス id の値を変更することも、id の種類を変更することもできます。そのためには、構成で <`identity`> 要素を使用するか、コードで id を設定します。 値 `contoso.com` を使用してドメイン ネーム システム (DNS) ID を設定する構成コードを次に示します。  

### <a name="setting-identity-programmatically"></a>プログラムによる ID の設定  
 WCF によって自動的に決定されるため、サービスで id を明示的に指定する必要はありません。 ただし、WCF では、必要に応じてエンドポイントの id を指定できます。 特定の DNS ID を持つ新しいサービス エンドポイントを追加するコードを次に示します。  
  
 [!code-csharp[C_Identity#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_identity/cs/source.cs#5)]
 [!code-vb[C_Identity#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_identity/vb/source.vb#5)]  
  
## <a name="see-also"></a>関連項目

- [方法 : カスタム クライアント ID 検証機能を作成する](how-to-create-a-custom-client-identity-verifier.md)
- [サービス ID と認証](../feature-details/service-identity-and-authentication.md)
