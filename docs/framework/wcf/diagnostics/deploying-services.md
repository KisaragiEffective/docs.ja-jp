---
description: '詳細情報: サービスの配置'
title: サービスの配置
ms.date: 03/30/2017
ms.assetid: ac361bfb-017d-4da9-a2d7-fc0fb72d65bb
ms.openlocfilehash: 6a0b1d88410b865f57cd1eb16f070842a75ddb07
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99645998"
---
# <a name="deploying-services"></a>サービスの配置

ここでは、Windows Communication Foundation (WCF) アプリケーションをランタイム環境に配置する方法を説明します。  
  
## <a name="choosing-the-hosting-environment-for-your-application"></a>アプリケーションのホスト環境の選択  

 WCF サービスは、マネージド コードをサポートする任意の Windows プロセスで実行されるように設計されています。 アクティブにするには、サービスを作成してそのコンテキストと有効期間を制御するランタイム環境内で、サービスをホストする必要があります。 ホスティング環境の範囲は、最も単純なコンソール アプリケーション内、Windows サービスやインターネット インフォメーション サービス (IIS) のようなサーバー環境、あるいは Windows アクティブ化サービス (WAS) で管理されるワーカー プロセス内での実行にまで及びます。 WCF アプリケーションに対する各種のホスティング オプションについては、「[ホスティング サービス](../hosting-services.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [ホスティング](../feature-details/hosting.md)
- [ホスティング サービス](../hosting-services.md)
