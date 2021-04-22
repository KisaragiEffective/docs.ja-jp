---
description: '詳細情報: ClickOnce を使用して WCF アプリケーションを展開する'
title: ClickOnce を使用して WCF アプリケーションを展開する
ms.date: 03/30/2017
ms.assetid: 1a11feee-2a47-4d3e-a28a-ad69d5ff93e0
ms.openlocfilehash: f0a78bab6bbe7ddfd431e8e12bfe1ca9c9143143
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99646089"
---
# <a name="deploying-wcf-applications-with-clickonce"></a>ClickOnce を使用して WCF アプリケーションを展開する

Windows Communication Foundation (WCF) を使用しているクライアント アプリケーションは、ClickOnce テクノロジを使用して展開できます。 このテクノロジを使用すると、クライアント アプリケーションが、信頼できる証明書でデジタル署名されている場合、コード アクセス セキュリティによって提供されるランタイム セキュリティ保護を受けられます。 ClickOnce アプリケーションの署名に使用される証明書は、信頼された発行者のストアに存在する必要があり、またそのクライアント コンピューターのローカル セキュリティ ポリシーでは、発行者の証明書で署名されたアプリケーションに対して、完全信頼のアクセス許可が構成される必要があります。  
  
 ClickOnce アプリケーションの構成と信頼された発行者の詳細については、「[Configuring ClickOnce Trusted Publishers](/previous-versions/dotnet/articles/ms996418(v=msdn.10))」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [信頼されたアプリケーションの配置の概要](/visualstudio/deployment/trusted-application-deployment-overview)
- [Windows フォーム アプリケーションの ClickOnce 配置](/previous-versions/visualstudio/visual-studio-2008/wh45kb66(v=vs.90))
