---
title: 暗黙的に型指定されるローカル変数 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- implicitly-typed local variables [C#]
- var [C#]
ms.assetid: b9218fb2-ef5d-4814-8a8e-2bc29b0bbc9b
ms.openlocfilehash: 692a0f8ad933f3ba4bef50681cb3487fa0a7eea9
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75714830"
---
# <a name="implicitly-typed-local-variables-c-programming-guide"></a>暗黙的に型指定されるローカル変数 (C# プログラミング ガイド)

ローカル変数は、明示的な型を指定しないで宣言できます。 `var` キーワードは、初期化ステートメントの右辺にある式から変数の型を推論するようにコンパイラに指示します。 推論される型は、組み込み型、匿名型、ユーザー定義型、または .NET Framework クラス ライブラリで定義されている型である可能性があります。 `var` で配列を初期化する方法の詳細については、「[暗黙的に型指定される配列](../arrays/implicitly-typed-arrays.md)」を参照してください。

ローカル変数を `var` で宣言するさまざまな方法を次の例に示します。

[!code-csharp[csProgGuideLINQ#43](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#43)]

`var` キーワードは "バリアント" を意味するのではなく、変数の厳密でない型指定や遅延バインディングを示すものでもないことを理解することが重要です。 単に、最も適切な型をコンパイラが決定して割り当てることを意味します。

`var` キーワードは、次のコンテキストで使用される場合があります。

- 前の例で示したようなローカル変数 (メソッドのスコープで宣言された変数)。

- [for](../../language-reference/keywords/for.md) 初期化ステートメント。

    ```csharp
    for (var x = 1; x < 10; x++)
    ```

- [foreach](../../language-reference/keywords/foreach-in.md) 初期化ステートメント。

    ```csharp
    foreach (var item in list) {...}
    ```

- [using](../../language-reference/keywords/using-statement.md) ステートメント。

    ```csharp
    using (var file = new StreamReader("C:\\myfile.txt")) {...}
    ```

詳細については、「[「クエリ式で暗黙的に型指定されるローカル変数および配列を使用する方法](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)」を参照してください。

## <a name="var-and-anonymous-types"></a>var と匿名型

多くの場合、`var` の使用は任意であり、構文上便利なだけです。 ただし、変数が匿名型で初期化される場合、後でオブジェクトのプロパティへのアクセスが必要になったら、変数を `var` として宣言する必要があります。 これは、LINQ クエリ式では一般的なシナリオです。 詳細については、「[匿名型](anonymous-types.md)」を参照してください。

ソース コードの観点から見ると、匿名型には名前がありません。 そのため、クエリ変数が `var` で初期化された場合、返されたオブジェクトのシーケンスのプロパティにアクセスするための唯一の方法は、`foreach` ステートメント内の繰り返し変数の型として `var` を使用することです。

[!code-csharp[csProgGuideLINQ#44](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csRef30LangFeatures_2.cs#44)]

## <a name="remarks"></a>Remarks

暗黙的に型指定される変数の宣言には、次の制限が適用されます。

- `var` を使用できるのは、同じステートメント内でローカル変数の宣言と初期化が行われる場合のみです。この変数は、null、メソッド グループ、または匿名関数に初期化することはできません。

- `var` は、クラス スコープのフィールドで使用できません。

- `var` を使用して宣言された変数は、初期化式では使用できません。 つまり、`int i = (i = 20);` という式は有効ですが、`var i = (i = 20);` という式はコンパイル時エラーが生成されます。

- 暗黙的に型指定された複数の変数を同じステートメント内で初期化することはできません。

- `var` という名前の型がスコープ内にある場合、`var` キーワードはその型名に解決され、暗黙的に型指定されたローカル変数の宣言の一部とは見なされません。

`var` キーワードによる暗黙の型指定は、ローカル メソッド スコープの変数にのみ適用できます。 暗黙の型指定はクラス フィールドには使用できません。C# コンパイラでコードを処理するときに論理的パラドックスにぶつかるためです。コンパイラはフィールドの型を認識する必要がありますが、代入式が分析されるまで型を判断できません。そして、型を認識していなければ、式を評価できません。 次のコードがあるとします。

```csharp
private var bookTitles;
```

`bookTitles` は、型が `var` のクラス フィールドです。 このフィールドには評価する式がないため、コンパイラでは `bookTitles` の型を推定できません。 さらに、(ローカル変数の場合と同様に) フィールドに式を追加するだけでは不十分です。

```csharp
private var bookTitles = new List<string>();
```

コードのコンパイル中にコンパイラでフィールドが検出されると、それに関連付けられている式を処理する前に各フィールドの型が記録されます。 コンパイラでの `bookTitles` の解析時に同じパラドックスにぶつかります。フィールドの型を認識する必要がありますが、通常、コンパイラは式を解析して `var` の型を判断します。これには、事前に型を認識している必要があります。

`var` は、クエリ式と使用する場合に便利なこともあります。クエリ変数の構築された型を厳密に判別することが難しい場合です。 このような状況は、グループ化と並べ替えの処理で発生することがあります。

`var` キーワードは、変数の特定の型をキーボードで入力するのが面倒な場合、その型が明白な場合、型によってコードが読みやすくならない場合にも役立ちます。 このような理由で `var` が有用な例の 1 つとして、グループ化処理で使用されるような入れ子にされたジェネリック型があります。 次のクエリでは、クエリ変数の型は `IEnumerable<IGrouping<string, Student>>` です。 コードの作成者とそのコードを保守する担当者がこの点を理解している限り、簡略化するために暗黙的な型指定を使用しても問題はありません。

[!code-csharp[cscsrefQueryKeywords#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsCsrefQueryKeywords/CS/Group.cs#13)]

ただし、`var` を使用すると、他の開発者がコードを理解しづらくなる可能性はあります。 このため、C# のドキュメントでは、通常、必要な場合にだけ `var` を使用しています。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../../language-reference/index.md)
- [暗黙的に型指定される配列](../arrays/implicitly-typed-arrays.md)
- [クエリ式で暗黙的に型指定されるローカル変数および配列を使用する方法](how-to-use-implicitly-typed-local-variables-and-arrays-in-a-query-expression.md)
- [匿名型](anonymous-types.md)
- [オブジェクト初期化子とコレクション初期化子](object-and-collection-initializers.md)
- [var](../../language-reference/keywords/var.md)
- [C# での LINQ](../../linq/index.md)
- [統合言語クエリ (LINQ)](../../linq/index.md)
- [for](../../language-reference/keywords/for.md)
- [foreach、in](../../language-reference/keywords/foreach-in.md)
- [using ステートメント](../../language-reference/keywords/using-statement.md)
