---
title: .NET の文字列からの文字のトリムと削除
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- strings [.NET Framework], removing characters
- Remove method
- TrimEnd method
- Trim method
- trimming characters
- TrimStart method
- removing characters
ms.assetid: ab248dab-70d4-4413-81c6-542d153fd195
ms.openlocfilehash: bdbe267bb178e90c0008422e6543a23178c2c4d8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78159989"
---
# <a name="trimming-and-removing-characters-from-strings-in-net"></a>.NET の文字列からの文字のトリムと削除
文章を個々の単語に分割すると、単語の先頭または末尾に空白が残る場合があります。 そのような場合は、**System.String** クラスのトリム メソッドのいずれかを使用して、文字列内の指定した位置から任意の数の空白またはその他の文字を削除できます。 使用できるトリム メソッドとその説明を次の表に示します。  
  
|メソッド名|用途|  
|-----------------|---------|  
|<xref:System.String.Trim%2A?displayProperty=nameWithType>|文字列の先頭と末尾から、空白または文字配列で指定した文字を削除します。|  
|<xref:System.String.TrimEnd%2A?displayProperty=nameWithType>|文字列の末尾から、文字配列で指定した文字を削除します。|  
|<xref:System.String.TrimStart%2A?displayProperty=nameWithType>|文字列の先頭から、文字配列で指定した文字を削除します。|  
|<xref:System.String.Remove%2A?displayProperty=nameWithType>|文字列内の指定したインデックス位置から、指定した数の文字を削除します。|  
  
## <a name="trim"></a>Trim

 <xref:System.String.Trim%2A?displayProperty=nameWithType> メソッドを使用すると、文字列の両端から空白を簡単に削除できます。メソッドの使用例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#17](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#17)]
 [!code-csharp[Conceptual.String.BasicOps#17](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#17)]
 [!code-vb[Conceptual.String.BasicOps#17](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#17)]  
  
 文字列の先頭と末尾から、文字配列で指定した文字を削除することもできます。 次に、空白文字、ピリオド、およびアスタリスクを削除する例を示します。  
  
 [!code-csharp[Conceptual.String.BasicOps#22](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trim2.cs#22)]
 [!code-vb[Conceptual.String.BasicOps#22](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trim2.vb#22)]  
  
## <a name="trimend"></a>TrimEnd

 **String.TrimEnd** メソッドは、文字列の末尾から文字を削除して新しい文字列オブジェクトを作成します。 このメソッドには、削除する文字を指定する文字配列が渡されます。 文字配列内での要素の順序は、トリム操作に影響しません。 トリム操作は、文字配列で指定された文字が見つからなくなった時点で停止します。  
  
 **TrimEnd** メソッドを使用して文字列の末尾の数文字を削除する例を次に示します。 この例では、文字配列内の文字の順序が操作に影響しないことを示すために、`'r'` という文字と `'W'` という文字の順序を逆にしてあります。 このコードは、`MyString` の最後の単語だけでなく、最初の単語の一部も削除します。  
  
 [!code-cpp[Conceptual.String.BasicOps#18](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#18)]
 [!code-csharp[Conceptual.String.BasicOps#18](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#18)]
 [!code-vb[Conceptual.String.BasicOps#18](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#18)]  
  
 このコードは、コンソールに `He` と出力します。  
  
 **TrimEnd** メソッドを使用して、文字列の最後の単語を削除する例を次に示します。 このコードでは、`Hello` という単語の後にコンマがありますが、削除する文字の配列にコンマは指定されていないため、コンマの位置でトリムが停止します。  
  
 [!code-cpp[Conceptual.String.BasicOps#19](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#19)]
 [!code-csharp[Conceptual.String.BasicOps#19](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#19)]
 [!code-vb[Conceptual.String.BasicOps#19](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#19)]  
  
 このコードは、コンソールに `Hello,` と出力します。  
  
## <a name="trimstart"></a>TrimStart

 **String.TrimStart** メソッドは **String.TrimEnd** メソッドと似ていますが、既存の文字列オブジェクトの先頭から文字を削除して新しい文字列を作成する点が異なります。 **TrimStart** メソッドには、削除する文字を指定する文字配列が渡されます。 **TrimEnd** メソッドの場合と同様に、文字配列内での要素の順序はトリム操作に影響しません。 トリム操作は、文字配列で指定された文字が見つからなくなった時点で停止します。  
  
 文字列の最初の単語を削除する例を次に示します。 この例では、文字配列内の文字の順序が操作に影響しないことを示すために、`'l'` という文字と `'H'` という文字の順序を逆にしてあります。  
  
 [!code-cpp[Conceptual.String.BasicOps#20](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#20)]
 [!code-csharp[Conceptual.String.BasicOps#20](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#20)]
 [!code-vb[Conceptual.String.BasicOps#20](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#20)]  
  
 このコードは、コンソールに `World!` と出力します。  
  
## <a name="remove"></a>削除

 <xref:System.String.Remove%2A?displayProperty=nameWithType> メソッドは、既存の文字列内の指定した位置を開始位置として、指定した数の文字を削除します。 このメソッドは、インデックスが 0 から始まっていることを前提としています。  
  
 文字列の 0 から始まる 5 番目のインデックスを開始位置として、10 文字を削除する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#21](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/trimming.cpp#21)]
 [!code-csharp[Conceptual.String.BasicOps#21](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/trimming.cs#21)]
 [!code-vb[Conceptual.String.BasicOps#21](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/trimming.vb#21)]  
  
## <a name="replace"></a>Replace

 また、<xref:System.String.Replace%28System.String%2CSystem.String%29?displayProperty=nameWithType> メソッドを呼び出し、置換対象として空の文字列 (<xref:System.String.Empty?displayProperty=nameWithType>) を指定することで、文字列から指定した文字または部分文字列を削除することもできます。 文字列からすべてのコンマを削除する例を次に示します。  
  
 [!code-csharp[Conceptual.String.BasicOps#23](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/replace1.cs#23)]
 [!code-vb[Conceptual.String.BasicOps#23](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/replace1.vb#23)]  
  
## <a name="see-also"></a>参照

- [基本的な文字列操作](../../../docs/standard/base-types/basic-string-operations.md)
