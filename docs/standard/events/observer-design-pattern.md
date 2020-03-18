---
title: オブサーバー デザイン パターン
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- IObserver(Of T) interface
- IObservable<T> interface
- IObserver<T> interface
- IObservable(Of T) interface
- observer design pattern [.NET Framework]
ms.assetid: 3680171f-f522-453c-aa4a-54f755a78f88
ms.openlocfilehash: 817337cec604a431f9f7d4eacb04378ee0d3c227
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73131573"
---
# <a name="observer-design-pattern"></a>オブサーバー デザイン パターン

オブザーバー デザイン パターンでは、プロバイダーにサブスクライバーを登録して通知を受信することができます。 このデザイン パターンは、プッシュ ベースの通知を必要とするあらゆるシナリオに適しています。 このパターンでは、1 つの*プロバイダー* (*サブジェクト*または*観察可能なオブジェクト*とも呼ばれます) と、0 個以上の*オブザーバー*を定義します。 プロバイダーにオブザーバーを登録すると、あらかじめ定義した条件、イベント、または状態変化が発生するたびに、プロバイダーのいずれかのメソッドが呼び出されて自動的にすべてのオブザーバーに通知されます。 このメソッド呼び出しで、プロバイダーからオブザーバーに現在の状態の情報を提供することもできます。 .NET Framework でオブザーバー デザイン パターンを適用するには、ジェネリック インターフェイスの <xref:System.IObservable%601?displayProperty=nameWithType> および <xref:System.IObserver%601?displayProperty=nameWithType> を実装します。 ジェネリック型パラメーターは、通知情報を提供する型を表します。

## <a name="applying-the-pattern"></a>パターンの適用

オブザーバー デザイン パターンは、データ ソース (ビジネス ロジック) 層とユーザー インターフェイス (表示) 層のように、2 つの異なるコンポーネントまたはアプリケーション層を明確に分離できるため、分散型のプッシュ ベースの通知に適しています。 このパターンは、プロバイダーがコールバックを使用して現在の情報をクライアントに提供するあらゆる場面で実装できます。

このパターンを実装するために必要なものを以下に示します。

- プロバイダー (サブジェクト)。オブザーバーに通知を送信するオブジェクトです。 プロバイダーは、<xref:System.IObservable%601> インターフェイスを実装するクラスまたは構造体で、 <xref:System.IObservable%601.Subscribe%2A?displayProperty=nameWithType> という 1 つのメソッドを実装する必要があります。このメソッドは、プロバイダーからの通知を受信するオブザーバーによって呼び出されます。

- オブザーバー。プロバイダーからの通知を受信するオブジェクトです。 オブザーバーは、<xref:System.IObserver%601> インターフェイスを実装するクラスまたは構造体で、 オブザーバーでは、次の 3 つのメソッドを実装する必要があります。これらはすべてプロバイダーによって呼び出されます。

  - <xref:System.IObserver%601.OnNext%2A?displayProperty=nameWithType>。新しい情報または現在の情報をオブザーバーに提供します。

  - <xref:System.IObserver%601.OnError%2A?displayProperty=nameWithType>。エラーが発生したことをオブザーバーに通知します。

  - <xref:System.IObserver%601.OnCompleted%2A?displayProperty=nameWithType>。プロバイダーが通知の送信を完了したことを示します。

- プロバイダーがオブザーバーを追跡するための機構。 通常、プロバイダーでは、<xref:System.Collections.Generic.List%601?displayProperty=nameWithType> オブジェクトなどのコンテナー オブジェクトを使用して、通知にサブスクライブした <xref:System.IObserver%601> の実装への参照を保持します。 このようにストレージ コンテナーを使用すると、プロバイダーで必要に応じていくつでもオブザーバーを処理できます。 オブザーバーが通知を受信する順序は定義されていないため、プロバイダーで自由に順序を決定できます。

- 通知が完了したときにプロバイダーがオブザーバーを削除できるようにするための <xref:System.IDisposable> の実装。 オブザーバーは、<xref:System.IDisposable> の実装への参照を <xref:System.IObservable%601.Subscribe%2A> メソッドから受け取ります。このため、プロバイダーが通知の送信を終了する前に、オブザーバーで <xref:System.IDisposable.Dispose%2A?displayProperty=nameWithType> メソッドを呼び出してサブスクリプションを解除することもできます。

- プロバイダーがオブザーバーに送信するデータを含むオブジェクト。 このオブジェクトの型は、<xref:System.IObservable%601> インターフェイスと <xref:System.IObserver%601> インターフェイスのジェネリック型パラメーターに対応します。 このオブジェクトは、<xref:System.IObservable%601> の実装と同じにすることもできますが、別の型にするのが最も一般的です。

> [!NOTE]
> オブザーバー デザイン パターンを実装するだけでなく、<xref:System.IObservable%601> インターフェイスと <xref:System.IObserver%601> インターフェイスを使用して構築されたライブラリを使用することもできます。 たとえば、[.NET (Rx) の反応拡張](https://docs.microsoft.com/previous-versions/dotnet/reactive-extensions/hh242985(v=vs.103))は、非同期プログラミングをサポートする一連の拡張メソッドと LINQ 標準シーケンス演算子で構成します。

## <a name="implementing-the-pattern"></a>パターンの実装

次の例では、オブザーバー デザイン パターンを使用して、空港の手荷物受取所の情報システムを実装します。 `BaggageInfo` クラスは、到着便と、各便の手荷物の受け取り場所に関する情報を提供します。 これを次の例に示します。

[!code-csharp[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#1)]
[!code-vb[Conceptual.ObserverDesignPattern#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#1)]

`BaggageHandler` クラスは、到着便と手荷物の受け取り場所に関する情報を受け取るクラスです。 その内部では、次の 2 つのコレクションが保持されます。

- `observers` - 更新された情報を受け取るクライアントのコレクション。

- `flights` - 飛行機の便と、各便に割り当てられた手荷物の受け取り場所のコレクション。

どちらのコレクションも、<xref:System.Collections.Generic.List%601> クラスのコンストラクターでインスタンス化されるジェネリック `BaggageHandler` オブジェクトによって表されます。 `BaggageHandler` クラスのソース コードを次の例に示します。

[!code-csharp[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#2)]
[!code-vb[Conceptual.ObserverDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#2)]

更新された情報を受け取るクライアントは、`BaggageHandler.Subscribe` メソッドを呼び出します。 以前に通知をサブスクライブしたことがないクライアントの場合は、クライアントの <xref:System.IObserver%601> の実装への参照が `observers` コレクションに追加されます。

オーバーロードされた `BaggageHandler.BaggageStatus` メソッドは、特定の便の手荷物が処理中であるか処理済みになったかを通知するために呼び出されます。 処理中の場合は、飛行機の便の番号、出発空港、および手荷物の受け取り場所がメソッドに渡されます。 処理済みの場合は、飛行機の便の番号のみがメソッドに渡されます。 手荷物が処理中の場合、メソッドは、渡された `BaggageInfo` の情報が `flights` コレクションに存在するかどうかを確認します。 存在しない場合は、メソッドによりその情報が追加され、各オブザーバーの `OnNext` メソッドが呼び出されます。 荷物を降ろさない便の場合、メソッドは、その便の情報が `flights` コレクションに格納されているかどうかを確認します。 格納されている場合、メソッドは、各オブザーバーの `OnNext` メソッドを呼び出し、`BaggageInfo` コレクションから `flights` オブジェクトを削除します。

1 日の最終便が到着して手荷物の処理が完了すると、`BaggageHandler.LastBaggageClaimed` メソッドが呼び出されます。 このメソッドは、各オブザーバーの `OnCompleted` メソッドを呼び出して、すべての通知が完了したことを示します。その後、`observers` コレクションをクリアします。

プロバイダーの <xref:System.IObservable%601.Subscribe%2A> メソッドが返す <xref:System.IDisposable> の実装を使用すると、<xref:System.IObserver%601.OnCompleted%2A> メソッドが呼び出される前に、オブザーバーで通知の受信を中止できます。 この `Unsubscriber(Of BaggageInfo)` クラスのソース コードを次の例に示します。 このクラスが `BaggageHandler.Subscribe` メソッドでインスタンス化されるときに、`observers` コレクションへの参照と、コレクションに追加されるオブザーバーへの参照が渡されます。 これらの参照はローカル変数に割り当てられます。 オブジェクトの `Dispose` メソッドが呼び出されると、オブザーバーがまだ `observers` コレクションに存在するかどうかが確認され、存在する場合、そのオブザーバーは削除されます。

[!code-csharp[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/provider.cs#3)]
[!code-vb[Conceptual.ObserverDesignPattern#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/provider.vb#3)]

次の例は、<xref:System.IObserver%601> という名前の `ArrivalsMonitor` の実装を示しています。これは、手荷物受取所の情報を表示する基本クラスです。 情報は、出発地名のアルファベット順に表示されます。 `ArrivalsMonitor` のメソッドは、`overridable` (Visual Basic) または `virtual` (C#) としてマークされているため、派生クラスによりすべてオーバーライドできます。

[!code-csharp[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/observer.cs#4)]
[!code-vb[Conceptual.ObserverDesignPattern#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/observer.vb#4)]

`ArrivalsMonitor` クラスには、`Subscribe` メソッドと `Unsubscribe` メソッドが含まれています。 `Subscribe` メソッドにより、このクラスは、<xref:System.IDisposable> の呼び出しから返された <xref:System.IObservable%601.Subscribe%2A> の実装をプライベート変数に保存できます。 `Unsubscribe` メソッドにより、このクラスは、プロバイダーの <xref:System.IDisposable.Dispose%2A> の実装を呼び出して通知のサブスクリプションを解除できます。 `ArrivalsMonitor` は、<xref:System.IObserver%601.OnNext%2A>、<xref:System.IObserver%601.OnError%2A>、<xref:System.IObserver%601.OnCompleted%2A> の各メソッドの実装も提供します。 そのうち、かなりの量のコードが含まれるのは <xref:System.IObserver%601.OnNext%2A> の実装だけです。 このメソッドは、到着便の出発空港と手荷物の受け取り場所に関する情報を並べ替えて保持する、プライベートなジェネリック <xref:System.Collections.Generic.List%601> オブジェクトを操作します。 `BaggageHandler` クラスから新しい便の到着が報告されると、<xref:System.IObserver%601.OnNext%2A> メソッドの実装はその便に関する情報をリストに追加します。 `BaggageHandler` クラスからその便の手荷物の処理が完了したことが報告されると、<xref:System.IObserver%601.OnNext%2A> メソッドはその便をリストから削除します。 変更が発生するたびに、リストが並べ替えられてコンソールに表示されます。

次の例に含まれているアプリケーション エントリ ポイントは、`BaggageHandler` クラスのインスタンスと、`ArrivalsMonitor` クラスの 2 つのインスタンスを作成し、`BaggageHandler.BaggageStatus` メソッドを使用して到着便に関する情報を追加および削除します。 いずれの場合も、オブザーバーが更新を受け取って、手荷物受取所の情報を正しく表示します。

[!code-csharp[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.observerdesignpattern/cs/program.cs#5)]
[!code-vb[Conceptual.ObserverDesignPattern#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.observerdesignpattern/vb/module1.vb#5)]

## <a name="related-topics"></a>関連トピック

|タイトル|[説明]|
|-----------|-----------------|
|[オブザーバー デザイン パターンのベスト プラクティス](../../../docs/standard/events/observer-design-pattern-best-practices.md)|オブザーバー デザイン パターンを実装するアプリケーションを開発するときのベスト プラクティスについて説明します。|
|[方法: プロバイダーを実装する](../../../docs/standard/events/how-to-implement-a-provider.md)|温度監視アプリケーションのプロバイダーを実装するための手順について説明します。|
|[方法: オブザーバーを実装する](../../../docs/standard/events/how-to-implement-an-observer.md)|温度監視アプリケーションのオブザーバーを実装するための手順について説明します。|
