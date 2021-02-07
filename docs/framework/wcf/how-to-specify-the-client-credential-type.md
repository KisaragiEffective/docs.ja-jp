---
description: '詳細については、「方法: クライアント資格情報の種類を指定する」を参照してください。'
title: '方法: クライアントの資格情報の種類を指定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- security credentials, adding to SOAP messages
- WCF, security
ms.assetid: 10f51bee-5f92-4c1a-9126-fa5418535d8f
ms.openlocfilehash: f6536778e8814a1b5a4c03e22c0fe4108f89b6eb
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99752536"
---
# <a name="how-to-specify-the-client-credential-type"></a>方法: クライアントの資格情報の種類を指定する

セキュリティ モード (トランスポートまたはメッセージ) を設定してから、クライアント資格情報の種類を指定することができます。 このプロパティでは、クライアントが認証時にサービスに提供する必要のある資格情報の種類を指定します。 セキュリティモード (クライアント資格情報の種類を設定する前に必要な手順) の設定の詳細については、「 [方法: セキュリティモードを設定する](how-to-set-the-security-mode.md)」を参照してください。  
  
### <a name="to-set-the-client-credential-type-in-code"></a>コードでクライアント資格情報の種類を設定するには  
  
1. サービスで使用されるバインドのインスタンスを作成します。 この例では、<xref:System.ServiceModel.WSHttpBinding> バインドを使用します。  
  
2. <xref:System.ServiceModel.WSHttpSecurity.Mode%2A> プロパティに適切な値を設定します。 この例では、メッセージ モードを使用します。  
  
3. <xref:System.ServiceModel.MessageSecurityOverHttp.ClientCredentialType%2A> プロパティに適切な値を設定します。 この例では、Windows 認証 (<xref:System.ServiceModel.MessageCredentialType.Windows>) を使用するように設定します。  
  
     [!code-csharp[c_ProgrammingSecurity#14](../../../samples/snippets/csharp/VS_Snippets_CFX/c_programmingsecurity/cs/source.cs#14)]
     [!code-vb[c_ProgrammingSecurity#14](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_programmingsecurity/vb/source.vb#14)]  
  
### <a name="to-set-the-client-credential-type-in-configuration"></a>構成でクライアント資格情報の種類を設定するには  
  
1. [\<system.serviceModel>](../configure-apps/file-schema/wcf/system-servicemodel.md)構成ファイルに要素を追加します。  
  
2. 子要素として、要素を追加し [\<bindings>](../configure-apps/file-schema/wcf/bindings.md) ます。  
  
3. 適切なバインドを追加します。 この例では、要素を使用 [\<wsHttpBinding>](../configure-apps/file-schema/wcf/wshttpbinding.md) します。  
  
4. 要素を追加 [\<binding>](../configure-apps/file-schema/wcf/bindings.md) し、 `name` 属性を適切な値に設定します。 この例では、"SecureBinding" という名前を使用します。  
  
5. `<security>` バインドを追加します。 `mode` 属性を適切な値に設定します。 この例では、`"Message"` に設定します。  
  
6. セキュリティ モードによって、`<message>` 要素と `<transport>` 要素のどちらかを追加します。 `clientCredentialType` 属性を適切な値に設定します。 この例では、`"Windows"` を使用します。  
  
    ```xml  
    <system.serviceModel>  
      <bindings>  
        <wsHttpBinding>  
          <binding name="SecureBinding">  
            <security mode="Message">  
                 <message clientCredentialType="Windows" />  
             </security>  
          </binding>  
        </wsHttpBinding>  
      </bindings>  
    </system.serviceModel>  
    ```  
  
## <a name="see-also"></a>関連項目

- [サービスのセキュリティ保護](securing-services.md)
- [方法: セキュリティ モードを設定する](how-to-set-the-security-mode.md)
