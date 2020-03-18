---
title: イベントベースの非同期パターンの実装
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
- AsyncCompletedEventArgs class
ms.assetid: 43402d19-8d30-426d-8785-1a4478233bfa
ms.openlocfilehash: 9865fa169e0776765f9a97ec0a7b4555bf253886
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67663701"
---
# <a name="implementing-the-event-based-asynchronous-pattern"></a>イベントベースの非同期パターンの実装

顕著な遅延が発生する可能性がある操作を伴うクラスを作成する場合は、[イベント ベースの非同期パターン](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-overview.md)を実装することによって、非同期機能を与えることを検討します。

イベント ベースの非同期パターンは、非同期機能を持つクラスをパッケージ化するための標準的な方法を提供します。 <xref:System.ComponentModel.AsyncOperationManager> などのヘルパー クラスで実装しても、クラスは、ASP.NET、コンソール アプリケーション、Windows フォーム アプリケーションを含むすべてのアプリケーション モデルで正常に動作します。

イベント ベースの非同期パターンを実装する例については、「[方法:イベントベースの非同期パターンをサポートするコンポーネントを実装する](../../../docs/standard/asynchronous-programming-patterns/component-that-supports-the-event-based-asynchronous-pattern.md)」を参照してください。

単純な非同期操作の場合は、<xref:System.ComponentModel.BackgroundWorker> コンポーネントが適切である可能性があります。 <xref:System.ComponentModel.BackgroundWorker> の詳細については、「[方法:バックグラウンドで操作を実行する](../../../docs/framework/winforms/controls/how-to-run-an-operation-in-the-background.md)」を参照してください。

次の一覧は、このトピックで説明するイベント ベースの非同期パターンの機能を示しています。

- イベントベースの非同期パターンを実装するチャンス

- 非同期メソッドの名前付け

- キャンセル処理の任意のサポート

- IsBusy プロパティの任意のサポート

- 進行状況レポートの任意のサポート

- 増分の結果を返すことの任意のサポート

- メソッドでの Out パラメーターと Ref パラメーターの処理

## <a name="opportunities-for-implementing-the-event-based-asynchronous-pattern"></a>イベントベースの非同期パターンを実装するチャンス

以下に該当する場合は、イベントベースの非同期パターンの実装を検討します。

- クラスのクライアントには、非同期操作に使用できる <xref:System.Threading.WaitHandle> と <xref:System.IAsyncResult> オブジェクトは必要ありません。つまり、ポーリング、<xref:System.Threading.WaitHandle.WaitAll%2A> または <xref:System.Threading.WaitHandle.WaitAny%2A> は、クライアントによってビルドされる必要があるということです。

- クライアントでの非同期操作を、使い慣れたイベント/デリゲート モデルを使用して管理したい。

どの操作も非同期を実装するための候補になりますが、長い待機時間が予想される操作を考慮してください。 特に適切なのは、クライアントがメソッドを呼び出し、完了時に通知を受け取ること以外の介入を必要としない操作です。 継続的に実行され、進捗状況、増分結果、または状態の変更をクライアントに定期的に通知する操作も適しています。

イベント ベースの非同期パターンをいつサポートするかを決定することの詳細については、「[イベントベースの非同期パターンの実装時期を決定する](../../../docs/standard/asynchronous-programming-patterns/deciding-when-to-implement-the-event-based-asynchronous-pattern.md)」 (イベントベースの非同期パターンをいつ実装するかの決定) を参照してください。

## <a name="naming-asynchronous-methods"></a>非同期メソッドの名前付け

対応する非同期メソッドを用意する同期メソッド *MethodName* に対して、次の操作を行います。

以下を実行する _MethodName_**Async** メソッドを定義します。

- `void` を返します。

- *MethodName* メソッドと同じパラメーターを受け取ります。

- 複数の呼び出しを受け入れます。

必要に応じて、_MethodName_**Async** と同一の _MethodName_**Async** オーバーロードを定義しますが、`userState` という追加のオブジェクト値パラメーターを使用して定義します。 これは、メソッドの複数の同時呼び出しを管理するために実行します。この場合、メソッドの呼び出しを区別するために `userState` 値がすべてのイベント ハンドラーに返されます。 単純に後で取得するためにユーザーの状態を格納する場所として実行することもできます。

個別の _MethodName_**Async** メソッドのシグネチャに対して、次の手順を実行します。

1. メソッドと同じクラス内に次のイベントを定義します。

    ```vb
    Public Event MethodNameCompleted As MethodNameCompletedEventHandler
    ```

    ```csharp
    public event MethodNameCompletedEventHandler MethodNameCompleted;
    ```

2. 次のデリゲートと <xref:System.ComponentModel.AsyncCompletedEventArgs> を定義します。 これらは、クラス自体の外部に定義される可能性がありますが、同じ名前空間に定義されます。

    ```vb
    Public Delegate Sub MethodNameCompletedEventHandler( _
        ByVal sender As Object, _
        ByVal e As MethodNameCompletedEventArgs)

    Public Class MethodNameCompletedEventArgs
        Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As MyReturnType
    End Property
    ```

    ```csharp
    public delegate void MethodNameCompletedEventHandler(object sender,
        MethodNameCompletedEventArgs e);

    public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
    {
        public MyReturnType Result { get; }
    }
    ```

    - _MethodName_**CompletedEventArgs** クラスで、フィールドではなくメンバーを読み取り専用プロパティとして公開していることを確認します。これは、フィールドはデータ バインドを妨げるためです。

    - 結果を生成しないメソッドに <xref:System.ComponentModel.AsyncCompletedEventArgs> の派生クラスを定義しないでください。 単純に <xref:System.ComponentModel.AsyncCompletedEventArgs> のインスタンス自体を使用します。

      > [!NOTE]
      > 実行可能かつ適切な場合、デリゲートと <xref:System.ComponentModel.AsyncCompletedEventArgs> 型を再利用することはまったく問題ありません。 この場合、特定のデリゲートと <xref:System.ComponentModel.AsyncCompletedEventArgs> が 1 つのメソッドに関連付けられることがないため、メソッド名のような名前の一貫性はなくなります。

## <a name="optionally-support-cancellation"></a>キャンセル処理の任意のサポート

クラスで非同期操作のキャンセルをサポートする場合は、以下に示すようにキャンセルをクライアントに公開する必要があります。 キャンセルのサポートを定義する前に、以下の 2 点について判断する必要があることに注意してください。

- 今後の予想される追加機能を含め、クラスには、キャンセルをサポートする非同期操作が 1 つだけあるかどうか。

- キャンセルをサポートする非同期操作が、複数の保留中の操作をサポートできるかどうか。 これは、_MethodName_**Async** メソッドが `userState` パラメーターを受け取り、いずれかの呼び出しが終了するまで待機する前に、複数の呼び出しを許可できるかどうかを意味します。

これら 2 つの質問に対する回答を次の表に当てはめて、キャンセル メソッドで使用するシグネチャを決定します。

### <a name="visual-basic"></a>Visual Basic

||複数の同時操作をサポート|一度に 1 つだけの操作|
|------|------------------------------------------------|----------------------------------|
|クラス全体で 1 つの非同期操作|`Sub MethodNameAsyncCancel(ByVal userState As Object)`|`Sub MethodNameAsyncCancel()`|
|クラスの複数の非同期操作|`Sub CancelAsync(ByVal userState As Object)`|`Sub CancelAsync()`|

### <a name="c"></a>C\#

||複数の同時操作をサポート|一度に 1 つだけの操作|
|------|------------------------------------------------|----------------------------------|
|クラス全体で 1 つの非同期操作|`void MethodNameAsyncCancel(object userState);`|`void MethodNameAsyncCancel();`|
|クラスの複数の非同期操作|`void CancelAsync(object userState);`|`void CancelAsync();`|

`CancelAsync(object userState)` メソッドを定義する場合、クライアントは、1 つの非同期メソッドのすべての呼び出しの間だけではなく、オブジェクトに対して呼び出されたすべての非同期メソッドの間で区別できるようにする状態値を慎重に選択する必要があります。

1 つの非同期操作のバージョンの _MethodName_**AsyncCancel** という名前は、Visual Studio の IntelliSense のようなデザイン環境でそのメソッドをより簡単に検出できるようにするために、このように決定されています。 これにより、関連するメンバーがグループ化され、非同期機能とは無関係の他のメンバーとは区別されます。 今後のバージョンで非同期操作を追加する可能性がある場合は、`CancelAsync` を定義することをお勧めします。

同じクラスに上の表の複数のメソッドを定義しないでください。 それは意味のない行為であるか、メソッドの増加によってクラス インターフェイスがわかりにくくなります。

これらのメソッドは、通常は直ちに結果が戻り、操作が実際にキャンセルされる場合もキャンセルされない場合もあります。 _MethodName_**Completed** イベント用のイベント ハンドラーでは、_MethodName_**CompletedEventArgs** オブジェクトに `Cancelled` フィールドが含まれます。クライアントはこれを使用して、キャンセルが発生したかどうかを判断できます。

「[イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)」に説明されているキャンセルのセマンティクスに従ってください。

## <a name="optionally-support-the-isbusy-property"></a>IsBusy プロパティの任意のサポート

クラスが複数の同時呼び出しをサポートしない場合は、`IsBusy` プロパティを公開することを検討してください。 開発者は、このプロパティによって、_MethodName_**Async** メソッドが _MethodName_**Async** メソッドからの例外をキャッチせずに実行されているかどうかを判断できます。

「[イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)」に説明されている `IsBusy` のセマンティクスに従ってください。

## <a name="optionally-provide-support-for-progress-reporting"></a>進行状況レポートの任意のサポート

非同期操作の操作中に進行状況を報告することをお勧めします。 イベント ベースの非同期パターンには、これを行うためのガイドラインがあります。

- 必要に応じて、非同期操作によって発生し、適切なスレッドで呼び出されるイベントを定義します。 <xref:System.ComponentModel.ProgressChangedEventArgs> オブジェクトは、0 ～ 100 の範囲であることが期待されている整数値の進行状況インジケーターを伝達します。

- このイベントには、次のように名前を付けます。

  - `ProgressChanged`: クラスに複数の非同期操作がある (または将来のバージョンで複数の非同期操作を含めることが予想される) 場合。

  - _MethodName_**ProgressChanged** クラスに非同期操作が 1 つだけある場合。

  この名前の付け方は、「キャンセルの任意のサポート」セクションで説明した、キャンセル メソッドに対する方法と対応しています。

このイベントは、<xref:System.ComponentModel.ProgressChangedEventHandler> デリゲート シグネチャと <xref:System.ComponentModel.ProgressChangedEventArgs> クラスを使用する必要があります。 別の方法として、ドメイン固有の進行状況インジケーター (読み取られたバイト数やダウンロード操作の合計バイト数など) を提供できる場合は、<xref:System.ComponentModel.ProgressChangedEventArgs> の派生クラスを定義します。

サポートされる非同期メソッドの数にかかわらず、クラスの `ProgressChanged` または _MethodName_**ProgressChanged** イベントは 1 つしかないことに注意してください。 クライアントは、_MethodName_**Async** メソッドに渡される `userState` オブジェクトを使用して、複数の同時操作に対する進行状況の更新を区別することが期待されています。

複数の操作で進行状況がサポートされ、それぞれが異なる進行状況インジケーターを返す状況が発生することがあります。 この場合は、1 つの `ProgressChanged` イベントでは不適切であり、複数の `ProgressChanged` のサポートを検討することができます。 この場合は、各 _MethodName_**Async** メソッドに対して、_MethodName_**ProgressChanged** の名前付けパターンを使用します。

「[イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)」に説明されている進行状況報告のセマンティクスに従ってください。

## <a name="optionally-provide-support-for-returning-incremental-results"></a>増分の結果を返すことの任意のサポート

場合によっては、非同期操作が完了する前に、増分結果が返ることがあります。 このシナリオをサポートするために使用できるオプションは多数あります。 以下に、いくつかの例を示します。

### <a name="single-operation-class"></a>単一操作クラス

クラスが 1 つの非同期操作のみをサポートし、その操作が増分結果を返すことができる場合は、次のようにします。

- <xref:System.ComponentModel.ProgressChangedEventArgs> 型を増分結果データを伝達するように拡張し、この拡張されたデータを使用する _MethodName_**ProgressChanged** イベントを定義します。

- 報告する増分結果があるときに、この _MethodName_**ProgressChanged** イベントを発生させます。

このソリューションは、_MethodName_**ProgressChanged** イベントと同じように、同じイベントの発生で "すべての操作" に対して増分結果を返しても何の問題もないため、特に単一非同期操作のクラスに適用されます。

### <a name="multiple-operation-class-with-homogeneous-incremental-results"></a>同じ型の増分結果を持つ複数操作クラス

この場合、クラスは、それぞれが増分結果を返すことができる複数の非同期メソッドをサポートし、増分結果はすべて同じ型のデータを持っています。

同じ <xref:System.EventArgs> 構造体がすべての増分結果で機能するため、上記の単一操作クラスで説明したモデルに従います。 複数の非同期メソッドに適用するため、_MethodName_**ProgressChanged** イベントの代わりに `ProgressChanged` イベントを定義します。

### <a name="multiple-operation-class-with-heterogeneous-incremental-results"></a>異なる型の増分結果を持つ複数操作クラス

クラスが複数の非同期メソッドをサポートし、それぞれが異なる型のデータを返す場合は、以下を行います。

- 増分結果の報告と進行状況の報告を分離します。

- 各非同期メソッドの増分結果データを処理する個別の _MethodName_**ProgressChanged** イベントを、適切な <xref:System.EventArgs> で定義します。

「[イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)」に説明されているように、そのイベント ハンドラーを適切なスレッドで呼び出します。

## <a name="handling-out-and-ref-parameters-in-methods"></a>メソッドでの Out パラメーターと Ref パラメーターの処理

一般に、.NET Framework では、`out` と `ref` の使用は避けることを推奨していますが、それらが存在するときのルールを次に示します。

非同期メソッド *MethodName* の場合:

- *MethodName* に対する `out` パラメーターは、_MethodName_**Async** の一部にしないでください。 代わりに、*MethodName* での同等のパラメーターと同じ名前の _MethodName_**CompletedEventArgs** の一部にする必要があります (より適切な名前がない限り)。

- *MethodName* に対する `ref` パラメーターは、_MethodName_**Async** の一部として出現し、*MethodName* での同等のパラメーターと同じ名前の _MethodName_**CompletedEventArgs** の一部として出現する必要があります (より適切な名前がない限り)。

次に例を示します。

```vb
Public Function MethodName(ByVal arg1 As String, ByRef arg2 As String, ByRef arg3 As String) As Integer
```

```csharp
public int MethodName(string arg1, ref string arg2, out string arg3);
```

非同期メソッドとその <xref:System.ComponentModel.AsyncCompletedEventArgs> クラスは、次のようになります。

```vb
Public Sub MethodNameAsync(ByVal arg1 As String, ByVal arg2 As String)

Public Class MethodNameCompletedEventArgs
    Inherits System.ComponentModel.AsyncCompletedEventArgs
    Public ReadOnly Property Result() As Integer
    End Property
    Public ReadOnly Property Arg2() As String
    End Property
    Public ReadOnly Property Arg3() As String
    End Property
End Class
```

```csharp
public void MethodNameAsync(string arg1, string arg2);

public class MethodNameCompletedEventArgs : System.ComponentModel.AsyncCompletedEventArgs
{
    public int Result { get; };
    public string Arg2 { get; };
    public string Arg3 { get; };
}
```

## <a name="see-also"></a>関連項目

- <xref:System.ComponentModel.ProgressChangedEventArgs>
- <xref:System.ComponentModel.AsyncCompletedEventArgs>
- [方法: イベントベースの非同期パターンをサポートするコンポーネントを実装する](../../../docs/standard/asynchronous-programming-patterns/component-that-supports-the-event-based-asynchronous-pattern.md)
- [方法: バックグラウンドで操作を実行する](../../../docs/framework/winforms/controls/how-to-run-an-operation-in-the-background.md)
- [方法: バックグラウンド操作を使用するフォームを実装する](../../../docs/framework/winforms/controls/how-to-implement-a-form-that-uses-a-background-operation.md)
- [イベントベースの非同期パターンをいつ実装するかの決定](../../../docs/standard/asynchronous-programming-patterns/deciding-when-to-implement-the-event-based-asynchronous-pattern.md)
- [イベントベースの非同期パターンを実装するための推奨される手順](../../../docs/standard/asynchronous-programming-patterns/best-practices-for-implementing-the-event-based-asynchronous-pattern.md)
- [イベント ベースの非同期パターン (EAP)](../../../docs/standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)
