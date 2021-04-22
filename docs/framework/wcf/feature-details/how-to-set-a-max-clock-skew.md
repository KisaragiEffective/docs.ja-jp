---
description: '詳細情報: 方法: 時刻のずれの最大値を設定する'
title: '方法: 時刻のずれの最大値を設定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MaxClockSkew property
- WCF, custom bindings
ms.assetid: 491d1705-eb29-43c2-a44c-c0cf996f74eb
ms.openlocfilehash: 688be1bb42dd7b30fce577082992741b76819e3e
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643203"
---
# <a name="how-to-set-a-max-clock-skew"></a>方法: 時刻のずれの最大値を設定する

時刻が重要な要素となる機能は、2 台のコンピューターで時刻の設定が異なっていると失敗する可能性があります。 この可能性を減らすには、`MaxClockSkew` プロパティを <xref:System.TimeSpan> に設定します。 このプロパティは、次の 2 つのクラスで使用できます。  
  
 <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>  
  
 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>  
  
> [!IMPORTANT]
> 通信のセキュリティを確保するため、`MaxClockSkew` プロパティに対する変更は、サービスまたはクライアントがブートストラップされたときに行う必要があります。 これを行うには、<xref:System.ServiceModel.Security.Tokens.SecureConversationSecurityTokenParameters.BootstrapSecurityBindingElement%2A?displayProperty=nameWithType> プロパティによって返された <xref:System.ServiceModel.Channels.SecurityBindingElement> に対してプロパティを設定する必要があります。  
  
 システム提供のバインディングの 1 つでこのプロパティを変更するには、バインディングのコレクションでセキュリティ バインド要素を見つけて、`MaxClockSkew` プロパティを新しい値に設定する必要があります。 <xref:System.ServiceModel.Channels.SecurityBindingElement> から派生される 2 つのクラスは、<xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> クラスおよび <xref:System.ServiceModel.Channels.AsymmetricSecurityBindingElement> クラスです。 コレクションからセキュリティ バインディングを取得する場合は、`MaxClockSkew` プロパティを正しく設定するために、これらの型のどちらかにキャストする必要があります。 次の例では、<xref:System.ServiceModel.WSHttpBinding> を使用していますが、これは <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> を使用します。 システム提供の各バインディングで使用するセキュリティ バインディングの型を示す一覧については、「[システム標準のバインディング](../system-provided-bindings.md)」を参照してください。  
  
## <a name="to-create-a-custom-binding-with-a-new-clock-skew-value-in-code"></a>コードを使用して時刻のずれの新しい値を持つカスタム バインドを作成するには  
  
> [!WARNING]
> コード内の次の名前空間に参照先を追加してください: <xref:System.ServiceModel.Channels>、<xref:System.ServiceModel.Description>、<xref:System.Security.Permissions>、および <xref:System.ServiceModel.Security.Tokens>。  
  
1. <xref:System.ServiceModel.WSHttpBinding> クラスのインスタンスを作成し、セキュリティ モードを <xref:System.ServiceModel.SecurityMode.Message?displayProperty=nameWithType> に設定します。  
  
2. <xref:System.ServiceModel.Channels.BindingElementCollection> メソッドを呼び出して、<xref:System.ServiceModel.WSHttpBinding.CreateBindingElements%2A> クラスの新しいインスタンスを作成します。  
  
3. <xref:System.ServiceModel.Channels.BindingElementCollection.Find%2A> クラスの <xref:System.ServiceModel.Channels.BindingElementCollection> メソッドを使用して、セキュリティ バインド要素を検索します。  
  
4. <xref:System.ServiceModel.Channels.BindingElementCollection.Find%2A> メソッドを使用する場合には、実際の型にキャストします。 次の例では <xref:System.ServiceModel.Channels.SymmetricSecurityBindingElement> 型にキャストしています。  
  
5. セキュリティ バインド要素の <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.MaxClockSkew%2A> プロパティを設定します。  
  
6. 適切なサービス型とベース アドレスを使用して、<xref:System.ServiceModel.ServiceHost> を作成します。  
  
7. <xref:System.ServiceModel.ServiceHost.AddServiceEndpoint%2A> メソッドを使用してエンドポイントを追加し、<xref:System.ServiceModel.Channels.CustomBinding> を含めます。  
  
     [!code-csharp[c_MaxClockSkew#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_maxclockskew/cs/source.cs#1)]
     [!code-vb[c_MaxClockSkew#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_maxclockskew/vb/source.vb#1)]  
  
## <a name="to-set-the-maxclockskew-in-configuration"></a>構成で MaxClockSkew を設定するには  
  
1. [\<bindings>](../../configure-apps/file-schema/wcf/bindings.md) 要素セクションに [\<customBinding>](../../configure-apps/file-schema/wcf/custombinding.md) を作成します。  
  
2. [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) 要素を作成し、`name` 属性を適切な値に設定します。 `MaxClockSkewBinding` に設定する方法の例を次に示します。  
  
3. エンコーディング要素を追加します。 次の例では、[\<textMessageEncoding>](../../configure-apps/file-schema/wcf/textmessageencoding.md) を追加します。  
  
4. [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) 要素を追加し、`authenticationMode` 属性を適切に設定します。 次の例では、この属性を `Kerberos` に設定して、サービスが Windows 認証を使用するように指定しています。  
  
5. [\<localServiceSettings>](../../configure-apps/file-schema/wcf/localservicesettings-element.md) を追加し、`maxClockSkew` 属性を `"##:##:##"` の書式の値に設定します。 次の例では 7 分に設定しています。 オプションとして、[\<localServiceSettings>](../../configure-apps/file-schema/wcf/localservicesettings-element.md) を追加し、`maxClockSkew` 属性を適切に設定します。  
  
6. transport 要素を追加します。 [\<httpTransport>](../../configure-apps/file-schema/wcf/httptransport.md) を使用する例を次に示します。  
  
7. セキュリティで保護されたメッセージ交換を行うために、セキュリティ設定は、[\<secureConversationBootstrap>](../../configure-apps/file-schema/wcf/secureconversationbootstrap.md) 要素のブートストラップ時に行う必要があります。  
  
    ```xml  
    <bindings>  
      <customBinding>  
        <binding name="MaxClockSkewBinding">  
            <textMessageEncoding />  
            <security authenticationMode="Kerberos">  
               <localClientSettings maxClockSkew="00:07:00" />  
               <localServiceSettings maxClockSkew="00:07:00" />  
               <secureConversationBootstrap>  
                  <localClientSettings maxClockSkew="00:30:00" />  
                  <localServiceSettings maxClockSkew="00:30:00" />  
               </secureConversationBootstrap>  
            </security>  
            <httpTransport />  
        </binding>  
      </customBinding>  
    </bindings>  
    ```  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Channels.LocalClientSecuritySettings>
- <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [方法: SecurityBindingElement を使用してカスタム バインドを作成する](how-to-create-a-custom-binding-using-the-securitybindingelement.md)
