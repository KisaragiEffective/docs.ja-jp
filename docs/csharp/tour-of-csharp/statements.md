---
title: C# のステートメント - C# 言語のツアー
description: ステートメントを使用して C# プログラムの処理を作成します
ms.date: 02/27/2020
ms.assetid: 5409c379-5622-4fae-88b5-1654276ea8d4
ms.openlocfilehash: ced13b1bfd17977acb98bf33c0a477161cf08a93
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159105"
---
# <a name="statements"></a>ステートメント

プログラムの処理は、"*ステートメント*" を使用して表されます。 C# はさまざまな種類のステートメントをサポートしており、その多くは埋め込みステートメントとして定義されています。

"*ブロック*" を使用すると、1 つのステートメントしか使用できないコンテキストで複数のステートメントを記述できます。 ブロックは、区切り記号 `{` と `}` の間に記述されたステートメントのリストから成ります。

"*宣言ステートメント*" は、ローカル変数および定数の宣言に使用します。

"*式ステートメント*" は、式の評価に使用します。 ステートメントとして使用できる式には、メソッドの呼び出し、`new` 演算子を使用したオブジェクトの割り当て、`=` 演算子と複合代入演算子を使用した代入、`++` 演算子と `--` 演算子を使用したインクリメント演算とデクリメント演算、および `await` 式があります。

"*選択ステートメント*" は、式の値に基づいて、実行できる多数のステートメントから 1 つを選択するために使用します。 このグループには、`if` および `switch` ステートメントが含まれています。

"*繰り返しステートメント*" は、埋め込みステートメントを繰り返し実行するために使用します。 このグループには、`while`、`do`、`for`、および `foreach` ステートメントが含まれています。

"*ジャンプ ステートメント*" は、制御を移すために使用します。 このグループには、`break`、`continue`、`goto`、`throw`、`return`、および `yield` ステートメントが含まれています。

`try`...`catch` ステートメントはブロックの実行中に発生した例外をキャッチするために使用し、`try`...`finally` ステートメントは例外が発生したかどうかにかかわらず常に実行される終了処理コードを指定するために使用します。

`checked` および `unchecked` ステートメントは、整数型の算術演算および変換に対するオーバーフロー チェック コンテキストを制御するために使用します。

`lock` ステートメントは、指定のオブジェクトに対する相互排他ロックを取得し、ステートメントを実行してからロックを解放するために使用します。

`using` ステートメントは、リソースを取得し、ステートメントを実行してからそのリソースを破棄するために使用します。

以下に、使用できるさまざまなステートメントの一覧とそれぞれの例を示します。

* ローカル変数の宣言:

 [!code-csharp[Declarations](../../../samples/snippets/csharp/tour/statements/Program.cs#L9-L15)]

* ローカル定数の宣言:

 [!code-csharp[ConstantDeclarations](../../../samples/snippets/csharp/tour/statements/Program.cs#L17-L22)]

* 式ステートメント:

 [!code-csharp[Expressions](../../../samples/snippets/csharp/tour/statements/Program.cs#L24-L31)]

* `if` ステートメント:

 [!code-csharp[IfStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L33-L43)]

* `switch` ステートメント:

 [!code-csharp[SwitchStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L45-L60)]

* `while` ステートメント:

 [!code-csharp[WhileStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L62-L70)]

* `do` ステートメント:

 [!code-csharp[DoStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L72-L81)]

* `for` ステートメント:

 [!code-csharp[ForStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L83-L89)]

* `foreach` ステートメント:

 [!code-csharp[ForEachStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L91-L97)]

* `break` ステートメント:

 [!code-csharp[BreakStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L99-L108)]

* `continue` ステートメント:

 [!code-csharp[ContinueStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L110-L118)]

* `goto` ステートメント:

 [!code-csharp[GotoStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L120-L129)]

* `return` ステートメント:

 [!code-csharp[ReturnStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L131-L139)]

* `yield` ステートメント:

 [!code-csharp[YieldStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L141-L155)]

* `throw` ステートメントと `try` ステートメント:

 [!code-csharp[TryThrow](../../../samples/snippets/csharp/tour/statements/Program.cs#L157-L183)]

* `checked` ステートメントと `unchecked` ステートメント:

 [!code-csharp[CheckedUncheckedStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L185-L196)]

* `lock` ステートメント:

 [!code-csharp[LockStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L257-L273)]

* `using` ステートメント:

 [!code-csharp[UsingStatement](../../../samples/snippets/csharp/tour/statements/Program.cs#L198-L206)]

>[!div class="step-by-step"]
>[前へ](expressions.md)
>[次へ](classes-and-objects.md)
