---
title: AsyncWaitHandle の使用によるアプリケーション実行のブロック
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- blocks, asynchronous operations
- ending asynchronous operations
- asynchronous programming, ending operations
- asynchronous programming, blocking applications
- stopping asynchronous operations
- blocking application execution
ms.assetid: 3e32daf2-8161-4e8f-addd-9fd9ff101b03
ms.openlocfilehash: 16b5a297c13cd9096548ed489e4994b72a48da67
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73121434"
---
# <a name="blocking-application-execution-using-an-asyncwaithandle"></a>AsyncWaitHandle の使用によるアプリケーション実行のブロック
非同期操作の結果の待機中に、他の作業を継続できないアプリケーションは、操作が完了するまでブロックする必要があります。 次のオプションのいずれかを使用して、非同期操作が完了するまでの待機中に、アプリケーションのメイン スレッドをブロックします。  
  
- 非同期操作の <xref:System.IAsyncResult.AsyncWaitHandle%2A>Begin<xref:System.IAsyncResult>**OperationName** メソッドによって返される _の_ プロパティを使用します。 このトピックでは、この方法のデモが実行されます。  
  
- 非同期操作の **End**_OperationName_ メソッドを呼び出します。 この方法をデモの例については、「[非同期操作の終了によるアプリケーション実行のブロック](../../../docs/standard/asynchronous-programming-patterns/blocking-application-execution-by-ending-an-async-operation.md)」を参照してください。  
  
 非同期操作が完了するまでブロックする 1 つ以上の <xref:System.Threading.WaitHandle> オブジェクトを使用するアプリケーションは、通常は **Begin**_OperationName_ メソッドを呼び出し、操作の結果なしで実行できるすべての作業を実行し、非同期操作が完了するまでブロックします。 アプリケーションは、<xref:System.Threading.WaitHandle.WaitOne%2A> を使用して <xref:System.IAsyncResult.AsyncWaitHandle%2A> メソッドのいずれかを呼び出し、単一の操作上でブロックできます。 非同期操作のセットが完了するまで待機しながらブロックするには、関連する <xref:System.IAsyncResult.AsyncWaitHandle%2A> オブジェクトを配列に格納し、<xref:System.Threading.WaitHandle.WaitAll%2A> メソッドのいずれかを呼び出します。 非同期操作のいずれかのセットが完了するまで待機しながらブロックするには、関連する <xref:System.IAsyncResult.AsyncWaitHandle%2A> オブジェクトを配列に格納し、<xref:System.Threading.WaitHandle.WaitAny%2A> メソッドのいずれかを呼び出します。  
  
## <a name="example"></a>例  
 次のコード例は、ユーザー指定のコンピューターのドメイン ネーム システム情報を取得するために、DNS クラスの非同期メソッドを使用してデモを実行します。 例では、非同期操作に関連付けられた <xref:System.Threading.WaitHandle> を使用して、ブロックのデモを実行します。 この方法を使用する場合は必要ないため、`null` (Visual Basic の場合は `Nothing`) は、<xref:System.Net.Dns.BeginGetHostByName%2A>`requestCallback` と `stateObject` パラメーターに渡されることに注意してください。  
  
 [!code-csharp[AsyncDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/Async_EndBlockWait.cs#2)]
 [!code-vb[AsyncDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/Async_EndBlockWait.vb#2)]  
  
## <a name="see-also"></a>参照

- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
- [イベントベースの非同期パターンの概要](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)
