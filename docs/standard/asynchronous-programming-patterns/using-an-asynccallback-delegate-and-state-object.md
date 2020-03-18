---
title: AsyncCallback デリゲートおよび状態オブジェクトの使用
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- asynchronous programming, delegates
- AsyncCallback delegate, samples
- asynchronous programming, state objects
- IAsyncResult interface, samples
ms.assetid: e3e5475d-c5e9-43f0-928e-d18df8ca1f1d
ms.openlocfilehash: c7bd0a7606b5f93289cf39d33794457265e7e453
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73094602"
---
# <a name="using-an-asynccallback-delegate-and-state-object"></a>AsyncCallback デリゲートおよび状態オブジェクトの使用
<xref:System.AsyncCallback> デリゲートを使用し、別個のスレッドで非同期操作の結果を処理するとき、状態オブジェクトを使用してコールバック間で情報を渡し、最終的な結果を取得できます。 このトピックでは、「[AsyncCallback デリゲートの使用による非同期操作の終了](../../../docs/standard/asynchronous-programming-patterns/using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md)」の例をさらに展開することでそれを実践します。  
  
## <a name="example"></a>例  
 次のコード例は、ユーザー指定のコンピューターのドメイン ネーム システム (DNS) 情報を取得するために、<xref:System.Net.Dns> クラスの非同期メソッドを使用してデモを実行します。 この例では、`HostRequest` クラスを定義して使用し、状態情報を格納します。 `HostRequest` オブジェクトは、ユーザーがコンピューター名を入力するたびに作成されます。 このオブジェクトは <xref:System.Net.Dns.BeginGetHostByName%2A> メソッドに渡されます。 `ProcessDnsInformation` メソッドは、要求が完了するたびに呼び出されます。 `HostRequest` オブジェクトは <xref:System.IAsyncResult.AsyncState%2A> プロパティを利用して取得されます。 `ProcessDnsInformation` メソッドは `HostRequest` オブジェクトを使用し、要求により返された <xref:System.Net.IPHostEntry> か、スローされた <xref:System.Net.Sockets.SocketException> を格納します。 要求がすべて完了すると、アプリケーションは `HostRequest` オブジェクトを反復処理し、DNS 情報か <xref:System.Net.Sockets.SocketException> エラー メッセージを表示します。  
  
 [!code-csharp[AsyncDesignPattern#5](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/AsyncDelegateWithStateObject.cs#5)]
 [!code-vb[AsyncDesignPattern#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/AsyncDelegateWithStateObject.vb#5)]  
  
## <a name="see-also"></a>参照

- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
- [イベントベースの非同期パターンの概要](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)
- [AsyncCallback デリゲートの使用による非同期操作の終了](../../../docs/standard/asynchronous-programming-patterns/using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md)
