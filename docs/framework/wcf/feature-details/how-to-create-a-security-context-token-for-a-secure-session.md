---
title: '方法 : セキュリティで保護されたセッションに対しセキュリティ コンテキスト トークンを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 640676b6-c75a-4ff7-aea4-b1a1524d71b2
ms.openlocfilehash: 804161dfe4c2b5b397505f25231b3afccb5a6476
ms.sourcegitcommit: fbb8a593a511ce667992502a3ce6d8f65c594edf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/16/2019
ms.locfileid: "74141705"
---
# <a name="how-to-create-a-security-context-token-for-a-secure-session"></a>方法 : セキュリティで保護されたセッションに対しセキュリティ コンテキスト トークンを作成する
セキュリティで保護されたセッションでステートフルなセキュリティ コンテキスト トークン (SCT: Security Context Token) を使用すると、そのセッションでサービスを再利用できます。 たとえば、セキュリティで保護されたセッションでステートレスな SCT を使用しているときにインターネット インフォメーション サービス (IIS) をリセットすると、サービスに関連付けられているセッション データが失われます。 このセッション データには、SCT キャッシュが含まれています。 このため、クライアントが次回ステートレスな SCT をサービスに送信すると、エラーが返されます。これは、SCT に関連付けられているキーを取得できないためです。 しかし、ステートフルな SCT を使用した場合、SCT に関連付けられているキーは、その SCT 内に格納されます。 キーが SCT 内、つまりメッセージ内に格納されているため、セキュリティで保護されたセッションは、サービスの再使用の影響を受けません。 既定では、Windows Communication Foundation (WCF) は、セキュリティで保護されたセッションでステートレスな SCTs を使用します。 ここでは、セキュリティで保護されたセッションでステートフルな SCT を使用する方法について詳しく説明します。  
  
> [!NOTE]
> <xref:System.ServiceModel.Channels.IDuplexChannel> から派生したコントラクトに関係する、セキュリティで保護されたセッションでは、ステートフルな SCT を使用できません。  
  
> [!NOTE]
> セキュリティで保護されたセッションでステートフルな SCT を使用するアプリケーションでは、サービスのスレッド ID は、関連付けられたユーザー プロファイルを持つユーザー アカウントである必要があります。 ユーザー プロファイルを持たないアカウント (`Local Service` など) でサービスを実行すると、例外がスローされる場合があります。  
  
> [!NOTE]
> Windows XP で偽装が必要な場合は、ステートフルな SCT を使用しない、セキュリティで保護されたセッションを使用します。 ステートフル SCT が偽装と共に使用されると、<xref:System.InvalidOperationException> がスローされます。 詳細については、「サポートされ[ないシナリオ](../../../../docs/framework/wcf/feature-details/unsupported-scenarios.md)」を参照してください。  
  
### <a name="to-use-stateful-scts-in-a-secure-session"></a>セキュリティで保護されたセッションでステートフルな SCT を使用するには  
  
- ステートフルな SCT を使用する、セキュリティで保護されたセッションによって SOAP メッセージを保護するように指定するカスタム バインディングを作成します。  
  
    1. サービスの構成ファイルに[\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)を追加することによって、カスタムバインディングを定義します。  
  
        ```xml  
        <customBinding>  
        ```  
  
    2. [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)に子要素[\<バインド](../../configure-apps/file-schema/wcf/bindings.md)を追加します。  
  
         `name` 属性を、構成ファイル内で一意の名前に設定してバンディング名を指定します。  
  
        ```xml  
        <binding name="StatefulSCTSecureSession">  
        ```  
  
    3. [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)に[\<security >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md) child 要素を追加することによって、このサービスとの間で送受信されるメッセージの認証モードを指定します。  
  
         `authenticationMode` 属性を `SecureConversation` に設定して、セキュリティで保護されたセッションを指定します。 `requireSecurityContextCancellation` 属性を `false` に設定して、ステートフルな SCT を使用するように指定します。  
  
        ```xml  
        <security authenticationMode="SecureConversation"  
                  requireSecurityContextCancellation="false">  
        ```  
  
    4. セキュリティで保護されたセッションの確立時にクライアントが認証される方法を指定するには、 [\<secureConversationBootstrap >](../../../../docs/framework/configure-apps/file-schema/wcf/secureconversationbootstrap.md)子要素を[\<セキュリティ >](../../../../docs/framework/configure-apps/file-schema/wcf/security-of-custombinding.md)に追加します。  
  
         クライアントの認証方法は、`authenticationMode` 属性を設定して指定します。  
  
        ```xml  
        <secureConversationBootstrap authenticationMode="UserNameForCertificate" />  
        ```  
  
    5. [\<textMessageEncoding >](../../../../docs/framework/configure-apps/file-schema/wcf/textmessageencoding.md)などのエンコーディング要素を追加して、メッセージのエンコーディングを指定します。  
  
        ```xml  
        <textMessageEncoding />  
        ```  
  
    6. トランスポート要素 ( [\<httpTransport >](../../../../docs/framework/configure-apps/file-schema/wcf/httptransport.md)など) を追加してトランスポートを指定します。  
  
        ```xml  
        <httpTransport />  
        ```  
  
     次のコード例では、構成を使用して、セキュリティで保護されたセッションでメッセージがステートフルな SCT と共に使用できるカスタム バインディングを指定します。  
  
    ```xml  
    <customBinding>  
      <binding name="StatefulSCTSecureSession">  
        <security authenticationMode="SecureConversation"  
                  requireSecurityContextCancellation="false">  
          <secureConversationBootstrap authenticationMode="UserNameForCertificate" />  
        </security>  
        <textMessageEncoding />  
        <httpTransport />  
      </binding>  
    </customBinding>  
    ```  
  
## <a name="example"></a>例  
 セキュリティで保護されたセッションをブートストラップするための <xref:System.ServiceModel.Configuration.AuthenticationMode.MutualCertificate> 認証モードを使用する、カスタム バインドを作成するコード例を次に示します。  
  
 [!code-csharp[c_CreateStatefulSCT#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_createstatefulsct/cs/secureservice.cs#2)]
 [!code-vb[c_CreateStatefulSCT#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_createstatefulsct/vb/secureservice.vb#2)]  
  
 Windows 認証をステートフルな SCT と組み合わせて使用すると、WCF は <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> プロパティに実際の呼び出し元の id を設定しません。代わりに、プロパティを anonymous に設定します。 WCF セキュリティでは、受信した SCT からのすべての要求に対してサービスセキュリティコンテキストの内容を再作成する必要があるため、サーバーはメモリ内のセキュリティセッションを追跡しません。 また、<xref:System.Security.Principal.WindowsIdentity> インスタンスは SCT にシリアル化できないため、<xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> プロパティは匿名 ID を返します。  
  
 次の構成は、この動作を示します。  
  
```xml  
<customBinding>  
  <binding name="Cancellation">  
       <textMessageEncoding />  
        <security   
            requireSecurityContextCancellation="false">  
              <secureConversationBootstrap />  
      </security>  
    <httpTransport />  
  </binding>  
</customBinding>  
```  
  
## <a name="see-also"></a>関連項目

- [\<customBinding >](../../../../docs/framework/configure-apps/file-schema/wcf/custombinding.md)
