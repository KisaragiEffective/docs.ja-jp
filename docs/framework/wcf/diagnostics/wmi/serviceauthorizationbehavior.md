---
description: 詳細については、「ServiceAuthorizationBehavior」を参照してください。
title: ServiceAuthorizationBehavior
ms.date: 03/30/2017
ms.assetid: 77dad8e8-fea4-4d1c-b366-2f01a2a87f78
ms.openlocfilehash: dc398621103774a04934aa23ae3d7208f4389717
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99715588"
---
# <a name="serviceauthorizationbehavior"></a>ServiceAuthorizationBehavior

ServiceAuthorizationBehavior  
  
## <a name="syntax"></a>構文  
  
```csharp
class ServiceAuthorizationBehavior : Behavior  
{  
  boolean ImpersonateCallerForAllOperations;  
  string PrincipalPermissionMode;  
  string RoleProvider;  
  string ServiceAuthorizationManager;  
};  
```  
  
## <a name="methods"></a>メソッド  

 ServiceAuthorizationBehavior クラスは、メソッドを一切定義しません。  
  
## <a name="properties"></a>プロパティ  

 ServiceAuthorizationBehavior クラスには、次のプロパティがあります。  
  
### <a name="impersonatecallerforalloperations"></a>impersonateCallerForAllOperations  

 データ型 : boolean  
  
 アクセスの種類: 読み取り専用  
  
 受信メッセージによって提供される資格情報を使用してサービスが偽装を試みるかどうかを制御する値。  
  
### <a name="principalpermissionmode"></a>PrincipalPermissionMode  

 データ型: 文字列  
  
 アクセスの種類: 読み取り専用  
  
 サーバーでの操作を実行するために使用されるプリンシパル。  
  
### <a name="roleprovider"></a>RoleProvider  

 データ型: 文字列  
  
 アクセスの種類: 読み取り専用  
  
 ASP.NET ロール プロバイダーの名前。  
  
### <a name="serviceauthorizationmanager"></a>ServiceAuthorizationManager  

 データ型: 文字列  
  
 アクセスの種類: 読み取り専用  
  
 カスタム承認で使用される承認マネージャー。  
  
## <a name="requirements"></a>要件  
  
|MOF|Servicemodel.mof にて宣言済み。|  
|---------|-----------------------------------|  
|名前空間|root\ServiceModel で定義|  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior>
