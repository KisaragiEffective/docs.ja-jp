---
title: イベントベースの非同期パターンを実装するための推奨される手順
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- Event-based Asynchronous Pattern
- ProgressChangedEventArgs class
- BackgroundWorker component
- events [.NET Framework], asynchronous
- AsyncOperationManager class
- threading [.NET Framework], asynchronous features
- AsyncOperation class
- AsyncCompletedEventArgs class
ms.assetid: 4acd2094-4f46-4eff-9190-92d0d9ff47db
ms.openlocfilehash: 439b862612d7997c9277ffb2cf4f15b14bd0b106
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78156050"
---
# <a name="best-practices-for-implementing-the-event-based-asynchronous-pattern"></a>イベントベースの非同期パターンを実装するための推奨される手順
イベントベースの非同期パターンは、使い慣れたイベントおよびデリゲートのセマンティクスと共に、クラス内の非同期動作を公開する効果的な方法を提供します。 イベント ベースの非同期パターンを実装するには、いくつかの固有の動作要件に従う必要があります。 以降のセクションでは、イベントベースの非同期パターンに従うクラスを実装する際に検討すべき要件とガイドラインについて説明します。  
  
 概要については、「[イベントベースの非同期パターンの実装](../../../docs/standard/asynchronous-programming-patterns/implementing-the-event-based-asynchronous-pattern.md)」を参照してください。  
  
## <a name="required-behavioral-guarantees"></a>必要な動作保証  
 イベントベースの非同期パターンを実装する場合は、クラスが適切に動作し、クラスのクライアントがそのような動作に依存できるようにするため、多数の保証を提供する必要があります。  
  
### <a name="completion"></a>完了  
 正常完了、エラー、またはキャンセルの場合に常に <em>MethodName</em>**Completed** イベント ハンドラーを呼び出します。 アプリケーションがアイドルになり完了しない状態が発生してはなりません。 この規則の唯一の例外として、非同期操作自体は完了することがないように設計されている場合があります。  
  
### <a name="completed-event-and-eventargs"></a>完了イベントおよび EventArg  
 個別の <em>MethodName</em>**Async** メソッドごとに、次の設計要件を適用します。  
  
- メソッドと同じクラスで <em>MethodName</em>**Completed** イベントを定義します。  
  
- <xref:System.ComponentModel.AsyncCompletedEventArgs> クラスから派生した <em>MethodName</em>**Completed** イベントの <xref:System.EventArgs> クラスと、これに付随するデリゲートを定義します。 既定のクラス名の形式は、<em>MethodName</em>**CompletedEventArgs** です。  
  
- <xref:System.EventArgs> クラスは、<em>MethodName</em> メソッドの戻り値に固有のクラスにしてください。 <xref:System.EventArgs> クラスを使用する場合は、開発者に対して結果をキャストすることを義務付けないでください。  
  
     設計要件の適切な実装と不適切な実装を次のコード例に示します。  
  
```csharp  
// Good design  
private void Form1_MethodNameCompleted(object sender, xxxCompletedEventArgs e)
{
    DemoType result = e.Result;  
}  
  
// Bad design  
private void Form1_MethodNameCompleted(object sender, MethodNameCompletedEventArgs e)
{
    DemoType result = (DemoType)(e.Result);  
}  
```  
  
- <xref:System.EventArgs> を返すメソッドのために `void` クラスを定義しないでください。 代わりに <xref:System.ComponentModel.AsyncCompletedEventArgs> クラスのインスタンスを使用してください。  
  
- 必ず常に <em>MethodName</em>**Completed** イベントを発生させます。 このイベントは、正常完了、エラー、またはキャンセル時に発生する必要があります。 アプリケーションがアイドルになり完了しない状態が発生してはなりません。  
  
- 非同期操作で発生した例外をすべてキャッチし、キャッチした例外を <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> プロパティに割り当ててください。  
  
- タスクの実行中にエラーが発生した場合は、結果にアクセスできないようにしてください。 <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> プロパティが `null` ではない場合は、<xref:System.EventArgs> 構造体のプロパティにアクセスすると例外が発生するようにしてください。 この検証を行うには、<xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A> メソッドを使用します。  
  
- タイムアウトをエラーとしてモデル化します。 タイムアウトが発生したら、<em>MethodName</em>**Completed** イベントを発生させ、<xref:System.TimeoutException> を <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> プロパティに割り当てます。  
  
- クラスで複数の同時呼び出しがサポートされている場合は、必ず <em>MethodName</em>**Completed** イベントに適切な `userSuppliedState` オブジェクトが含まれるようにします。  
  
- <em>MethodName</em>**Completed** イベントが、適切なスレッドでアプリケーション サイクルの適切な時点で発生するようにしてください。 詳細については、「スレッド処理およびコンテキスト」を参照してください。  
  
### <a name="simultaneously-executing-operations"></a>操作の同時実行  
  
- クラスで複数の同時呼び出しがサポートされている場合は、開発者が各呼び出しを個別に追跡できるようにするため、`userSuppliedState` というオブジェクト値状態パラメーター、またはタスク ID を受け取る <em>MethodName</em>**Async** オーバーライドを定義します。 このパラメーターは、常に <em>MethodName</em>**Async** メソッドのシグネチャの最終パラメーターにする必要があります。  
  
- オブジェクト値状態パラメーターまたはタスク ID を受け取る <em>MethodName</em>**Async** オーバーロードがクラスによって定義される場合は、そのタスク ID の操作の有効期間を追跡し、完了ハンドラーに戻す必要があります。 役に立つヘルパー クラスがあります。 コンカレンシー管理の詳細については、「[方法: イベントベースの非同期パターンをサポートするコンポーネントを実装する](../../../docs/standard/asynchronous-programming-patterns/component-that-supports-the-event-based-asynchronous-pattern.md)」を参照してください。  
  
- クラスによって、状態パラメーターなしで <em>MethodName</em>**Async** メソッドが定義され、このクラスで複数の同時呼び出しがサポートされていない場合、直前の <em>MethodName</em>**Async** 呼び出しが完了する前に <em>MethodName</em>**Async** を呼び出そうとすると、<xref:System.InvalidOperationException> が発生するようにします。  
  
- 一般に、`userSuppliedState` パラメーターのない <em>MethodName</em>**Async** メソッドを複数回呼び出すときには、複数の未完了操作が存在するようにするため、例外を発生させないでください。 例外を発生させることができるのは、クラスがその状況に対処できないことが明らかであるものの、開発者がこのような区別できない複数のコールバックを処理できる場合です。  
  
### <a name="accessing-results"></a>結果へのアクセス  
  
- 非同期操作の実行中にエラーが発生した場合、その結果にはアクセスできないようにしてください。 <xref:System.ComponentModel.AsyncCompletedEventArgs> が <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> ではない場合に `null` のプロパティにアクセスすると、<xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A> により参照される例外が発生するようにしてください。 <xref:System.ComponentModel.AsyncCompletedEventArgs> クラスには、この目的で使用する <xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A> メソッドが用意されています。  
  
- 結果にアクセスしようとすると、操作がキャンセルされたことを示す <xref:System.InvalidOperationException> が発生するようにしてください。 この検証を行うには、<xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A?displayProperty=nameWithType> メソッドを使用します。  
  
### <a name="progress-reporting"></a>進行状況のレポート  
  
- 可能であれば、進行状況レポートをサポートします。 これにより、開発者はクラスを使用する際に、より優れたアプリケーション ユーザー エクスペリエンスを提供できます。  
  
- **ProgressChanged** または <em>MethodName</em>**ProgressChanged** イベントを実装する場合は、操作の <em>MethodName</em>**Completed** イベントが発生した後で、特定の非同期操作についてそのようなイベントが発生していないようにしてください。  
  
- 標準 <xref:System.ComponentModel.ProgressChangedEventArgs> の値が設定される場合は、<xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> が常にパーセンテージとして解釈できるようにしてください。 パーセンテージは正確である必要はありませんが、パーセンテージを表している必要があります。 進行状況レポート メトリックがパーセンテージ以外でなければならない場合は、<xref:System.ComponentModel.ProgressChangedEventArgs> クラスからクラスを派生し、<xref:System.ComponentModel.ProgressChangedEventArgs.ProgressPercentage%2A> は 0 のままにしておきます。 パーセンテージ以外のレポート メトリックは使用しないでください。  
  
- `ProgressChanged` イベントが、適切なスレッドでアプリケーション サイクルの適切な時点で発生するようにしてください。 詳細については、「スレッド処理およびコンテキスト」を参照してください。  
  
### <a name="isbusy-implementation"></a>IsBusy 実装  
  
- クラスが複数の同時呼び出しをサポートしている場合は、`IsBusy` プロパティを公開しないでください。 たとえば XML Web サービス プロキシは、非同期メソッドの複数同時呼び出しをサポートしているため、`IsBusy` プロパティを公開しません。  
  
- `IsBusy` プロパティは、<em>MethodName</em>**Async** メソッドが呼び出されてから、<em>MethodName</em>**Completed** イベントが発生するまでの間は、`true` を返す必要があります。 それ以外の場合は、`false` を返す必要があります。 <xref:System.ComponentModel.BackgroundWorker> プロパティを公開するクラスの例として、<xref:System.Net.WebClient> および `IsBusy` コンポーネントがあります。  
  
### <a name="cancellation"></a>キャンセル  
  
- 可能であれば、キャンセルをサポートしてください。 これにより、開発者はクラスを使用する際に、より優れたアプリケーション ユーザー エクスペリエンスを提供できます。  
  
- キャンセルの場合、<xref:System.ComponentModel.AsyncCompletedEventArgs.Cancelled%2A> オブジェクトに <xref:System.ComponentModel.AsyncCompletedEventArgs> フラグを設定します。  
  
- 結果にアクセスしようとすると、操作がキャンセルされたことを示す <xref:System.InvalidOperationException> が発生するようにしてください。 この検証を行うには、<xref:System.ComponentModel.AsyncCompletedEventArgs.RaiseExceptionIfNecessary%2A?displayProperty=nameWithType> メソッドを使用します。  
  
- キャンセル メソッドの呼び出しは常に正常に戻り、例外を発生させないようにしてください。 一般に、特定の時点で操作が実際にキャンセル可能かどうかと、前に発行したキャンセルが正常に実行されたかどうかはクライアントに通知されません。 ただし、キャンセルが正常に完了すると常にアプリケーションに通知が送られます。これは、アプリケーションが完了ステータスに関与しているためです。  
  
- 操作がキャンセルされた場合は、<em>MethodName</em>**Completed** イベントを発生させます。  
  
### <a name="errors-and-exceptions"></a>エラーおよび例外  
  
- 非同期操作で発生した例外をすべてキャッチし、その例外の <xref:System.ComponentModel.AsyncCompletedEventArgs.Error%2A?displayProperty=nameWithType> プロパティの値を設定します。  
  
### <a name="threading-and-contexts"></a>スレッド処理およびコンテキスト  
 クラスを正しく操作するには、特定のアプリケーション モデル (ASP.NET および Windows フォーム アプリケーションを含む) の適切なスレッドまたはコンテキストで、クライアントのイベント ハンドラーが呼び出されることが重要です。 非同期クラスがどのアプリケーション モデルでも正しく動作するように、<xref:System.ComponentModel.AsyncOperation> と <xref:System.ComponentModel.AsyncOperationManager> という 2 つの重要なヘルパー クラスが用意されています。  
  
 <xref:System.ComponentModel.AsyncOperationManager> にはメソッド <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> が含まれています。このメソッドは <xref:System.ComponentModel.AsyncOperation> を返します。 <em>MethodName</em>**Async** メソッドは <xref:System.ComponentModel.AsyncOperationManager.CreateOperation%2A> を呼び出し、クラスは返される <xref:System.ComponentModel.AsyncOperation> を使用して非同期タスクの有効期間を追跡します。  
  
 進行状況、インクリメンタル結果、および完了をクライアントに報告するため、<xref:System.ComponentModel.AsyncOperation.Post%2A> で <xref:System.ComponentModel.AsyncOperation.OperationCompleted%2A> メソッドと <xref:System.ComponentModel.AsyncOperation> メソッドを呼び出します。 <xref:System.ComponentModel.AsyncOperation> は、クライアントのイベント ハンドラーに対する呼び出しを適切なスレッドまたはコンテキストにマーシャリングします。  
  
> [!NOTE]
> アプリケーション モデルのポリシーに対し明示的に準拠しないものの、イベント ベースの非同期パターンを使用する他のメリットを利用したい場合は、これらの規則を回避できます。 たとえば、Windows Forms でのクラス操作をフリー スレッド化するとします。 開発者がフリー スレッド化クラスの暗黙的な制限を理解している場合は、フリースレッド化クラスを作成できます。 コンソール アプリケーションは <xref:System.ComponentModel.AsyncOperation.Post%2A> 呼び出しの実行を同期しません。 これが原因で、`ProgressChanged` イベントが正しくない順序で発生することがあります。 <xref:System.ComponentModel.AsyncOperation.Post%2A> 呼び出しを順次実行するには、<xref:System.Threading.SynchronizationContext?displayProperty=nameWithType> クラスを実装およびインストールします。  
  
 <xref:System.ComponentModel.AsyncOperation> と <xref:System.ComponentModel.AsyncOperationManager> を使用して非同期操作を使用可能にする方法の詳細については、「[方法: イベントベースの非同期パターンをサポートするコンポーネントを実装する](../../../docs/standard/asynchronous-programming-patterns/component-that-supports-the-event-based-asynchronous-pattern.md)」を参照してください。  
  
## <a name="guidelines"></a>ガイドライン  
  
- 各メソッド呼び出しが相互に独立していることが理想的です。 呼び出しを共有リソースと組み合わせないでください。 リソースを複数の呼び出し間で共有する場合は、実装に適切な同期メカニズムを提供する必要があります。  
  
- クライアントが同期を実装する必要がある設計は推奨されません。 たとえば、グローバルな静的オブジェクトをパラメータとして受け取る非同期メソッドがあり、この非同期メソッドを同時に複数呼び出すと、データの破損またはデッドロックが発生することがあります。  
  
- 複数呼び出しのオーバーロード (シグネチャの `userState`) を使用してメソッドを実装する場合、ユーザー状態、タスク ID、対応する保留操作のコレクションをクラスで管理する必要があります。 さまざまな呼び出しでこのコレクションの `lock` オブジェクトが追加および削除されるため、このコレクションを `userState` 領域で保護する必要があります。  
  
- 可能かつ適切な場合は、`CompletedEventArgs` クラスの再利用を検討してください。 この場合、特定のデリゲートと <xref:System.EventArgs> 型が 1 つのメソッドに関連付けられていないため、名前付けにはメソッド名との整合性がありません。 ただし、開発者に対して <xref:System.EventArgs> のプロパティから取得した値をキャストすることを強制することはできません。  
  
- <xref:System.ComponentModel.Component> から派生したクラスを編集する場合、独自の <xref:System.Threading.SynchronizationContext> クラスを実装およびインストールしないでください。 使用される <xref:System.Threading.SynchronizationContext> を制御するのは、コンポーネントではなくアプリケーション モデルです。  
  
- どのようなマルチスレッドを使用する場合でも、深刻かつ複雑なバグが発生する可能性があります。 マルチスレッドを使用するソリューションを実装する前に、「[マネージド スレッド処理の実施](../../../docs/standard/threading/managed-threading-best-practices.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.ComponentModel.AsyncOperation>
- <xref:System.ComponentModel.AsyncOperationManager>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.BackgroundWorker>
- [イベントベースの非同期パターンの実装](../../../docs/standard/asynchronous-programming-patterns/implementing-the-event-based-asynchronous-pattern.md)
- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
- [イベントベースの非同期パターンをいつ実装するかの決定](../../../docs/standard/asynchronous-programming-patterns/deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
- [イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [方法: イベントベースの非同期パターンをサポートするコンポーネントを使用する](../../../docs/standard/asynchronous-programming-patterns/how-to-use-components-that-support-the-event-based-asynchronous-pattern.md)
- [方法: イベントベースの非同期パターンをサポートするコンポーネントを実装する](../../../docs/standard/asynchronous-programming-patterns/component-that-supports-the-event-based-asynchronous-pattern.md)
