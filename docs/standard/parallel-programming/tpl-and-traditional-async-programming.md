---
title: TPL と従来の .NET Framework 非同期プログラミング
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- tasks, with other asynchronous models
ms.assetid: e7b31170-a156-433f-9f26-b1fc7cd1776f
ms.openlocfilehash: 27766c10d0624b5eda8256a3211662036a1b16b3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73139951"
---
# <a name="tpl-and-traditional-net-framework-asynchronous-programming"></a>TPL と従来の .NET Framework 非同期プログラミング
.NET Framework の I/O バインドの非同期操作および計算主体の非同期操作には、次の 2 つの標準パターンがあります。  
  
- 非同期操作が Begin/End メソッドのペアによって表される非同期プログラミング モデル (APM)。<xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType>、<xref:System.IO.Stream.EndRead%2A?displayProperty=nameWithType> など。  
  
- イベントベースの非同期パターン (EAP)。たとえば *や* など、*OperationName*Async および <xref:System.Net.WebClient.DownloadStringAsync%2A?displayProperty=nameWithType>OperationName<xref:System.Net.WebClient.DownloadStringCompleted?displayProperty=nameWithType>Completed という名前のメソッドとイベントのペアによって非同期操作が表されています。 (EAP は、.NET Framework Version 2.0 で導入されました)。  
  
 タスク並列ライブラリ (TPL) は、これらの非同期パターンと共にさまざまな方法で使用されます。 ライブラリ使用時に、APM 操作と EAP 操作をタスクとして公開するか、または APM パターンを公開し、タスク オブジェクトを使用して内部的に実装することができます。 どちらの場合でも、タスク オブジェクトを使用することでコードを簡易化し、次のような便利な機能を活用できます。  
  
- タスクの継続の形で、タスク開始後に随時コールバックを登録する。  
  
- `Begin_` および <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> メソッド、または <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> メソッドまたは <xref:System.Threading.Tasks.Task.WaitAll%2A> メソッドを使用し、<xref:System.Threading.Tasks.Task.WaitAny%2A> メソッドに対する応答として実行される複数の操作を調整する。  
  
- 非同期 I/O バインド操作と計算主体の操作を、同じタスク オブジェクトにカプセル化する。  
  
- タスク オブジェクトの状態を監視する。  
  
- <xref:System.Threading.Tasks.TaskCompletionSource%601> を使用し、タスク オブジェクトへの操作の状態をマーシャリングする。  
  
## <a name="wrapping-apm-operations-in-a-task"></a>APM 操作のタスクへのラッピング  
 <xref:System.Threading.Tasks.TaskFactory?displayProperty=nameWithType> クラスと <xref:System.Threading.Tasks.TaskFactory%601?displayProperty=nameWithType> クラスには、1 つの <xref:System.Threading.Tasks.TaskFactory.FromAsync%2A?displayProperty=nameWithType> インスタンスまたは <xref:System.Threading.Tasks.TaskFactory%601.FromAsync%2A?displayProperty=nameWithType> インスタンスに APM の Begin/End メソッドのペアをカプセル化できる <xref:System.Threading.Tasks.Task> メソッドと <xref:System.Threading.Tasks.Task%601> メソッドの複数のオーバーロードが用意されています。 さまざまなオーバーロードが、0 ～ 3 個の入力パラメーターを持つ Begin/End メソッドのペアに対応します。  
  
 値 (Visual Basic の場合は `End`) を返す `Function` メソッドを持つペアの場合は、<xref:System.Threading.Tasks.TaskFactory%601> を作成する <xref:System.Threading.Tasks.Task%601> のメソッドを使用します。 Void (Visual Basic の場合は `End`) を返す `Sub` メソッドの場合は、<xref:System.Threading.Tasks.TaskFactory> を作成する <xref:System.Threading.Tasks.Task> のメソッドを使用します。  
  
 ごくまれなケースですが、`Begin` メソッドが 3 個以上のパラメーターを持つ場合や、`ref` パラメーターまたは `out` パラメーターが含まれる場合は、`FromAsync` メソッドだけをカプセル化する `End` オーバーロードが追加で提供されます。  
  
 `FromAsync` メソッドおよび <xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> メソッドと一致する <xref:System.IO.FileStream.EndRead%2A?displayProperty=nameWithType> オーバーロードの署名を、次のコード例に示します。 このオーバーロードは、次のように、3 つの入力パラメーターを取ります。  
  
 [!code-csharp[FromAsync#01](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#01)]
 [!code-vb[FromAsync#01](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#01)]  
  
 1 つ目のパラメーターは <xref:System.Func%606> デリゲートで、<xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> メソッドの署名と一致します。 2 つ目のパラメーターは、<xref:System.Func%602> を受け取り、<xref:System.IAsyncResult> を返す `TResult` デリゲートです。 <xref:System.IO.FileStream.EndRead%2A> は整数を返すので、コンパイラは `TResult` の型を <xref:System.Int32> と推論し、タスクの型を <xref:System.Threading.Tasks.Task> と推論します。 最後の 4 つのパラメーターは、<xref:System.IO.FileStream.BeginRead%2A?displayProperty=nameWithType> メソッドのパラメーターと同じです。  
  
- ファイル データを格納するバッファー。  
  
- データ書き込みの開始位置を示すバッファー内のオフセット。  
  
- ファイルから読み取る最大データ量。  
  
- コールバックに渡されるユーザー定義の状態データを格納する、オプションのオブジェクト。  
  
### <a name="using-continuewith-for-the-callback-functionality"></a>コールバック関数での ContinueWith の使用  
 バイト数だけでなく、ファイルのデータにアクセスする必要がある場合は、<xref:System.Threading.Tasks.TaskFactory.FromAsync%2A> メソッドだけでは不十分です。 代わりに、<xref:System.Threading.Tasks.Task> プロパティにファイル データが含まれている `Result` を使用します。 これを実行するには、元のタスクに継続を追加します。 継続は、通常、<xref:System.AsyncCallback> デリゲートによって実行される作業を実行します。 継続元が完了したとき、およびデータ バッファーが満杯になったときに呼び出されます。 (<xref:System.IO.FileStream> オブジェクトは、返される前に終了している必要があります)。  
  
 <xref:System.Threading.Tasks.Task> クラスの BeginRead/EndRead ペアをカプセル化する <xref:System.IO.FileStream> を返す方法を、次の例に示します。  
  
 [!code-csharp[FromAsync#03](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#03)]
 [!code-vb[FromAsync#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#03)]  
  
 このメソッドは、次のようにして呼び出すことができます。  
  
 [!code-csharp[FromAsync#04](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#04)]
 [!code-vb[FromAsync#04](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#04)]  
  
### <a name="providing-custom-state-data"></a>カスタム状態データの提供  
 一般的な <xref:System.IAsyncResult> 操作では、<xref:System.AsyncCallback> デリゲートにカスタム状態データが必要な場合、最終的にコールバック メソッドに渡される `Begin` オブジェクトにデータをパッケージ化できるよう、<xref:System.IAsyncResult> メソッドの最後のパラメーターを通じてカスタム状態データを渡す必要があります。 通常、`FromAsync` メソッドが使用される場合は必要ありません。 カスタム データが継続に対して既知の場合は、継続のデリゲートで直接キャプチャできます。 次の例は前の例と似ていますが、継続は継続元の `Result` プロパティを検証せずに、継続のユーザー デリゲートに直接アクセスできるカスタム状態データを検証します。  
  
 [!code-csharp[FromAsync#05](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#05)]
 [!code-vb[FromAsync#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#05)]  
  
### <a name="synchronizing-multiple-fromasync-tasks"></a>複数の FromAsync タスクの同期化  
 静的な <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAll%2A> メソッドと <xref:System.Threading.Tasks.TaskFactory.ContinueWhenAny%2A> メソッドは、`FromAsync` メソッドと共に使用することによって、柔軟性を強化できます。 複数の非同期 I/O 操作を開始し、継続を実行する前にすべての操作が完了するまで待機する方法を次の例に示します。  
  
 [!code-csharp[FromAsync#06](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#06)]
 [!code-vb[FromAsync#06](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#06)]  
  
### <a name="fromasync-tasks-for-only-the-end-method"></a>End メソッドだけの FromAsync タスク  
 ごくまれなケースですが、`Begin` メソッドに 3 個以上の入力パラメーターが必要な場合や、`ref` パラメーターまたは `out` パラメーターが含まれる場合は、`FromAsync` など、<xref:System.Threading.Tasks.TaskFactory%601.FromAsync%28System.IAsyncResult%2CSystem.Func%7BSystem.IAsyncResult%2C%600%7D%29?displayProperty=nameWithType> メソッドだけを表す `End` オーバーロードを使用します。 これらのメソッドは、<xref:System.IAsyncResult> が渡され、それをタスクにカプセル化するシナリオでも使用できます。  
  
 [!code-csharp[FromAsync#07](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#07)]
 [!code-vb[FromAsync#07](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#07)]  
  
### <a name="starting-and-canceling-fromasync-tasks"></a>FromAsync タスクの開始とキャンセル  
 `FromAsync` メソッドから返されるタスクの状態は WaitingForActivation で、タスク作成後のある時点でシステムによって開始されます。 このようなタスクで Start を呼び出そうとすると、例外が発生します。  
  
 基になる .NET Framework の API は、現時点ではファイルまたはネットワーク I/O の処理中の取り消しをサポートしていないので、`FromAsync` タスクを取り消すことはできません。 `FromAsync` の呼び出しをカプセル化するメソッドにキャンセル機能を追加することはできますが、キャンセルに応答できるのは `FromAsync` が呼び出される前、または完了した後 (たとえば、継続タスク上など) だけです。  
  
 <xref:System.Net.WebClient> など、EAP をサポートするいくつかのクラスはキャンセルをサポートしていないため、ネイティブのキャンセル機能を統合するには、キャンセル トークンを使用します。  
  
## <a name="exposing-complex-eap-operations-as-tasks"></a>複雑な EAP 操作のタスクとしての公開  
 TPL は、`FromAsync` 系のメソッドが <xref:System.IAsyncResult> パターンをラップするのとは異なる方法で、イベント ベースの非同期操作をカプセル化するよう特別に設計されたメソッドを提供します。 ただし、TPL は <xref:System.Threading.Tasks.TaskCompletionSource%601?displayProperty=nameWithType> クラスを提供するので、これを使用して任意の操作セットを <xref:System.Threading.Tasks.Task%601> として表すことができます。 操作は、同期または非同期、I/O バインドまたは計算主体、またはその両方です。  
  
 <xref:System.Threading.Tasks.TaskCompletionSource%601> を使用し、非同期の <xref:System.Net.WebClient> 操作を基本的な <xref:System.Threading.Tasks.Task%601> としてクライアント コードに公開する方法を、次の例に示します。 このメソッドを使用すると、Web URL の配列および検索対象の用語または名前を入力し、各サイトで検索用語が使用される回数を返すことができます。  
  
 [!code-csharp[FromAsync#10](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/snippet10.cs#10)]
 [!code-vb[FromAsync#10](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/snippet10.vb#10)]  
  
 追加の例外処理を含み、クライアント コードからメソッドを呼び出す方法を示す、より包括的な例については、「[方法: タスクに EAP パターンをラップする](../../../docs/standard/parallel-programming/how-to-wrap-eap-patterns-in-a-task.md)」を参照してください。  
  
 <xref:System.Threading.Tasks.TaskCompletionSource%601> によって作成されたタスクは、その TaskCompletionSource によって開始されるので、ユーザー コードがそのタスクで Start メソッドを呼び出さないようにする必要があります。  
  
## <a name="implementing-the-apm-pattern-by-using-tasks"></a>タスクを使用した APM パターンの実装  
 一部のシナリオでは、API で Begin/End メソッド ペアを使用することにより、<xref:System.IAsyncResult> を直接公開する方が望ましいこともあります。 たとえば、既存の API との一貫性を保ちたいときや、このパターンを必要とする自動ツールがある場合などです。 そのような場合は、タスクを使用し、APM パターンを内部的に実装する方法を簡素化できます。  
  
 タスクを使用し、長期間実行される計算主体のメソッドに APM の Begin/End メソッド ペアを実装する方法を、次の例に示します。  
  
 [!code-csharp[FromAsync#09](../../../samples/snippets/csharp/VS_Snippets_Misc/fromasync/cs/fromasync.cs#09)]
 [!code-vb[FromAsync#09](../../../samples/snippets/visualbasic/VS_Snippets_Misc/fromasync/vb/module1.vb#09)]  
  
## <a name="using-the-streamextensions-sample-code"></a>StreamExtensions サンプル コードの使用  
 「[Samples for Parallel Programming with the .NET Framework 4](https://code.msdn.microsoft.com/ParExtSamples)」(.NET Framework 4 での並列プログラミングの例) に示す Streamextensions.cs ファイルには、非同期ファイルおよびネットワーク I/O に対してタスク オブジェクトを使用する参照実装がいくつか含まれています。  
  
## <a name="see-also"></a>参照

- [タスク並列ライブラリ (TPL)](../../../docs/standard/parallel-programming/task-parallel-library-tpl.md)
