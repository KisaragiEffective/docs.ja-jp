---
title: '方法 : イベントベースの非同期パターンをサポートするコンポーネントを実装する'
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
- components [.NET Framework], asynchronous
- AsyncOperation class
- threading [Windows Forms], asynchronous features
- AsyncCompletedEventArgs class
ms.assetid: 61f676b5-936f-40f6-83ce-f22805ec9c2f
ms.openlocfilehash: 44a1019ac8169138aa95b03e2027d9539cbf8391
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71957366"
---
# <a name="how-to-implement-a-component-that-supports-the-event-based-asynchronous-pattern"></a>方法 : イベントベースの非同期パターンをサポートするコンポーネントを実装する
顕著な遅延が発生する可能性がある操作を伴うクラスを作成する場合は、[イベント ベースの非同期パターン](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)を実装することによって、非同期機能を与えることを検討します。  
  
 このチュートリアルでは、イベント ベースの非同期パターンを実装するコンポーネントの作成方法を示します。 これは、<xref:System.ComponentModel?displayProperty=nameWithType> 名前空間のヘルパー クラスを使用して実装します。これにより、コンポーネントは任意のアプリケーション モデルで正常に動作します (ASP.NET、コンソール アプリケーション、Windows フォーム アプリケーションなど)。 また、このコンポーネントは、<xref:System.Windows.Forms.PropertyGrid> コントロールや独自のカスタム デザイナーで設計可能です。  
  
 このチュートリアルを完了すると、素数を非同期に計算するアプリケーションが作成されます。 アプリケーションには、メイン ユーザー インターフェイス (UI) スレッドと各素数の計算用のスレッドがあります。 大きな数が素数かどうかを調べるにはかなりの時間がかかることがありますが、この遅延によってメイン UI スレッドが中断されることはなく、計算中もフォームは応答性を維持します。 いくつでも好きなだけ計算を同時に実行し、保留中の計算を選択的に取り消すことができます。  
  
 このチュートリアルでは、以下のタスクを行います。  
  
- コンポーネントの作成  
  
- パブリックの非同期イベントとデリゲートの定義  
  
- プライベート デリゲートの定義  
  
- パブリック イベントの実装  
  
- 完了メソッドの実装  
  
- ワーカー メソッドの実装  
  
- 開始メソッドとキャンセル メソッドの実装  
  
 このトピックのコードを単一のリストとしてコピーするには、「[方法 : イベントベースの非同期パターンのクライアントを実装する](../../../docs/standard/asynchronous-programming-patterns/how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)」をご覧ください。  
  
## <a name="creating-the-component"></a>コンポーネントの作成  
 最初に、イベント ベースの非同期パターンを実装するコンポーネントを作成します。  
  
### <a name="to-create-the-component"></a>コンポーネントを作成するには  
  
- `PrimeNumberCalculator` を継承する <xref:System.ComponentModel.Component> というクラスを作成します。  
  
## <a name="defining-public-asynchronous-events-and-delegates"></a>パブリックの非同期イベントとデリゲートの定義  
 コンポーネントは、イベントを使ってクライアントと通信します。 _MethodName_**Completed** イベントは非同期タスクの完了をクライアントに通知し、_MethodName_**ProgressChanged** イベントは非同期タスクの進行状況をクライアントに通知します。  
  
### <a name="to-define-asynchronous-events-for-clients-of-your-component"></a>コンポーネントのクライアント用の非同期イベントを定義するには:  
  
1. ファイルの先頭で <xref:System.Threading?displayProperty=nameWithType> および <xref:System.Collections.Specialized?displayProperty=nameWithType> 名前空間をインポートします。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#11](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#11)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#11](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#11)]  
  
2. `PrimeNumberCalculator` クラスの定義の前で、進行状況イベントと完了イベントのデリゲートを宣言します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#7](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#7)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#7](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#7)]  
  
3. `PrimeNumberCalculator` クラスの定義で、進行状況と完了をクライアントに報告するイベントを宣言します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#8](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#8)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#8](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#8)]  
  
4. `PrimeNumberCalculator` クラスの定義の後で、`CalculatePrimeCompletedEventArgs` イベントに対するクライアントのイベント ハンドラーに各計算の結果を報告するための `CalculatePrimeCompleted` クラスを派生します。 `AsyncCompletedEventArgs` プロパティに加えて、このクラスにより、クライアントはテストされた値、その値が素数かどうか、素数でない場合は最初の約数を知ることができます。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#6](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#6)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#6](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#6)]  
  
## <a name="checkpoint"></a>チェックポイント  
 この段階で、コンポーネントをビルドすることができます。  
  
### <a name="to-test-your-component"></a>コンポーネントをテストするには  
  
- コンポーネントをコンパイルします。  
  
     コンパイラの警告が 2 つ表示されます。  
  
    ```console  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.ProgressChanged' is never used  
    warning CS0067: The event 'AsynchronousPatternExample.PrimeNumberCalculator.CalculatePrimeCompleted' is never used  
    ```  
  
     これらの警告は次のセクションでなくなります。  
  
## <a name="defining-private-delegates"></a>プライベート デリゲートの定義  
 `PrimeNumberCalculator` コンポーネントの非同期の側面は、<xref:System.Threading.SendOrPostCallback> と呼ばれる特別なデリゲートを使って内部的に実装されます。 <xref:System.Threading.SendOrPostCallback> は、<xref:System.Threading.ThreadPool> スレッドで実行するコールバック メソッドを表します。 コールバック メソッドは、<xref:System.Object> 型のパラメーターを 1 つ受け取るシグネチャを持つ必要があります。つまり、ラッパー クラスでデリゲートに状態を渡す必要があります。 詳細については、<xref:System.Threading.SendOrPostCallback> を参照してください。  
  
### <a name="to-implement-your-components-internal-asynchronous-behavior"></a>コンポーネントの内部非同期動作を実装するには:  
  
1. <xref:System.Threading.SendOrPostCallback> クラスで `PrimeNumberCalculator` デリゲートを宣言して作成します。 <xref:System.Threading.SendOrPostCallback> という名前のユーティリティ メソッドで `InitializeDelegates` オブジェクトを作成します。  
  
     2 つのデリゲートが必要です。1 つはクライアントに進行状況を報告するためのもので、もう 1 つはクライアントに完了を報告するためのものです。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#9](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#9)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#9](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#9)]  
    [!code-csharp[System.ComponentModel.AsyncOperationManager#20](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#20)]
    [!code-vb[System.ComponentModel.AsyncOperationManager#20](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#20)]  
  
2. コンポーネントのコンストラクターで `InitializeDelegates` メソッドを呼び出します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#21](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#21)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#21](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#21)]  
  
3. 実際の処理を非同期に行う `PrimeNumberCalculator` クラスのデリゲートを宣言します。 このデリゲートは、数値が素数かどうかをテストするワーカー メソッドをラップします。 デリゲートは <xref:System.ComponentModel.AsyncOperation> パラメーターを受け取ります。このパラメーターは、非同期操作の有効期間を追跡するために使われます。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#22](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#22)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#22](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#22)]  
  
4. 保留中の非同期操作の有効期間を管理するためのコレクションを作成します。 クライアントは操作の実行と完了を追跡する手段が必要であり、この追跡は、クライアントが非同期メソッドを呼び出すときに渡す必要のある一意のトークン (タスク ID) を使って行われます。 `PrimeNumberCalculator` コンポーネントは、タスク ID を対応する呼び出しと関連付けることによって、各呼び出しの追跡を維持する必要があります。 クライアントが一意でないタスク ID を渡した場合、`PrimeNumberCalculator` コンポーネントは例外を生成する必要があります。  
  
     `PrimeNumberCalculator` コンポーネントは、<xref:System.Collections.Specialized.HybridDictionary> という特別なコレクション クラスを使ってタスク ID を追跡します。 クラスの定義で、<xref:System.Collections.Specialized.HybridDictionary> という名前の `userTokenToLifetime` を作成します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#23](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#23)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#23](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#23)]  
  
## <a name="implementing-public-events"></a>パブリック イベントの実装  
 イベント ベースの非同期パターンを実装するコンポーネントは、イベントを使ってクライアントに通信します。 これらのイベントは、<xref:System.ComponentModel.AsyncOperation> クラスを使って適切なスレッドで呼び出されます。  
  
### <a name="to-raise-events-to-your-components-clients"></a>コンポーネントのクライアントに対するイベントを生成するには:  
  
1. クライアントに報告するためのパブリック イベントを実装します。 進行状況報告用のイベントと、完了報告用のイベントが必要です。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#24](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#24)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#24](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#24)]  
  
## <a name="implementing-the-completion-method"></a>完了メソッドの実装  
 完了デリゲートは、非同期操作が正常完了、エラー、またはキャンセルで終了すると、基になっているフリー スレッドの非同期動作が呼び出すメソッドです。 この呼び出しは、任意のスレッドで発生します。  
  
 このメソッドでは、クライアントのタスク ID が一意のクライアント トークンの内部コレクションから削除されます。 また、このメソッドは、対応する <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> で <xref:System.ComponentModel.AsyncOperation> メソッドを呼び出すことにより、特定の非同期操作の有効期間を終了させます。 この呼び出しでは、アプリケーション モデルの適切なスレッドで完了イベントが発生します。 <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> メソッドが呼び出された後、<xref:System.ComponentModel.AsyncOperation> のこのインスタンスは使えなくなり、それ以降にこれを使おうとするとすべて例外がスローされます。  
  
 `CompletionMethod` のシグネチャは、非同期操作の結果を記述するために必要なすべての状態を保持する必要があります。 この特定の非同期操作によってテストされた値の状態、その値が素数かどうか、素数の場合は最初の約数を保持しています。 また、発生した例外を記述する状態、およびこの特定のタスクに対応する <xref:System.ComponentModel.AsyncOperation> も保持します。  
  
### <a name="to-complete-an-asynchronous-operation"></a>非同期操作を完了するには:  
  
- 完了メソッドを実装します。 このメソッドは 6 つのパラメーターを受け取り、それを使って、クライアントの `CalculatePrimeCompletedEventArgs` によってクライアントに返される `CalculatePrimeCompletedEventHandler` を設定します。 また、クライアントのタスク ID トークンを内部コレクションから削除し、<xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> を呼び出して非同期操作の有効期間を終了します。 <xref:System.ComponentModel.AsyncOperation> は、アプリケーション モデルに適したスレッドまたはコンテキストへの呼び出しをマーシャリングします。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#26](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#26)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#26](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#26)]  
  
## <a name="checkpoint"></a>チェックポイント  
 この段階で、コンポーネントをビルドすることができます。  
  
### <a name="to-test-your-component"></a>コンポーネントをテストするには  
  
- コンポーネントをコンパイルします。  
  
     コンパイラの警告が 1 つ表示されます。  
  
    ```console  
    warning CS0169: The private field 'AsynchronousPatternExample.PrimeNumberCalculator.workerDelegate' is never used  
    ```  
  
     この警告は次のセクションで解決されます。  
  
## <a name="implementing-the-worker-methods"></a>ワーカー メソッドの実装  
 ここまでで、`PrimeNumberCalculator` コンポーネントをサポートする非同期コードを実装できました。 次に、実際の処理を行うコードを実装できます。 3 つのメソッド `CalculateWorker`、`BuildPrimeNumberList`、`IsPrime` を実装します。 また、`BuildPrimeNumberList` と `IsPrime` は、Sieve of Eratosthenes と呼ばれるよく知られたアルゴリズムを構成します。これは、テストする数値の平方根までのすべての素数を調べることで、その数値が素数かどうかを判定します。 平方根までに約数が見つからない場合、テスト対象の値は素数です。  
  
 このコンポーネントが最大限の効率性で作成されているとしたら、異なるテスト数値のさまざまな呼び出しによって検出されたすべての素数を記憶しています。 また、2、3、5 などの自明の約数もチェックします。 ただし、この例の目的は時間のかかる操作を非同期に実行する方法を示すことなので、これらの最適化は自習のために残しておきます。  
  
 `CalculateWorker` メソッドはデリゲートにラップされており、`BeginInvoke` の呼び出しとは非同期に呼び出されます。  
  
> [!NOTE]
> 進行状況の報告は、`BuildPrimeNumberList` メソッドで実装されています。 高速なコンピューターでは、`ProgressChanged` イベントが立て続けに発生する可能性があります。 これらのイベントが生成されるクライアント スレッドは、このような状況を処理できる必要があります。 ユーザー インターフェイスのコードがメッセージであふれ、処理が追いつかなくなり、応答しなくなる可能性があります。 この状況を処理するユーザー インターフェイスの例については、「[方法: イベントベースの非同期パターンのクライアントを実装する](../../../docs/standard/asynchronous-programming-patterns/how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)」をご覧ください。  
  
### <a name="to-execute-the-prime-number-calculation-asynchronously"></a>素数の計算を非同期に実行するには:  
  
1. `TaskCanceled` ユーティリティ メソッドを実装します。 このメソッドは、タスク有効期間コレクションで指定されたタスク ID をチェックし、タスク ID が見つからない場合は `true` を返します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#32](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#32)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#32](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#32)]  
  
2. `CalculateWorker` メソッドを実装します。 このメソッドは、テストする数と <xref:System.ComponentModel.AsyncOperation> の 2 つのパラメーターを受け取ります。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#27](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#27)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#27](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#27)]  
  
3. `BuildPrimeNumberList`を実装します。 このメソッドは、テストする数と <xref:System.ComponentModel.AsyncOperation> の 2 つのパラメーターを受け取ります。 <xref:System.ComponentModel.AsyncOperation> を使って、進行状況とインクリメンタルな結果を報告します。 これにより、アプリケーション モデルの適切なスレッドまたはコンテキストで、クライアントのイベント ハンドラーが呼び出されます。 `BuildPrimeNumberList` は、素数を見つけると、`ProgressChanged` イベントに対するクライアントのイベント ハンドラーに、インクリメンタルな結果としてこれを報告します。 これには <xref:System.ComponentModel.ProgressChangedEventArgs> から派生した `CalculatePrimeProgressChangedEventArgs` という名前のクラスが必要であり、このクラスには `LatestPrimeNumber` という名前の追加プロパティが 1 つあります。  
  
     `BuildPrimeNumberList` メソッドは `TaskCanceled` メソッドも定期的に呼び出し、このメソッドが `true` を返すと終了します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#5](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#5)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#5](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#5)]  
  
4. `IsPrime`を実装します。 このメソッドは、既知の素数のリスト、テスト対象の数、および見つかった最初の約数の出力パラメーターの、3 つのパラメーターを受け取ります。 素数のリストを基に、テスト対象の数が素数かどうかを特定します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#28](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#28)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#28](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#28)]  
  
5. `CalculatePrimeProgressChangedEventArgs` から <xref:System.ComponentModel.ProgressChangedEventArgs> を派生します。 このクラスは、`ProgressChanged` イベントに対するクライアントのイベント ハンドラーにインクリメンタルな結果を報告するために必要です。 `LatestPrimeNumber` という名前の追加プロパティが 1 つあります。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#29](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#29)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#29](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#29)]  
  
## <a name="checkpoint"></a>チェックポイント  
 この段階で、コンポーネントをビルドすることができます。  
  
### <a name="to-test-your-component"></a>コンポーネントをテストするには  
  
- コンポーネントをコンパイルします。  
  
     残っているのは、非同期操作を開始およびキャンセルするメソッドである `CalculatePrimeAsync` と `CancelAsync` です。  
  
## <a name="implementing-the-start-and-cancel-methods"></a>開始メソッドとキャンセル メソッドの実装  
 ラップするデリゲートで `BeginInvoke` を呼び出すことにより、専用のスレッドでワーカー メソッドを開始します。 特定の非同期操作の有効期間を管理するには、<xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> ヘルパー クラスの <xref:System.ComponentModel.AsyncOperationManager> メソッドを呼び出します。 このメソッドが返す <xref:System.ComponentModel.AsyncOperation> は、クライアントのイベント ハンドラーに対する呼び出しを適切なスレッドまたはコンテキストにマーシャリングします。  
  
 特定の保留中操作を取り消すには、対応する <xref:System.ComponentModel.AsyncOperation.PostOperationCompleted%2A> で <xref:System.ComponentModel.AsyncOperation> を呼び出します。 このメソッドはその操作を終了するので、それ以降に <xref:System.ComponentModel.AsyncOperation> を呼び出すと例外がスローされます。  
  
### <a name="to-implement-start-and-cancel-functionality"></a>開始とキャンセルの機能を実装するには:  
  
1. `CalculatePrimeAsync` メソッドを実装します。 クライアントが提供したトークン (タスク ID) が、現在保留中のタスクを表すすべてのトークンの間で一意であることを確認します。 クライアントが一意ではないトークンを渡した場合、`CalculatePrimeAsync` は例外を生成します。 一意の場合は、トークンはタスク ID のコレクションに追加されます。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#3](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#3)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#3](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#3)]  
  
2. `CancelAsync` メソッドを実装します。 トークン コレクションに `taskId` パラメーターが存在する場合は、削除されます。 これにより、開始する前に取り消されたタスクが実行するのを防ぎます。 タスクが実行中の場合、`BuildPrimeNumberList` メソッドは、タスク ID が有効期間コレクションから削除されたことを検出すると終了します。  
  
     [!code-csharp[System.ComponentModel.AsyncOperationManager#4](../../../samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/CS/primenumbercalculatormain.cs#4)]
     [!code-vb[System.ComponentModel.AsyncOperationManager#4](../../../samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AsyncOperationManager/VB/primenumbercalculatormain.vb#4)]  
  
## <a name="checkpoint"></a>チェックポイント  
 この段階で、コンポーネントをビルドすることができます。  
  
### <a name="to-test-your-component"></a>コンポーネントをテストするには  
  
- コンポーネントをコンパイルします。  
  
 `PrimeNumberCalculator` コンポーネントが完全して使用できるようになります。  
  
 `PrimeNumberCalculator` コンポーネントを使うクライアントの例については、「[方法: イベントベースの非同期パターンのクライアントを実装する](../../../docs/standard/asynchronous-programming-patterns/how-to-implement-a-client-of-the-event-based-asynchronous-pattern.md)」をご覧ください。  
  
## <a name="next-steps"></a>次の手順  
 `CalculatePrime` メソッドに相当する同期メソッドである `CalculatePrimeAsync` を作成して、この例を拡張できます。 このようにすると、`PrimeNumberCalculator` コンポーネントはイベント ベースの非同期パターンに完全に準拠するようになります。  
  
 異なるテスト対象の数に対するさまざまな呼び出しによって検出されたすべての素数のリストを保持することで、この例を改良できます。 この方法を使うと、各タスクはそれより前のタスクで行われた作業の結果を流用できます。 異なるスレッドによるリストへのアクセスがシリアル化されるように、このリストを `lock` 領域で保護することに注意してください。  
  
 2、3、5 などの自明の約数をテストすることにより、この例を改良することもできます。  
  
## <a name="see-also"></a>参照

- [方法: バックグラウンドで操作を実行する](../../../docs/framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [イベントベースの非同期パターンの概要](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)
- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
