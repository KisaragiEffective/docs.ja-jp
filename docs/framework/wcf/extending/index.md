---
description: '詳細情報: WCF の拡張'
title: WCF の拡張
ms.date: 03/30/2017
helpviewer_keywords:
- WCF, extensibility
- extensibility [WCF]
- Windows Communication Foundation, extensibility
ms.assetid: c145e2f6-f402-41f5-8b5a-eee03978737b
ms.openlocfilehash: e243e03a72e6530716959499c311bfe37a39b054
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99644152"
---
# <a name="extending-wcf"></a>WCF の拡張

Windows Communication Foundation (WCF) では、実行時コンポーネントを変更し、拡張することによって、サービス ベースのアプリケーションを正確に制御および拡張できます。 このセクションのトピックでは、その拡張アーキテクチャについて詳しく説明します。 基本的なプログラミングの詳細については、「[基本的な WCF プログラミング](../basic-wcf-programming.md)」を参照してください。  
  
## <a name="in-this-section"></a>このセクションの内容  

 [ServiceHost とサービス モデル レイヤーの拡張](extending-servicehost-and-the-service-model-layer.md)  
 サービス モデル レイヤーには、基になるチャネルから受信メッセージを取得し、そのメッセージをアプリケーション コードでのメソッド呼び出しに変換し、結果を呼び出し元に送信するという役割があります。  サービス モデル拡張は、ディスパッチャーの機能、カスタム動作、メッセージとパラメーターの途中受信、およびその他の拡張機能に関連する実行や通信の動作と機能を変更または実装します。  
  
 [バインディングの拡張](extending-bindings.md)  
 バインディングはエンドポイントに接続するために必要な通信の詳細設定を記述するオブジェクトです。 バインディングの拡張やカスタム バインドは、アプリケーションの各種機能をサポートするために必要なカスタム通信機能を実装します。  
  
 [チャネル レイヤーの拡張](extending-the-channel-layer.md)  
 チャネル レイヤーは、サービス モデル レイヤーより下に位置し、クライアントとサービス間のメッセージの交換を担います。 チャネル拡張は、セキュリティなどの新しいプロトコル機能を実装できます。 また、SOAP メッセージを伝達する新しいネットワーク トランスポートの実装など、トランスポート機能も実装できます。  
  
 [セキュリティの拡張](extending-security.md)  
 WCF のセキュリティは、転送セキュリティ (整合性、機密性、および認証)、アクセス制御 (承認)、および監査で構成されます。 `IdentityModel` 名前空間にある各クラスは、WCF でアクセス制御のために使用されます。 セキュリティ アーキテクチャを理解することによって、カスタムのアクセス制御システムに対応したカスタムのクレーム タイプを作成できます。  
  
 [メタデータ システムの拡張](extending-the-metadata-system.md)  
 WCF のメタデータ システムは複数のクラスのグループおよびインターフェイスで、サービス ベースのアプリケーションを実装するために必要なメタデータを表します。 クラスを変更または拡張するか、WSDL (Web サービス記述言語) の拡張子やカスタム WS-PolicyAttachments アサーションなどのカスタム メタデータをエクスポート/インポートするインターフェイスを実装して構成します。  
  
 [エンコーダーとシリアライザーの拡張](extending-encoders-and-serializers.md)  
 エンコーダーとシリアライザーは、データをある形式から別の形式に変換します。 このセクションのトピックでは、提供されたクラスを特別な要件に合わせて拡張する方法を説明します。  
  
## <a name="reference"></a>関連項目  

 <xref:System.ServiceModel>  
  
 <xref:System.ServiceModel.Channels>  
  
 <xref:System.ServiceModel.Description>  
  
 <xref:System.IdentityModel.Claims>  
  
 <xref:System.IdentityModel.Policy>  
  
 <xref:System.IdentityModel.Selectors>  
  
 <xref:System.IdentityModel.Tokens>  
  
## <a name="related-sections"></a>関連項目  

 [基本的な WCF プログラミング](../basic-wcf-programming.md)  
  
 [WCF 機能の詳細](../feature-details/index.md)  
  
 [ガイドラインと最適な使用方法](../guidelines-and-best-practices.md)
