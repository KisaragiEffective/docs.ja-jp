---
title: .NET Core 2.2 の新機能
description: .NET Core 2.2 の新機能について。
dev_langs:
- csharp
- vb
ms.date: 12/04/2018
ms.openlocfilehash: e045c39240c99777d05ca86ee0a8cd1fa4309c4f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78156583"
---
# <a name="whats-new-in-net-core-22"></a>.NET Core 2.2 の新機能

.NET Core 2.2 には、アプリケーションの配置、ランタイム サービスのイベント処理、Azure SQL データベースに対する認証、JIT コンパイラのパフォーマンス、`Main` メソッド実行前のコード インジェクションに関する機能強化が含まれています。

## <a name="new-deployment-mode"></a>新しい配置モード

.NET Core 2.2 以降、[フレームワークに依存する実行可能ファイル](../deploying/index.md#publish-runtime-dependent)を配置できます。これは **.dll** ファイルではなく **.exe** ファイルとなります。 機能的にはフレームワーク依存の配置と似ていますが、フレームワークに依存する実行可能ファイル (FDE) は引き続き .NET Core のシステム全体の共有バージョンに依存して実行されます。 アプリに含まれるものは、ご自身のコードとサードパーティの依存関係のみです。 フレームワーク依存の配置とは異なり、FDE はプラットフォーム固有です。

この新しい配置モードの際立った利点は、ライブラリの代わりに実行可能ファイルをビルドすることです。これは、最初に `dotnet` を呼び出すことなく、直接アプリを実行できることを意味します。

## <a name="core"></a>コア

**ランタイム サービスでのイベントの処理**

GC、JIT、ThreadPool などのランタイム サービスがどのようにアプリケーションで使われているのか監視して、それがアプリケーションに与えている影響を把握したい場合がよくあります。 Windows システムでは、これは通常、現在のプロセスの ETW イベントを監視することによって行われます。 これは引き続き正常に動作しますが、権限の低い環境内、または Linux や macOS 上で実行している場合は、常に ETW を使えるとは限りません。

.NET Core 2.2 以降では、<xref:System.Diagnostics.Tracing.EventListener?displayProperty=nameWithType> クラスを使って CoreCLR イベントを使えるようになりました。 これらのイベントでは、GC、JIT、ThreadPool、および相互運用などのランタイム サービスの動作が説明されます。 これらは、CoreCLR ETW プロバイダーの一部として公開されるものと同じイベントです。  このため、アプリケーションでは、これらのイベントを使うかトランスポート機構を使って、それらをテレメトリ集計サービスに送信できます。 次のコード サンプルでイベントをサブスクライブする方法を確認できます。

```csharp
internal sealed class SimpleEventListener : EventListener
{
    // Called whenever an EventSource is created.
    protected override void OnEventSourceCreated(EventSource eventSource)
    {
        // Watch for the .NET runtime EventSource and enable all of its events.
        if (eventSource.Name.Equals("Microsoft-Windows-DotNETRuntime"))
        {
            EnableEvents(eventSource, EventLevel.Verbose, (EventKeywords)(-1));
        }
    }

    // Called whenever an event is written.
    protected override void OnEventWritten(EventWrittenEventArgs eventData)
    {
        // Write the contents of the event to the console.
        Console.WriteLine($"ThreadID = {eventData.OSThreadId} ID = {eventData.EventId} Name = {eventData.EventName}");
        for (int i = 0; i < eventData.Payload.Count; i++)
        {
            string payloadString = eventData.Payload[i]?.ToString() ?? string.Empty;
            Console.WriteLine($"\tName = \"{eventData.PayloadNames[i]}\" Value = \"{payloadString}\"");
        }
        Console.WriteLine("\n");
    }
}
```

さらに、.NET Core 2.2 では次の 2 つのプロパティが <xref:System.Diagnostics.Tracing.EventWrittenEventArgs> クラスに追加され、ETW イベントに関する追加情報が提供されます。

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.OSThreadId?displayProperty=nameWithType>

- <xref:System.Diagnostics.Tracing.EventWrittenEventArgs.TimeStamp?displayProperty=nameWithType>

## <a name="data"></a>データ

**SqlConnection.AccessToken プロパティを使った Azure SQL データベースに対する AAD 認証**

.NET Core 2.2 以降では、Azure Active Directory によって発行されたアクセス トークンを、Azure SQL データベースへの認証に使用できます。 アクセス トークンをサポートするために、<xref:System.Data.SqlClient.SqlConnection> クラスに <xref:System.Data.SqlClient.SqlConnection.AccessToken> プロパティが追加されています。 AAD 認証を利用するには、バージョン 4.6 の System.Data.SqlClient NuGet パッケージをダウンロードします。 この機能を使うために、[`Microsoft.IdentityModel.Clients.ActiveDirectory`](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/) NuGet パッケージに含まれる [.NET 用 Active Directory 認証ライブラリ](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)を使ってアクセス トークンの値を取得できます。

## <a name="jit-compiler-improvements"></a>JIT コンパイラの機能強化

**階層型コンパイルはオプトイン機能のまま**

.NET Core 2.1 では、JIT コンパイラに新しいコンパイラ テクノロジである "*階層型コンパイル*" がオプトイン機能として実装されました。 階層型コンパイルの目標はパフォーマンスの向上です。 JIT コンパイラで実行される重要なタスクの 1 つはコード実行の最適化です。 ただし、使用頻度の低いコード パスについては、最適化されていないコードの実行にランタイムが費やす時間よりも、コードの最適化にコンパイラが費やす時間の方が多くなる場合があります。 階層型コンパイルによって JIT コンパイルに次の 2 つのステージが導入されます。

- **第 1 階層**: コードをできるだけ早く生成します。

- **第 2 階層**: 頻繁に実行されるメソッドに対して、最適化されたコードを生成します。 コンパイルの第 2 階層は、パフォーマンスの向上と並行して実行されます。

階層型コンパイルによって得られるパフォーマンスの向上について詳しくは、「[Announcing .NET Core 2.2 Preview 2](https://devblogs.microsoft.com/dotnet/announcing-net-core-2-2-preview-2/)」(.NET Core 2.2 Preview 2 の発表) をご覧ください。

.NET Core 2.2 Preview 2 では、階層型コンパイルが既定で有効になっていました。 しかし、階層型コンパイルを既定で有効にする準備はまだ整っていないと判断されました。 このため .NET Core 2.2 では、階層型コンパイルはオプトイン機能のままとなります。 階層型コンパイルに対するオプトインについて詳しくは、「[.NET Core 2.1 の新機能](dotnet-core-2-1.md)」の「[JIT コンパイラの機能強化](dotnet-core-2-1.md#jit-compiler-improvements)」をご覧ください。

## <a name="runtime"></a>ランタイム

**Main メソッド実行前のコードの挿入**

.NET Core 2.2 以降では、スタートアップ フックを使って、アプリケーションの Main メソッドを実行する前にコードを挿入できます。 スタートアップ フックを使うと、ホストはアプリケーションが配置された後に、再コンパイルしたりアプリケーションを変更したりすることなくその動作をカスタマイズできます。

ホスティング プロバイダーは、 <xref:System.Runtime.Loader.AssemblyLoadContext?displayProperty=nameWithType> 動作など、メイン エントリ ポイントの読み込み動作に影響を与える可能性がある設定を含む、カスタム構成とポリシーを定義することが想定されています。 フックは、トレースまたはテレメトリの挿入を設定するため、処理用のコールバックを設定するため、またはその他の環境依存の動作を定義するために使用できます。 フックはエントリ ポイントから独立しているため、ユーザー コードを変更する必要はありません。

詳細については、「[Host startup hook](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/host-startup-hook.md)」(スタートアップ フックのホスト) をご覧ください。

## <a name="see-also"></a>関連項目

- [.NET Core の新機能](index.md)
- [ASP.NET Core 2.2 の新機能](/aspnet/core/release-notes/aspnetcore-2.2)
- [EF Core 2.2 の新機能](/ef/core/what-is-new/ef-core-2.2)
