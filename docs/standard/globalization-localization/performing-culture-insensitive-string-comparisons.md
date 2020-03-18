---
title: カルチャを認識しない文字列比較の実行
ms.date: 08/22/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- String.CompareTo method
- String.Compare method
- string comparison [.NET Framework], culture-insensitive
- strings [.NET Framework], comparing
- culture-insensitive string operations, comparisons
- culture parameter
ms.assetid: abae50ef-32f7-4a50-a540-fd256fd1aed0
ms.openlocfilehash: 85ba91b63ab0edbccc768e2d1ad4aaef31fd2f21
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73120831"
---
# <a name="performing-culture-insensitive-string-comparisons"></a>カルチャを認識しない文字列比較の実行
既定では、<xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドはカルチャを認識し、大文字と小文字を区別する比較を実行します。 また、このメソッドには、使用するカルチャを `culture` パラメーターで指定し、使用する比較規則を `comparisonType` パラメーターで指定できる、複数のオーバーロードが含まれています。 既定のオーバーロードの代わりにこれらのメソッドを呼び出すと、特定のメソッド呼び出しで使用する規則に関するあいまいさが解消され、特定の比較がカルチャに依存するかどうかが明確になります。  
  
> [!NOTE]
> <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドの両方のオーバーロードでは、カルチャに依存した比較と大文字と小文字を区別する比較を実行します。このメソッドを使用してカルチャに依存しない比較を実行することはできません。 コードをわかりやすくするために、<xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを代わりに使用することをお勧めします。  
  
 カルチャに依存する操作の場合は、<xref:System.StringComparison.CurrentCulture?displayProperty=nameWithType> パラメーターとして <xref:System.StringComparison.CurrentCultureIgnoreCase?displayProperty=nameWithType> 列挙値または `comparisonType` 列挙値を指定します。 現在のカルチャ以外の指定されたカルチャを使用して、カルチャに依存した比較を実行する場合は、<xref:System.Globalization.CultureInfo> パラメーターとして、そのカルチャを表す `culture` オブジェクトを指定します。  
  
 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドでサポートされている、カルチャに依存しない文字列比較は、言語的な比較 (インバリアント カルチャの並べ替え規則に基づきます) または非言語的な比較 (文字列内の文字の序数値に基づきます) です。 カルチャに依存しないほとんどの文字列比較は非言語的な比較です。 このような比較の場合は、<xref:System.StringComparison.Ordinal?displayProperty=nameWithType> パラメーターとして <xref:System.StringComparison.OrdinalIgnoreCase?displayProperty=nameWithType> 列挙値または `comparisonType` 列挙値を指定します。 たとえば、セキュリティに関する決定 (ユーザー名やパスワードの比較など) が文字列比較の結果に基づいて行われる場合は、この操作をカルチャに依存しない非言語的な比較に指定して、操作の結果が特定のカルチャまたは言語の規則に影響されないようにする必要があります  
  
 複数のカルチャからの対応する文字列を一貫した方法で言語的に処理する場合は、カルチャに依存しない言語的な文字列比較を使用します。 たとえば、アプリケーションのリスト ボックスに表示する単語の一覧で複数の文字セットが使用されている場合などに、現在のカルチャに関係なく同じ順序で単語を表示することが必要になります。 カルチャに依存しない言語的な比較用に、英語の言語規則に基づくインバリアント カルチャが .NET Framework によって定義されています。 カルチャに依存しない言語的な比較を実行するには、<xref:System.StringComparison.InvariantCulture?displayProperty=nameWithType> パラメーターとして <xref:System.StringComparison.InvariantCultureIgnoreCase?displayProperty=nameWithType> または `comparisonType` を指定します。  
  
 次の例では、カルチャに依存しない 2 つの非言語的な文字列比較を実行します。 最初の例では大文字小文字が区別されますが、2 番目の例では区別されません。  
  
 [!code-csharp[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/cs/cultureinsensitive1.cs#1)]
 [!code-vb[Conceptual.Strings.CultureInsensitiveComparison#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.strings.cultureinsensitivecomparison/vb/cultureinsensitive1.vb#1)]  

Windows オペレーティング システムの並べ替え操作と比較操作で使用される文字の重みに関する情報を含む一連のテキスト ファイルである[並べ替え重みテーブル](https://www.microsoft.com/download/details.aspx?id=10921) と、Linux と macOS 用の並べ替え重みテーブルである [デフォルト Unicode 照合基本テーブル](https://www.unicode.org/Public/UCA/latest/allkeys.txt)をダウンロードできます。

## <a name="see-also"></a>参照

- <xref:System.String.Compare%2A?displayProperty=nameWithType>
- <xref:System.String.CompareTo%2A?displayProperty=nameWithType>
- [カルチャを認識しない文字列操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)
- [文字列を使用するためのベスト プラクティス](../../../docs/standard/base-types/best-practices-strings.md)
