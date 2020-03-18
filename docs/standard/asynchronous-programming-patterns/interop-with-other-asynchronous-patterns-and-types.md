---
title: 他の非同期パターンと型との相互運用
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- .NET Framework, and TAP
- asynchronous design patterns, .NET Framework
- TAP, .NET Framework support for
- Task-based Asynchronous Pattern, .NET Framework support for
- .NET Framework, asynchronous design patterns
ms.assetid: f120a5d9-933b-4d1d-acb6-f034a57c3749
ms.openlocfilehash: 981c13c68eaf1eb0c19f95eb1b097935ea02a16d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159755"
---
# <a name="interop-with-other-asynchronous-patterns-and-types"></a>他の非同期パターンと型との相互運用
.NET Framework 1.0 では、 <xref:System.IAsyncResult> とも、 [非同期プログラミング モデル (APM)](../../../docs/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm.md)パターンとも呼ばれる `Begin/End` パターンが導入されました。  .NET Framework 2.0 では、 [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)が追加されました。  .NET Framework 4 以降では、 [タスク ベースの非同期パターン (TAP)](../../../docs/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md) が APM および EAP よりも優先されますが、以前のパターンからの移行ルーチンを簡単にビルドする機能が提供されます。  
  
 このトピックの内容:  
  
- [タスクおよび APM](#APM) (「[APM から TAP へ](#ApmToTap) 」または「 [TAP から APM へ](#TapToApm)」)  
  
- [タスクおよび EAP](#EAP)  
  
- [タスクおよび待機ハンドル](#WaitHandles) (「[待機ハンドルから TAP へ](#WHToTap) 」または「 [TAP から待機ハンドルへ](#TapToWH)」)  
  
<a name="APM"></a>
## <a name="tasks-and-the-asynchronous-programming-model-apm"></a>タスクおよび非同期プログラミング モデル (APM)  
  
<a name="ApmToTap"></a>
### <a name="from-apm-to-tap"></a>APM から TAP へ  
 [非同期プログラミング モデル (APM)](../../../docs/standard/asynchronous-programming-patterns/asynchronous-programming-model-apm.md) パターンは高度に構造化されているため、ラッパーをビルドして APM 実装を TAP 実装として公開するのはとても簡単です。 実際、.NET Framework では、.NET Framework 4 以降で、この変換を行うために <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> メソッドのオーバーロードという形でヘルパー ルーチンが用意されています。  
  
 <xref:System.IO.Stream> クラスと、そのクラスの <xref:System.IO.Stream.BeginRead%2A> および <xref:System.IO.Stream.EndRead%2A> メソッドについて考えてみましょう。これらのメソッドは、同期 <xref:System.IO.Stream.Read%2A> メソッドに対応する APM 側のメソッドです。  
  
 [!code-csharp[Conceptual.AsyncInterop#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#1)]
 [!code-vb[Conceptual.AsyncInterop#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#1)]  
[!code-csharp[Conceptual.AsyncInterop#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#2)]
[!code-vb[Conceptual.AsyncInterop#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#2)]  
[!code-csharp[Conceptual.AsyncInterop#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Stream1.cs#3)]
[!code-vb[Conceptual.AsyncInterop#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/stream1.vb#3)]  
  
 <xref:System.Threading.Tasks.TaskFactory%601.FromAsync%2A?displayProperty=nameWithType> メソッドを次のように使用すると、この操作に対する TAP ラッパーを実装できます。  
  
 [!code-csharp[Conceptual.AsyncInterop#4](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wrap1.cs#4)]
 [!code-vb[Conceptual.AsyncInterop#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wrap1.vb#4)]  
  
 この実装は次のようになります。  
  
 [!code-csharp[Conceptual.AsyncInterop#5](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wrap2.cs#5)]
 [!code-vb[Conceptual.AsyncInterop#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wrap2.vb#5)]  
  
<a name="TapToApm"></a>
### <a name="from-tap-to-apm"></a>TAP から APM へ  
 既存のインフラストラクチャで APM パターンを使用することを想定している場合は、TAP の実装を用意し、APM の実装を想定している場所でその TAP の実装を使用します。  タスクは合成することができ、 <xref:System.Threading.Tasks.Task> クラスが <xref:System.IAsyncResult>を実装するため、これは単純なヘルパー関数を使用して実現することができます。 次のコードでは、 <xref:System.Threading.Tasks.Task%601> クラスの拡張を使用しますが、ほぼ同じ関数を非ジェネリック タスクに使用できます。  
  
 [!code-csharp[Conceptual.AsyncInterop#6](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM1.cs#6)]
 [!code-vb[Conceptual.AsyncInterop#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM1.vb#6)]  
  
 ここで、TAP 実装がある次のような例を考えてみます。  
  
 [!code-csharp[Conceptual.AsyncInterop#7](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#7)]
 [!code-vb[Conceptual.AsyncInterop#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#7)]  
  
 そして、次の APM 実装を使用します。  
  
 [!code-csharp[Conceptual.AsyncInterop#8](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#8)]
 [!code-vb[Conceptual.AsyncInterop#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#8)]  
[!code-csharp[Conceptual.AsyncInterop#9](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#9)]
[!code-vb[Conceptual.AsyncInterop#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#9)]  
  
 次のコードは、APM への移行の一例です。  
  
 [!code-csharp[Conceptual.AsyncInterop#10](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/APM2.cs#10)]
 [!code-vb[Conceptual.AsyncInterop#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/APM2.vb#10)]  
  
<a name="EAP"></a>
## <a name="tasks-and-the-event-based-asynchronous-pattern-eap"></a>タスクとイベント ベースの非同期パターン (EAP)  
 [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md) 実装をラップする手順は、APM パターンをラップするより複雑です。EAP パターンのほうが APM パターンよりもバリエーションが多く、構造化の程度が低いためです。  例を示すために、次のコードでは `DownloadStringAsync` メソッドをラップしています。  `DownloadStringAsync` は、URI を受け付け、進行状況として複数の統計情報をレポートするためにダウンロード中に `DownloadProgressChanged` イベントを発生させ、完了時に `DownloadStringCompleted` イベントを発生させます。  最終結果は、指定された URI にあるページのコンテンツを含む文字列です。  
  
 [!code-csharp[Conceptual.AsyncInterop#11](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/EAP1.cs#11)]
 [!code-vb[Conceptual.AsyncInterop#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/EAP1.vb#11)]  
  
<a name="WaitHandles"></a>
## <a name="tasks-and-wait-handles"></a>タスクおよび待機ハンドル  
  
<a name="WHToTap"></a>
### <a name="from-wait-handles-to-tap"></a>待機ハンドルから TAP へ  
 待機ハンドルは非同期パターンを実装しませんが、上級開発者は、待機ハンドルが設定されるときに非同期の通知を行うため、 <xref:System.Threading.WaitHandle> クラスおよび <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A?displayProperty=nameWithType> メソッドを使用できます。  <xref:System.Threading.ThreadPool.RegisterWaitForSingleObject%2A> メソッドをラップすると、待機ハンドルのすべての同期待機をタスク ベースの待機にできます。  
  
 [!code-csharp[Conceptual.AsyncInterop#12](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wait1.cs#12)]
 [!code-vb[Conceptual.AsyncInterop#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wait1.vb#12)]  
  
 このメソッドにより、非同期メソッドで既存の <xref:System.Threading.WaitHandle> 実装を使用できます。  たとえば、特定の時点に実行する非同期操作の数を絞り込む場合は、セマフォ (<xref:System.Threading.SemaphoreSlim?displayProperty=nameWithType> オブジェクト) を利用できます。  セマフォのカウントを *N* に初期化し、操作を実行するときには常にセマフォを待機し、操作完了時にセマフォを解放することで、同時実行される操作の数を *N*に絞り込むことができます。  
  
 [!code-csharp[Conceptual.AsyncInterop#13](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Semaphore1.cs#13)]
 [!code-vb[Conceptual.AsyncInterop#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Semaphore1.vb#13)]  
  
 また、待機ハンドルに依存せず、代わりにタスクで完全に動作する非同期セマフォもビルドできます。 これを行うには、 [T:System.Threading.Tasks.Task](../../../docs/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md) の上位にデータ構造をビルドするという「 <xref:System.Threading.Tasks.Task>が追加されました。  
  
<a name="TapToWH"></a>
### <a name="from-tap-to-wait-handles"></a>TAP から待機ハンドルへ  
 前述のとおり、 <xref:System.Threading.Tasks.Task> クラスは、 <xref:System.IAsyncResult>を実装し、その実装は <xref:System.Threading.Tasks.Task.System%23IAsyncResult%23AsyncWaitHandle%2A> プロパティを公開します。このプロパティは、 <xref:System.Threading.Tasks.Task> が完了したときに設定される待機ハンドルを返します。  <xref:System.Threading.WaitHandle> の <xref:System.Threading.Tasks.Task> は次のように取得できます。  
  
 [!code-csharp[Conceptual.AsyncInterop#14](../../../samples/snippets/csharp/VS_Snippets_CLR/Conceptual.AsyncInterop/cs/Wait1.cs#14)]
 [!code-vb[Conceptual.AsyncInterop#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Conceptual.AsyncInterop/vb/Wait1.vb#14)]  
  
## <a name="see-also"></a>参照

- [タスク ベースの非同期パターン (TAP)](../../../docs/standard/asynchronous-programming-patterns/task-based-asynchronous-pattern-tap.md)
- [タスク ベースの非同期パターンの実装](../../../docs/standard/asynchronous-programming-patterns/implementing-the-task-based-asynchronous-pattern.md)
- [タスク ベースの非同期パターンの利用](../../../docs/standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)
