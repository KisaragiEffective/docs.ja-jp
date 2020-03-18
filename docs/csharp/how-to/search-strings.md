---
title: 文字列を検索する方法 (C# ガイド)
ms.date: 02/21/2018
helpviewer_keywords:
- searching strings [C#]
- strings [C#], searching with String methods
- strings [C#], searching with regular expressions
ms.assetid: fb1d9a6d-598d-4a35-bd5f-b86012edcb2b
ms.openlocfilehash: 15ea77d13a93d88bd996a22b6fe1aaad81df572d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74959703"
---
# <a name="how-to-search-strings"></a>文字列を検索する方法

2 つの主な戦略を使用して、文字列のテキストを検索することができます。 <xref:System.String> クラスのメソッドは特定のテキストを検索します。 正規表現はテキストのパターンを検索します。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

[string](../language-reference/builtin-types/reference-types.md#the-string-type) 型は、<xref:System.String?displayProperty=nameWithType> クラスのエイリアスであり、文字列の内容を検索するための多数の便利なメソッドを提供します。 その中に <xref:System.String.Contains%2A>、<xref:System.String.StartsWith%2A>、<xref:System.String.EndsWith%2A>、<xref:System.String.IndexOf%2A>、<xref:System.String.LastIndexOf%2A> が含まれています。 <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> クラスでは、テキストのパターンを検索するための豊富なボキャブラリが提供されます。 この記事では、これらの手法と、ニーズに最適なメソッドを選択する方法について説明します。

## <a name="does-a-string-contain-text"></a>文字列にテキストが含まれていますか?

<xref:System.String.Contains%2A?displayProperty=nameWithType>、<xref:System.String.StartsWith%2A?displayProperty=nameWithType> および <xref:System.String.EndsWith%2A?displayProperty=nameWithType>メソッドでは、特定のテキストの文字列を検索します。 次の例は、これらの各メソッドと、大文字と小文字を区別しない検索を使用するバリエーションを示しています。

[!code-csharp-interactive[search strings using methods](../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs#1)]

上の例は、これらのメソッドを使用する重要なポイントを示しています。 既定では、検索で**大文字と小文字が区別**されます。 大文字と小文字を区別しない検索を指定する場合は、<xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType> 列挙値を使用します。

## <a name="where-does-the-sought-text-occur-in-a-string"></a>検索対象テキストは文字列のどこにありますか?

<xref:System.String.IndexOf%2A> および <xref:System.String.LastIndexOf%2A> メソッドでは、文字列のテキストも検索します。 これらのメソッドは、検索されるテキストの場所を返します。 テキストが見つからない場合は、`-1` が返されます。 次の例では、"methods" という単語の最初と最後の出現箇所の検索を示し、その間のテキストを表示します。
  
[!code-csharp-interactive[search strings for indices](../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs#2)]

## <a name="finding-specific-text-using-regular-expressions"></a>正規表現を使用する特定のテキストの検索

文字列の検索には、<xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType> クラスを使用できます。 これらの検索の複雑さは、単純なテキスト パターンから複雑なテキスト パターンまでさまざまです。

次のコード例では、文章内の "the" または "their" という単語を検索します (大文字と小文字の区別は無視されます)。 静的メソッドの <xref:System.Text.RegularExpressions.Regex.IsMatch%2A?displayProperty=nameWithType> で検索を実行します。 検索対象の文字列と、検索パターンを指定します。 この例では、3 番目の引数で大文字と小文字を区別しない検索を指定します。 詳細については、<xref:System.Text.RegularExpressions.RegexOptions?displayProperty=nameWithType> を参照してください。  

検索パターンで検索対象のテキストを説明します。 次の表では、検索パターンの各要素について説明します (以下の表では、C# 文字列で `\` としてエスケープされる必要がある、単一の `\\` を使用します)。

| pattern  | 意味     |
| -------- |-------------|
| the      | テキスト "the" と一致 |
| (eir)?   | "eir" の 0 個または 1 個の出現箇所と一致 |
| \s       | 空白文字と一致    |
  
[!code-csharp-interactive[Search using regular expressions](../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs#3)]
  
> [!TIP]
> `string` メソッドは、通常、正確な文字列を検索するときに選択することをお勧めします。 正規表現は、ソース文字列で何らかのパターンを検索する場合に適しています。

## <a name="does-a-string-follow-a-pattern"></a>文字列はパターンに従っていますか?

次のコードでは正規表現を使用して、配列の各文字列の形式を検証します。 各文字列が電話番号の形式であることが検証されます。つまり、3 グループの数値がダッシュで区切られ、最初の 2 グループには 3 桁の数値が含まれ、3 つ目のグループには 4 桁の数値が含まれることが検証されます。 検索パターンでは正規表現の `^\\d{3}-\\d{3}-\\d{4}$` を使用します。 詳細については、「[正規表現言語 - クイック リファレンス](../../standard/base-types/regular-expression-language-quick-reference.md)」をご覧ください。

| pattern  | 意味                             |
| -------- |-------------------------------------|
| ^        | 文字列の先頭と一致 |
| \d{3}    | 3 桁の文字と完全に一致  |
| -        | '-' 文字と一致           |
| \d{3}    | 3 桁の文字と完全に一致  |
| -        | '-' 文字と一致           |
| \d{4}    | 4 桁の文字と完全に一致  |
| $        | 文字列の末尾と一致       |

[!code-csharp-interactive[csProgGuideStrings#4](../../../samples/snippets/csharp/how-to/strings/SearchStrings.cs#4)]

この単一の検索パターンは多くの有効な文字列と一致します。 単一のテキスト文字列ではなく、パターンの検索やパターンに対する検証を行う場合は、正規表現が適しています。

[GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/strings)のコードを見て、これらのサンプルを試すことができます。 または、サンプルを [zip ファイルとして](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/strings.zip)ダウンロードすることができます。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../programming-guide/index.md)
- [文字列](../programming-guide/strings/index.md)
- [LINQ と文字列](../programming-guide/concepts/linq/linq-and-strings.md)
- <xref:System.Text.RegularExpressions.Regex?displayProperty=nameWithType>
- [.NET Framework 正規表現](../../standard/base-types/regular-expressions.md)
- [正規表現言語 - クイック リファレンス](../../standard/base-types/regular-expression-language-quick-reference.md)
- [.NET の文字列を使用するためのベスト プラクティス](../../standard/base-types/best-practices-strings.md)
