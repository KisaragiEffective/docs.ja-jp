---
title: WCF での承認
ms.date: 03/30/2017
helpviewer_keywords:
- authorization [WCF]
- security [WCF], authorization
ms.assetid: 8ea0b552-af65-45b0-a157-c6c111b8ce5e
ms.openlocfilehash: 0097adc3d9677d9ce5595a3ac632b51d94d53f6f
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964684"
---
# <a name="authorization-in-wcf"></a>WCF での承認
承認は、サービスやファイルなどのリソースへのアクセスと権限を制御するプロセスです。 このセクションのトピックでは、さまざまな方法で Windows Communication Foundation (WCF) でこの基本的なタスクを実行する方法について説明します。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [アクセス制御機構](../../../../docs/framework/wcf/feature-details/access-control-mechanisms.md)  
 WCF の承認メカニズムの概要と、推奨される使用方法について説明します。  
  
 [方法: PrincipalPermissionAttribute クラスでアクセスを制限する](../../../../docs/framework/wcf/how-to-restrict-access-with-the-principalpermissionattribute-class.md)  
 <xref:System.Security.Permissions.PrincipalPermissionAttribute> を使用してサービスへのアクセスを制限するプロセスを示します。  
  
 [方法 : ASP.NET のロール プロバイダーとサービスを使用する](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-role-provider-with-a-service.md)  
 サービスの構成に従って、ASP.NET のロールプロバイダー機能を使用できるようにします。  
  
 [方法 : ASP.NET の承認マネージャー ロール プロバイダーとサービスを使用する](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service.md)  
 ASP.NET は、承認マネージャーを使用して、Web サイトの承認を管理できます。 WCF では、クライアントの承認に ASP.NET/Authorization Manager の組み合わせを利用することもできます。  
  
 [ID モデルを使用したクレームと承認の管理](../../../../docs/framework/wcf/feature-details/managing-claims-and-authorization-with-the-identity-model.md)  
 ID モデル インフラストラクチャをクレーム ベースの承認に使用する際の基本について説明します。  
  
 [委任と偽装](../../../../docs/framework/wcf/feature-details/delegation-and-impersonation-with-wcf.md)  
 委任と偽装の違いについて説明します。  
  
## <a name="reference"></a>参照先  
 <xref:System.ServiceModel.Security>  
  
 <xref:System.ServiceModel.Description.PrincipalPermissionMode>  
  
 <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>  
  
 <xref:System.Security.Permissions.PrincipalPermissionAttribute>  
  
## <a name="related-sections"></a>関連セクション  
 [認証](../../../../docs/framework/wcf/feature-details/authentication-in-wcf.md)  
  
## <a name="see-also"></a>関連項目

- [セキュリティの概要](../../../../docs/framework/wcf/feature-details/security-overview.md)
- [Windows Server App Fabric のセキュリティモデル](https://docs.microsoft.com/previous-versions/appfabric/ee677202(v=azure.10))
