---
title: 数学関数
ms.date: 01/27/2020
helpviewer_keywords:
- math functions, Visual Basic
- arithmetic operations, math functions
- math routines
- Atn function
ms.assetid: 4d2d82e7-6924-42fe-a4a7-b4dd5bebbd0c
ms.openlocfilehash: ea30ae3b30484c1a13d6d540f121c03afb30ba26
ms.sourcegitcommit: 13e79efdbd589cad6b1de634f5d6b1262b12ab01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/28/2020
ms.locfileid: "76794592"
---
# <a name="math-functions-visual-basic"></a>数値演算関数 (Visual Basic)

<xref:System.Math?displayProperty=nameWithType> クラスのメソッドは、三角関数、対数、およびその他の一般的な数学関数を提供します。

## <a name="remarks"></a>コメント

<xref:System.Math?displayProperty=nameWithType> クラスのメソッドの一覧を次の表に示します。 これらは、Visual Basic プログラムで使用できます。
  
|.NET メソッド|説明|
|---------------------------|-----------------|
|<xref:System.Math.Abs%2A>|数値の絶対値を返します。|
|<xref:System.Math.Acos%2A>|コサインが指定数となる角度を返します。|
|<xref:System.Math.Asin%2A>|サインが指定数となる角度を返します。|
|<xref:System.Math.Atan%2A>|タンジェントが指定数となる角度を返します。|
|<xref:System.Math.Atan2%2A>|タンジェントが 2 つの指定された数の商である角度を返します。|
|<xref:System.Math.BigMul%2A>|2 32 ビットの数値の完全な積を返します。|
|<xref:System.Math.Ceiling%2A>|指定された `Decimal` または `Double`以上の最小の整数値を返します。|
|<xref:System.Math.Cos%2A>|指定された角度のコサインを返します。|
|<xref:System.Math.Cosh%2A>|指定された角度のハイパーボリック コサインを返します。|
|<xref:System.Math.DivRem%2A>|2 32 ビットまたは64ビット符号付き整数の商を返し、出力パラメーターの剰余も返します。|
|<xref:System.Math.Exp%2A>|E (自然対数の底) を指定された指数で累乗した値を返します。|
|<xref:System.Math.Floor%2A>|指定された `Decimal` または `Double` 数値以下の最大の整数を返します。|
|<xref:System.Math.IEEERemainder%2A>|指定された数を別の指定数だけ除算した結果の剰余を返します。|
|<xref:System.Math.Log%2A>|指定した数値または指定した底の指定した数値の対数を示す自然対数 (底 e) を返します。|
|<xref:System.Math.Log10%2A>|指定した数の底 10 の対数を返します。|
|<xref:System.Math.Max%2A>|2つの数値のうち、大きい方を返します。|
|<xref:System.Math.Min%2A>|2 つの数のうち、小さい方を返します。|
|<xref:System.Math.Pow%2A>|指定の数値を指定した値で累乗した値を返します。|
|<xref:System.Math.Round%2A>|最も近い整数値または指定した小数部の桁数に丸められた `Decimal` または `Double` の値を返します。|
|<xref:System.Math.Sign%2A>|数値の符号を示す `Integer` 値を返します。|
|<xref:System.Math.Sin%2A>|指定された角度のサインを返します。|
|<xref:System.Math.Sinh%2A>|指定された角度のハイパーボリック サインを返します。|
|<xref:System.Math.Sqrt%2A>|指定された数値の平方根を返します。|
|<xref:System.Math.Tan%2A>|指定された角度のタンジェントを返します。|
|<xref:System.Math.Tanh%2A>|指定された角度のハイパーボリック タンジェントを返します。|
|<xref:System.Math.Truncate%2A>|指定した `Decimal` または `Double` 数値の整数部を計算します。|

次の表に、.NET Framework に存在しないが .NET Standard または .NET Core に追加される <xref:System.Math?displayProperty=nameWithType> クラスのメソッドを示します。

|.NET メソッド|説明|で利用可能|
|---------------------------|-----------------|-----------|
|<xref:System.Math.Acosh%2A>|ハイパーボリック コサインが指定数となる角度を返します。|.NET Core 2.1 および .NET Standard 2.1 以降|
|<xref:System.Math.Asinh%2A>|ハイパーボリック サインが指定数となる角度を返します。|.NET Core 2.1 および .NET Standard 2.1 以降|
|<xref:System.Math.Atanh%2A>|ハイパーボリック タンジェントが指定数となる角度を返します。|.NET Core 2.1 および .NET Standard 2.1 以降|
|<xref:System.Math.BitDecrement%2A>|`x` 未満を比較する、次に小さい値を返します。|.NET Core 3.0 以降|
|<xref:System.Math.BitIncrement%2A>|`x` を超える比較値の次に大きい値を返します。|.NET Core 3.0 以降|
|<xref:System.Math.Cbrt%2A>|指定された数値の立方根を返します。|.NET Core 2.1 および .NET Standard 2.1 以降|
|<xref:System.Math.Clamp%2A>|`min` 以上 `max` 以下の範囲に固定される `value` を返します。|.NET Core 2.0 および .NET Standard 2.1 以降|
|<xref:System.Math.CopySign%2A>|`x` の絶対値と符号 `y` の値を返します。|.NET Core 3.0 以降|
|<xref:System.Math.FusedMultiplyAdd%2A>|1つの三項演算として丸められた (x \* y) + z を返します。|.NET Core 3.0 以降|
|<xref:System.Math.ILogB%2A>|指定した数の底 2 の整数の対数を返します。|.NET Core 3.0 以降|
|<xref:System.Math.Log2%2A>|指定した数の底 2 の対数を返します。|.NET Core 3.0 以降|
|<xref:System.Math.MaxMagnitude%2A>|2 つの倍精度浮動小数点数のうち、大きい絶対値を返します。|.NET Core 3.0 以降|
|<xref:System.Math.MinMagnitude%2A>|2 つの倍精度浮動小数点数のうち、小さい絶対値を返します。|.NET Core 3.0 以降|
|<xref:System.Math.ScaleB%2A>|X \* 2 ^ n が効率的に計算された値を返します。|.NET Core 3.0 以降|

これらの関数を修飾なしで使用するには、ソースファイルの先頭に次のコードを追加して、<xref:System.Math?displayProperty=nameWithType> 名前空間をプロジェクトにインポートします。

```vb
Imports System.Math
```

## <a name="example---abs"></a>例-Abs

この例では、<xref:System.Math> クラスの <xref:System.Math.Abs%2A> メソッドを使用して、数値の絶対値を計算します。

```vb
Dim x As Double = Math.Abs(50.3)
Dim y As Double = Math.Abs(-50.3)
Console.WriteLine(x)
Console.WriteLine(y)
' This example produces the following output:
' 50.3
' 50.3
```  

## <a name="example---atan"></a>例-Atan

この例では、<xref:System.Math> クラスの <xref:System.Math.Atan%2A> メソッドを使用して、pi の値を計算します。

```vb
Public Function GetPi() As Double
    ' Calculate the value of pi.
    Return 4.0 * Math.Atan(1.0)
End Function
```

> [!NOTE]
> <xref:System.Math?displayProperty=nameWithType> クラスには <xref:System.Math.PI?displayProperty=nameWithType> 定数フィールドが含まれています。 それを計算するのではなく、使用することができます。

## <a name="example---cos"></a>例-Cos

この例では、<xref:System.Math> クラスの <xref:System.Math.Cos%2A> メソッドを使用して、角度のコサインを返します。

```vb
Public Function Sec(angle As Double) As Double
    ' Calculate the secant of angle, in radians.
    Return 1.0 / Math.Cos(angle)
End Function
```

## <a name="example---exp"></a>例-Exp

この例では、<xref:System.Math> クラスの <xref:System.Math.Exp%2A> メソッドを使用して、e の累乗を返します。

```vb
Public Function Sinh(angle As Double) As Double
    ' Calculate hyperbolic sine of an angle, in radians.
    Return (Math.Exp(angle) - Math.Exp(-angle)) / 2.0
End Function
```

## <a name="example---log"></a>例-ログ

この例では、<xref:System.Math> クラスの <xref:System.Math.Log%2A> メソッドを使用して、数値の自然対数を返します。

```vb
Public Function Asinh(value As Double) As Double
    ' Calculate inverse hyperbolic sine, in radians.
    Return Math.Log(value + Math.Sqrt(value * value + 1.0))
End Function
```

## <a name="example---round"></a>例-Round

この例では、<xref:System.Math> クラスの <xref:System.Math.Round%2A> メソッドを使用して、数値を最も近い整数に丸めます。

```vb
Dim myVar2 As Double = Math.Round(2.8)
Console.WriteLine(myVar2)
' The code produces the following output:
' 3
```

## <a name="example---sign"></a>例-Sign

この例では、<xref:System.Math> クラスの <xref:System.Math.Sign%2A> メソッドを使用して、数値の符号を決定します。

```vb
Dim mySign1 As Integer = Math.Sign(12)
Dim mySign2 As Integer = Math.Sign(-2.4)
Dim mySign3 As Integer = Math.Sign(0)
Console.WriteLine(mySign1)
Console.WriteLine(mySign2)
Console.WriteLine(mySign3)
' The code produces the following output:
' 1
' -1
' 0
```

## <a name="example---sin"></a>例-Sin

この例では、<xref:System.Math> クラスの <xref:System.Math.Sin%2A> メソッドを使用して、角度のサインを返します。

```vb
Public Function Csc(angle As Double) As Double
    ' Calculate cosecant of an angle, in radians.
    Return 1.0 / Math.Sin(angle)
End Function
```

## <a name="example---sqrt"></a>例-Sqrt

この例では、<xref:System.Math> クラスの <xref:System.Math.Sqrt%2A> メソッドを使用して、数値の平方根を計算します。

```vb
Dim mySqrt1 As Double = Math.Sqrt(4)
Dim mySqrt2 As Double = Math.Sqrt(23)
Dim mySqrt3 As Double = Math.Sqrt(0)
Dim mySqrt4 As Double = Math.Sqrt(-4)
Console.WriteLine(mySqrt1)
Console.WriteLine(mySqrt2)
Console.WriteLine(mySqrt3)
Console.WriteLine(mySqrt4)
' The code produces the following output:
' 2
' 4.79583152331272
' 0
' NaN
```

## <a name="example---tan"></a>例-Tan

この例では、<xref:System.Math> クラスの <xref:System.Math.Tan%2A> メソッドを使用して、角度のタンジェントを返します。

```vb
Public Function Ctan(angle As Double) As Double
    ' Calculate cotangent of an angle, in radians.
    Return 1.0 / Math.Tan(angle)
End Function
```

## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.VBMath.Rnd%2A>
- <xref:Microsoft.VisualBasic.VBMath.Randomize%2A>
- <xref:System.Double.NaN>
- [数値演算関数の導出](../keywords/derived-math-functions.md)
- [算術演算子](../operators/arithmetic-operators.md)
