---
title: String.Split を使用して文字列を解析する方法 (C# ガイド)
description: String.Split は、一連の区切り記号で分割された文字列の配列を返します。 文字列を解析する簡単な方法です。
ms.date: 01/03/2018
helpviewer_keywords:
- splitting strings [C#]
- Split method [C#]
- strings [C#], splitting
- parse strings
ms.assetid: 729c2923-4169-41c6-9c90-ef176c1e2953
ms.custom: mvc
ms.openlocfilehash: b46429f3b55658e1f2a7d21eed714c1d02236c57
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73973236"
---
# <a name="how-to-parse-strings-using-stringsplit-c-guide"></a>String.Split を使用して文字列を解析する方法 (C# ガイド)

<xref:System.String.Split%2A?displayProperty=nameWithType> メソッドは、1 つまたは複数の区切り記号に基づいて入力文字列を分割することで部分文字列の配列を作成します。 英語のように単語の間にスペースがある文章の場合に、単語の境界で文字列を分割する最も簡単な方法になります。 他の特定の文字や文字列で文字列を分割する際にも利用されます。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

次のコードは一般的なフレーズを単語ごとの文字列の配列に分割します。

[!code-csharp-interactive[split strings on word boundaries](../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs#1)]

区切り文字のインスタンスごとに、返される配列で値が生成されます。 連続する区切り文字により、返される配列の値として空の文字列が生成されます。  次の例でこれを確認できます。次の例では区切り文字としてスペースが使用されています。

[!code-csharp-interactive[split strings with repeated separators](../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs#2)]

この動作により、コンマ区切り値 (CSV) ファイルなどの形式で表形式データを簡単に表すことができます。 連続するコンマは空の列を表します。

任意の <xref:System.StringSplitOptions.RemoveEmptyEntries?displayProperty=nameWithType> パラメーターを渡し、返される配列で空の文字列を除外できます。 返されるコレクションの処理が複雑な場合、[LINQ](../programming-guide/concepts/linq/index.md) を使用し、結果のシーケンスを操作できます。

<xref:System.String.Split%2A?displayProperty=nameWithType> では、複数の区切り文字を使用できます。
次の例ではスペース、コンマ、ピリオド、コロン、タブを使用しています。すべて、これらの区切り文字を格納した配列で <xref:System.String.Split%2A> に渡されます。
コードの一番下にあるループは、返される配列の各単語を表示します。  

[!code-csharp-interactive[split strings using multiple separators](../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs#3)]

区切りの連続するインスタンスにより、出力配列で空の文字列が生成されます。

[!code-csharp-interactive[split strings using multiple consecutive separators](../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs#4)]

<xref:System.String.Split%2A?displayProperty=nameWithType> は、文字列の配列 (1 つの文字ではなく、対象の文字列を解析するための区切り記号として機能する文字シーケンス) を受け取ることができます。  
  
[!code-csharp-interactive[split strings using strings as separators](../../../samples/snippets/csharp/how-to/strings/ParseStringsUsingSplit.cs#5)]

[GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/strings)のコードを見て、これらのサンプルを試すことができます。 または、サンプルを [zip ファイルとして](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/strings.zip)ダウンロードすることができます。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../programming-guide/index.md)
- [文字列](../programming-guide/strings/index.md)
- [.NET の正規表現](../../standard/base-types/regular-expressions.md)
