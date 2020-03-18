---
title: is - C# リファレンス
ms.date: 06/21/2019
f1_keywords:
- is_CSharpKeyword
- is
helpviewer_keywords:
- is keyword [C#]
ms.assetid: bc62316a-d41f-4f90-8300-c6f4f0556e43
ms.openlocfilehash: 1a1f539e80f8d843f40640fa798cf6122f316a9f
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75715234"
---
# <a name="is-c-reference"></a>is (C# リファレンス)

`is` 演算子では、式の結果と指定された型との間に互換性があるかどうかが確認されるか、または (C# 7.0 以降では) パターンに対して式がテストされます。 型テストの `is` 演算子について詳しくは、[型テストとキャスト演算子](../operators/type-testing-and-cast.md)に関する記事の「[is 演算子](../operators/type-testing-and-cast.md#is-operator)」セクションをご覧ください。

## <a name="pattern-matching-with-is"></a>`is` を使用したパターン マッチング

C# 7.0 以降では、`is` および [switch](switch.md) ステートメントでパターン マッチングがサポートされています。 `is` キーワードでは、以下のパターンがサポートされています。

- [型パターン](#type-pattern)では、式を指定された型に変換できるかどうかがテストされ、変換できる場合はその型の変数にキャストされます。

- [定数パターン](#constant-pattern): 式の評価が指定された定数値になるかどうかをテストします。

- [var パターン](#var-pattern): 照合が常に成功し、式の値が新しいローカル変数にバインドされます。

### <a name="type-pattern"></a>型パターン

型パターンを使用してパターン マッチングを実行すると、式を指定された型に変換できるかどうかが `is` によってテストされ、変換できる場合はその型の変数にキャストされます。 `is` ステートメントのわかりやすい拡張機能であり、型の評価および変換を簡潔に記述できます。 `is` 型パターンの一般的な形式は次のとおりです。

```csharp
   expr is type varname
```

ここで *expr* は何らかの型のインスタンスに評価される式、*type* は *expr* の結果が変換される型の名前、*varname* は `is` のテスト結果が `true` である場合に *expr* の結果が変換されるオブジェクトをそれぞれ表しています。 

*expr* が `null` ではなく、以下のいずれかの条件が true である場合に `is` 式は `true` となります。

- *expr* が *type* と同じ型のインスタンスである。

- *expr* が *type* から派生した型のインスタンスである。 つまり、*expr* の結果を *type* のインスタンスにアップキャストできる。

- *expr* のコンパイル時の型が *type* の基底クラスであり、*expr* の実行時の型が *type* または *type* から派生した型である。 変数の "*コンパイル時の型*" とは、その変数の宣言で定義されている型です。 変数の "*実行時の型*" とは、その変数に代入されているインスタンスの型です。

- *expr* が、*type* インターフェイスを実装する型のインスタンスである。

C#7.1 以降、*expr* はジェネリック型パラメーターとその制約によって定義されるコンパイル時型を持つことができます。

*expr* が `true` であり `is` が `if` ステートメントに使用されている場合は、*varname* は `if` ステートメント内のみに割り当てられます。 *varname* のスコープは、`is` 式から `if` ステートメントを閉じるブロックの末尾までになります。 他の任意の場所に *varname* を使用すると、割り当てられていない変数の使用によるコンパイル時エラーが生成されます。

次の例では、`is` 型のパターンを使用して、型 <xref:System.IComparable.CompareTo(System.Object)?displayProperty=nameWithType> のメソッドの実装を提供します。

[!code-csharp[is#5](../../../../samples/snippets/csharp/language-reference/keywords/is/is-type-pattern5.cs#5)]

パターン マッチングを使用しない場合、このコードは次のように記述できます。 型パターン マッチングを使用することにより、変換結果が `null` であるかどうかをテストする必要がなくなるため、コードがよりコンパクトで読みやすくなります。  

[!code-csharp[is#6](../../../../samples/snippets/csharp/language-reference/keywords/is/is-type-pattern6.cs#6)]

`is` 型パターンを使用すると、値の型の種類を判定する場合によりコンパクトなコードを記述できます。 次の例では、`is` 型パターンを使用し、オブジェクトが `Person` インスタンスか `Dog` インスタンスかを判定した後に適切なプロパティの値を表示しています。

[!code-csharp[is#9](../../../../samples/snippets/csharp/language-reference/keywords/is/is-type-pattern9.cs#9)]

パターン マッチングを使用せずにこれと同等のコードを記述する場合は、明示的なキャストを含む代入処理を個別に行う必要があります。

[!code-csharp[is#10](../../../../samples/snippets/csharp/language-reference/keywords/is/is-type-pattern10.cs#10)]

### <a name="constant-pattern"></a>定数パターン

定数パターンを使用してパターン マッチングを実行すると、式が指定された定数と等しいかどうかが `is` によってテストされます。 C# 6 以前のバージョンでの定数パターンは [switch](switch.md) ステートメントでサポートされています。 C# 7.0 以降では、`is` ステートメントでもサポートされています。 構文は次のとおりです。

```csharp
   expr is constant
```

ここで *expr* は評価する式、*constant* はテストする値を表しています。 *constant* には以下のいずれかの定数式を指定できます。

- リテラル値。

- 宣言された `const` 変数の名前。

- 列挙定数。

定数式は以下のように評価されます。

- *expr* と *constant* が整数型の場合、式から `true` が返されるかどうか (つまり、`expr == constant` であるかどうか) が C# の等値演算子によって判定されます。

- それ以外の場合、式の値は静的 [Object.Equals(expr, constant)](xref:System.Object.Equals(System.Object,System.Object)) メソッドの呼び出しによって判定されます。  

次の例では、型パターンと定数パターンを組み合わせてオブジェクトが `Dice` インスタンスであるかどうかをテストし、そうである場合はサイコロ振り操作の値が 6 であるかどうかをテストします。

[!code-csharp[is#7](../../../../samples/snippets/csharp/language-reference/keywords/is/is-const-pattern7.cs#7)]

定数パターンを使用して、`null` のチェックを実行できます。 `is` ステートメントで `null` キーワードがサポートされています。 構文は次のとおりです。

```csharp
   expr is null
```

`null` チェックの比較を示す例を次に示します。

[!code-csharp[is#11](../../../../samples/snippets/csharp/language-reference/keywords/is/is-const-pattern11.cs#11)]

### <a name="var-pattern"></a>var パターン

`var` パターンは、すべての型または値に対応します。 *expr* の値は、常に *expr* のコンパイル時の型と同じ型のローカル変数に割り当てられます。 `is` 式の結果は常に `true` です。 構文は次のとおりです。

```csharp
   expr is var varname
```

次の例では、var パターンを使用して式を `obj` という変数に代入しています。 その後、`obj` の値と型が表示されます。

[!code-csharp[is#8](../../../../samples/snippets/csharp/language-reference/keywords/is/is-var-pattern8.cs#8)]

## <a name="c-language-specification"></a>C# 言語仕様
  
詳細については、[C# 言語仕様](~/_csharplang/spec/introduction.md)の「[The is operator (is 演算子)](~/_csharplang/spec/expressions.md#the-is-operator)」セクションおよび以下の C# 言語提案を参照してください。

- [パターン マッチング](~/_csharplang/proposals/csharp-7.0/pattern-matching.md)
- [ジェネリックを使用したパターン マッチング](~/_csharplang/proposals/csharp-7.1/generics-pattern-match.md)
  
## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# キーワード](index.md)
- [型テストとキャスト演算子](../operators/type-testing-and-cast.md)
