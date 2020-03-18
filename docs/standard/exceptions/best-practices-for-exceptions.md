---
title: 例外の推奨事項 - .NET
ms.date: 12/05/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- exceptions, best practices
ms.assetid: f06da765-235b-427a-bfb6-47cd219af539
ms.openlocfilehash: 1de231b01e3fa97e78a87ae6b0595a9b5536374e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78160171"
---
# <a name="best-practices-for-exceptions"></a>例外の推奨事項

適切にデザインされたアプリケーションは、アプリケーションのクラッシュを防ぐために、例外やエラーを処理します。 このセクションでは、例外の処理と作成のためのベスト プラクティスについて説明します。

## <a name="use-trycatchfinally-blocks-to-recover-from-errors-or-release-resources"></a>try/catch/finally ブロックを使用し、エラーから回復させるか、リソースを解放する

例外が発生する可能性があるコードの前後に `try`/`catch` を使用します。***すると***、その例外からコードは回復できます。 `catch` ブロックでは、例外を常に最も強い派生型から最も弱い派生型の順序で並べ替えます。 すべての例外は <xref:System.Exception> から派生します。 最も強い派生型の例外は、基礎例外クラスの catch 句に続く catch 句では処理されません。 コードが例外から回復できないとき、その例外はキャッチしないでください。 可能であれば、回復させる呼び出し履歴のずっと上でメソッドを有効にします。

`using` ステートメントか `finally` ブロックで割り当てられているリソースをクリーンアップします。 例外がスローされたとき、リソースを自動クリーンアップするには `using` ステートメントを選択します。 `finally` を実装しないリソースをクリーンアップするには <xref:System.IDisposable> ブロックを使用します。 `finally` 句のコードは常に、例外がスローされるときでも実行されます。

## <a name="handle-common-conditions-without-throwing-exceptions"></a>例外をスローせずに一般的な状態を処理する

発生する可能性があり、例外をトリガーする可能性がある状態に対し、例外を回避する方法で処理することを検討します。 たとえば、既に終了している接続を終了しようとすると、`InvalidOperationException` を受け取ります。 `if` ステートメントを使用して、終了しようとする前に接続状態を確認することで、これを回避することができます。

[!code-cpp[Conceptual.Exception.Handling#2](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#2)]
[!code-csharp[Conceptual.Exception.Handling#2](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#2)]
[!code-vb[Conceptual.Exception.Handling#2](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#2)]

終了する前に接続状態を確認しない場合は、`InvalidOperationException` 例外をキャッチする可能性があります。

[!code-cpp[Conceptual.Exception.Handling#3](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#3)]
[!code-csharp[Conceptual.Exception.Handling#3](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#3)]
[!code-vb[Conceptual.Exception.Handling#3](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#3)]

選択するメソッドは、予期されるイベント発生頻度によって決まります。

- イベントが頻繁には発生しない場合、つまり、イベントが本当に例外的であり、エラー (予期しないファイルの終端の検出など) を示す場合に、例外処理を使用します。 例外処理を使用すると、通常の状況では、実行されるコードが少なくなります。

- イベントが定期的に発生し、通常の実行の一部であると見なせる場合は、コード内でエラー条件をチェックします。 一般的なエラー条件をチェックするときに、例外を回避するためにより少ないコードが実行されます。

## <a name="design-classes-so-that-exceptions-can-be-avoided"></a>例外を回避するようにクラスを設計する

クラスは、例外をトリガーする呼び出しを行うことを回避できるようにするメソッドとプロパティを提供できます。 たとえば、<xref:System.IO.FileStream> クラスには、ファイルの終端に到達したかどうかを判別するために役立つメソッドが用意されています。 これらは、ファイルの終端を越えて読み取りを実行しようとした場合にも例外がスローされないようにするために使用できます。 次の例では、例外をトリガーすることなく、ファイルの末尾まで読み取る方法を示します。

[!code-cpp[Conceptual.Exception.Handling#5](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#5)]
[!code-csharp[Conceptual.Exception.Handling#5](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#5)]
[!code-vb[Conceptual.Exception.Handling#5](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#5)]

例外が返されるのを回避するもう 1 つの方法は、非常に一般的なエラーの場合に、例外をスローする代わりに null (または既定値) を返すことです。 非常に一般的なエラーは、通常の制御の流れと見なすことができます。 このような場合は、null (または既定値) を返すことによって、アプリケーションのパフォーマンスへの影響を最小限に抑えることができます。

値の型の場合、`Nullable<T>` または既定値をエラー インジケーターとして使用するかどうかを特定のアプリに関して検討します。 `Nullable<Guid>` を使用すると、`default` は `null` ではなく `Guid.Empty` になります。 `Nullable<T>` を追加すると、値があるときとないときがはっきりすることがあります。 `Nullable<T>` を追加すると、不要な確認事項が増え、潜在的なエラーの原因にしかならないこともあります。

## <a name="throw-exceptions-instead-of-returning-an-error-code"></a>エラー コードを返す代わりに、例外をスローする

呼び出しのコードはリターン コードを確認しないので、例外によって、エラーを見過ごさないようにします。

## <a name="use-the-predefined-net-exception-types"></a>事前定義済みの .NET の例外の種類を使用する

事前定義の例外クラスが適用されない場合に限り、新しい例外クラスを導入します。 次に例を示します。

- オブジェクトの現在の状態に対して、プロパティの設定またはメソッドの呼び出しが適切でない場合は、<xref:System.InvalidOperationException> をスローします。

- <xref:System.ArgumentException> 例外または <xref:System.ArgumentException> から派生する定義済みのクラスの 1 つをスローします。

## <a name="end-exception-class-names-with-the-word-exception"></a>例外クラス名の末尾に `Exception` という単語を付加する

カスタム例外が必要な場合は、適切に名前を付け、<xref:System.Exception> クラスから派生させます。 次に例を示します。

[!code-cpp[Conceptual.Exception.Handling#4](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#4)]
[!code-csharp[Conceptual.Exception.Handling#4](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#4)]
[!code-vb[Conceptual.Exception.Handling#4](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#4)]

## <a name="include-three-constructors-in-custom-exception-classes"></a>カスタム例外クラスに 3 つのコンストラクターを含める

独自の例外クラスを作成するときに、少なくとも 3 つの共通コンストラクターを使用します。それらは、パラメーターなしのコンストラクター、文字列メッセージを受け取るコンストラクター、および文字列メッセージと内部例外を受け取るコンストラクターです。

- 既定の値を使用する <xref:System.Exception.%23ctor>。

- 文字列メッセージを受け取る <xref:System.Exception.%23ctor%28System.String%29>。

- 文字列メッセージと内部例外を受け取る <xref:System.Exception.%23ctor%28System.String%2CSystem.Exception%29>。

例については、「[方法: ユーザー定義の例外を作成する](how-to-create-user-defined-exceptions.md)」を参照してください。

## <a name="ensure-that-exception-data-is-available-when-code-executes-remotely"></a>コードがリモートで実行されるときに、例外データが利用できるようにする

ユーザー定義例外を作成するときには、リモートで実行されるコードで例外のメタデータを使用できるようにします。

たとえば、アプリ ドメインをサポートする .NET 実装では、アプリ ドメイン間で例外が発生する可能性があります。 アプリ ドメイン A が、例外をスローするコードを実行するアプリ ドメイン B を作成するとします。 アプリ ドメイン A が例外を適切にキャッチして処理するには、アプリ ドメイン B によりスローされた例外が含まれているアセンブリを検出できる必要があります。アプリ ドメイン B が、アプリ ドメイン A のアプリケーション ベースではなく、自身のアプリケーション ベースの下のアセンブリに含まれている例外をスローする場合、アプリ ドメイン A は、例外を検出できなくなり、共通言語ランタイムが <xref:System.IO.FileNotFoundException> 例外をスローします。 このような状況を回避するには、例外情報が格納されているアセンブリを次のいずれかの方法で配置します。

- 2 つのアプリ ドメインが共有する共通アプリケーション ベースにアセンブリを配置する。

    \- または

- ドメインが共通アプリケーション ベースを共有していない場合には、例外情報が格納されているアセンブリに厳密な名前で署名し、グローバル アセンブリ キャッシュにこのアセンブリを配置する。

## <a name="use-grammatically-correct-error-messages"></a>文法的に正しいエラー メッセージを使用する

明確な文を記述し、末尾に句点を含めます。 <xref:System.Exception.Message?displayProperty=nameWithType>プロパティに割り当てられた文字列のそれぞれの文がピリオドで終わる必要があります。 たとえば、"ログ テーブルがオーバーフローしました。" は、 適切なメッセージ文字列です。

## <a name="include-a-localized-string-message-in-every-exception"></a>すべての例外に、ローカライズした文字列メッセージを含める

ユーザーに対して表示されるエラー メッセージは、例外クラスの名前ではなく、スローされた例外の <xref:System.Exception.Message?displayProperty=nameWithType> プロパティから派生されます。 通常は、メッセージ文字列を<xref:System.Exception.Message?displayProperty=nameWithType>例外コンストラクター`message`の [ 引数に渡すことで、](xref:System.Exception.%23ctor%2A) プロパティに値を割り当てます。

ローカライズされたアプリケーションの場合は、アプリケーションがスローできるすべての例外に、ローカライズされたメッセージ文字列を指定する必要があります。 ローカライズされたエラー メッセージを指定するには、リソース ファイルを使用します。 アプリケーションのローカライズとローカライズされた文字列の取得の詳細については、次の記事を参照してください。

- [方法: ローカライズされた例外メッセージを使用するユーザー定義の例外を作成する](how-to-create-localized-exception-messages.md)
- [デスクトップ アプリケーションのリソース](../../framework/resources/index.md)
- <xref:System.Resources.ResourceManager?displayProperty=nameWithType>

## <a name="in-custom-exceptions-provide-additional-properties-as-needed"></a>カスタム例外で、必要に応じて追加のプロパティを提供する

プログラミングの点で追加情報が役立つ場合にだけ、(カスタム メッセージ文字列以外の) 例外の追加プロパティを含めてください。 たとえば、<xref:System.IO.FileNotFoundException> には <xref:System.IO.FileNotFoundException.FileName> プロパティがあります。

## <a name="place-throw-statements-so-that-the-stack-trace-will-be-helpful"></a>スタック トレースが役に立つように throw ステートメントを配置する

例外がスローされたステートメントからスタック トレースが開始され、例外をキャッチした `catch` ステートメントでトレースが終了します。

## <a name="use-exception-builder-methods"></a>例外ビルダー メソッドを使用する

一般に、クラスはクラス実装内の複数の位置で同一の例外をスローします。 コードが長くなることを防ぐため、例外を作成して返すヘルパー メソッドを使用します。 次に例を示します。

[!code-cpp[Conceptual.Exception.Handling#6](~/samples/snippets/cpp/VS_Snippets_CLR/conceptual.exception.handling/cpp/source.cpp#6)]
[!code-csharp[Conceptual.Exception.Handling#6](~/samples/snippets/csharp/VS_Snippets_CLR/conceptual.exception.handling/cs/source.cs#6)]
[!code-vb[Conceptual.Exception.Handling#6](~/samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.exception.handling/vb/source.vb#6)]

場合によっては、例外のコンストラクターを使用して例外を作成する方が適切な場合もあります。 <xref:System.ArgumentException> などのグローバル例外クラスはその一例です。

## <a name="restore-state-when-methods-dont-complete-due-to-exceptions"></a>例外に起因してメソッドが完了しないとき、状態を復元する

呼び出し元が、メソッドから例外がスローされるときに副作用が発生しないと仮定できる必要があります。 たとえば、ある口座から現金を引き出して別の口座に預金することで送金を行うコードがあって、預金の実行中に例外がスローされた場合、引き出しが有効のままにしておきたくはないはずです。

```csharp
public void TransferFunds(Account from, Account to, decimal amount)
{
    from.Withdrawal(amount);
    // If the deposit fails, the withdrawal shouldn't remain in effect.
    to.Deposit(amount);
}
```

```vb
Public Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    from.Withdrawal(amount)
    ' If the deposit fails, the withdrawal shouldn't remain in effect.
    [to].Deposit(amount)
End Sub
```

上記のメソッドでは例外は直接スローされませんが、預金操作が失敗した場合、引き出しが取り消されるよう、安全性を優先して記述する必要があります。

この状況に対処する方法の 1 つは、預金トランザクションによってスローされた例外をキャッチし、引き出しをロールバックすることです。

```csharp
private static void TransferFunds(Account from, Account to, decimal amount)
{
    string withdrawalTrxID = from.Withdrawal(amount);
    try
    {
        to.Deposit(amount);
    }
    catch
    {
        from.RollbackTransaction(withdrawalTrxID);
        throw;
    }
}
```

```vb
Private Shared Sub TransferFunds(from As Account, [to] As Account, amount As Decimal)
    Dim withdrawalTrxID As String = from.Withdrawal(amount)
    Try
        [to].Deposit(amount)
    Catch
        from.RollbackTransaction(withdrawalTrxID)
        Throw
    End Try
End Sub
```

この例では、`throw` を使用して、元の例外を再スローすることを示しています。これにより、呼び出し元が <xref:System.Exception.InnerException> プロパティを確認することなく、問題の本当の原因を容易に確認できるようになります。 別の方法は、新しい例外をスローして元の例外を内部例外として含めることです。

```csharp
catch (Exception ex)
{
    from.RollbackTransaction(withdrawalTrxID);
    throw new TransferFundsException("Withdrawal failed.", innerException: ex)
    {
        From = from,
        To = to,
        Amount = amount
    };
}
```

```vb
Catch ex As Exception
    from.RollbackTransaction(withdrawalTrxID)
    Throw New TransferFundsException("Withdrawal failed.", innerException:=ex) With
    {
        .From = from,
        .[To] = [to],
        .Amount = amount
    }
End Try
```

## <a name="see-also"></a>参照

- [例外](index.md)
