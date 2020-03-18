---
title: フェデレーションと発行済みトークン
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, federation
- issued tokens [WCF]
- federation [WCF], issued tokens
ms.assetid: 4c31ee7d-a820-4067-8b84-a83049021bb6
ms.openlocfilehash: d566388279f9210f70ebdb5c42512aea0425a47e
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964593"
---
# <a name="federation-and-issued-tokens"></a>フェデレーションと発行済みトークン
Windows Communication Foundation (WCF) を使用すると、WS-FEDERATION および WS-TRUST 仕様を実装するサービスと安全に通信するクライアントを作成できます。 これらの仕様では、異なる信頼領域間での認証と承認を可能にする機構を提供するために、XML、SOAP、および Web サービス記述言語 (WSDL: Web Services Description Language) が使用されます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [フェデレーション](../../../../docs/framework/wcf/feature-details/federation.md)  
 フェデレーションの概要を説明します。  
  
 [フェデレーションと信頼](../../../../docs/framework/wcf/feature-details/federation-and-trust.md)  
 フェデレーション サービスまたはクライアントを作成するときに注意する必要がある設計上の問題を示します。  
  
 [方法 : フェデレーション クライアントを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-federated-client.md)  
 WCF を使用したフェデレーションクライアントの作成の基本について説明します。  
  
 [方法 : フェデレーション サービスで資格情報を設定する](../../../../docs/framework/wcf/feature-details/how-to-configure-credentials-on-a-federation-service.md)  
 フェデレーション サービスを作成する手順を説明します。  
  
 [方法 : WSFederationHttpBinding を作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-wsfederationhttpbinding.md)  
 `WSFederationHttpBinding` を使用するクライアントおよびサービスを構成する方法を説明します。  
  
 [方法 : セキュリティ トークン サービスを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-security-token-service.md)  
 セキュリティ トークン サービスを作成する手順を説明します。  
  
 [SAML (Security Assertions Markup Language) トークンとクレーム](../../../../docs/framework/wcf/feature-details/saml-tokens-and-claims.md)  
 多様なクレームの種類を作成できる拡張可能な SAML (Security Assertions Markup Language) トークンについて説明します。  
  
 [方法 : ローカル発行者を設定する](../../../../docs/framework/wcf/feature-details/how-to-configure-a-local-issuer.md)  
 セキュリティ トークンのローカル発行者の作成方法を説明します。  
  
 [方法 : WSFederationHttpBinding のセキュリティで保護されたセッションを無効にする](../../../../docs/framework/wcf/feature-details/how-to-disable-secure-sessions-on-a-wsfederationhttpbinding.md)  
 `WSFederationHttpBinding` のセキュリティで保護されたセッションを無効にする方法を説明します。 クライアントごとにセッションが必要になる Web ファームを作成する場合には、セキュリティで保護されたセッションを無効化する必要があります。  
  
## <a name="reference"></a>参照先  
 <xref:System.IdentityModel.Claims>  
  
 <xref:System.ServiceModel.ServiceAuthorizationManager>  
  
 <xref:System.IdentityModel.Tokens.SamlSecurityToken>  
  
 <xref:System.ServiceModel.Security.IssuedTokenClientCredential>  
  
 <xref:System.ServiceModel.Security.IssuedTokenServiceCredential>  
  
 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenParameters>  
  
 <xref:System.ServiceModel.Security.Tokens.IssuedSecurityTokenProvider>  
  
 <xref:System.ServiceModel.WSFederationHttpBinding>  
  
## <a name="see-also"></a>関連項目

- [承認](../../../../docs/framework/wcf/feature-details/authorization-in-wcf.md)
- [カスタム トークン](../../../../docs/framework/wcf/extending/custom-tokens.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
