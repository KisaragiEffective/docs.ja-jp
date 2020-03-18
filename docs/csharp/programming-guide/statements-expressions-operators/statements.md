---
title: ステートメント - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- statements [C#], about statements
- C# language, statements
ms.assetid: 901bcde7-87de-4e15-833c-f9cfd40c8ce3
ms.openlocfilehash: d50b50bb291d0d087015ea5bd075ae90ced66ff5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75711936"
---
# <a name="statements-c-programming-guide"></a>ステートメント (C# プログラミング ガイド)

プログラムが実行する処理は、ステートメントとして表されます。 一般的な処理には、変数の宣言、値の代入、メソッドの呼び出し、コレクションに対するループ処理、条件に応じたコード ブロックへの分岐などがあります。 プログラム内でステートメントが実行される順序は、制御フローまたは実行フローと呼ばれます。 制御フローは、実行時に渡された入力に対するプログラムの応答に応じて、プログラムを実行するたびに変わる可能性があります。

ステートメントは、セミコロンで終わる単一行のコードか、1 つのブロックを形成する一連の単一行ステートメントで構成されます。 ステートメント ブロックは中かっこ {} で囲み、入れ子になったブロックを含めることができます。 次のコードは、2 つの単一行ステートメントの例と、1 つの複数行ステートメント ブロックを示しています。

[!code-csharp[csProgGuideStatements#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#1)]

## <a name="types-of-statements"></a>ステートメントの種類

次の表は、C# のさまざまな種類のステートメントと、それぞれに関連付けられているキーワードの一覧です。詳細が記載されているトピックへのリンクも示しています。

|カテゴリ|C# のキーワード/メモ|
|--------------|---------------------------|
|[宣言ステートメント](#declaration-statements)|宣言ステートメントは、新しい変数または定数を定義します。 変数宣言では、必要に応じて変数に値を代入することができます。 定数宣言では、常に代入が必要です。|
|[式ステートメント](expressions.md)|値を計算する式ステートメントでは、変数に値を格納する必要があります。 詳細については、「[ステートメント](#expression-statements)」をご覧ください。|
|選択ステートメント|選択ステートメントでは、1 つ以上の指定した条件に応じて、コードのさまざまなセクションに分岐することができます。 詳細については、以下のトピックを参照してください。 <ul><li>[if](../../language-reference/keywords/if-else.md)</li><li>[else](../../language-reference/keywords/if-else.md)</li><li>[switch](../../language-reference/keywords/switch.md)</li><li>[case](../../language-reference/keywords/switch.md)</li></ul>|
|繰り返しステートメント|繰り返しステートメントを使用すると、配列などのコレクションをループ処理したり、指定した条件が満たされるまで同じステートメントのセットを繰り返し実行したりできます。 詳細については、以下のトピックを参照してください。 <ul><li>[do](../../language-reference/keywords/do.md)</li><li>[for](../../language-reference/keywords/for.md)</li><li>[foreach](../../language-reference/keywords/foreach-in.md)</li><li>[in](../../language-reference/keywords/foreach-in.md)</li><li>[while](../../language-reference/keywords/while.md)</li></ul>|
|ジャンプ ステートメント|ジャンプ ステートメントでは、別のコード セクションに制御を移します。 詳細については、以下のトピックを参照してください。 <ul><li>[break](../../language-reference/keywords/break.md)</li><li>[continue](../../language-reference/keywords/continue.md)</li><li>[既定値](../../language-reference/keywords/switch.md)</li><li>[goto](../../language-reference/keywords/goto.md)</li><li>[return](../../language-reference/keywords/return.md)</li><li>[yield](../../language-reference/keywords/yield.md)</li></ul>|
|例外処理ステートメント|例外処理ステートメントを使用すると、実行時に発生する例外状態から適切に回復できます。 詳細については、以下のトピックを参照してください。 <ul><li>[throw](../../language-reference/keywords/throw.md)</li><li>[try-catch](../../language-reference/keywords/try-catch.md)</li><li>[try-finally](../../language-reference/keywords/try-finally.md)</li><li>[try-catch-finally](../../language-reference/keywords/try-catch-finally.md)</li></ul>|
|[checked と unchecked](../../language-reference/keywords/checked-and-unchecked.md)|checked ステートメントと unchecked ステートメントを使用すると、結果の値を保持するには小さすぎる変数に結果が格納される場合に、数値演算でオーバーフローが発生するのを許可するかどうかを指定できます。 詳細については、「[checked](../../language-reference/keywords/checked.md)」および「[unchecked](../../language-reference/keywords/unchecked.md)」を参照してください。|
|`await` ステートメント|メソッドに [async](../../language-reference/keywords/async.md) 修飾子を付けると、そのメソッドで [await](../../language-reference/operators/await.md) 演算子を使用できます。 コントロールが非同期メソッドの `await` 式に到達すると、コントロールは呼び出し元に戻り、待機中のタスクが完了するまでメソッドの進行状況は中断されます。 タスクが完了すると、メソッドで実行を再開できます。<br /><br /> 簡単な例については、「[メソッド](../classes-and-structs/methods.md)」の「非同期メソッド」セクションを参照してください。 詳細については、「[Async および Await を使用した非同期プログラミング](../concepts/async/index.md)」を参照してください。|
|`yield return` ステートメント|反復子は、リストや配列など、コレクションに対するカスタム イテレーションを実行します。 反復子は、[yield return](../../language-reference/keywords/yield.md) ステートメントを使用して、各要素を 1 回に 1 つ返します。 `yield return` ステートメントに達すると、コードの現在の場所が記憶されます。 反復子が次回呼び出されたとき、この場所から実行が再開されます。<br /><br /> 詳細については、「 [反復子](../concepts/iterators.md)」を参照してください。|
|`fixed` ステートメント|fixed ステートメントは、移動可能な変数がガベージ コレクターにより再配置されることを防ぎます。 詳細については、「[fixed](../../language-reference/keywords/fixed-statement.md)」を参照してください。|
|`lock` ステートメント|lock ステートメントを使用すると、一度に 1 つのスレッドしかコード ブロックにアクセスしないように制限できます。 詳細については、「[lock](../../language-reference/keywords/lock-statement.md)」を参照してください。|
|ラベル付きステートメント|ステートメントにラベルを付与し、[goto](../../language-reference/keywords/goto.md) キーワードを使用して、そのラベル付きステートメントにジャンプできます (次の行の例を参照してください)。|
|[空のステートメント](#the-empty-statement)|空のステートメントは、1 つのセミコロンで構成されます。 このステートメントは何も実行しませんが、ステートメントが必要な場所で、どのような処理も実行する必要がない場合に使用できます。|

## <a name="declaration-statements"></a>宣言ステートメント

次のコードでは、最初の割り当てを使用した場合と使用しない場合の変数宣言の例と、必要な初期化を使用した定数宣言の例を示します。

[!code-csharp[csProgGuideStatements#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#23)]

## <a name="expression-statements"></a>式ステートメント

次のコードでは、割り当て、割り当てによるオブジェクトの作成、およびメソッドの呼び出しなど、式ステートメントの例を示します。

[!code-csharp[csProgGuideStatements#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#24)]

## <a name="the-empty-statement"></a>空のステートメント

空のステートメントの 2 つの使用例を次に示します。

[!code-csharp[csProgGuideStatements#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#25)]

## <a name="embedded-statements"></a>埋め込みステートメント

[do](../../language-reference/keywords/do.md)、[while](../../language-reference/keywords/while.md)、[for](../../language-reference/keywords/for.md)、[foreach](../../language-reference/keywords/foreach-in.md) などの一部のステートメントでは、その後に必ず埋め込みステートメントが続きます。 この埋め込みステートメントは、単一のステートメントか、または複数のステートメントを中かっこ {} で囲んだステートメント ブロックです。 単一行の埋め込みステートメントでも、次の例に示すように中かっこ {} で囲むことができます。

[!code-csharp[csProgGuideStatements#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#26)]

中かっこ {} で囲まれていない埋め込みステートメントは、宣言ステートメントやラベル付きステートメントにはできません。 以下の例を参照してください。

[!code-csharp[csProgGuideStatements#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#27)]

エラーを修正するには、次のように埋め込みステートメントをブロックに配置します。

[!code-csharp[csProgGuideStatements#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#28)]

## <a name="nested-statement-blocks"></a>入れ子になったステートメント ブロック

次のコードに示すように、ステートメント ブロックを入れ子にすることができます。

[!code-csharp[csProgGuideStatements#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#29)]

## <a name="unreachable-statements"></a>到達できないステートメント

コンパイラは、どのような状況でも特定のステートメントに制御フローが到達できないと判断すると、次の例のように、警告 CS0162 を生成します。

[!code-csharp[csProgGuideStatements#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#22)]

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/statements.md)の「[ステートメント](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [ステートメントのキーワード](../../language-reference/keywords/statement-keywords.md)  
- [式](expressions.md)  
