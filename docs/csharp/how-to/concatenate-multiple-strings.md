---
title: 複数の文字列を連結する方法 (C# ガイド)
description: C# で文字列を連結する方法は複数あります。 各選択のオプションと、それを選ぶ理由も示します。
ms.date: 02/20/2018
helpviewer_keywords:
- joining strings [C#]
- concatenating strings [C#]
- strings [C#], concatenation
ms.assetid: 8e16736f-4096-4f3f-be0f-9d4c3ff63520
ms.openlocfilehash: 9a0640a7ce73fa8454442cd301157bf5c265f9de
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713898"
---
# <a name="how-to-concatenate-multiple-strings-c-guide"></a>複数の文字列を連結する方法 (C# ガイド)

*連結*とは、ある文字列を別の文字列の末尾に追加するプロセスです。 文字列を連結するには、`+` 演算子を使用します。 文字列リテラルと文字列定数の場合、連結はコンパイル時に行われ、実行時には行われません。 文字列変数の連結は実行時にのみ行われます。

[!INCLUDE[interactive-note](~/includes/csharp-interactive-note.md)]

次の例では、ソース コードを読みやすくするために、連結を使用して長い文字列リテラルを短い文字列に分割しています。 これらの部分は、コンパイル時に単一の文字列に連結されます。 関係する文字列の数に関係なく、実行時にパフォーマンス コストは発生しません。  
  
 [!code-csharp-interactive[Combining strings at compile time](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#1)]  

文字列変数を連結する場合は、`+` または `+=` 演算子、[文字列補間](../language-reference/tokens/interpolated.md)または <xref:System.String.Format%2A?displayProperty=nameWithType>、<xref:System.String.Concat%2A?displayProperty=nameWithType>、<xref:System.String.Join%2A?displayProperty=nameWithType>、<xref:System.Text.StringBuilder.Append%2A?displayProperty=nameWithType> メソッドを使用できます。 `+` 演算子は使い方が簡単で、直感的なコードにすることができます。 1 つのステートメントで複数の `+` 演算子を使用している場合でも、文字列の内容がコピーされるのは 1 回のみです。 次のコードは、`+` および `+=` 演算子を使用して文字列を連結する例を示しています。

[!code-csharp-interactive[combining strings using +](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#2)]  

式によっては、次のコードに示すように、文字列補間を使用して文字列を連結する方が簡単な場合があります。
  
[!code-csharp-interactive[building strings using string interpolation](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#3)]  
  
> [!NOTE]
> 文字列連結操作において、C# コンパイラでは null 文字列は空の文字列と同様に扱われます。

文字列を連結する他のメソッドとして、<xref:System.String.Format%2A?displayProperty=nameWithType> があります。 このメソッドは、少数のコンポーネント文字列から文字列を作成する場合に有効です。

他にも、ループ内の文字列を結合する場合があります。このケースでは、結合するソース文字列の数はわからず、ソース文字列の実際の数は非常に大きくなる可能性があります。 <xref:System.Text.StringBuilder> クラスは、このようなシナリオのために設計されています。 次のコードでは、<xref:System.Text.StringBuilder.Append%2A> クラスの <xref:System.Text.StringBuilder> メソッドを使用して文字列を連結しています。  
  
[!code-csharp-interactive[string concatenation using string builder](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#4)]  

[文字列の連結または `StringBuilder` クラスを選択する理由](xref:System.Text.StringBuilder#StringAndSB)に関するページで詳細をご確認ください。

コレクションからの文字列を結合する別のオプションとして、<xref:System.String.Concat%2A?displayProperty=nameWithType> メソッドを使用する方法があります。 ソース文字列を区切り記号で区切る必要がある場合は、<xref:System.String.Join%2A?displayProperty=nameWithType> メソッドを使用します。 次のコードでは、両方のメソッドを使用して単語の配列を結合します。

[!code-csharp-interactive[concatenation of string collection](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#5)]

最後に、[LINQ](../programming-guide/concepts/linq/index.md) および <xref:System.Linq.Enumerable.Aggregate%2A?displayProperty=nameWithType> メソッドを使用して、コレクションからの文字列を結合できます。 このメソッドは、ラムダ式を使用してソース文字列を結合します。 ラムダ式は、各文字列を既存の蓄積に追加する処理を行います。 次の例では、配列内の各単語の間にスペースを追加することによって単語の配列を結合します。

[!code-csharp-interactive[string concatenation using LINQ expressions](../../../samples/snippets/csharp/how-to/strings/Concatenate.cs#6)]  

[GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/strings)のコードを見て、これらのサンプルを試すことができます。 または、サンプルを [zip ファイルとして](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/strings.zip)ダウンロードすることができます。

## <a name="see-also"></a>参照

- <xref:System.String>
- <xref:System.Text.StringBuilder>
- [C# プログラミングガイド](../programming-guide/index.md)
- [文字列](../programming-guide/strings/index.md)
