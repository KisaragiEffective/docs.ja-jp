---
title: '方法 : ASP.NET の承認マネージャー ロール プロバイダーとサービスを使用する'
ms.date: 03/30/2017
ms.assetid: f21deb81-91ef-49ef-94d6-494785143271
ms.openlocfilehash: 20955578ce4d344c2057036c0944557edf737389
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76212222"
---
# <a name="how-to-use-the-aspnet-authorization-manager-role-provider-with-a-service"></a>方法 : ASP.NET の承認マネージャー ロール プロバイダーとサービスを使用する
ASP.NET が Web サービスをホストする場合、承認マネージャーをアプリケーションに統合して、サービスに承認を提供できます。 承認マネージャーを使用して、アプリケーション開発者は個々の操作を定義できます。また、個々の操作をグループ化してタスクを形成できます。 次に管理者は、ロールを承認して特定のタスクまたは個々の操作を実行できます。 承認マネージャーでは、ロール、タスク、操作、ユーザーを管理する管理ツールとして Microsoft 管理コンソール (MMC) スナップインが提供されます。 管理者は、承認マネージャーのポリシー ストアを XML ファイル、Active Directory、または Active Directory アプリケーション モード (ADAM) ストアに構成します。  
  
 承認マネージャーは、Web サービスをホストする ASP.NET アプリケーションの承認マネージャー ASP.NET ロールプロバイダーを構成することによって、アプリケーションに統合されます。 他の ASP.NET ロールプロバイダーと同様に、Authorization Manager ASP.NET ロールプロバイダーは <`providers`> 要素を使用して構成されます。  
  
 承認マネージャーをアプリケーションに統合する Web サービスの構成ファイルの一部を示すコード例を次に示します。  
  
```xml  
<system.web>  
    <roleManager enabled="true" defaultProvider="AzManRoleProvider">  
      <providers>  
        <add name="AzManRoleProvider"  
             type="System.Web.Security.AuthorizationStoreRoleProvider, System.Web, Version=2.0.0.0, Culture=neutral, publicKeyToken=b03f5f7f11d50a3a"  
             connectionStringName="AzManPolicyStoreConnectionString"   
             applicationName="SecureService"/>  
      </providers>  
    </roleManager>  
</system.web>  
```  
  
 ASP.NET ロールプロバイダーと WCF アプリケーションの統合の詳細については、「[方法: ASP.NET Role プロバイダーをサービスで使用](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-role-provider-with-a-service.md)する」を参照してください。 ASP.NET で承認マネージャーを使用する方法の詳細については、「[方法: 承認マネージャー (AzMan) と ASP.NET 2.0 を使用](https://docs.microsoft.com/previous-versions/msp-n-p/ff649313(v=pandp.10))する」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [方法 : ASP.NET のロール プロバイダーとサービスを使用する](../../../../docs/framework/wcf/feature-details/how-to-use-the-aspnet-role-provider-with-a-service.md)
