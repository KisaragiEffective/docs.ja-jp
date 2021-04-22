---
description: '詳細情報: 方法: 探索プロキシをテストする'
title: '方法: 探索プロキシをテストする'
ms.date: 03/30/2017
ms.assetid: d96e3fa2-3c42-4e5d-8244-2694081bdc32
ms.openlocfilehash: 32360fd1f3724f2a557182ce2e11d346ba5c959d
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643112"
---
# <a name="how-to-test-the-discovery-proxy"></a>方法: 探索プロキシをテストする

これは、探索プロキシの実装方法に関する 4 つのトピックのうちの 4 番目のトピックです。 前のトピック「[方法: 探索プロキシを使用してサービスを検索するクライアント アプリケーションを実装する](client-app-discovery-proxy-to-find-a-service.md)」では、探索プロキシを使用してサービスを検索し、そのサービスを呼び出す WCF クライアント アプリケーションを実装しました。 このトピックでは、探索プロキシ、サービス、およびクライアント アプリケーションが予期したとおりに動作することを確認する方法について説明します。  
  
### <a name="run-the-discovery-proxy"></a>探索プロキシの実行  
  
1. コマンド プロンプトを管理者として開きます。  
  
2. "このプログラムの機能のいくつかが Windows ファイアウォールでブロックされています" という内容のダイアログが表示される場合があります。 このメッセージが表示された場合は、 **[ブロックを解除する]** をクリックします。  
  
3. コマンド プロンプトから、探索プロキシ DiscoveryProxy.exe を実行します。  
  
4. 「`Proxy started. Hit Enter to exit`」というテキストが表示されます。  
  
### <a name="run-the-discoverable-service"></a>探索サービスの実行  
  
1. コマンド プロンプトを管理者として開きます。  
  
2. コマンド プロンプトから、探索サービス Service.exe を実行します。  
  
3. DiscoveryProxy.exe により、次のテキストが表示されます: `******* Adding the following service: ** [Service Contract Name] ** [Service Endpoint Addr] 3.******* Done *******`。  
  
### <a name="run-the-client-application"></a>クライアント アプリケーションの実行  
  
1. コマンド プロンプトを開きます。  
  
2. コマンド プロンプトから、client.exe アプリケーションを実行します。  
  
3. 数秒後に、クライアント アプリケーションにより「サービス エンドポイント に接続しています」というテキストが表示されます。  
  
4. 次に、service.exe により「Greeting request received, I will respond」というテキストが表示されます。  
  
5. その後、client.exe により「Hello Client!」というテキストが表示されます。  
  
### <a name="shut-down-the-applications"></a>アプリケーションのシャットダウン  
  
1. クライアント アプリケーションをシャットダウンします。  
  
2. サービスをシャットダウンします。 探索プロキシにより、次のテキストが表示されます。`******* Removing the following service: ** [Service Contract Name] ** [Service Endpoint Addr] 2.3.******* Done *******`  
  
3. 探索プロキシをシャットダウンします。  
  
## <a name="see-also"></a>関連項目

- [WCF Discovery の概要](wcf-discovery-overview.md)
- [方法: 探索プロキシを実装する](how-to-implement-a-discovery-proxy.md)
- [方法: 探索プロキシで登録される探索可能なサービスの実装する](discoverable-service-that-registers-with-the-discovery-proxy.md)
- [方法: 探索プロキシを使用してサービスを検索するクライアント アプリケーションを実装する](client-app-discovery-proxy-to-find-a-service.md)
