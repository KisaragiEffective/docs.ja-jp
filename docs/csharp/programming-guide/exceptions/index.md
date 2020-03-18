---
title: 例外と例外処理 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- exception handling [C#]
- exceptions [C#]
- C# language, exceptions
ms.assetid: 0001887f-4fa2-47e2-8034-2819477e2344
ms.openlocfilehash: b883012cf8f72247ff4e0b47a46eee1854e2d534
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "76735647"
---
# <a name="exceptions-and-exception-handling-c-programming-guide"></a>例外と例外処理 (C# プログラミング ガイド)

C# 言語の例外処理機能は、プログラムの実行時に発生する予期しない状況や例外的な状況を扱うのに役立ちます。 例外処理では、キーワード `try`、`catch`、および `finally` を使用して、成功しない可能性があるアクションを試行し、適切な場合はエラーを処理して、後からリソースをクリーンアップします。 例外の発生元は、共通言語ランタイム (CLR)、.NET Framework、または任意のサード パーティ ライブラリ、またはアプリケーション コードなどさまざまです。 例外は、`throw` キーワードを使用して作成されます。

コードが直接呼び出したメソッドではなく、呼び出し履歴の下の方にある別のメソッドによって例外がスローされることも多くあります。 その場合、CLR は履歴をアンワインドし、`catch` ブロックを持つメソッドを探して特定の例外の種類がないかを調べ、もしあれば最初に見つかった `catch` ブロックを実行します。 適切な `catch` ブロックが呼び出し履歴にない場合は、プロセスが終了し、ユーザーにメッセージが表示されます。

この例では、メソッドが 0 による除算をテストしてエラーをキャッチします。 例外処理せずにプログラムは終了し、"**DivideByZeroException はハンドルされませんでした。** " というエラーが表示されます。

[!code-csharp[csProgGuideExceptions#18](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideExceptions/CS/Exceptions.cs#18)]

## <a name="exceptions-overview"></a>例外の概要

例外には、次の特徴があります。

- 例外はすべて最終的に `System.Exception` から派生した種類になります。
- 例外をスローする可能性のあるステートメントの周囲で `try` ブロックを使用します。
- `try` ブロックで例外が発生すると、コントロールのフローが、呼び出し履歴内の関連付けられている最初の例外ハンドラーにジャンプします。 C# では、`catch` キーワードは例外ハンドラーの定義に使用されます。
- 特定の例外の例外ハンドラーが存在しない場合、プログラムは実行を停止し、エラー メッセージを表示します。
- 処理できない例外はキャッチしないようにして、アプリケーションを既知の状態に保ちます。 `System.Exception` をキャッチした場合は、`throw` ブロックの最後で `catch` キーワードを使用して、それを再スローします。
- `catch` ブロックで例外変数を定義した場合、それを使用して、発生した例外の種類に関する詳細を入手することができます。
- 例外は、`throw` キーワードを使用してプログラムで明示的に生成することができます。
- 例外オブジェクトには、呼び出し履歴の状態やエラーの説明など、エラーに関する詳細情報が含まれています。
- `finally` ブロック内のコードは、例外がスローされた場合でも実行されます。 `finally` ブロックを使用してリソースを解放します。たとえば、`try` ブロックで開かれたストリームまたはファイルを閉じます。
- .NET Framework のマネージド例外は、Win32 構造化例外処理メカニズムの上に実装されます。 詳細については、「[構造化例外処理 (C/C++)](/cpp/cpp/structured-exception-handling-c-cpp)」と「[A Crash Course on the Depths of Win32 Structured Exception Handling (Win32 構造化例外処理に関する短期集中コース)」](http://bytepointer.com/resources/pietrek_crash_course_depths_of_win32_seh.htm)を参照してください。

## <a name="related-sections"></a>関連項目

例外と例外処理の詳細については、次のアーティクルを参照してください。

- [例外の使用](using-exceptions.md)
- [例外処理](exception-handling.md)
- [例外の作成とスロー](creating-and-throwing-exceptions.md)
- [コンパイラにより生成された例外](compiler-generated-exceptions.md)
- [try/catch を使用して例外を処理する方法 (C# プログラミング ガイド)](how-to-handle-an-exception-using-try-catch.md)
- [finally を使用してクリーンアップ コードを実行する方法](how-to-execute-cleanup-code-using-finally.md)
- [CLS に準拠していない例外をキャッチする方法](how-to-catch-a-non-cls-exception.md)

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語の仕様](~/_csharplang/spec/exceptions.md)」の「[例外](/dotnet/csharp/language-reference/language-specification/introduction)」を参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>参照

- <xref:System.SystemException>
- [C# プログラミングガイド](../index.md)
- [C# のキーワード](../../language-reference/keywords/index.md)
- [throw](../../language-reference/keywords/throw.md)
- [try-catch](../../language-reference/keywords/try-catch.md)
- [try-finally](../../language-reference/keywords/try-finally.md)
- [try-catch-finally](../../language-reference/keywords/try-catch-finally.md)
- [例外](../../../standard/exceptions/index.md)
