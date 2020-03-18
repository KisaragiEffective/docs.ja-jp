---
title: try-finally - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- finally
- finally_CSharpKeyword
helpviewer_keywords:
- finally keyword [C#]
- try-finally statement [C#]
ms.assetid: c27623fb-7261-4464-862c-7a369d3c8f0a
ms.openlocfilehash: 2c4c69b1e104aed968bc24bac690de83026643a0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713015"
---
# <a name="try-finally-c-reference"></a>try-finally (C# リファレンス)

`finally` ブロックを使用すると、[try](try-catch.md) ブロックで割り当てられたリソースをクリーンアップし、`try` ブロックで例外が発生してもコードを実行することができます。 通常、制御が `finally` ステートメントを離れると、`try` ブロックのステートメントが実行されます。 制御の移動は、`break` ステートメント、`continue` ステートメント、`goto` ステートメント、`return` またはステートメントの正常な実行の結果や、`try` ステートメントで生じた例外の反映の結果として生じます。

ハンドルされている例外では、関連する `finally` ブロックの実行が保証されます。 ただし、例外がハンドルされていない場合、`finally` ブロックの実行は、例外のアンワインド操作のトリガー方法に依存します。 つまり、コンピューターの設定に依存するということでもあります。

通常、ハンドルされていない例外によってアプリケーションが終了した場合、`finally` ブロックが実行されるかどうかは重要ではありません。 ただし、このような状況でも実行する必要のあるステートメントが `finally` ブロックにある場合は、`catch``try`- ステートメントに `finally` ブロックを追加するという方法があります。 または、`try``try`- ステートメントの `finally` ブロックでスローされた例外があれば、コール スタックの上の方でキャッチすることもできます。 つまり、`try`-`finally` ステートメントが含まれているメソッドを呼び出すメソッド内でも、そのメソッドを呼び出すメソッド内でも、コール スタック内の任意のメソッド内でも、例外をキャッチできるということです。 例外がキャッチされない場合、`finally` ブロックの実行は、例外のアンワインド操作がオペレーティング システムによってトリガーされるかどうかに依存します。

## <a name="example"></a>例

次の例では、無効な変換ステートメントによって `System.InvalidCastException` 例外が発生しています。 この例外はハンドルされていません。

[!code-csharp[csrefKeywordsExceptions#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#4)]

次の例では、コール スタックのずっと上の方のメソッド内で `TryCast` メソッドからの例外がキャッチされます。

[!code-csharp[csrefKeywordsExceptions#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsExceptions/CS/csrefKeywordsExceptions.cs#6)]

`finally` の詳細については、「[try-catch-finally](try-catch-finally.md)」を参照してください。

C# には、[ オブジェクトに対して同様の機能を便利な構文で提供する ](using-statement.md)using ステートメント<xref:System.IDisposable>も含まれています。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/statements.md#the-try-statement)」の「[try ステートメント](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [try、throw、catch ステートメント (C++)](/cpp/cpp/try-throw-and-catch-statements-cpp)
- [throw](throw.md)
- [try-catch](try-catch.md)
- [方法: 例外を明示的にスローする](../../../standard/exceptions/how-to-explicitly-throw-exceptions.md)
