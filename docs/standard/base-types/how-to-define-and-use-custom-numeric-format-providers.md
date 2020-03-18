---
title: '方法: カスタム数値書式プロバイダーを定義して使用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- numeric format strings [.NET Framework]
- formatting [.NET Framework], numbers
- number formatting [.NET Framework]
- custom numeric format strings
- numbers [.NET Framework], custom numeric format strings
- displaying date and time data
- format providers [.NET Framework]
- custom format strings
ms.assetid: a281bfbf-6596-45ed-a2d6-3782d535ada2
ms.openlocfilehash: 151bf40cf042517b7441b89688122373259dc7dc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73140066"
---
# <a name="how-to-define-and-use-custom-numeric-format-providers"></a>方法: カスタム数値書式プロバイダーを定義して使用する
.NET Framework では、数値の文字列形式を広範囲に制御できます。 数値の書式をカスタマイズするため、次の機能をサポートしています。  
  
- 標準の数値書式指定文字列: 数値をその文字列形式に変換するための定義済みの書式セットを提供します。 これらは、`format` パラメーターを持つ <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType> などの数値書式指定メソッドと共に使用できます。 詳細については、「[標準の数値書式指定文字列](../../../docs/standard/base-types/standard-numeric-format-strings.md)」を参照してください。  
  
- カスタム数値書式指定文字列: カスタム数値書式指定子を定義するために結合できる記号のセットを提供します。 これらは、`format` パラメーターを持つ <xref:System.Decimal.ToString%28System.String%29?displayProperty=nameWithType> などの任意の数値書式指定メソッドでも使用できます。 詳細については、「[カスタム数値書式指定文字列](../../../docs/standard/base-types/custom-numeric-format-strings.md)」を参照してください。  
  
- カスタムの <xref:System.Globalization.CultureInfo> オブジェクトまたは <xref:System.Globalization.NumberFormatInfo> オブジェクト: 記号を定義し、数値の文字列形式を表示するために使用されるパターンの書式を設定します。 これらは、`provider` パラメーターを持つ <xref:System.Int32.ToString%2A> などの数値書式指定メソッドと共に使用できます。 通常、`provider` パラメーターは、カルチャに固有の書式を指定するために使用されます。  
  
 一部の例には (アプリケーションで書式設定されたアカウント番号や ID 番号、郵便番号を表示する必要がある場合など)、これら 3 つの方法は適していません。 .NET Framework では、<xref:System.Globalization.CultureInfo> オブジェクトでも <xref:System.Globalization.NumberFormatInfo> オブジェクトでもない書式設定オブジェクトを定義して、数値を書式設定する方法を決定することもできます。 このトピックでは、このようなオブジェクトを実装するため、詳細な手順を説明し、電話番号の書式を設定する例について説明します。  
  
### <a name="to-define-a-custom-format-provider"></a>カスタム書式プロバイダーを定義するには  
  
1. <xref:System.IFormatProvider> および <xref:System.ICustomFormatter>インターフェイスを実装するクラスを定義します。  
  
2. <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> メソッドを実装します。 <xref:System.IFormatProvider.GetFormat%2A> は、書式指定メソッド (<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドなど) が実際にカスタム書式設定の実行を担当するオブジェクトを取得するために呼び出すコールバック メソッドです。 <xref:System.IFormatProvider.GetFormat%2A> の一般的な実装では、次が実行されます。  
  
    1. メソッド パラメーターとして渡される <xref:System.Type> オブジェクトが <xref:System.ICustomFormatter> インターフェイスを表すかどうかを決定します。  
  
    2. パラメーターが <xref:System.ICustomFormatter> インターフェイスを表す場合、<xref:System.IFormatProvider.GetFormat%2A> はカスタム書式設定の提供を担当する <xref:System.ICustomFormatter> インターフェイスを実装するオブジェクトを返します。 通常、カスタム書式指定オブジェクトは、それ自体を返します。  
  
    3. パラメーターが <xref:System.ICustomFormatter> インターフェイスを表していない場合、<xref:System.IFormatProvider.GetFormat%2A> は、`null` を返します。  
  
3. <xref:System.ICustomFormatter.Format%2A> メソッドを実装します。 このメソッドは、<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドによって呼び出され、数値の文字列形式を返します。 メソッドの実装には通常、次の作業を行います。  
  
    1. 必要に応じて、`provider` パラメーターを調べることで、メソッドが書式指定サービスを提供する正当なものであることを確認します。 <xref:System.IFormatProvider> と <xref:System.ICustomFormatter> の両方を実装する書式設定オブジェクトの場合、現在の書式設定オブジェクトと等しいことを確認するため、`provider` パラメーターをテストする必要があります。  
  
    2. 書式指定オブジェクトが、カスタム書式指定子をサポートするかどうかを決定します (たとえば、"N" 書式指定子は、米国の電話番号を NANP 形式で出力することを示し、"I" はITU-T 推奨 E.123 形式での出力を示すことができます)。書式指定子を使用している場合、メソッドが特定の書式指定子を処理する必要があります。 これが `format` パラメーターのメソッドに渡されます。 指定子がない場合、`format` パラメーターの値は <xref:System.String.Empty?displayProperty=nameWithType> です。  
  
    3. `arg` パラメーターとしてメソッドに渡される数値を取得します。 文字列形式に変換するために必要な操作を実行します。  
  
    4. `arg` パラメーターの文字列表現を返します。  
  
### <a name="to-use-a-custom-numeric-formatting-object"></a>カスタム数値書式設定オブジェクトを使用するには  
  
1. カスタム書式指定クラスの新しいインスタンスを作成します。  
  
2. それをカスタム書式指定オブジェクト、書式指定子 (使用されていない場合は <xref:System.String.Empty?displayProperty=nameWithType>)、および書式設定する数値に渡す、<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> 書式指定メソッドを呼び出します。  
  
## <a name="example"></a>例  
 次の例では、米国の電話番号を表す数値を NANP または E.123 形式に変換する、`TelephoneFormatter` という名前のカスタム数値書式プロバイダーを定義します。 このメソッドは、"N" (NANP 形式を出力) と "I" (国際 E.123 形式を出力) の 2 つの書式指定子を処理します。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#1](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#1)]
 [!code-vb[Formatting.HowTo.NumericValue#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#1)]  
  
 カスタム数値書式プロバイダーは、<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドのみで使用できます。 <xref:System.IFormatProvider> 型のパラメーターがある数値書式指定メソッド (`ToString` など) の他のオーバーロードはすべて、<xref:System.Globalization.NumberFormatInfo> 型を表す <xref:System.Type> オブジェクトの <xref:System.IFormatProvider.GetFormat%2A?displayProperty=nameWithType> 実装を渡します。 代わりに、これらはメソッドが <xref:System.Globalization.NumberFormatInfo> オブジェクトを返すことを期待します。 そうでない場合、カスタム数値書式プロバイダーは無視され、現在のカルチャの <xref:System.Globalization.NumberFormatInfo> オブジェクトがその代わりに使用されます。 この例では、メソッド パラメーターを検査し、<xref:System.ICustomFormatter> 以外の型を表す場合には `null` を返すことで、`TelephoneFormatter.GetFormat` メソッドはそれが書式指定メソッドに不適切に渡される可能性を処理します。  
  
 カスタム数値書式プロバイダーが一連の書式指定子をサポートしている場合、<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドの呼び出しで使用される書式指定項目で書式指定子が指定されていない場合に、既定の動作を提供することを確認します。 例では、"N" が既定の書式指定子です。 これにより、明示的な書式指定子を提供することによって、数値を書式設定された電話番号に変換できます。 このようなメソッドの呼び出しの例を次に示します。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#2](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#2)]
 [!code-vb[Formatting.HowTo.NumericValue#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#2)]  
  
 ただし、書式指定子が存在しない場合に、変換を行うようにすることもできます。 このようなメソッドの呼び出しの例を次に示します。  
  
 [!code-csharp[Formatting.HowTo.NumericValue#3](../../../samples/snippets/csharp/VS_Snippets_CLR/Formatting.HowTo.NumericValue/cs/Telephone1.cs#3)]
 [!code-vb[Formatting.HowTo.NumericValue#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/Formatting.HowTo.NumericValue/vb/Telephone1.vb#3)]  
  
 既定の書式指定子が定義されていない場合、<xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> メソッドの実装には、使用しているコードでサポートされていない書式設定を .NET が提供できるように、次のようなコードを含める必要があります。  
  
 [!code-csharp[System.ICustomFormatter.Format#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.ICustomFormatter.Format/cs/format.cs#1)]
 [!code-vb[System.ICustomFormatter.Format#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.ICustomFormatter.Format/vb/Format.vb#1)]  
  
 この例の場合、<xref:System.ICustomFormatter.Format%2A?displayProperty=nameWithType> を実装するメソッドは、<xref:System.String.Format%28System.IFormatProvider%2CSystem.String%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドのコールバック メソッドを果たす目的があります。 そのため、`formatProvider` パラメーターを調べ、現在の `TelephoneFormatter` オブジェクトへの参照が含まれるかどうかを判断します。 ただし、メソッドはコードから直接呼び出すこともできます。 その場合は、`formatProvider` パラメーターを使用して、カルチャに固有の書式情報を提供する <xref:System.Globalization.CultureInfo> オブジェクトまたは <xref:System.Globalization.NumberFormatInfo> オブジェクトを提供することができます。  
  
## <a name="see-also"></a>関連項目

- [書式設定操作の実行](../../../docs/standard/base-types/performing-formatting-operations.md)
