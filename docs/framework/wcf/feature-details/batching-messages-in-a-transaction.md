---
description: '詳細情報: トランザクションに含まれるメッセージのバッチ処理'
title: トランザクションに含まれるメッセージのバッチ処理
ms.date: 03/30/2017
helpviewer_keywords:
- batching messages [WCF]
ms.assetid: 53305392-e82e-4e89-aedc-3efb6ebcd28c
ms.openlocfilehash: 0c84d87bc043f4ce1ae4d0a7674e862aa10011ac
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99643697"
---
# <a name="batching-messages-in-a-transaction"></a>トランザクションに含まれるメッセージのバッチ処理

キューに置かれたアプリケーションは、トランザクションを使用してメッセージの正確性および信頼性のある配信を確保します。 ただし、トランザクション操作には負荷がかかるため、メッセージのスループットが大幅に低下する可能性があります。 メッセージ スループットを向上する方法の 1 つは、アプリケーションにより、単一のトランザクション内で複数のメッセージを読み取って処理することです。 ただし、その場合、バッチ内のメッセージ数が増えるにつれて、トランザクションをロールバックしたときに必要な回復の作業量も増えるため、パフォーマンスと回復の間のトレードオフを考慮する必要があります。 したがって、トランザクションでのメッセージのバッチ処理とセッションの違いに注目することが重要です。 *セッション* は、単一のアプリケーションによって処理され、1 つの単位としてコミットされる関連メッセージのグループです。 セッションは、一般に、関連メッセージのグループをまとめて処理する必要がある場合に使用されます。 この例として、オンライン ショッピング Web サイトがあります。 "*バッチ*" は、関連性のない複数のメッセージを、メッセージ スループットを向上できる方法で処理するために使用されます。 セッションの詳細については、「[セッションでキューに置かれたメッセージのグループ化](grouping-queued-messages-in-a-session.md)」を参照してください。 バッチ内のメッセージもまた、単一のアプリケーションによって処理され、1 つの単位としてコミットされますが、バッチ内のメッセージが関連している必要はありません。 トランザクションでのメッセージのバッチ処理は、アプリケーションの実行方法を変更しない最適化です。  
  
## <a name="entering-batching-mode"></a>バッチ処理モードの開始  

 バッチ処理は、<xref:System.ServiceModel.Description.TransactedBatchingBehavior> エンドポイント動作によって制御されます。 このエンドポイント動作をサービス エンドポイントに追加すると、Windows Communication Foundation (WCF) はメッセージをトランザクション内でバッチ処理するように指示されます。 すべてのメッセージがトランザクションを必要とするわけではないので、バッチにはトランザクションを必要とするメッセージのみ、特に `TransactionScopeRequired` = `true` および `TransactionAutoComplete` = `true` とマークされている操作から送信されてきたメッセージのみを配置します。 サービス コントラクトのすべての操作が `TransactionScopeRequired` = `false` および `TransactionAutoComplete` = `false` とマークされている場合、バッチ処理モードには入りません。  
  
## <a name="committing-a-transaction"></a>トランザクションのコミット  

 バッチ トランザクションは、次の基準に基づいてコミットされます。  
  
- `MaxBatchSize`. <xref:System.ServiceModel.Description.TransactedBatchingBehavior> 動作のプロパティ。 このプロパティは、バッチに含められるメッセージの最大数を決定します。 この数に達すると、バッチがコミットされます。 この値は厳密に定められたものではないため、この数のメッセージを受信する前にバッチをコミットすることもできます。  
  
- `Transaction Timeout`. トランザクションのタイムアウトの 80% が経過すると、バッチがコミットされ、新しいバッチが作成されます。 つまり、トランザクションが完了するために指定された時間の残りが 20% 以下になると、バッチがコミットされます。  
  
- `TransactionScopeRequired`. メッセージのバッチを処理しているときに WCF では `TransactionScopeRequired` = `false` を持つメッセージが見つかると、バッチがコミットされ、`TransactionScopeRequired` = `true` および `TransactionAutoComplete` = `true` とマークされた最初のメッセージを受信したときに新しいバッチが再度開かれます。  
  
- キューのメッセージがなくなると、`MaxBatchSize` に達していない場合やトランザクションのタイムアウトの 80% が経過していない場合でも、現在のバッチがコミットされます。  
  
## <a name="leaving-batching-mode"></a>バッチ処理モードの終了  

 バッチ内のメッセージによってトランザクションが中止されたときの動作は、次のとおりです。  
  
1. メッセージのバッチ全体がロールバックされます。  
  
2. 読み取られたメッセージの数がバッチの最大サイズの 2 倍を超えるまで、一度に 1 つのメッセージが読み取られます。  
  
3. 再度、バッチ モードに入ります。  
  
## <a name="choosing-the-batch-size"></a>バッチ サイズの選択  

 バッチのサイズは、アプリケーションに依存します。 アプリケーションに最適なバッチ サイズは、経験に基づいて決定するのが最良の方法です。 バッチ サイズを選択する際には、アプリケーションの実際の展開モデルに応じたサイズを選択することが重要です。 たとえば、アプリケーションの展開で、リモート コンピューターに SQL サーバーを配置し、キューとその SQL サーバーにまたがるトランザクションを実行する必要がある場合は、そのとおりの構成を実行してバッチ サイズを決定するのが最良の方法です。  
  
## <a name="concurrency-and-batching"></a>コンカレンシーとバッチ処理  

 スループットを向上させるために、多数のバッチを同時に実行することもできます。 同時バッチ処理を有効にするには、`ConcurrencyMode.Multiple` の `ServiceBehaviorAttribute` を設定します。  
  
 *サービス調整* は、サービスで実行できる同時呼び出しの最大数を示すために使用されるサービス動作です。 この動作をバッチ処理で使用すると、実行できる同時バッチの数と解釈されます。 WCF では、サービス調整が設定されていない場合、同時呼び出しの最大数は既定で 16 になります。 したがって、バッチ動作が既定で追加されると、最大で 16 個のバッチが同時にアクティブになります。 容量に基づいてサービス調整とバッチ処理を調整することをお勧めします。 たとえば、キューに 100 個のメッセージがあり、20 個のメッセージを含むバッチを実行する必要がある場合、同時呼び出しの最大数を 16 に設定しても実用的ではありません。それは、スループットによっては、バッチ処理をオンにしていない場合と同様に、16 個のトランザクションがアクティブになる可能性があるからです。 したがって、パフォーマンスを微調整するためには、同時バッチ処理を設定しないか、正しいサービス調整のサイズで同時バッチ処理を設定します。  
  
## <a name="batching-and-multiple-endpoints"></a>バッチ処理と複数のエンドポイント  

 エンドポイントは、アドレスとコントラクトで構成されます。 同じバインディングを共有するエンドポイントが複数ある場合があります。 2 つのエンドポイントが同じバインディングと、リッスン URI (Uniform Resource Identifier) つまりキュー アドレスを共有することもできます。 2 つのエンドポイントが同じキューから読み取りを行っている場合、トランザクション バッチ動作を両方のエンドポイントに追加すると、指定したバッチ サイズ間で競合が発生する可能性があります。 この問題を解決するには、2 つのトランザクション バッチ動作の間に指定される最小バッチ サイズを使用してバッチを実装します。 このシナリオでは、どちらかのエンドポイントがトランザクション バッチを指定していない場合、両方のエンドポイントがバッチ処理を使用しません。  
  
## <a name="example"></a>例  

 構成ファイルで `TransactedBatchingBehavior` を指定する方法を次の例に示します。  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="TransactedBatchingBehavior"
              maxBatchSize="100" />
  </endpointBehaviors>
</behaviors>
```  
  
 コードで <xref:System.ServiceModel.Description.TransactedBatchingBehavior> を指定する方法を次の例に示します。  
  
```csharp
using (ServiceHost serviceHost = new ServiceHost(typeof(OrderProcessorService)))
{
     ServiceEndpoint sep = ServiceHost.AddServiceEndpoint(typeof(IOrderProcessor), new NetMsmqBinding(), "net.msmq://localhost/private/ServiceModelSamplesTransacted");
     sep.Behaviors.Add(new TransactedBatchingBehavior(100));

     // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();
  
    // The service can now be accessed.
    Console.WriteLine("The service is ready.");
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.WriteLine();
    Console.ReadLine();
  
    // Close the ServiceHostB to shut down the service.
    serviceHost.Close();
}  
```  
  
## <a name="see-also"></a>関連項目

- [キューの概要](queues-overview.md)
- [WCF でのキュー](queuing-in-wcf.md)
