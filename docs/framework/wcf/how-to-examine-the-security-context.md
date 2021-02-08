---
description: '詳細については、「方法: セキュリティコンテキストを調べる」を参照してください。'
title: '方法: セキュリティ コンテキストを調べる'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ServiceSecurityContext class
- WCF, security
- Claimset class
ms.assetid: 389b5a57-4175-4bc0-ada0-fc750d51149f
ms.openlocfilehash: e82491a877cf1f741767c91d1e7c304ba7676e6c
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99779401"
---
# <a name="how-to-examine-the-security-context"></a>方法: セキュリティ コンテキストを調べる

Windows Communication Foundation (WCF) サービスのプログラミングでは、サービスセキュリティコンテキストを使用して、サービスでの認証に使用されるクライアント資格情報と要求の詳細を確認できます。 これは、<xref:System.ServiceModel.ServiceSecurityContext> クラスのプロパティを使用することで可能になります。  
  
 たとえば、<xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> プロパティまたは <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> プロパティを使用すると、現在のクライアントの ID を取得できます。 クライアントが匿名であるかどうかを確認するには、<xref:System.ServiceModel.ServiceSecurityContext.IsAnonymous%2A> プロパティを使用します。  
  
 また、<xref:System.ServiceModel.ServiceSecurityContext.AuthorizationContext%2A> プロパティにあるクレームのコレクションを反復処理することによって、クライアントのためにどのようなクレームが作成されているのかを確認することもできます。  
  
### <a name="to-get-the-current-security-context"></a>現在のセキュリティ コンテキストを取得するには  
  
- 現在のセキュリティ コンテキストを取得するためには、静的プロパティ <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> にアクセスします。 参照から現在のコンテキストの任意のプロパティを調べます。  
  
### <a name="to-determine-the-identity-of-the-caller"></a>呼び出し元の ID を確認するには  
  
1. <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> プロパティおよび <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> プロパティの値を表示します。  
  
### <a name="to-parse-the-claims-of-a-caller"></a>呼び出し元のクレームを解析するには  
  
1. 現在の <xref:System.IdentityModel.Policy.AuthorizationContext> クラスを返します。 <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> プロパティを使用して、現在のサービス セキュリティ コンテキストを返し、次に `AuthorizationContext` プロパティを使用して <xref:System.ServiceModel.ServiceSecurityContext.AuthorizationContext%2A> を返します。  
  
2. <xref:System.IdentityModel.Claims.ClaimSet> クラスの <xref:System.IdentityModel.Policy.AuthorizationContext.ClaimSets%2A> プロパティによって返された <xref:System.IdentityModel.Policy.AuthorizationContext> オブジェクトのコレクションを解析します。  
  
## <a name="example"></a>例  

 次の例では、現在のセキュリティ コンテキストの <xref:System.ServiceModel.ServiceSecurityContext.WindowsIdentity%2A> プロパティおよび <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> プロパティの値、<xref:System.IdentityModel.Claims.Claim.ClaimType%2A> プロパティの値、クレームのリソース値、および現在のセキュリティ コンテキストのすべてのクレームの <xref:System.IdentityModel.Claims.Claim.Right%2A> プロパティを表示します。  
  
 [!code-csharp[c_PrincipalPermissionAttribute#4](../../../samples/snippets/csharp/VS_Snippets_CFX/c_principalpermissionattribute/cs/source.cs#4)]
 [!code-vb[c_PrincipalPermissionAttribute#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_principalpermissionattribute/vb/source.vb#4)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  

 コードでは、次の名前空間を使用します。  
  
- <xref:System>  
  
- <xref:System.ServiceModel>  
  
- <xref:System.IdentityModel.Policy>  
  
- <xref:System.IdentityModel.Claims>  
  
## <a name="see-also"></a>関連項目

- [サービスのセキュリティ保護](securing-services.md)
- [サービス ID と認証](./feature-details/service-identity-and-authentication.md)
