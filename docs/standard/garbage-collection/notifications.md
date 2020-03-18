---
title: ガベージ コレクションの通知
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- garbage collection, notifications
ms.assetid: e12d8e74-31e3-4035-a87d-f3e66f0a9b89
ms.openlocfilehash: d5646c4969c95350ab4cd63b16f6f99ffba3a4ec
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73131531"
---
# <a name="garbage-collection-notifications"></a>ガベージ コレクションの通知
共通言語ランタイムによるフル ガベージ コレクション (つまり、ジェネレーション 2 のコレクション) がパフォーマンスに悪影響を及ぼす場合があります。 これは、特に要求を大量に処理するサーバーで問題となることがあります。この例では、長時間のガベージ コレクションによって、要求のタイムアウトが発生します。重要な期間にフル ガベージ コレクションが発生しないようにするには、フル ガベージ コレクションが近づいていることの通知を受けたら、ワークロードを別のサーバー インスタンスにリダイレクトするアクションを取ります。 現在のサーバー インスタンスで要求を処理する必要がない場合、自分でコレクションを強制実行することもできます。  
  
 ランタイムがフル ガーベッジ コレクションが近づいていることを検出すると、<xref:System.GC.RegisterForFullGCNotification%2A> メソッドによって通知の発生が登録されます。 この通知には、フル ガベージ コレクションが近づいている場合と、フル ガベージ コレクションが完了した場合の 2 つがあります。  
  
> [!WARNING]
> 通知は、ガベージ コレクションをブロックすることによってのみ発生します。 [\<gcConcurrent>](../../../docs/framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) 構成要素が有効になっている場合、バック グラウンドのガベージ コレクションでは、通知は発生しません。  
  
 通知が発生したことを判断するには、<xref:System.GC.WaitForFullGCApproach%2A> と <xref:System.GC.WaitForFullGCComplete%2A> メソッドを使用します。 通常、これらのメソッドは、通知の状態を示す <xref:System.GCNotificationStatus> 列挙体を継続的に取得する `while` ループで使用します。 その値が <xref:System.GCNotificationStatus.Succeeded> の場合、次を行うことができます。  
  
- <xref:System.GC.WaitForFullGCApproach%2A> メソッドで取得した通知を受け、作業負荷をリダイレクトします。ガベージ コレクションを手動で強制的に実行することもできます。  
  
- <xref:System.GC.WaitForFullGCComplete%2A> メソッドで取得した通知を受け、現在のサーバー インスタンスで再度要求を処理できるようにします。 情報を収集することもできます。 たとえば、<xref:System.GC.CollectionCount%2A> メソッドを使用して、コレクションの数を記録することもできます。  
  
 <xref:System.GC.WaitForFullGCApproach%2A> と <xref:System.GC.WaitForFullGCComplete%2A> メソッドは、連携して動作するように設計されています。 片方のみを使用する場合、結果が不正確になる場合があります。  
  
## <a name="full-garbage-collection"></a>フル ガベージ コレクション  
 ランタイムでは、次のいずれかのシナリオに当てはまる場合にフル ガベージ コレクションを実行します。  
  
- 十分なメモリがジェネレーション 2 に昇格し、次のジェネレーション 2 のガベージ コレクションが発生する場合。  
  
- 十分なメモリが、大きいオブジェクトのヒープに昇格し、次のジェネレーション 2 のガベージ コレクションが発生する場合。  
  
- その他の要因により、ジェネレーション 1 のガベージ コレクションがジェネレーション 2 のガベージ コレクションにエスカレートする場合。  
  
 <xref:System.GC.RegisterForFullGCNotification%2A> メソッドに指定したしきい値は、最初の 2 つのシナリオに該当します。 ただし、最初のシナリオでは、以下の 2 つの理由から、指定したしきい値に比例したタイミングで常に通知を受け取るとは限りません。  
  
- ランタイムは、パフォーマンス上の理由から、小さなオブジェクトの割り当てをチェックしません。  
  
- メモリがジェネレーション 2 に移動されるのは、ジェネレーション 1 のガベージ コレクションが実行された場合だけです。  
  
 3 番目のシナリオによっても、通知がいつあるかが不確実になります。 保証されるわけではないものの、この間要求をリダイレクトしたり、都合の良いときに自分でガベージ コレクションを強制的に実行したりして、タイミングの悪いフル ガベージ コレクションの影響を緩和することは有効な方法です。  
  
## <a name="notification-threshold-parameters"></a>通知しきい値パラメーター  
 <xref:System.GC.RegisterForFullGCNotification%2A> メソッドには、ジェネレーション 2 のオブジェクトと大きなオブジェクトのヒープのしきい値を指定するパラメーターが 2 つあります。 これらの値を超えると、ガベージ コレクションの通知が発行されます。 次の表で、これらのパラメーターについて説明します。  
  
|パラメーター|説明|  
|---------------|-----------------|  
|`maxGenerationThreshold`|ジェネレーション 2 に昇格したオブジェクト数に基づいて通知を発行するタイミングを指定する、1 ～ 99 の数値。|  
|`largeObjectHeapThreshold`|大きなオブジェクトのヒープに割り当てられたオブジェクト数に基づいて通知を発行するタイミングを指定する、1 ～ 99 の数値。|  
  
 指定した値が高すぎる場合、通知を受け取る可能性は高くなりますが、ランタイムでコレクションが発生するまでに、長い時間がかかりります。 コレクションを自分で強制実行する場合、ランタイムによってコレクションが解放されるときよりも、より多くのオブジェクトを回収できます。  
  
 指定した値が小さすぎる場合、通知を受け取るのに十分な時間がない前に、ランタイムによってコレクションが実行されるおそれがあります。  
  
## <a name="example"></a>例  
  
### <a name="description"></a>説明  
 次の例では、サーバーのグループが受信した Web 要求を処理します。 要求を処理するワークロードのシミュレートのため、バイト配列が <xref:System.Collections.Generic.List%601> コレクションに追加されています。 各サーバーはガベージ コレクションの通知を登録し、`WaitForFullGCProc`ユーザー メソッドでスレッドを開始して、<xref:System.GC.WaitForFullGCApproach%2A> メソッドと <xref:System.GC.WaitForFullGCComplete%2A> メソッドで返される <xref:System.GCNotificationStatus> 列挙型を継続的に監視します。  
  
 通知が発生すると、<xref:System.GC.WaitForFullGCApproach%2A> および <xref:System.GC.WaitForFullGCComplete%2A> メソッドによって、それぞれのイベント処理ユーザー メソッドが呼び出されます。  
  
- `OnFullGCApproachNotify`  
  
     このメソッドはユーザー メソッド `RedirectRequests`を呼び出し、要求をキューに格納するサーバーに対して、このサーバーへの要求の送信を一時停止するよう指示します。 これは、これ以上オブジェクトが割り当てられないように、クラスレベルの変数 `bAllocate` を `false` に設定することによってシミュレートできます。  
  
     次に、`FinishExistingRequests` ユーザー メソッドが呼び出され、保留中のサーバー要求の処理が完了します。 これは、<xref:System.Collections.Generic.List%601> コレクションをクリアすることによってシミュレートできます。  
  
     最後に、作業負荷が軽いため、ガベージ コレクションが強制的に実行されます。  
  
- `OnFullGCCompleteNotify`  
  
     サーバーがフル ガベージ コレクションの影響を受ける可能性が低くなったため、ユーザー メソッド `AcceptRequests` を呼び出し、要求の受付を再開します。 このアクションは、オブジェクトが <xref:System.Collections.Generic.List%601> コレクションに追加開始されるよう `bAllocate` 変数を `true` に設定することによってシミュレートできます。  
  
 次のコードには、この例の `Main` メソッドを含んでいます。  
  
 [!code-cpp[GCNotification#2](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#2)]
 [!code-csharp[GCNotification#2](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#2)]
 [!code-vb[GCNotification#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#2)]  
  
 次のコードは、ユーザー メソッド `WaitForFullGCProc` のコードであり、ガベージ コレクションの通知を継続的にチェックする while ループが含まれています。  
  
 [!code-cpp[GCNotification#8](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#8)]
 [!code-csharp[GCNotification#8](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#8)]
 [!code-vb[GCNotification#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#8)]  
  
 次のコードには、次から呼び出される `OnFullGCApproachNotify` メソッドを含んでいます。  
  
 `WaitForFullGCProc` メソッド  
  
 [!code-cpp[GCNotification#5](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#5)]
 [!code-csharp[GCNotification#5](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#5)]
 [!code-vb[GCNotification#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#5)]  
  
 次のコードには、次から呼び出される `OnFullGCApproachComplete` メソッドを含んでいます。  
  
 `WaitForFullGCProc` メソッド  
  
 [!code-cpp[GCNotification#6](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#6)]
 [!code-csharp[GCNotification#6](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#6)]
 [!code-vb[GCNotification#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#6)]  
  
 次のコードには、`OnFullGCApproachNotify` および `OnFullGCCompleteNotify` メソッドから呼び出されるユーザー メソッドを含んでいます。 このユーザー メソッドは、要求のリダイレクト、既存の要求の終了、フル ガベージ コレクションの実行後の要求の受け付け再開を実行します。  
  
 [!code-cpp[GCNotification#9](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#9)]
 [!code-csharp[GCNotification#9](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#9)]
 [!code-vb[GCNotification#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#9)]  
  
 コード全体のサンプルは次のとおりです。  
  
 [!code-cpp[GCNotification#1](../../../samples/snippets/cpp/VS_Snippets_CLR/GCNotification/cpp/program.cpp#1)]
 [!code-csharp[GCNotification#1](../../../samples/snippets/csharp/VS_Snippets_CLR/GCNotification/cs/Program.cs#1)]
 [!code-vb[GCNotification#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/GCNotification/vb/program.vb#1)]  
  
## <a name="see-also"></a>関連項目

- [ガベージ コレクション](../../../docs/standard/garbage-collection/index.md)
