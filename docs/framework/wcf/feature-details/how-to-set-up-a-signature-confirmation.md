---
description: '詳細情報: 方法: 署名確認を設定する'
title: '方法: 署名確認を設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- signature confirmation
- WCF, security
ms.assetid: 2424c137-c7c2-4aa9-8d5d-a066e12fefda
ms.openlocfilehash: 158ec2a5f74038f5c1ca1af847f57457a8881974
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643190"
---
# <a name="how-to-set-up-a-signature-confirmation"></a>方法: 署名確認を設定する

*署名確認* は、メッセージのイニシエーターが受信した応答が、送信者の元のメッセージへの応答として生成されたものであることを確認するメカニズムです。 署名確認は、WS-Security 1.1 仕様で定義されています。 エンドポイントが WS-Security 1.0 をサポートしている場合は、署名確認を使用できません。

以下の手順では、<xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> を使用して署名確認を有効にする方法を示します。 <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> でも同じ手順を使用できます。 この手順は、「[方法: SecurityBindingElement を使用してカスタム バインドを作成する](how-to-create-a-custom-binding-using-the-securitybindingelement.md)」に記載されている基本的な手順に基づいています。

### <a name="to-enable-signature-confirmation-in-code"></a>コードを使用して署名確認を有効にするには

1. <xref:System.ServiceModel.Channels.BindingElementCollection> クラスのインスタンスを作成します。

2. <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> クラスのインスタンスを作成します。

3. <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.RequireSignatureConfirmation%2A> を `true` に設定します。

4. バインディング コレクションにセキュリティ要素を追加します。

5. 「[方法: SecurityBindingElement を使用してカスタム バインドを作成する](how-to-create-a-custom-binding-using-the-securitybindingelement.md)」の記載に従って、カスタム バインディングを作成します。

### <a name="to-enable-signature-confirmation-in-configuration"></a>構成を使用して署名確認を有効にするには

1. `<customBinding>` 要素を構成ファイルの `<bindings>` セクションに追加します。

2. `<binding>` 要素を追加し、名前属性を適切な値に設定します。

3. 適切なエンコード要素を追加します。 次の例では `<TextMessageEncoding>` 要素を追加しています。

4. `<security>` 子要素を追加し、`requireSignatureConfirmation` 属性を `true` に設定します。

5. 任意。 ブートストラップ中に署名確認を有効にするには、[\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 子要素を追加し、`requireSignatureConfirmation` 属性を `true` に設定します。

6. 適切なトランスポート要素を追加します。 次の例では、[\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) を追加します。

    ```xml
    <bindings>
      <customBinding>
        <binding name="SignatureConfirmationBinding">
          <security requireSignatureConfirmation="true">
            <secureConversationBootstrap requireSignatureConfirmation="true" />
              </security>
           <textMessageEncoding />
             <httpTransport />
        </binding>
      </customBinding>
    </bindings>
    ```

## <a name="example"></a>例

次のコードでは、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> のインスタンスを作成し、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement.RequireSignatureConfirmation%2A> プロパティを `true` に設定しています。 この例では、前の例で示した `<secureConversationBootstrap>` 要素は使用していません。 この例は、Windows (Kerberos プロトコル) トークンを使用した場合の署名確認を示しています。 この場合、クライアントの署名はサービスからのすべての応答で返され、クライアントによって確認されます。

[!code-csharp[c_SignatureConfirmation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_signatureconfirmation/cs/source.cs#1)]
[!code-vb[c_SignatureConfirmation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_signatureconfirmation/vb/source.vb#1)]

## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement>
- <xref:System.ServiceModel.Channels.SecurityBindingElement.CreateMutualCertificateBindingElement%2A>
- [方法: SecurityBindingElement を使用してカスタム バインドを作成する](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
- [方法: 指定した認証モード用の SecurityBindingElement を作成する](how-to-create-a-securitybindingelement-for-a-specified-authentication-mode.md)
