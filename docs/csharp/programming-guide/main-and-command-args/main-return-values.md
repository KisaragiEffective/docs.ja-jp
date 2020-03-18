---
title: Main() の戻り値 - C# プログラミング ガイド
ms.date: 08/02/2017
helpviewer_keywords:
- Main method [C#], return values
ms.assetid: c2f5a1d8-1676-4bea-bc7e-44a97e72d5bc
ms.openlocfilehash: eaa78c33613093bb0e108870669392d07d346a95
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77504001"
---
# <a name="main-return-values-c-programming-guide"></a>Main() の戻り値 (C# プログラミング ガイド)

`Main` メソッドは `void` を返すことができます。

 [!code-csharp[csProgGuideMain#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#12)]

`int` を返すこともできます。

 [!code-csharp[csProgGuideMain#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#13)]

`Main` からの戻り値を使用しない場合は、`void` を返すと少し簡単なコードにすることができます。 ただし、整数値を返すことによって、プログラムが状態の情報を、実行可能ファイルを呼び出す他のプログラムまたはスクリプトに伝達することができます。 `Main` からの戻り値は、プロセスの終了コードとして扱われます。 `void` が `Main` から返された場合、終了コードは暗黙的に `0` になります。 次の例では、`Main` からの戻り値にアクセスする方法を示します。

## <a name="example"></a>例

この例では、[.NET Core](../../../core/index.md) コマンド ライン ツールを使用します。 .NET Core コマンド ライン ツールに慣れていない場合は、この[概要のトピック](../../../core/tutorials/cli-create-console-app.md)を参照してください。

`Main`program.cs*の* メソッドを次のように変更します。

 [!code-csharp[csProgGuideMain#14](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideMain/CS/Class3.cs#14)]

プログラムを Windows で実行する場合、`Main` 関数からの戻り値はすべて 1 つの環境変数に格納されます。 この環境変数を取得するには、バッチ ファイルから `ERRORLEVEL` を使用するか、PowerShell から `$LastExitCode` を使用します。

[dotnet CLI](../../../core/tools/dotnet.md) の `dotnet build` コマンドを使用してアプリケーションを構築できます。

次に、PowerShell スクリプトを使用してアプリケーションを実行し、結果を表示します。 次のコードをテキスト ファイルに貼り付け、プロジェクトが保存されているフォルダーに `test.ps1` として保存します。 PowerShell プロンプトに「`test.ps1`」と入力して PowerShell スクリプトを実行します。

コードがゼロを返すため、バッチ ファイルで成功が報告されます。 ただし、MainReturnValTest.cs が 0 以外の値を返すように変更して、プログラムを再コンパイルする場合、PowerShell スクリプトの後続の実行では失敗が報告されます。

```dotnetcli
dotnet run
```

```powershell
if ($LastExitCode -eq 0) {
    Write-Host "Execution succeeded"
} else
{
    Write-Host "Execution Failed"
}
Write-Host "Return value = " $LastExitCode
```

## <a name="sample-output"></a>サンプル出力

```txt
Execution succeeded
Return value = 0
```

## <a name="async-main-return-values"></a>非同期 Main の戻り値

非同期 Main の戻り値によって、`Main` 内の非同期メソッドを呼び出すために必要な定型コードを、コンパイラで生成されるコードに移動します。 以前のバージョンでは、このコンストラクトを出力して非同期コードを呼び出し、非同期操作が完了するまでプログラムが実行されるようにする必要がありました。

```csharp
public static void Main()
{
    AsyncConsoleWork().GetAwaiter().GetResult();
}

private static async Task<int> AsyncConsoleWork()
{
    // Main body here
    return 0;
}
```

現在のバージョンでは、以下のように書き換えられるようになりました。

[!code-csharp[AsyncMain](../../../../samples/snippets/csharp/main-arguments/program.cs#AsyncMain)]

新しい構文を使用すると、コンパイラから常に正しいコードが生成されるという利点があります。

## <a name="compiler-generated-code"></a>コンパイラから生成されるコード

アプリケーションのエントリ ポイントから `Task` または `Task<int>` が返されると、コンパイラによって、アプリケーション コードで宣言されたエントリ ポイント メソッドを呼び出す新しいエントリ ポイントが生成されます。 このエントリ ポイント名が `$GeneratedMain` だとすると、これらのエントリ ポイントについて次のコードが生成されます。

- `static Task Main()` の結果、`private static void $GeneratedMain() => Main().GetAwaiter().GetResult();` と同等のコードが生成されます。
- `static Task Main(string[])` の結果、`private static void $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` と同等のコードが生成されます。
- `static Task<int> Main()` の結果、`private static int $GeneratedMain() => Main().GetAwaiter().GetResult();` と同等のコードが生成されます。
- `static Task<int> Main(string[])` の結果、`private static int $GeneratedMain(string[] args) => Main(args).GetAwaiter().GetResult();` と同等のコードが生成されます。

> [!NOTE]
>この例の `async` メソッドで `Main` 修飾子を使用した場合、同じコードが生成されます。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [C# リファレンス](../index.md)
- [Main() とコマンドライン引数](index.md)
- [コマンド ライン引数を表示する方法](./how-to-display-command-line-arguments.md)
