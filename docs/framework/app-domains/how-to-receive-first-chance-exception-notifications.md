---
title: '方法: 初回例外通知を受け取る'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- first-chance exception notifications
- exceptions, first chance notifications
ms.assetid: 66f002b8-a97d-4a6e-a503-2cec01689113
ms.openlocfilehash: da60dfca424f7c10e810b5e083ff7fa63a688218
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73119875"
---
# <a name="how-to-receive-first-chance-exception-notifications"></a>方法: 初回例外通知を受け取る
<xref:System.AppDomain> クラスの <xref:System.AppDomain.FirstChanceException>イベントを使用すると、共通言語ランタイムが例外ハンドラーの検索を開始する前に、例外がスローされたことを知らせる通知を受け取ることができます。

 このイベントは、アプリケーション ドメイン レベルで発生します。 実行スレッドは複数のアプリケーション ドメインを通過する可能性があるため、あるアプリケーション ドメインでハンドルされない例外が別のアプリケーション ドメインでハンドルされることもあります。 通知は、イベントのハンドラーを追加した各アプリケーション ドメインで発生し、いずれかのアプリケーション ドメインで例外がハンドルされるまで続行されます。

 ここで紹介するプロシージャと例では、1 つのアプリケーション ドメインを実装する単純なプログラムで初回例外通知を受け取る方法と、作成したアプリケーション ドメインで初回例外通知を受け取る方法について説明します。

 複数のアプリケーション ドメインにわたる複雑な例については、<xref:System.AppDomain.FirstChanceException> イベントの例を参照してください。

## <a name="receiving-first-chance-exception-notifications-in-the-default-application-domain"></a>既定のアプリケーション ドメインで初回例外通知を受け取る
 次のプロシージャでは、アプリケーションのエントリ ポイントである `Main()`メソッドが既定のアプリケーション ドメインで実行されます。

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-default-application-domain"></a>既定のアプリケーション ドメインで初回例外通知の動作を確認するには

1. ラムダ関数を使用して <xref:System.AppDomain.FirstChanceException> イベントのイベント ハンドラーを定義し、それをイベントにアタッチします。 この例では、イベント ハンドラーによって、イベントが処理されたアプリケーション ドメインの名前と、例外の <xref:System.Exception.Message%2A> プロパティを出力します。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#2)]

2. 例外をスローし、それをキャッチします。 ランタイムが例外ハンドラーを見つける前に、<xref:System.AppDomain.FirstChanceException> イベントが発生し、メッセージが表示されます。 このメッセージの後に、例外ハンドラーによるメッセージが表示されます。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#3)]

3. 例外をスローしますが、それをキャッチしません。 ランタイムが例外ハンドラーを検索する前に、<xref:System.AppDomain.FirstChanceException> イベントが発生し、メッセージが表示されます。 例外ハンドラーがないため、アプリケーションは終了します。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#4)]

     このプロシージャの最初の 3 つのステップに示されているコードは、完全なコンソール アプリケーションを形成します。 既定のアプリケーション ドメインの名前は .exe ファイルの名前と拡張子から構成されるため、アプリケーションからの出力は .exe ファイルの名前によって異なります。 次のサンプル出力を参照してください。

     [!code-csharp[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto_simple#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto_simple/vb/example.vb#5)]

## <a name="receiving-first-chance-exception-notifications-in-another-application-domain"></a>別のアプリケーション ドメインで初回例外通知を受け取る
 プログラムに複数のアプリケーション ドメインが含まれている場合は、通知を受け取るアプリケーション ドメインを選択できます。

#### <a name="to-receive-first-chance-exception-notifications-in-an-application-domain-that-you-create"></a>作成したアプリケーション ドメインで初回例外通知を受け取るには

1. <xref:System.AppDomain.FirstChanceException> イベントのイベント ハンドラーを定義します。 この例では、`static` メソッド (Visual Basic では `Shared` メソッド) を使用して、イベントが処理されたアプリケーション ドメインの名前と、例外の <xref:System.Exception.Message%2A> プロパティを出力します。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#3)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#3)]

2. アプリケーション ドメインを作成し、そのアプリケーション ドメインの <xref:System.AppDomain.FirstChanceException> イベントにイベント ハンドラーを追加します。 この例では、アプリケーション ドメインの名前は `AD1` です。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#2)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#2)]

     既定のアプリケーション ドメインでも、同じようにこのイベントを処理できます。 既定のアプリケーション ドメインへの参照を取得するには、`static` (Visual Basic では `Shared`) <xref:System.AppDomain.CurrentDomain%2A?displayProperty=nameWithType> プロパティを `Main()` で使用します。

#### <a name="to-demonstrate-first-chance-exception-notifications-in-the-application-domain"></a>アプリケーション ドメインで初回例外通知の動作を確認するには

1. 前のプロシージャで作成したアプリケーション ドメインに `Worker` オブジェクトを作成します。 `Worker` クラスは、パブリックで、<xref:System.MarshalByRefObject> から派生する必要があります。この記事の最後にある完全な例を参照してください。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#4)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#4)]

2. 例外をスローする `Worker` オブジェクトのメソッドを呼び出します。 この例では、`Thrower` メソッドが 2 回呼び出されます。 1 回目のメソッド引数は `true` であるため、メソッドは自身の例外をキャッチします。 2 回目の引数は `false` です。この場合、`Main()` メソッドが既定のアプリケーション ドメインで例外をキャッチします。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#6)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#6)]

3. `Thrower` メソッドにコードを追加して、メソッドが自身の例外をハンドルするかどうかを制御します。

     [!code-csharp[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#5)]
     [!code-vb[System.AppDomain.FirstChanceException_howto#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#5)]

## <a name="example"></a>例
 次の例では、`AD1` という名前のアプリケーション ドメインを作成し、このアプリケーション ドメインの <xref:System.AppDomain.FirstChanceException> イベントにイベント ハンドラーを追加します。 この例では、アプリケーション ドメインに `Worker` クラスのインスタンスを作成し、`Thrower` という名前のメソッドを呼び出します。このメソッドにより、<xref:System.ArgumentException>がスローされます。 メソッドは、引数の値に応じて、例外をキャッチするか、例外のハンドルに失敗します。

 `Thrower` メソッドが `AD1` で例外をスローするたびに、`AD1` で <xref:System.AppDomain.FirstChanceException> イベントが発生し、イベント ハンドラーによりメッセージが表示されます。 次に、ランタイムによって例外ハンドラーが検索されます。 最初のケースでは、例外ハンドラーは `AD1` で見つかります。 2 番目のケースでは、例外は `AD1` でハンドルされず、代わりに既定のアプリケーション ドメインでキャッチされます。

> [!NOTE]
> 既定のアプリケーション ドメインの名前は、実行可能ファイルの名前と同じです。

 既定のアプリケーション ドメインに <xref:System.AppDomain.FirstChanceException> イベントのハンドラーを追加すると、既定のアプリケーション ドメインが例外をハンドルする前に、イベントが発生して処理されます。 これを確認するには、`Main()` の先頭に C# コード `AppDomain.CurrentDomain.FirstChanceException += FirstChanceException;` (Visual Basic では `AddHandler AppDomain.CurrentDomain.FirstChanceException, FirstChanceException`) を追加します。

 [!code-csharp[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/cs/example.cs#1)]
 [!code-vb[System.AppDomain.FirstChanceException_howto#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.appdomain.firstchanceexception_howto/vb/example.vb#1)]

## <a name="see-also"></a>関連項目

- <xref:System.AppDomain.FirstChanceException>
