---
description: '詳細情報: AJAX の統合と JSON のサポート'
title: AJAX の統合と JSON のサポート
ms.date: 03/30/2017
helpviewer_keywords:
- AJAX integration and JSON support [WCF]
ms.assetid: 3851a8fc-d861-4ac1-873c-96af0343d3a7
ms.openlocfilehash: 13e52a3013e9c04f57d6303caf18fd23a41ebf25
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643892"
---
# <a name="ajax-integration-and-json-support"></a>AJAX の統合と JSON のサポート

Windows Communication Foundation (WCF) が ASP.NET AJAX (Asynchronous JavaScript and XML) と JSON (JavaScript Object Notation) データ形式をサポートするため、WCF サービスは AJAX クライアントに操作を公開できます。 AJAX クライアントは、JavaScript コードを実行し、HTTP 要求を使用してこのような WCF サービスにアクセスする Web ページです。 このセクションのトピックでは、このサポートと実装方法について説明します。  
  
 ASP.NET AJAX とその ASP.NET 2.0 との統合について詳しくは、「[ASP.NET AJAX の概要](/previous-versions/aspnet/bb398874(v=vs.100))」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  

 [ASP.NET AJAX 用の WCF サービスの作成](creating-wcf-services-for-aspnet-ajax.md)  
 構成によって、または、自動的に AJAX エンドポイントを構成するサービス ホストを生成するようにカスタマイズされたサービス ホスト ファクトリを使用することによって適切な AJAX エンドポイントを追加することで、WCF サービスを AJAX クライアントに公開する方法について説明します。  
  
 [ASP.NET を使用せずに WCF AJAX サービスを作成する方法](creating-wcf-ajax-services-without-aspnet.md)  
 ASP.NET を使用しないで WCF サービスを作成する方法について説明します。  
  
 [JSON などのデータ転送形式のサポート](support-for-json-and-other-data-transfer-formats.md)  
 ASP.NET AJAX サービスとのメッセージングで、XML の代わりに一般に使用される JSON 形式のサポートについて説明します。  
  
 [方法: AJAX 対応 ASP.NET Web サービスを WCF に移行する](how-to-migrate-ajax-enabled-aspnet-web-services-to-wcf.md)  
 AJAX 対応の ASP.NET Web サービスを WCF Web サービスに移行する方法について説明します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ServiceModel.Activation.WebScriptServiceHostFactory>
- [WCF Web HTTP プログラミング モデル](wcf-web-http-programming-model.md)
