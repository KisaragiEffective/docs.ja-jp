---
title: .NET で文字列を比較する
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- value comparisons of strings
- LastIndexOf method
- CompareTo method
- IndexOf method
- Compare method
- strings [.NET Framework], comparing
- CompareOrdinal method
- EndsWith method
- Equals method
- StartsWith method
ms.assetid: 977dc094-fe19-4955-98ec-d2294d04a4ba
ms.openlocfilehash: e63b2a8ac44d6171f9c48990882780ea420f8c76
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73101672"
---
# <a name="comparing-strings-in-net"></a>.NET で文字列を比較する
.NET は、文字列の値を比較するためのメソッドをいくつか提供します。 これらの値の比較メソッドとその説明を次の表に示します。  
  
|メソッド名|用途|  
|-----------------|---------|  
|<xref:System.String.Compare%2A?displayProperty=nameWithType>|2 つの文字列の値を比較します。 整数値を返します。|  
|<xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType>|ローカル カルチャに関係なく、2 つの文字列を比較します。 整数値を返します。|  
|<xref:System.String.CompareTo%2A?displayProperty=nameWithType>|現在の文字列オブジェクトを別の文字列と比較します。 整数値を返します。|  
|<xref:System.String.StartsWith%2A?displayProperty=nameWithType>|文字列が、渡された文字列で始まるかどうかを確認します。 ブール値を返します。|  
|<xref:System.String.EndsWith%2A?displayProperty=nameWithType>|文字列が、渡された文字列で終わるかどうかを確認します。 ブール値を返します。|  
|<xref:System.String.Equals%2A?displayProperty=nameWithType>|2 つの文字列が等しいかどうかを確認します。 ブール値を返します。|  
|<xref:System.String.IndexOf%2A?displayProperty=nameWithType>|検索対象文字列の先頭から開始して、特定の文字または文字列が見つかったインデックス位置を返します。 整数値を返します。|  
|<xref:System.String.LastIndexOf%2A?displayProperty=nameWithType>|検索対象文字列の末尾から開始して、特定の文字または文字列が見つかったインデックス位置を返します。 整数値を返します。|  
  
## <a name="compare"></a>比較  
 静的な <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドは、2 つの文字列を詳細に比較する手段を提供します。 このメソッドはカルチャに対応しています。 この機能は、2 つの文字列、または 2 つの文字列の部分文字列を比較するために使用できます。 また、大文字と小文字の区別やカルチャの違いを考慮または無視するためのオーバーロードも用意されています。 このメソッドによって返される可能性のある 3 つの整数値を次の表に示します。  
  
|戻り値|条件|  
|------------------|---------------|  
|負の整数|最初の文字列は、並べ替え順序が 2 番目の文字列の前に置かれます。<br /><br /> または<br /><br /> 最初の文字列は `null`です。|  
|0|最初の文字列と 2 番目の文字列は等価です。<br /><br /> または<br /><br /> 両方の文字列が `null`です。|  
|正の整数<br /><br /> または<br /><br /> 1|最初の文字列は、並べ替え順序が 2 番目の文字列の後に続きます。<br /><br /> または<br /><br /> 第 2 文字列は `null`です。|  
  
> [!IMPORTANT]
> <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドは、主に文字列の並べ替えに使用するものです。 等価性をテストする (つまり、ある文字列が別の文字列より大きいか小さいかを問題にせずに戻り値 0 を明示的に検索する) 目的では、 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを使用しないでください。 2 つの文字列が等価かどうかを判断するには、 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> メソッドを使用してください。  
  
 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを使用して、2 つの文字列の相対値を確認する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#6)]
 [!code-csharp[Conceptual.String.BasicOps#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#6)]
 [!code-vb[Conceptual.String.BasicOps#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#6)]  
  
 この例は、コンソールに `-1` と出力します。  
  
 既定では、上の例はカルチャによって異なります。 カルチャに依存しない文字列操作を実行するには、 <xref:System.String.Compare%2A?displayProperty=nameWithType> culture *パラメーターを指定することによって使用するカルチャを指定することを可能にする* メソッドのオーバーロードを使用します。 <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドを使用してカルチャに依存しない比較を実行する例については、「 [カルチャを認識しない文字列比較の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-comparisons.md)」を参照してください。  
  
## <a name="compareordinal"></a>CompareOrdinal  
 <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> メソッドは、ローカル カルチャを考慮せずに 2 つの文字列オブジェクトを比較します。 このメソッドの戻り値は、上の表で示した **Compare** メソッドによって返される値と同じです。  
  
> [!IMPORTANT]
> <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> メソッドは、主に文字列の並べ替えに使用するものです。 等価性をテストする (つまり、ある文字列が別の文字列より大きいか小さいかを問題にせずに戻り値 0 を明示的に検索する) 目的では、 <xref:System.String.CompareOrdinal%2A?displayProperty=nameWithType> メソッドを使用しないでください。 2 つの文字列が等価かどうかを判断するには、 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> メソッドを使用してください。  
  
 **CompareOrdinal** メソッドを使用して、2 つの文字列の値を比較する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#7)]
 [!code-csharp[Conceptual.String.BasicOps#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#7)]
 [!code-vb[Conceptual.String.BasicOps#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#7)]  
  
 この例は、コンソールに `-32` と出力します。  
  
## <a name="compareto"></a>CompareTo  
 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドは、現在の文字列オブジェクトがカプセル化している文字列を別の文字列またはオブジェクトと比較します。 このメソッドの戻り値は、上の表で示した <xref:System.String.Compare%2A?displayProperty=nameWithType> メソッドによって返される値と同じです。  
  
> [!IMPORTANT]
> <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドは、主に文字列の並べ替えに使用するものです。 等価性をテストする (つまり、ある文字列が別の文字列より大きいか小さいかを問題にせずに戻り値 0 を明示的に検索する) 目的では、 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドを使用しないでください。 2 つの文字列が等価かどうかを判断するには、 <xref:System.String.Equals%28System.String%2CSystem.String%2CSystem.StringComparison%29?displayProperty=nameWithType> メソッドを使用してください。  
  
 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドを使用して、 `string1` オブジェクトを `string2` オブジェクトと比較する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#8)]
 [!code-csharp[Conceptual.String.BasicOps#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#8)]
 [!code-vb[Conceptual.String.BasicOps#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#8)]  
  
 この例は、コンソールに `-1` と出力します。  
  
 <xref:System.String.CompareTo%2A?displayProperty=nameWithType> メソッドのすべてのオーバーロードは、既定で、カルチャに依存して大文字小文字を区別する比較を実行します。 このメソッドのオーバーロードで、カルチャに依存しない比較を実行できるものはありません。 コードを理解しやすくするために、 **String.Compare** メソッドを使用することをお勧めします。その際、カルチャに依存する操作には <xref:System.Globalization.CultureInfo.CurrentCulture%2A?displayProperty=nameWithType> を指定し、カルチャに依存しない操作には <xref:System.Globalization.CultureInfo.InvariantCulture%2A?displayProperty=nameWithType> を指定します。 **String.Compare** メソッドを使用してカルチャに依存する比較とカルチャに依存しない比較の両方を実行する例については、「 [カルチャを認識しない文字列比較の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-comparisons.md)」を参照してください。  
  
## <a name="equals"></a>等しい  
 **String.Equals** メソッドを使用すると、2 つの文字列が等しいかどうかを簡単に確認できます。 このメソッドは大文字と小文字を区別し、 **True** または **False** の Boolean 値を返します。 このメソッドは、次の例に示すように、既存のクラスで使用できます。 **Equals** メソッドを使用して、文字列オブジェクトに "Hello World" という語句が含まれているかどうかを確認する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#9)]
 [!code-csharp[Conceptual.String.BasicOps#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#9)]
 [!code-vb[Conceptual.String.BasicOps#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#9)]  
  
 この例は、コンソールに `True` と出力します。  
  
 このメソッドは、静的メソッドとしても使用できます。 静的メソッドを使用して、2 つの文字列オブジェクトを比較する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#10](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#10)]
 [!code-csharp[Conceptual.String.BasicOps#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#10)]
 [!code-vb[Conceptual.String.BasicOps#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#10)]  
  
 この例は、コンソールに `True` と出力します。  
  
## <a name="startswith-and-endswith"></a>StartsWith と EndsWith  
 **String.StartsWith** メソッドを使用すると、文字列オブジェクトが、別の文字列を構成している文字と同じ文字で始まっているかどうかを確認できます。 このメソッドは大文字と小文字を区別し、現在の文字列オブジェクトが、渡された文字列で始まる場合は **true** 、それ以外の場合は **false** を返します。 このメソッドを使用して、文字列オブジェクトが "Hello" で始まるかどうかを確認する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#11](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#11)]
 [!code-csharp[Conceptual.String.BasicOps#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#11)]
 [!code-vb[Conceptual.String.BasicOps#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#11)]  
  
 この例は、コンソールに `True` と出力します。  
  
 **String.EndsWith** メソッドは、渡された文字列を現在の文字列オブジェクトの末尾にある文字列と比較します。 このメソッドも Boolean 値を返します。 **EndsWith** メソッドを使用して、文字列の末尾を確認する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#12](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#12)]
 [!code-csharp[Conceptual.String.BasicOps#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#12)]
 [!code-vb[Conceptual.String.BasicOps#12](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#12)]  
  
 この例は、コンソールに `False` と出力します。  
  
## <a name="indexof-and-lastindexof"></a>IndexOf と LastIndexOf  
 **String.IndexOf** メソッドを使用すると、特定の文字が文字列内で最初に出現する位置を確認できます。 このメソッドは大文字と小文字を区別し、文字列の先頭からカウントを開始し、0 から始まるインデックスを使用して、渡された文字が出現する位置を返します。 指定した文字が見つからない場合は、値 –1 を返します。  
  
 **IndexOf** メソッドを使用して、'`l`' という文字が文字列内で最初に出現する位置を検索する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#13](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#13)]
 [!code-csharp[Conceptual.String.BasicOps#13](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#13)]
 [!code-vb[Conceptual.String.BasicOps#13](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#13)]  
  
 この例は、コンソールに `2` と出力します。  
  
 **String.LastIndexOf** メソッドは **String.IndexOf** メソッドに似ていますが、指定した文字列が文字列内で最後に出現する位置を返すという点が異なります。 このメソッドは大文字と小文字を区別し、0 から始まるインデックスを使用します。  
  
 **LastIndexOf** メソッドを使用して、'`l`' という文字が文字列内で最後に出現する位置を検索する例を次に示します。  
  
 [!code-cpp[Conceptual.String.BasicOps#14](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.string.basicops/cpp/compare.cpp#14)]
 [!code-csharp[Conceptual.String.BasicOps#14](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.string.basicops/cs/compare.cs#14)]
 [!code-vb[Conceptual.String.BasicOps#14](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.string.basicops/vb/compare.vb#14)]  
  
 この例は、コンソールに `9` と出力します。  
  
 いずれのメソッドも、**String.Remove** メソッドと組み合わせて使用すると便利です。 **IndexOf** メソッドまたは **LastIndexOf** メソッドのいずれかを使用して文字の位置を取得し、その位置を **Remove** メソッドに渡すことによって、その文字またはその文字で始まる単語を削除できます。  
  
## <a name="see-also"></a>参照

- [基本的な文字列操作](../../../docs/standard/base-types/basic-string-operations.md)
- [カルチャを認識しない文字列操作の実行](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations.md)
- [並べ替え重みテーブル (Windows 上の .NET 用)](https://www.microsoft.com/download/details.aspx?id=10921)
- [デフォルト Unicode 照合基本テーブル (Linux と macOS 上の .NET Core 用)](https://www.unicode.org/Public/UCA/latest/allkeys.txt)
