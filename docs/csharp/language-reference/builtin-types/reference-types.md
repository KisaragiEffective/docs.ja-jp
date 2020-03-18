---
title: 組み込みの参照型 - C# リファレンス
description: 宣言するために使用できる C# キーワードがある参照型について説明します。
ms.date: 06/25/2019
f1_keywords:
- object_CSharpKeyword
- object
- delegate_CSharpKeyword
- delegate
- dynamic_CSharpKeyword
- string
- string_CSharpKeyword
helpviewer_keywords:
- object keyword [C#]
- delegate keyword [C#]
- function pointers [C#]
- dynamic [C#]
- dynamic keyword [C#]
- strings [C#], reference
- '@ string literal'
- string literals [C#]
- string keyword [C#]
ms.openlocfilehash: c2c03f47babd9ccf87eb60d33b9d65d1a9c82e2e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398311"
---
# <a name="built-in-reference-types-c-reference"></a>組み込みの参照型 (C# リファレンス)

C# には複数の組み込み参照型があります。 それらには、.NET ライブラリでの型に対するシノニムであるキーワードまたは演算子があります。

## <a name="the-object-type"></a>オブジェクトの型

`object` 型は .NET での <xref:System.Object?displayProperty=nameWithType> の別名です。 C# の統一型システムでは、すべての型 (定義済み、ユーザー定義、参照型、および値型) が、直接または間接的に <xref:System.Object?displayProperty=nameWithType> を継承します。 `object` 型の変数には、任意の型の値を割り当てることができます。 すべての `object` 変数には、リテラル `null` を使って既定値を割り当てることができます。 値型の変数が object に変換されることを、*ボックス化*されると言います。 `object` 型の変数が値型に変換されることを、*ボックス化解除*されると言います。 詳細については、「[ボックス化とボックス化解除](../../programming-guide/types/boxing-and-unboxing.md)」を参照してください。

## <a name="the-string-type"></a>文字列型

`string` 型は、0 個以上の Unicode 文字のシーケンスを表します。 `string` は .NET の <xref:System.String?displayProperty=nameWithType> の別名です。

`string` は参照型ですが、[等値演算子 `==` および `!=`](../operators/equality-operators.md#string-equality) は、`string` オブジェクトの参照ではなく、値を比較するように定義されています。 これにより、文字列が等しいかを直感的にテストできます。 次に例を示します。

```csharp-interactive
string a = "hello";
string b = "h";
// Append to contents of 'b'
b += "ello";
Console.WriteLine(a == b);
Console.WriteLine(object.ReferenceEquals(a, b));
```

文字列の内容が等しいので、"True"、"False" の順に表示されますが、`a` および `b` は同じ文字列インスタンスを参照しません。

[+ 演算子](../operators/addition-operator.md#string-concatenation)では、文字列が連結されます。

```csharp
string a = "good " + "morning";
```

これは、"good morning" を含む文字列オブジェクトを作成します。

文字列は "*変更不可*" です。文字列オブジェクトの作成後、そのコンテンツを変更することはできません。構文では変更可能に見えても、変更不可です。 たとえば、このコードを作成すると、コンパイラによって新しい文字列オブジェクトを格納する新しいシーケンス オブジェクトが生成され、その新しいオブジェクトが `b` に割り当てられます。 `b` に割り当てられたメモリは (文字列 "h" が含まれている場合)、ガベージ コレクションの対象になります。

```csharp
string b = "h";
b += "ello";
```

`[]` [演算子](../operators/member-access-operators.md#indexer-operator-)は、文字列の各文字への読み取り専用アクセスに使用できます。 有効なインデックス値は `0` から始まり、文字列の長さ未満である必要があります。

```csharp
string str = "test";
char x = str[2];  // x = 's';
```

同様に、`[]` 演算子を使って文字列内の各文字を反復処理することもできます。

```csharp-interactive
string str = "test";

for (int i = 0; i < str.Length; i++)
{
  Console.Write(str[i] + " ");
}
// Output: t e s t
```

文字列リテラルは `string` 型であり、二重引用符で囲む形式と、`@` 付きの二重引用符で囲む形式の 2 種類があります。 二重引用符で囲む場合は、リテラル文字列の前後に二重引用符 (") を付けます。

```csharp
"good morning"  // a string literal
```

リテラル文字列には、任意の文字リテラルを含めることができます。 これにはエスケープ シーケンスが含まれます。 次の例では、円記号にエスケープ シーケンス `\\`、文字 f に `\u0066`、改行に `\n` を使用しています。

```csharp-interactive
string a = "\\\u0066\n F";
Console.WriteLine(a);
// Output:
// \f
//  F
```

> [!NOTE]
> エスケープ コード `\udddd` (`dddd` は 4 桁の数字) は、Unicode 文字 U +`dddd` を表します。 8 桁の Unicode エスケープ コード `\Udddddddd` も認識できます。

[verbatim 文字列リテラル](../tokens/verbatim.md)の場合は、先頭に `@` を付け、さらに前後に二重引用符を付けます。 次に例を示します。

```csharp
@"good morning"  // a string literal
```

verbatim 文字列の場合の利点は、エスケープ シーケンスが "*処理されない*" ため、たとえば、完全修飾 Windows ファイル名が書きやすくなることです。

```csharp
@"c:\Docs\Source\a.txt"  // rather than "c:\\Docs\\Source\\a.txt"
```

@-quoted に続いて引用符で囲まれた文字列に二重引用符を含めるには、二重引用符を二重にします。

```csharp
@"""Ahoy!"" cried the captain." // "Ahoy!" cried the captain.
```

## <a name="the-delegate-type"></a>デリゲート型

デリゲート型の宣言は、メソッド シグネチャに似ています。 戻り値 1 つのほか、任意の型のパラメーターをいくつでも指定することができます。

```csharp
public delegate void MessageDelegate(string message);
public delegate int AnotherDelegate(MyType m, long num);
```

.NET では、`System.Action` 型と `System.Func` 型により、多くの一般的なデリゲートに対するジェネリック定義が提供されます。 おそらく、新しいカスタム デリゲート型を定義する必要はありません。 代わりに、提供されたジェネリック型のインスタンス化を作成できます。

`delegate` は、名前付きメソッドまたは匿名メソッドをカプセル化することができる参照型です。 デリゲートは C++ の関数ポインターに似ていますが、タイプ セーフであり安全です。 デリゲートの使い方については、[デリゲート](../../programming-guide/delegates/index.md)と[汎用デリゲート](../../programming-guide/generics/generic-delegates.md)に関するページを参照してください。 デリゲートは[イベント](../../programming-guide/events/index.md)の土台となる働きをします。 デリゲートは名前付きメソッドまたは匿名メソッドに関連付けることによって、インスタンス化することができます。

デリゲートは、適合する入力パラメーターと戻り値の型を持ったメソッドまたはラムダ式でインスタンス化する必要があります。 メソッドのシグネチャでどの程度の変性が許容されるかについて詳しくは、[デリゲートの変性](../../programming-guide/concepts/covariance-contravariance/using-variance-in-delegates.md)に関するページを参照してください。 匿名メソッドで使用する場合は、デリゲートとそれに関連付けるコードとを一緒に宣言します。

## <a name="the-dynamic-type"></a>dynamic 型

`dynamic` 型は、変数およびそのメンバーに対する参照の使用が、コンパイル時の型チェックをバイパスすることを示します。 代わりに、演算は実行時に解決されます。 `dynamic` 型により、Office オートメーション API などの COM API、IronPython ライブラリなどの動的 API、および HTML ドキュメント オブジェクト モデル (DOM: Document Object Model) へのアクセスが容易になります。

ほとんどの環境で、`dynamic` 型は `object` 型のように動作します。 具体的には、null 以外の任意の式を `dynamic` 型に変換できます。 `dynamic` 型は `object` と異なり、`dynamic` 型の式を含む演算はコンパイラによって解決または型チェックされません。 コンパイラは演算に関する情報をまとめてパッケージ化します。その情報が後で実行時に演算を評価するために使用されます。 このプロセスの過程で、`dynamic` 型の変数は `object` 型の変数にコンパイルされます。 そのため、`dynamic` 型はコンパイル時にのみ存在し、実行時には存在しません。

`dynamic` 型の変数と `object` 型の変数の違いを次に示します。 コンパイル時に各変数の型を確認するには、`dyn` ステートメントの `obj` または `WriteLine` にマウス ポインターを置きます。 IntelliSense が使用可能なエディターに、次のコードをコピーします。 IntelliSense 機能によって、**には**dynamic`dyn`、**には**object`obj` が表示されます。

[!code-csharp[csrefKeywordsTypes#21](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic1.cs#21)]

<xref:System.Console.WriteLine%2A> ステートメントは `dyn` および `obj` の実行時の型を表示します。 その時点では、両方が同じ整数型を持ちます。 次の出力が生成されます。

```console
System.Int32
System.Int32
```

コンパイル時の `dyn` と `obj` の違いを確認するには、前の例の宣言と `WriteLine` ステートメントの間に次の 2 行を追加します。

```csharp
dyn = dyn + 3;
obj = obj + 3;
```

 式 `obj + 3` に整数およびオブジェクトを追加しようとしたことに対してコンパイル エラーが報告されます。 ただし、`dyn + 3` に関するエラーは報告されません。 `dyn` を含む式はコンパイル時にはチェックされません。これは、`dyn` の型が `dynamic` であるためです。

さまざまな宣言で `dynamic` を使用する例を次に示します。 また、`Main` メソッドで、コンパイル時の型チェックと実行時の型チェックの違いを確認できます。

[!code-csharp[csrefKeywordsTypes#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsTypes/CS/dynamic2.cs#25)]

### <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# のキーワード](../keywords/index.md)
- [イベント](../../programming-guide/events/index.md)
- [dynamic 型の使用](../../programming-guide/types/using-type-dynamic.md)
- [文字列を使用するためのベスト プラクティス](../../../standard/base-types/best-practices-strings.md)
- [基本的な文字列操作](../../../standard/base-types/basic-string-operations.md)
- [新しい文字列の作成](../../../standard/base-types/creating-new.md)
- [型テストとキャスト演算子](../operators/type-testing-and-cast.md)
- [パターン マッチング、is 演算子、as 演算子を使用して安全にキャストする方法](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md)
- [チュートリアル: 動的オブジェクトの作成と使用](../../programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)
- <xref:System.Object?displayProperty=nameWithType>
- <xref:System.String?displayProperty=nameWithType>
- <xref:System.Dynamic.DynamicObject?displayProperty=nameWithType>
