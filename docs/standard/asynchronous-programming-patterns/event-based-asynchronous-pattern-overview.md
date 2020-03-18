---
title: イベントベースの非同期パターンの概要
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET Framework], asynchronous
- Asynchronous Pattern
- AsyncOperationManager class
- threading [.NET Framework], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 792aa8da-918b-458e-b154-9836b97735f3
ms.openlocfilehash: cce01a7c87f6f20b5e6c46881b8c863bb5a72a88
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160068"
---
# <a name="event-based-asynchronous-pattern-overview"></a>イベントベースの非同期パターンの概要
多数のタスクを同時に実行しながら、ユーザーの操作にも応答するアプリケーションには、通常、複数のスレッドを使用するデザインが必要です。 <xref:System.Threading> 名前空間は、高性能なマルチスレッド アプリケーションを作成するのに必要なすべてのツールを提供します。ただし、これらのツールを効果的に使用するには、マルチスレッド ソフトウェア エンジニアリングの豊富な経験が必要です。 比較的単純なマルチスレッド アプリケーションの場合は、<xref:System.ComponentModel.BackgroundWorker> コンポーネントが簡単なソリューションを提供します。 より高度な非同期アプリケーションの場合は、イベント ベースの非同期パターンに準拠したクラスの実装を検討してください。  
  
 イベント ベースの非同期パターンを使用すると、マルチスレッド デザイン固有の多くの複雑な問題を気にせずに、マルチスレッド アプリケーションの利点を活用できます。 このパターンをサポートするクラスを使用すると、次のことが可能になります。  
  
- ダウンロードやデータベース操作などの時間がかかるタスクを、アプリケーションを中断せずに、"バックグラウンド" で行うことができます。  
  
- 複数の操作を同時に実行し、それぞれの操作が完了するたびに通知を受け取ることができます。  
  
- アプリケーションを停止 ("ブロック") せずに、リソースが使用可能な状態になるまで待機できます。  
  
- 使い慣れたイベントおよびデリゲートのモデルを使用して、保留中の非同期操作と通信できます。 イベント ハンドラーおよびデリゲートの使い方の詳細については、[イベント](../../../docs/standard/events/index.md)に関するページを参照してください。  
  
 イベント ベースの非同期パターンをサポートするクラスには、1 つまたは複数の _MethodName_**Async** という名前のメソッドが含まれます。 これらのメソッドは、同期バージョンに対応するもので、現在のスレッドで同じ操作を行います。 クラスには、_MethodName_**Completed** イベントや _MethodName_**AsyncCancel** (または単に **CancelAsync**) メソッドが含まれる場合もあります。  
  
 <xref:System.Windows.Forms.PictureBox> は、イベント ベースの非同期パターンをサポートする一般的なコンポーネントです。 イメージを同期的にダウンロードするには、その <xref:System.Windows.Forms.PictureBox.Load%2A>メソッドを呼び出します。ただし、イメージのサイズが大きい場合や、ネットワークの接続速度が遅い場合は、ダウンロード操作が完了して <xref:System.Windows.Forms.PictureBox.Load%2A> の呼び出しが返されるまで、アプリケーションが応答を停止します。  
  
 イメージの読み込み中にもアプリケーションを実行し続けるには、<xref:System.Windows.Forms.PictureBox.LoadAsync%2A> メソッドを呼び出して、他のイベントを処理する場合と同様に、<xref:System.Windows.Forms.PictureBox.LoadCompleted> イベントを処理します。 <xref:System.Windows.Forms.PictureBox.LoadAsync%2A> メソッドを呼び出すと、ダウンロードを別のスレッドで ("バックグラウンド" で) 続行しながら、アプリケーションを実行し続けることができます。 イメージの読み込み操作が完了すると、イベント ハンドラーが呼び出されます。イベント ハンドラーは、<xref:System.ComponentModel.AsyncCompletedEventArgs> パラメーターを調べて、ダウンロードが正常に完了したかどうかを確認できます。  
  
 イベント ベースの非同期パターンでは、非同期操作をキャンセルできる必要があります。<xref:System.Windows.Forms.PictureBox> コントロールでは、その <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> メソッドでこの要件をサポートしています。 <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> を呼び出すと、保留中のダウンロードを停止する要求が送信されます。タスクがキャンセルされると、<xref:System.Windows.Forms.PictureBox.LoadCompleted> イベントが発生します。  
  
> [!CAUTION]
> <xref:System.Windows.Forms.PictureBox.CancelAsync%2A> 要求が作成されると同時に、ダウンロードが終了する可能性もあります。このような場合、<xref:System.ComponentModel.AsyncCompletedEventArgs.Cancelled%2A> にはキャンセルの要求が反映されません。 これは*競合状態*と呼ばれる、マルチスレッド プログラミングの一般的な問題です。 マルチスレッド プログラミングの問題の詳細については、「[マネージド スレッド処理のベスト プラクティス](../../../docs/standard/threading/managed-threading-best-practices.md)」 (管理されたスレッドのベスト プラクティス) を参照してください。  
  
## <a name="characteristics-of-the-event-based-asynchronous-pattern"></a>イベント ベースの非同期パターンの特性  
 イベント ベースの非同期パターンには、特定のクラスでサポートされている操作の複雑さに応じて、複数の形式があります。 最もシンプルなクラスには、単一の _MethodName_**Async** メソッドと、このメソッドに対応する _MethodName_**Completed** イベントが含まれる場合があります。 より複雑なクラスには、複数の _MethodName_**Async** メソッドと、それぞれに対応する _MethodName_**Completed** イベント、およびこれらのメソッドの同期バージョンが含まれる場合があります。 クラスでは、各非同期メソッドの、キャンセル、進行状況のレポート、およびインクリメンタル結果をオプションでサポートできます。  
  
 また、非同期メソッドでは複数の保留中の呼び出し (複数の同時呼び出し) をサポートして、他の保留中の操作が完了するまで、コードを何度も呼び出せるようにできます。 このような状況を適切に処理するには、アプリケーションで各操作の完了を追跡する必要があります。  
  
### <a name="examples-of-the-event-based-asynchronous-pattern"></a>イベント ベースの非同期パターンの例  
 <xref:System.Media.SoundPlayer> および <xref:System.Windows.Forms.PictureBox> コンポーネントは、イベント ベースの非同期パターンのシンプルな実装です。 <xref:System.Net.WebClient> および <xref:System.ComponentModel.BackgroundWorker> コンポーネントは、イベント ベースの非同期パターンのより複雑な実装です。  
  
 パターンに準拠したクラス宣言の例を次に示します。  
  
```vb  
Public Class AsyncExample  
    ' Synchronous methods.  
    Public Function Method1(ByVal param As String) As Integer
    Public Sub Method2(ByVal param As Double)
  
    ' Asynchronous methods.  
    Overloads Public Sub Method1Async(ByVal param As String)
    Overloads Public Sub Method1Async(ByVal param As String, ByVal userState As Object)
    Public Event Method1Completed As Method1CompletedEventHandler  
  
    Overloads Public Sub Method2Async(ByVal param As Double)
    Overloads Public Sub Method2Async(ByVal param As Double, ByVal userState As Object)
    Public Event Method2Completed As Method2CompletedEventHandler  
  
    Public Sub CancelAsync(ByVal userState As Object)
  
    Public ReadOnly Property IsBusy () As Boolean  
  
    ' Class implementation not shown.  
End Class  
```  
  
```csharp  
public class AsyncExample  
{  
    // Synchronous methods.  
    public int Method1(string param);  
    public void Method2(double param);  
  
    // Asynchronous methods.  
    public void Method1Async(string param);  
    public void Method1Async(string param, object userState);  
    public event Method1CompletedEventHandler Method1Completed;  
  
    public void Method2Async(double param);  
    public void Method2Async(double param, object userState);  
    public event Method2CompletedEventHandler Method2Completed;  
  
    public void CancelAsync(object userState);  
  
    public bool IsBusy { get; }  
  
    // Class implementation not shown.  
}  
```  
  
 架空の `AsyncExample` クラスには 2 つのメソッドがあり、いずれも同期および非同期の呼び出しをサポートしています。 同期のオーバーロードは、呼び出し元スレッドで操作を呼び出しおよび実行する任意のメソッドと同様に動作します。操作に時間がかかる場合は、呼び出しから戻るまでにかなりの遅延が発生する場合があります。 非同期のオーバーロードは、別のスレッドで操作を開始して即座に戻ります。これにより、操作を "バックグラウンド" で実行しながら、呼び出し元スレッドを続行できます。  
  
### <a name="asynchronous-method-overloads"></a>非同期メソッドのオーバーロード  
 非同期操作には、基本的に単一呼び出しと複数呼び出しの 2 つのオーバーロードがあります。 これら 2 つの形式はそのメソッド シグネチャで区別できます。複数呼び出しの形式には、`userState` という名前の追加のパラメーターがあります。 この形式を使用すると、保留中の非同期操作が完了するのを待たずに、コードで `Method1Async(string param, object userState)` を複数回呼び出すことができます。 一方、直前の呼び出しが完了する前に `Method1Async(string param)` を呼び出そうとすると、メソッドにより <xref:System.InvalidOperationException> が発生します。  
  
 複数呼び出しのオーバーロードの `userState` パラメーターにより、非同期操作を区別できます。 `Method1Async(string param, object userState)` の各呼び出しに一意な値 (GUID やハッシュ コードなど) を指定すると、各操作が完了したときに、イベント ハンドラーは、どの操作のインスタンスが完了イベントを発生させたのかを確認できます。  
  
### <a name="tracking-pending-operations"></a>保留中の操作の追跡  
 複数呼び出しのオーバーロードを使用する場合は、保留中のタスクの `userState` オブジェクト (タスク ID) を追跡する必要があります。 `Method1Async(string param, object userState)` の呼び出しごとに、通常は新しい一意な `userState` オブジェクトを生成し、それをコレクションに追加します。 この `userState` オブジェクトに対応するタスクが完了イベントを発生させると、完了メソッドの実装は <xref:System.ComponentModel.AsyncCompletedEventArgs.UserState%2A?displayProperty=nameWithType> を調べて、それをコレクションから削除します。 このように使用すると、`userState` パラメーターはタスク ID の役割を果たします。  
  
> [!NOTE]
> 複数呼び出しのオーバーロードの呼び出しでは、`userState` に一意な値を指定するように注意が必要です。 タスク ID が一意でないと、非同期クラスが <xref:System.ArgumentException> をスローします。  
  
### <a name="canceling-pending-operations"></a>保留中の操作のキャンセル  
 非同期操作を完了前にいつでもキャンセルできることは重要です。 イベント ベースの非同期パターンを実装するクラスには、`CancelAsync` メソッド (非同期メソッドが 1 つだけの場合) または _MethodName_**AsyncCancel** メソッド (複数の非同期メソッドがある場合) が含まれます。  
  
 複数呼び出しを可能にするメソッドは `userState` パラメーターを受け取ります。これは、各タスクの有効期間を追跡するのに使用できます。 `CancelAsync` は `userState` パラメーターを受け取ります。このパラメーターにより、特定の保留中のタスクを取り消すことができます。  
  
 保留中の操作を一度に 1 つだけサポートする `Method1Async(string param)` のようなメソッドは、キャンセルできません。  
  
### <a name="receiving-progress-updates-and-incremental-results"></a>進行状況の更新およびインクリメンタル結果の受信  
 イベント ベースの非同期パターンに準拠するクラスは、進行状況およびインクリメンタル結果を追跡するイベントをオプションで提供します。 通常、これは `ProgressChanged` または _MethodName_**ProgressChanged** という名前で、その対応するイベント ハンドラーは <xref:System.ComponentModel.ProgressChangedEventArgs> パラメーターを受け取ります。  
  
 `ProgressChanged` イベントのイベント ハンドラーは、<xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A?displayProperty=nameWithType> プロパティを調べて、非同期タスクの何パーセントが完了したのかを確認できます。 このプロパティは 0 ～ 100 の範囲です。これを使用すると、<xref:System.Windows.Forms.ProgressBar.Value%2A>ar の <xref:System.Windows.Forms.ProgressBar> プロパティを更新できます。 複数の非同期操作が保留中の場合は、<xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A?displayProperty=nameWithType> プロパティを使用して、どの操作が進行状況をレポートしているのかを判別できます。  
  
 一部のクラスは、非同期操作の進行に応じて、インクリメンタル結果をレポートします。 これらの結果は <xref:System.ComponentModel.ProgressChangedEventArgs> から派生したクラス内に保存され、派生クラスのプロパティとして表示されます。 `ProgressChanged` イベントのイベント ハンドラーのこれらの結果へは、<xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> プロパティにアクセスするのと同じようにアクセスできます。 複数の非同期操作が保留中の場合は、<xref:System.ComponentModel.ProgressChangedEventArgs.UserState%2A> プロパティを使用して、どの操作がインクリメンタル結果をレポートしているのかを判別できます。  
  
## <a name="see-also"></a>参照

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.BackgroundWorker>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [方法: イベントベースの非同期パターンをサポートするコンポーネントを使用する](../../../docs/standard/asynchronous-programming-patterns/how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)
- [方法: バックグラウンドで操作を実行する](../../../docs/framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [方法: バックグラウンド操作を使用するフォームを実装する](../../../docs/framework/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation.md)
- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
- [イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [イベントベースの非同期パターンをいつ実装するかの決定](../../../docs/standard/asynchronous-programming-patterns/deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
