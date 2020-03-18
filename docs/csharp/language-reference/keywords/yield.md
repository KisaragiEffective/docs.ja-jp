---
title: コンテキスト キーワード yield - C# リファレンス
ms.date: 07/20/2015
f1_keywords:
- yield
- yield_CSharpKeyword
helpviewer_keywords:
- yield keyword [C#]
ms.assetid: 1089194f-9e53-46a2-8642-53ccbe9d414d
ms.openlocfilehash: e3c9e37e7b543eaddae837a85604c4ba91fbc744
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712781"
---
# <a name="yield-c-reference"></a>yield (C# リファレンス)

ステートメントで `yield` [コンテキスト キーワード](index.md#contextual-keywords)を使用した場合、メソッド、演算子、または `get` アクセサーが反復子であることを示します。 `yield` を使用して反復子を定義すると、カスタム コレクション型の <xref:System.Collections.Generic.IEnumerator%601> および <xref:System.Collections.IEnumerable> パターンを実装するときに明示的な余分なクラス (列挙の状態を保持するクラス。たとえば <xref:System.Collections.IEnumerator> を参照) が不要になります。

`yield` ステートメントの 2 つの形式を次の例に示します。

```csharp
yield return <expression>;
yield break;
```

## <a name="remarks"></a>解説

各要素を 1 つずつ返すには、`yield return` ステートメントを使用します。

Iterator メソッドから返されるシーケンスを、[foreach](foreach-in.md) ステートメントまたは LINQ クエリを使って使用することができます。 `foreach` ループの各イテレーションは、Iterator メソッドを呼び出します。 `yield return` ステートメントが Iterator メソッドに到達すると、`expression` が返され、コードの現在の位置が保持されます。 次回、Iterator 関数が呼び出されると、この位置から実行が再開されます。

`yield break` ステートメントを使用すると、反復を終了できます。

反復子について詳しくは、「[Iterators](../../iterators.md)」をご覧ください。

## <a name="iterator-methods-and-get-accessors"></a>Iterator メソッドと get アクセサー

反復子の宣言は、次の要件を満たす必要があります。

- 戻り値の型は、<xref:System.Collections.IEnumerable>、<xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.IEnumerator>、または <xref:System.Collections.Generic.IEnumerator%601> であることが必要です。

- この宣言には、[in](in-parameter-modifier.md)、[ref](ref.md)、または [out](out-parameter-modifier.md) パラメーターを含めることはできません。

`yield` または <xref:System.Collections.IEnumerable> を返す反復子の <xref:System.Collections.IEnumerator> 型は `object` です。  反復子が <xref:System.Collections.Generic.IEnumerable%601> または <xref:System.Collections.Generic.IEnumerator%601> を返す場合、`yield return` ステートメント内の式の型から、ジェネリック型パラメーターへの暗黙的な変換が存在する必要があります。

`yield return` または `yield break` ステートメントを以下に含めることはできません。

- [ラムダ式](../../programming-guide/statements-expressions-operators/lambda-expressions.md)および[匿名メソッド](../operators/delegate-operator.md)。

- unsafe ブロックを含むメソッド。 詳しくは、「[unsafe](unsafe.md)」をご覧ください。

## <a name="exception-handling"></a>例外処理

`yield return` ステートメントは、try-catch ブロックに配置することはできません。 `yield return` ステートメントは、try-finally ステートメントの try ブロックに配置できます。

`yield break` ステートメントは、try ブロックまたは catch ブロックには配置できますが、finally ブロックには配置できません。

`foreach` 本体 (Iterator メソッドの外部) で例外がスローされた場合、Iterator メソッドの `finally` ブロックが実行されます。

## <a name="technical-implementation"></a>技術的な実装

次のコードは、iterator メソッドから `IEnumerable<string>` を返した後、要素を反復処理します。

```csharp
IEnumerable<string> elements = MyIteratorMethod();
foreach (string element in elements)
{
   ...
}
```

`MyIteratorMethod` への呼び出しでは、メソッドの本体は実行されません。 この呼び出しでは、`IEnumerable<string>` が `elements` 変数に返されます。

`foreach` ループの反復処理では、<xref:System.Collections.IEnumerator.MoveNext%2A> について `elements` メソッドが呼び出されます。 この呼び出しでは、次の `MyIteratorMethod` ステートメントに到達するまで、`yield return` の本体が実行されます。 `yield return` ステートメントによって返される式は、ループ本体による処理に対する `element` 変数の値だけでなく、<xref:System.Collections.Generic.IEnumerator%601.Current%2A> である、`elements` の `IEnumerable<string>` プロパティも決定します。

`foreach` ループの以降の各反復処理では、反復子本体の実行が中断した場所から続行し、`yield return` ステートメントに到達したときに再度停止します。 iterator メソッドまたは `foreach` ステートメントの最後に到達すると、`yield break` ループは完了します。

## <a name="example"></a>例

次の例では、`yield return` ループ内に `for` ステートメントが含まれます。 `foreach` メソッド内の `Main` ステートメント本体の各反復処理では、`Power` Iterator 関数が呼び出されます。 Iterator 関数を呼び出すごとに、`yield return` ステートメントの次の実行に進みます。これは、`for` ループの次の反復処理で行われます。

Iterator メソッドの戻り値の型は <xref:System.Collections.IEnumerable> であり、これは反復子インターフェイス型です。 Iterator メソッドが呼び出されると、数値の累乗を含む列挙可能なオブジェクトが返されます。

[!code-csharp[csrefKeywordsContextual#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#5)]

## <a name="example"></a>例

次の例は、反復子である `get` アクセサーを示しています。 この例では、各 `yield return` ステートメントがユーザー定義クラスのインスタンスを返します。

[!code-csharp[csrefKeywordsContextual#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsContextual/CS/csrefKeywordsContextual.cs#21)]

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>参照

- [C# リファレンス](../../language-reference/index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [foreach、in](foreach-in.md)
- [反復子](../../iterators.md)
