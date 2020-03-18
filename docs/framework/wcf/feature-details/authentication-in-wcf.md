---
title: WCF での認証
ms.date: 03/30/2017
helpviewer_keywords:
- authentication [WCF]
- security [WCF], authentication
ms.assetid: 9254d873-843d-4c6e-bea4-8184ac3e44f4
ms.openlocfilehash: abe7aff025207ad8bdf8657daba3584e6a1b2e7f
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964692"
---
# <a name="authentication-in-wcf"></a>WCF での認証
次のトピックでは、Windows 認証、x.509 証明書、ユーザー名とパスワードなどの認証を提供する Windows Communication Foundation (WCF) のさまざまなメカニズムについて説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [方法 : ASP.NET メンバーシップ プロバイダーを使用する](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-membership-provider.md)  
 ASP.NET の機能には、メンバーシップとロール プロバイダー、認証のためのユーザー名とパスワードの組み合わせを格納するデータベース、および承認のためのユーザー ロールがあります。 このトピックでは、WCF サービスが同じデータベースを使用してユーザーを認証および承認する方法について説明します。  
  
 [方法 : カスタム ユーザー名およびパスワード検証を使用する](../../../../docs/framework/wcf/feature-details/how-to-use-a-custom-user-name-and-password-validator.md)  
 カスタム ユーザー名およびパスワード検証を統合する方法を示します。  
  
 [サービス ID と認証](../../../../docs/framework/wcf/feature-details/service-identity-and-authentication.md)  
 追加の保護手段として、クライアントはサービスの予期される*id*を指定することによってサービスを認証できます。 予想される ID とサービスから返される ID が一致しない場合、認証は失敗します。  
  
 [セキュリティ ネゴシエーションとタイムアウト](../../../../docs/framework/wcf/feature-details/security-negotiation-and-timeouts.md)  
 <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings.NegotiationTimeout%2A> クラスの <xref:System.ServiceModel.Channels.LocalServiceSecuritySettings> プロパティの使用方法について説明します。  
  
 [Windows 認証エラーのデバッグ](../../../../docs/framework/wcf/feature-details/debugging-windows-authentication-errors.md)  
 Windows 認証の使用時に発生する一般的な問題について重点的に説明します。  
  
## <a name="reference"></a>参照先  
 <xref:System.ServiceModel>  
  
## <a name="related-sections"></a>関連セクション  
 [一般的なセキュリティ シナリオ](../../../../docs/framework/wcf/feature-details/common-security-scenarios.md)  
  
## <a name="see-also"></a>関連項目

- [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
