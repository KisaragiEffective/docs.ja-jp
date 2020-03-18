---
title: using ステートメント - C# リファレンス
ms.date: 10/15/2019
helpviewer_keywords:
- using statement [C#]
ms.assetid: afc355e6-f0b9-4240-94dd-0d93f17d9fc3
ms.openlocfilehash: 52cde99fd029ce50f159b2a87fbfbf47fc79dccc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712963"
---
# <a name="using-statement-c-reference"></a>using ステートメント (C# リファレンス)

<xref:System.IDisposable> オブジェクトの正しい使用を保証する簡易構文を提供します。

## <a name="example"></a>例

`using` ステートメントを使用する方法の例を次に示します。

[!code-csharp[csrefKeywordsNamespace#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#4)]

C# 8.0 以降では、中かっこを必要としない `using` ステートメントに次の代替構文を使用できます。

[!code-csharp[csrefKeywordsNamespace#New](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#ModernUsing)]

## <a name="remarks"></a>解説

<xref:System.IO.File> と <xref:System.Drawing.Font> は、アンマネージド リソース (この場合はファイル ハンドルとデバイス コンテキスト) にアクセスするマネージド型の例です。 アンマネージ リソースや、それをカプセル化するクラス ライブラリ型は他にもたくさんあります。 このような型はすべて、<xref:System.IDisposable> インターフェイスを実装する必要があります。

`IDisposable` オブジェクトの有効期間が 1 つのメソッドに限定されているとき、それを `using` ステートメントで宣言し、インスタンス化してください。 `using` ステートメントは、オブジェクトで正しく <xref:System.IDisposable.Dispose%2A> メソッドを呼び出します。(前述のようにこのステートメントを使用する場合) <xref:System.IDisposable.Dispose%2A> が呼び出されるとすぐに、オブジェクト自体がスコープの外側に出されます。 オブジェクトは、`using` ブロック内では読み取り専用です。変更したり再割り当てしたりすることはできません。

`using` ステートメントにより、<xref:System.IDisposable.Dispose%2A> ブロックで例外が発生した場合でも必ず `using` が呼び出されます。 オブジェクトを `try` ブロックに配置し、<xref:System.IDisposable.Dispose%2A> ブロックで `finally` を呼び出しても、同じ結果が得られます。実際には、コンパイラは `using` ステートメントをこのように変換します。 前のコード例は、コンパイル時に次のコードに展開されます (オブジェクトのスコープ制限を定義する中かっこが加えられていることに注意してください)。

[!code-csharp[csrefKeywordsNamespace#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#5)]

新しい `using` ステートメントの構文は、非常に似たコードに変換されます。 変数が宣言されている場所で `try` ブロックが開きます。 `finally` ブロックは、囲んでいるブロックの終わり、通常はメソッドの最後に追加されます。

`try`-`finally` ステートメントの詳細については、「[try-finally](try-finally.md)」のトピックを参照してください。

次の例のように、`using` ステートメントでは型の複数のインスタンスを宣言できます。

[!code-csharp[csrefKeywordsNamespace#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#6)]

C# 8 で導入された新しい構文を使用して、同じ型の複数の宣言を結合することもできます。 以下の例を参照してください。

[!code-csharp[csrefKeywordsNamespace#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#MultipleUsing)]

リソース オブジェクトをインスタンス化してから、変数を `using` ステートメントに渡すことはできますが、これはベスト プラクティスではありません。 この場合、アンマネージド リソースへのアクセスがなくなっている可能性が高いのにもかかわらず、制御が `using` ブロックを離れた後もオブジェクトはスコープ内に残ります。 つまり、完全に初期化されることはなくなります。 `using` ブロックの外側でオブジェクトを使用しようとすると、例外がスローされる可能性があります。 このため、通常は、オブジェクトを `using` ステートメントでインスタンス化して、そのスコープを `using` ブロックに制限することをお勧めします。

[!code-csharp[csrefKeywordsNamespace#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsNamespace/CS/csrefKeywordsNamespace.cs#7)]

`IDisposable` オブジェクトの破棄に関する詳細については、「[IDisposable を実装するオブジェクトの使用](../../../standard/garbage-collection/using-objects.md)」を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/statements.md#the-using-statement)に関するページの [using ステートメント](/dotnet/csharp/language-reference/language-specification/introduction)に関するセクションを参照してください。 言語仕様は、C# の構文と使用法に関する信頼性のある情報源です。

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [using ディレクティブ](using-directive.md)
- [ガベージ コレクション](../../../standard/garbage-collection/index.md)
- [IDisposable を実装するオブジェクトの使用](../../../standard/garbage-collection/using-objects.md)
- [IDisposable インターフェイス](xref:System.IDisposable)
- [C# 8.0 の using ステートメント](~/_csharplang/proposals/csharp-8.0/using.md)
