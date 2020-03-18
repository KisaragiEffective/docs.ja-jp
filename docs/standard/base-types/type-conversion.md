---
title: .NET Framework における型変換
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- widening conversions
- explicit conversions
- narrowing conversions
- type conversion, about type conversion
- type conversion
- converting types
- narrowing coercion
- Explicit operator
- IConvertible interface
- base types, converting
- op_Implicit method
- widening coercion
- op_Explicit method
- Convert class
- implicit conversions
- Implicit operator
- data types [.NET Framework], converting
ms.assetid: ba36154f-064c-47d3-9f05-72f93a7ca96d
ms.openlocfilehash: 0e88303f2bac2dae90a97f9d2de92af1d2a0f80d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73976488"
---
# <a name="type-conversion-in-the-net-framework"></a>.NET Framework における型変換
すべての値には関連付けられた型があり、その値に割り振られる容量、可能な値の範囲、使用できるメンバーなどの属性を定義しています。 多くの値は複数の型として表現できます。 たとえば、値 4 は整数または浮動小数点数として表現できます。 型変換を実行すると、変換元の型の値と等価な値が新しい型で作成されますが、それが元のオブジェクトと同一である (値が正確に一致する) とは限りません。  
  
 次の変換は、.NET Framework によって自動的にサポートされます。  
  
- 派生クラスから基底クラスへの変換。 これは、たとえば、任意のクラスまたは構造体のインスタンスを <xref:System.Object> インスタンスに変換できることを意味します。  この変換では、キャスト演算子または変換演算子は必要ありません。  
  
- 基本クラスから元の派生クラスへの変換。 C# の場合、この変換ではキャスト演算子が必要です。 Visual Basic では、`Option Strict` がオンの場合、`CType` 演算子が必要です。  
  
- インターフェイスを実装する型から、そのインターフェイスを表すインターフェイス オブジェクトへの変換。 この変換では、キャスト演算子または変換演算子は必要ありません。  
  
- インターフェイス オブジェクトから、そのインターフェイスを実装する元の型への変換。  C# の場合、この変換ではキャスト演算子が必要です。 Visual Basic では、`Option Strict` がオンの場合、`CType` 演算子が必要です。  
  
 これらの自動変換に加えて、.NET Framework ではカスタムの型変換をサポートするいくつかの機能を提供しています。 次に例を示します。  
  
- `Implicit` 演算子。型の間で使用できる拡大変換を定義します。 詳細については、「[Implicit 演算子を使用する暗黙的な変換](#implicit-conversion-with-the-implicit-operator)」を参照してください。  
  
- `Explicit` 演算子。型の間で使用できる縮小変換を定義します。 詳細については、「[Explicit 演算子を使用する明示的な変換](#explicit-conversion-with-the-explicit-operator)」を参照してください。  
  
- <xref:System.IConvertible> インターフェイス。 .NET Framework の各基本データ型への変換を定義します。 詳細については、「[IConvertible インターフェイス](#the-iconvertible-interface)」を参照してください。  
  
- <xref:System.Convert> クラス。<xref:System.IConvertible> インターフェイスにメソッドを実装する一連のメソッドを提供します。 詳細については、「[Convert クラス](#the-convert-class)」を参照してください。  
  
- <xref:System.ComponentModel.TypeConverter> クラス。指定された型から他の型への変換をサポートするように拡張できる基本クラスです。 詳細については、「[TypeConverter クラス](#the-typeconverter-class)」を参照してください。  

## <a name="implicit-conversion-with-the-implicit-operator"></a>Implicit 演算子を使用する暗黙的な変換  
 拡大変換では、変換後の型より範囲の制限が厳しい既存の型またはメンバー リストが制限されている既存の型の値から新しい値が作成されます。 拡大変換でデータ損失が発生することはありません (ただし、精度が失われることはあります)。 データが失われないため、コンパイラは明示的な変換メソッドまたはキャスト演算子を使用するように要求する必要がなく、暗黙的または透過的に変換を処理できます。  
  
> [!NOTE]
> 暗黙的な変換を実行するコードで変換メソッドを呼び出したり、キャスト演算子を使用したりすることはできますが、暗黙的な変換がサポートされるコンパイラでは必要ありません。  
  
 たとえば、<xref:System.Decimal> 型については、<xref:System.Byte>、<xref:System.Char>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.UInt16>、<xref:System.UInt32>、および <xref:System.UInt64> の各型の値からの暗黙的な変換がサポートされます。 <xref:System.Decimal> 変数への値の代入時に、これらの暗黙的な変換の一部が実行される例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#1)]
 [!code-vb[Conceptual.Conversion#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#1)]  
  
 特定の言語のコンパイラがカスタム演算子をサポートする場合、独自のカスタム型でも暗黙的な変換を定義できます。 `ByteWithSign` という名前が付けられた、符号および絶対値による表現を使用する符号付きバイト データ型の実装の一部を次の例に示します。 この実装では、<xref:System.Byte> 値および <xref:System.SByte> 値から `ByteWithSign` 値への暗黙的な変換がサポートされます。  
  
 [!code-csharp[Conceptual.Conversion#2](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#2)]
 [!code-vb[Conceptual.Conversion#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#2)]  
  
 これにより、明示的な変換を実行したり、キャスト演算子を使用したりすることなく、クライアント コードで `ByteWithSign` 変数を宣言し、この変数に <xref:System.Byte> 値および <xref:System.SByte> 値を代入できます。この例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#3](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/implicit1.cs#3)]
 [!code-vb[Conceptual.Conversion#3](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/implicit1.vb#3)]  

## <a name="explicit-conversion-with-the-explicit-operator"></a>Explicit 演算子を使用する明示的な変換  
 縮小変換では、変換後の型より範囲が広い既存の型またはメンバー リストが多い既存の型の値から新しい値が作成されます。 縮小変換ではデータ損失が発生する可能性があるため、コンパイラでは多くの場合、変換メソッドの呼び出しまたはキャスト演算子を通じて変換を明示的に行うことが要求されます。 つまり、変換は、開発者のコード内で明示的に処理する必要があります。  
  
> [!NOTE]
> 縮小変換で変換メソッドまたはキャスト演算子を必須とする主な目的は、コード内で処理できるように、データ損失の可能性または <xref:System.OverflowException>  について開発者に認識してもらうことにあります。 ただし、一部のコンパイラでは、この要件が緩和されている場合もあります。 たとえば、Visual Basic で `Option Strict` がオフの場合 (既定の設定)、Visual Basic コンパイラは縮小変換を暗黙的に実行します。  
  
 たとえば、<xref:System.UInt32>、<xref:System.Int64>、および <xref:System.UInt64> の各データ型の範囲は、次の表に示すように、<xref:System.Int32> データ型より広くなっています。  
  
|種類|Int32 の範囲との比較|  
|----------|------------------------------------|  
|<xref:System.Int64>|<xref:System.Int64.MaxValue?displayProperty=nameWithType> が <xref:System.Int32.MaxValue?displayProperty=nameWithType> より大きく、<xref:System.Int64.MinValue?displayProperty=nameWithType> が <xref:System.Int32.MinValue?displayProperty=nameWithType> より小さく (負の値の範囲が広く) なっています。|  
|<xref:System.UInt32>|<xref:System.UInt32.MaxValue?displayProperty=nameWithType> が <xref:System.Int32.MaxValue?displayProperty=nameWithType> より大きくなっています。|  
|<xref:System.UInt64>|<xref:System.UInt64.MaxValue?displayProperty=nameWithType> が <xref:System.Int32.MaxValue?displayProperty=nameWithType> より大きくなっています。|  
  
 このような縮小変換を処理するため、.NET Framework では、型で `Explicit` 演算子を定義できます。 これにより、個別の言語コンパイラで独自の構文を使用してこの演算子を実装するか、または <xref:System.Convert> クラスのメンバーを呼び出して変換を実行できます (<xref:System.Convert> クラスの詳細については、このトピックの下記の「[Convert クラス](#the-convert-class)」を参照してください)。これらの範囲外の可能性がある整数値を <xref:System.Int32> 値に明示的に変換する処理を行う言語機能の使用例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#4](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#4)]
 [!code-vb[Conceptual.Conversion#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#4)]  
  
 明示的な変換で生成される結果は言語によって異なることがあり、これらの結果は対応する <xref:System.Convert> メソッドによって返される値とは異なる可能性があります。 たとえば、<xref:System.Double> 型の値 12.63251 を <xref:System.Int32> に変換する場合、Visual Basic の `CInt` メソッドと .NET Framework の <xref:System.Convert.ToInt32%28System.Double%29?displayProperty=nameWithType> メソッドは <xref:System.Double> を丸めて値 13 を返しますが、C# の `(int)` 演算子は <xref:System.Double> を切り捨てて値 12 を返します。 同様に、C# の `(int)` 演算子はブール型から整数型への変換をサポートしませんが、Visual Basic の `CInt` メソッドは値 `true` を -1 に変換します。 一方、<xref:System.Convert.ToInt32%28System.Boolean%29?displayProperty=nameWithType> メソッドは値 `true` を 1 に変換します。  
  
 ほとんどのコンパイラでは、チェックを行う形でも行わない形でも明示的な変換を実行できます。 チェックを行う変換を実行すると、変換する型の値が変換先の型の範囲外になる場合に <xref:System.OverflowException> がスローされます。 チェックを行わない変換を同じ条件で実行した場合、例外がスローされることがなくても、実際の動作は不定となり、結果が正しくない値になる可能性があります。  
  
> [!NOTE]
> C# では、キャスト演算子と共に `checked` キーワードを使用するか、または `/checked+` コンパイラ オプションを指定することで、チェックを行う変換を実行できます。 反対に、チェックを行わない変換を実行する場合は、キャスト演算子と共に `unchecked` キーワードを使用するか、または `/checked-` コンパイラ オプションを指定します。 既定では、明示的な変換はチェックされません。 Visual Basic では、プロジェクトの **[コンパイラの詳細設定]** ダイアログ ボックスで **[整数オーバーフローのチェックを解除]** チェック ボックスをオフにするか、または `/removeintchecks-` コンパイラ オプションを指定することで、チェックを行う変換を実行できます。 反対に、チェックを行わない変換を実行する場合は、プロジェクトの **[コンパイラの詳細設定]** ダイアログ ボックスで **[整数オーバーフローのチェックを解除]** チェック ボックスをオンにするか、または `/removeintchecks+` コンパイラ オプションを指定します。 既定では、明示的な変換のチェックが行われます。  
  
 次の C# の例では、`checked` キーワードと `unchecked` キーワードを使用して、<xref:System.Byte> の範囲外の値を <xref:System.Byte> に変換したときの動作の違いを示しています。 チェックを行う変換では例外がスローされますが、チェックを行わない変換では <xref:System.Byte.MaxValue?displayProperty=nameWithType> 変数に <xref:System.Byte> が代入されます。  
  
 [!code-csharp[Conceptual.Conversion#12](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#12)]  
  
 特定の言語のコンパイラで演算子のカスタム オーバーロードがサポートされている場合は、独自に作成したカスタム型の明示的な変換も定義できます。 `ByteWithSign` という名前が付けられた、符号および絶対値による表現を使用する符号付きバイト データ型の実装の一部を次の例に示します。 この実装では、<xref:System.Int32> 値および <xref:System.UInt32> 値から `ByteWithSign` 値への明示的な変換がサポートされます。  
  
 [!code-csharp[Conceptual.Conversion#5](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#5)]
 [!code-vb[Conceptual.Conversion#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#5)]  
  
 これにより、代入処理にキャスト演算子または変換メソッドが含まれている場合は、クライアント コードで `ByteWithSign` 変数を宣言し、この変数に <xref:System.Int32> 値および <xref:System.UInt32> 値を代入できます。この例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/explicit1.cs#6)]
 [!code-vb[Conceptual.Conversion#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/explicit1.vb#6)]  

## <a name="the-iconvertible-interface"></a>IConvertible インターフェイス  
 任意の型から共通言語ランタイムの基本型への変換をサポートするため、.NET Framework には <xref:System.IConvertible> インターフェイスが用意されています。 実装する型には、次のメソッドが必要です。  
  
- 実装する型の <xref:System.TypeCode> を返すメソッド。  
  
- 実装する型を共通言語ランタイムの各基本型 (<xref:System.Boolean>、<xref:System.Byte>、<xref:System.DateTime>、<xref:System.Decimal>、<xref:System.Double> など) へ変換するメソッド。  
  
- 実装する型のインスタンスを他の特定の型へ変換する汎用的な変換メソッド。 サポートされていない変換では、<xref:System.InvalidCastException> をスローする必要があります。  
  
 共通言語ランタイムの各基本型 (<xref:System.Boolean>、<xref:System.Byte>、<xref:System.Char>、<xref:System.DateTime>、<xref:System.Decimal>、<xref:System.Double>、<xref:System.Int16>、<xref:System.Int32>、<xref:System.Int64>、<xref:System.SByte>、<xref:System.Single>、<xref:System.String>、<xref:System.UInt16>、<xref:System.UInt32>、および <xref:System.UInt64>) と、<xref:System.DBNull> 型および <xref:System.Enum> 型には、<xref:System.IConvertible> インターフェイスが実装されます。 ただし、これらは明示的なインターフェイスの実装です。次の例に示すように、変換メソッドは、<xref:System.IConvertible> インターフェイス変数を介してのみ呼び出すことができます。 この例では、<xref:System.Int32> 値が対応する <xref:System.Char> 値に変換されます。  
  
 [!code-csharp[Conceptual.Conversion#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible1.cs#7)]
 [!code-vb[Conceptual.Conversion#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible1.vb#7)]  
  
 実装する型ではなく、そのインターフェイス上で変換メソッドを呼び出すという要件があるため、明示的なインターフェイスの実装の負荷は相対的に大きくなります。 代わりに、<xref:System.Convert> クラスの適切なメンバーを呼び出して共通言語ランタイムの基本型間の変換を行うことをお勧めします。 詳細については、次のセクションの「[Convert クラス](#the-convert-class)」を参照してください。  
  
> [!NOTE]
> .NET Framework で提供される <xref:System.IConvertible> インターフェイスおよび <xref:System.Convert> クラスに加えて、各言語独自の変換方法が提供されている場合もあります。 たとえば C# ではキャスト演算子が使用され、Visual Basic ではコンパイラで実装された `CType`、`CInt`、`DirectCast` などの変換関数が使用されます。  
  
 一般に、<xref:System.IConvertible> インターフェイスは、.NET Framework の基本型間の変換をサポートするために設計されています。 ただし、このインターフェイスをカスタム型に実装して、その型から他のカスタム型への変換をサポートすることもできます。 詳細については、このトピックの「[ChangeType メソッドを使用するカスタム変換](#custom-conversions-with-the-changetype-method)」を参照してください。

## <a name="the-convert-class"></a>Convert クラス
 型変換を実行するために基本型の <xref:System.IConvertible> インターフェイス実装を呼び出すこともできますが、ある基本型から他の基本型への変換では、言語に依存しない方法である <xref:System.Convert?displayProperty=nameWithType> クラスのメソッドの呼び出しを推奨します。 また、<xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドは、特定のカスタム型から他のカスタム型への変換にも使用できます。  
  
### <a name="conversions-between-base-types"></a>基本型どうしの変換  
 <xref:System.Convert> クラスは、言語に依存しない方法で基本型どうしの変換を実行し、共通言語ランタイムに対応するすべての言語で使用できます。 このクラスには、拡大変換と縮小変換のための一連のメソッドがすべて用意されています。また、サポートされない変換 (<xref:System.InvalidCastException> 値から整数値への変換など) に対しては <xref:System.DateTime> がスローされます。 縮小変換は checked コンテキストで実行され、変換が失敗すると <xref:System.OverflowException> がスローされます。  
  
> [!IMPORTANT]
> <xref:System.Convert> クラスには各基本型どうしの変換を実行するメソッドが含まれているため、各基本型の明示的な <xref:System.IConvertible> インターフェイス実装を呼び出す必要はありません。  
  
 <xref:System.Convert?displayProperty=nameWithType> クラスを使用して .NET Framework 基本型どうしの拡大変換および縮小変換を実行する例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#8)]
 [!code-vb[Conceptual.Conversion#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#8)]  
  
 特に変換前または変換後の値が浮動小数点型の場合など、変換によって精度が低下することがありますが、<xref:System.OverflowException> はスローされません。 このように精度が低下する例を次に示します。 最初の例では、<xref:System.Decimal> 値を <xref:System.Double> に変換したときに精度が低く (有効桁数が少なく) なります。 2 番目の例では、変換を完了させるために、<xref:System.Double> 値の 42.72 が 43 に丸められています。  
  
 [!code-csharp[Conceptual.Conversion#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/convert1.cs#9)]
 [!code-vb[Conceptual.Conversion#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/convert1.vb#9)]  
  
 <xref:System.Convert> クラスでサポートされる拡大変換と縮小変換の一覧表については、「[型変換の表](../../../docs/standard/base-types/conversion-tables.md)」を参照してください。  

### <a name="custom-conversions-with-the-changetype-method"></a>ChangeType メソッドを使用するカスタム変換  
 <xref:System.Convert> クラスは、各基本型どうしの変換をサポートするだけでなく、カスタム型を 1 つ以上の定義済みの型に変換できます。 この変換は <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドによって実行されます。さらに、このメソッドが、<xref:System.IConvertible.ToType%2A?displayProperty=nameWithType> パラメーターの `value` メソッドに対する呼び出しをラップします。 したがって、`value` パラメーターで指定されるオブジェクトに、<xref:System.IConvertible> インターフェイスが実装されている必要があります。  
  
> [!NOTE]
> <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%29?displayProperty=nameWithType> および <xref:System.Convert.ChangeType%28System.Object%2CSystem.Type%2CSystem.IFormatProvider%29?displayProperty=nameWithType> メソッドは <xref:System.Type> オブジェクトを使用して `value` の変換後の型を指定するため、コンパイル時には型がわからないオブジェクトへの動的な変換に使用することができます。 ただし、<xref:System.IConvertible> の `value` の実装も、この変換をサポートする必要があることに注意してください。  
  
 <xref:System.IConvertible> オブジェクトから `TemperatureCelsius` オブジェクトへの変換およびその逆の変換を実行できる `TemperatureFahrenheit` インターフェイスとして考えられる実装を次の例に示します。 この例では、`Temperature` インターフェイスを実装し、<xref:System.IConvertible> メソッドをオーバーライドする基本クラス <xref:System.Object.ToString%2A?displayProperty=nameWithType> を定義します。 派生クラスの `TemperatureCelsius` および `TemperatureFahrenheit` クラスが、基本クラスの `ToType` メソッドおよび `ToString` メソッドをそれぞれオーバーライドします。  
  
 [!code-csharp[Conceptual.Conversion#10](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#10)]
 [!code-vb[Conceptual.Conversion#10](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#10)]  
  
 これらの <xref:System.IConvertible> 実装を呼び出して `TemperatureCelsius` オブジェクトから `TemperatureFahrenheit` オブジェクトへの変換またはその逆の変換を実行する例を次に示します。  
  
 [!code-csharp[Conceptual.Conversion#11](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.conversion/cs/iconvertible2.cs#11)]
 [!code-vb[Conceptual.Conversion#11](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.conversion/vb/iconvertible2.vb#11)]  

## <a name="the-typeconverter-class"></a>TypeConverter クラス  
 .NET Framework では、<xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType> クラスを拡張し、<xref:System.ComponentModel.TypeConverterAttribute?displayProperty=nameWithType> 属性を使用して型コンバーターを型に関連付けることによって、カスタム型の型コンバーターも定義できます。 次の表に、この方法と、カスタム型に対して <xref:System.IConvertible> インターフェイスを実装する方法の相違点をまとめます。  
  
> [!NOTE]
> カスタム型に対してデザイン時サポートが有効になるのは、そのカスタム型の型コンバーターが定義されている場合だけです。  
  
|TypeConverter を使用した変換|IConvertible を使用した変換|  
|------------------------------------|-----------------------------------|  
|<xref:System.ComponentModel.TypeConverter> から独立したクラスを派生させることにより、カスタム型に実装されます。 この派生クラスは、<xref:System.ComponentModel.TypeConverterAttribute> 属性を適用することにより、カスタム型と関連付けられます。|変換を実行するためにカスタム型によって実装されます。 その型を使用するときに、その型に対して <xref:System.IConvertible> 変換メソッドを呼び出します。|  
|デザイン時と実行時に使用できます。|実行時だけ使用できます。|  
|リフレクションを使用するため、<xref:System.IConvertible> での変換よりも処理に時間がかかります。|リフレクションを使用しません。|  
|カスタム型から他のデータ型への変換と、他のデータ型からカスタム型への変換という双方向の変換を実行できます。 たとえば、<xref:System.ComponentModel.TypeConverter> に対して定義される `MyType` を使用することによって、`MyType` から <xref:System.String> への変換および <xref:System.String> から `MyType` への変換を実行できます。|カスタム型から他のデータ型への変換は実行できますが、他のデータ型からカスタム型への変換は実行できません。|  
  
 型コンバーターを使用した変換の詳細については、<xref:System.ComponentModel.TypeConverter?displayProperty=nameWithType> を参照してください。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Convert?displayProperty=nameWithType>
- <xref:System.IConvertible>
- [型変換の表](../../../docs/standard/base-types/conversion-tables.md)
