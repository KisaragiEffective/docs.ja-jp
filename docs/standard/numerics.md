---
title: .NET における数値
ms.date: 10/18/2018
ms.technology: dotnet-standard
helpviewer_keywords:
- SIMD
- System.Numerics.Vectors
- vectors
- scientific computing
- Complex
- numerics
- BigInteger
ms.assetid: dfebc18e-acde-4510-9fa7-9a0f4aa3bd11
ms.openlocfilehash: e5815058898cac165e7a47d761ee86bb9c4cb940
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73091589"
---
# <a name="numerics-in-net"></a>.NET における数値

.NET には、さまざまな数値整数と浮動小数点型のプリミティブが用意されています。さらに、理論上の上限や下限のない整数型の <xref:System.Numerics.BigInteger?displayProperty=nameWithType>、複素数を表す <xref:System.Numerics.Complex?displayProperty=nameWithType>、<xref:System.Numerics> 名前空間の SIMD が有効な型のセットも用意されています。
  
## <a name="integer-types"></a>整数型

.NET では、次の表にあるように、符号付きと符号なしの両方の 8 ビット、16 ビット、32 ビット、64 ビット整数型がサポートされています。
  
|[種類]|符号付き/符号なし|サイズ (バイト単位)|最小値|最大値|  
|----------|----------------------|--------------------|-------------------|-------------------|  
|<xref:System.Byte?displayProperty=nameWithType>|符号なし|1|0|255|  
|<xref:System.Int16?displayProperty=nameWithType>|符号付き|2|-32,768|32,767|  
|<xref:System.Int32?displayProperty=nameWithType>|符号付き|4|-2,147,483,648|2,147,483,647|  
|<xref:System.Int64?displayProperty=nameWithType>|符号付き|8|-9,223,372,036,854,775,808|9,223,372,036,854,775,807|  
|<xref:System.SByte?displayProperty=nameWithType>|符号付き|1|-128|127|  
|<xref:System.UInt16?displayProperty=nameWithType>|符号なし|2|0|65,535|  
|<xref:System.UInt32?displayProperty=nameWithType>|符号なし|4|0|4,294,967,295|  
|<xref:System.UInt64?displayProperty=nameWithType>|符号なし|8|0|18,446,744,073,709,551,615|  
  
いずれの整数型でも、標準的な算術演算子セットがサポートされています。 <xref:System.Math?displayProperty=nameWithType> クラスでは、広範囲の数学関数セットのためのメソッドが提供されます。

<xref:System.BitConverter?displayProperty=nameWithType> クラスを使用して、整数値の個々 のビットを操作することもできます。  

> [!NOTE]  
> 符号なし整数型は CLS に準拠していません。 詳しくは、「[言語への非依存性、および言語非依存コンポーネント](language-independence-and-language-independent-components.md)」を参照してください。

## <a name="biginteger"></a>BigInteger

<xref:System.Numerics.BigInteger?displayProperty=nameWithType> 構造体は、理論的には値に上限や下限がない、サイズの大きい任意の整数を表す不変の型です。 <xref:System.Numerics.BigInteger> 型のメソッドは、他の整数型のメソッドとかなり類似しています。
  
## <a name="floating-point-types"></a>浮動小数点型

.NET には、次の表に示す 3 種類のプリミティブ浮動小数点型が含まれています。
  
|[種類]|サイズ (バイト単位)|おおよその範囲|Precision|  
|----------|--------|---------------------|--------------------|  
|<xref:System.Single?displayProperty=nameWithType>|4|±1.5 x 10<sup>−45</sup> から ±3.4 x 10<sup>38</sup>|~6 ～9 桁|  
|<xref:System.Double?displayProperty=nameWithType>|8|±5.0 × 10<sup>−324</sup> - ±1.7 × 10<sup>308</sup>|~15-17 桁|  
|<xref:System.Decimal?displayProperty=nameWithType>|16|±1.0 x 10<sup>-28</sup> から ±7.9228 x 10<sup>28</sup>|28 から 29 桁の数字|  
  
<xref:System.Single> 型と <xref:System.Double> 型の両方で、非数と無限を表す特殊な値がサポートされています。 たとえば、<xref:System.Double> 型では、<xref:System.Double.NaN?displayProperty=nameWithType>、<xref:System.Double.NegativeInfinity?displayProperty=nameWithType>、<xref:System.Double.PositiveInfinity?displayProperty=nameWithType> 値が提供されます。 これらの特殊な値のテストには <xref:System.Double.IsNaN%2A?displayProperty=nameWithType>、<xref:System.Double.IsInfinity%2A?displayProperty=nameWithType>、<xref:System.Double.IsPositiveInfinity%2A?displayProperty=nameWithType>、<xref:System.Double.IsNegativeInfinity%2A?displayProperty=nameWithType> メソッドを使用します。

各浮動小数点型で標準的な算術演算子セットがサポートされています。 <xref:System.Math?displayProperty=nameWithType> クラスでは、広範囲の数学関数セットのためのメソッドが提供されます。 .NET Core 2.0 以降には、<xref:System.MathF?displayProperty=nameWithType> 型の引数を受け取るメソッドを提供する <xref:System.Single> クラスが含まれています。

<xref:System.Double> クラスを使用して、<xref:System.Single> および <xref:System.BitConverter?displayProperty=nameWithType> の値の個々のビットを操作することもできます。 <xref:System.Decimal?displayProperty=nameWithType> 構造体には、10 進値の個々のビットを操作するための独自のメソッド (<xref:System.Decimal.GetBits%2A?displayProperty=nameWithType> と <xref:System.Decimal.%23ctor%28System.Int32%5B%5D%29?displayProperty=nameWithType>)、および追加の数学演算を実行するための独自のメソッド セットがあります。
  
<xref:System.Double> 型と <xref:System.Single> 型は、本来正確ではない値 (2 つの星の間の距離など) での使用と、高い精度や小さな丸め誤差の検出が必要ではないアプリケーションでの使用が想定されています。 より高い精度が必要とされ、丸め誤差を最小限に抑えるべきケースでは、<xref:System.Decimal?displayProperty=nameWithType> 型を使用します。

> [!NOTE]
> <xref:System.Decimal> 型の場合、丸めの必要がなくなります。 丸めに起因するエラーが最小限に抑えられます。
  
## <a name="complex"></a>複合

<xref:System.Numerics.Complex?displayProperty=nameWithType> 構造体は、複素数、つまり、実数部と虚数部からなる数を表します。 算術演算子、比較演算子、等価演算子、明示的な変換の演算子、暗黙的な変換の演算子からなる標準セットがサポートされ、さらに数学、代数、三角関数のメソッドもサポートされます。  
  
## <a name="simd-enabled-types"></a>SIMD が有効な型

<xref:System.Numerics> 名前空間には、.NET SIMD 対応型のセットが含まれています。 SIMD (Single Instruction Multiple Data) 演算はハードウェア レベルで並列化できます。 それにより、数学、科学、グラフィックス アプリで一般的な、ベクター化された計算のスループットが向上します。
  
.NET SIMD 対応の型には次のような型があります。

- それぞれ 2、3、4 つの <xref:System.Numerics.Vector2> 値を表す <xref:System.Numerics.Vector3> 型、<xref:System.Numerics.Vector4> 型、<xref:System.Single> 型。

- 2 つのマトリックス型。3 x 2 行列を表す <xref:System.Numerics.Matrix3x2> と 4 x 4 行列を表す <xref:System.Numerics.Matrix4x4>。

- 3 次元空間の平面を表す <xref:System.Numerics.Plane> 型。

- 3 次元物理回転をエンコードするために使用されるベクトルを表す <xref:System.Numerics.Quaternion> 型。

- 指定の数値型のベクトルを表し、SIMD サポートが活かされる広範囲の演算子セットを提供する <xref:System.Numerics.Vector%601> 型。 <xref:System.Numerics.Vector%601> インスタンスの数は固定ですが、その値 <xref:System.Numerics.Vector%601.Count%2A?displayProperty=nameWithType> はコードが実行されるコンピューターの CPU に依存します。
  > [!NOTE]
  > <xref:System.Numerics.Vector%601> 型は .NET Framework に含まれません。 この型にアクセスするには、[System.Numerics.Vectors](https://www.nuget.org/packages/System.Numerics.Vectors) NuGet パッケージをインストールする必要があります。
  
SIMD 対応の型は、それが SIMD 非対応のハードウェアや JIT コンパイラと共に使用できるように実装されます。 SIMD 命令を活用するために、64 ビット アプリは RyuJIT コンパイラを使用するランタイムで実行する必要があります。このコンパイラは .NET Core と 4.6 以降の .NET Framework に含まれています。 64 ビット プロセッサを対象にするとき、SIMD サポートが追加されます。

## <a name="see-also"></a>参照

- [アプリケーションの基本事項](application-essentials.md)
- [標準の数値書式指定文字列](base-types/standard-numeric-format-strings.md)
