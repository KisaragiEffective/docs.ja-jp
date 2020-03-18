---
title: 浮動小数点数値型 - C# リファレンス
description: 組み込みの C# 浮動小数点型 (float、double、decimal) について説明します
ms.date: 02/10/2020
f1_keywords:
- float
- float_CSharpKeyword
- double
- double_CSharpKeyword
- decimal_CSharpKeyword
- decimal
helpviewer_keywords:
- floating-point numbers [C#]
- ranges of floating-point types [C#]
- size of floating-point types [C#]
- types [C#], floating-point types
- float keyword [C#]
- floating-point numbers [C#], float keyword
- double data type [C#]
- decimal keyword [C#]
ms.openlocfilehash: 95b7f266654bbbcdcd0f81e3aa11cfc94af9f0e5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77215249"
---
# <a name="floating-point-numeric-types-c-reference"></a>浮動小数点数値型 (C# リファレンス)

"*浮動小数点数値型*" は実数を表します。 浮動小数点数値型は、[値の型](value-types.md)です。 また、[単純型](value-types.md#built-in-value-types)でもあり、[リテラル](#real-literals)を使用して初期化することができます。 すべての浮動小数点数値型は、[算術](../operators/arithmetic-operators.md)、[比較](../operators/comparison-operators.md)、および[等値](../operators/equality-operators.md)演算子をサポートしています。

## <a name="characteristics-of-the-floating-point-types"></a>浮動小数点型の特性

C# では、次の定義済みの浮動小数点型がサポートされています。
  
|C# 型/キーワード|おおよその範囲|Precision|Size|.NET の種類|
|----------|-----------------------|---------------|--------------|--------------|
|`float`|±1.5 x 10<sup>−45</sup> から ±3.4 x 10<sup>38</sup>|~6 ～9 桁|4 バイト|<xref:System.Single?displayProperty=nameWithType>|
|`double`|±5.0 × 10<sup>−324</sup> - ±1.7 × 10<sup>308</sup>|~15-17 桁|8 バイト|<xref:System.Double?displayProperty=nameWithType>|
|`decimal`|±1.0 x 10<sup>-28</sup> から ±7.9228 x 10<sup>28</sup>|28 から 29 桁の数字|16 バイト|<xref:System.Decimal?displayProperty=nameWithType>|

上の表で、左端の列にある各 C# 型のキーワードは、対応する .NET 型の別名です。 これらは交換可能です。 たとえば、次の宣言では同じ型の変数が宣言されています。

```csharp
double a = 12.3;
System.Double b = 12.3;
```

各浮動小数点型の既定値はゼロ `0` です。 各浮動小数点型には、その型の最小および最大有限値を指定する `MinValue` および `MaxValue` 定数があります。 `float` および `double` 型では、数字ではない値や無限値を表す定数も提供されています。 たとえば、`double` 型では、定数 <xref:System.Double.NaN?displayProperty=nameWithType>、<xref:System.Double.NegativeInfinity?displayProperty=nameWithType>、<xref:System.Double.PositiveInfinity?displayProperty=nameWithType> が提供されています。

`decimal` 型は、`float` と `double` の両方よりも有効桁数が多く、範囲が狭いため、財務や金融の計算に適しています。

[整数](integral-numeric-types.md)型と `float` および `double` 型を 1 つの式の中で混在させることができます。 この場合、整数型は、浮動小数点型の 1 つに暗黙的に変換されます。また、必要に応じて、`float` 型は `double` に暗黙的に変換されます。 この式は、次のように評価されます。

- 式に `double` 型がある場合、リレーショナル比較と等価比較で、式は `double`、または [`bool`](bool.md) に評価されます。
- 式に `double` 型がない場合、リレーショナル比較と等価比較で、式は `float`、または `bool` に評価されます。

また、整数型と `decimal` 型を 1 つの式の中で混在させることもできます。 この場合、整数型は `decimal` 型に暗黙的に変換され、リレーショナル比較と等価比較で、式は `decimal`、または `bool` に評価されます。

`decimal` 型と `float` および `double` 型を 1 つの式の中で混在させることはできません。 この場合、算術演算、比較演算、または等値演算を実行するには、次の例に示すように、`decimal` 型との間でオペランドを明示的に変換する必要があります。

```csharp-interactive
double a = 1.0;
decimal b = 2.1m;
Console.WriteLine(a + (double)b);
Console.WriteLine((decimal)a + b);
```

浮動小数点値の書式指定には、[標準の数値書式指定文字列](../../../standard/base-types/standard-numeric-format-strings.md)または[カスタムの数値書式指定文字列](../../../standard/base-types/custom-numeric-format-strings.md)のいずれかを使用できます。

## <a name="real-literals"></a>実数リテラル

実数リテラルの型は、サフィックスによって次のように決まります。

- サフィックスがない、または `d` または `D` のリテラルは `double` 型です
- サフィックスが `f` または `F` のリテラルは `float` 型です
- サフィックスが `m` または `M` のリテラルは `decimal` 型です

次のコードは、それぞれの例を示しています。

```csharp
double d = 3D;
d = 4d;
d = 3.934_001;

float f = 3_000.5F;
f = 5.4f;

decimal myMoney = 3_000.5m;
myMoney = 400.75M;
```

前述の例は、C# 7.0 以降でサポートされている "`_`桁区切り記号 *" としての*  の使用法も示しています。 数字区切り記号は、あらゆる種類の数値リテラルで使用できます。

次の例に示すように、指数表記を使用して、実数リテラルの指数部を指定することもできます。

```csharp-interactive
double d = 0.42e2;
Console.WriteLine(d);  // output 42;

float f = 134.45E-2f;
Console.WriteLine(f);  // output: 1.3445

decimal m = 1.5E6m;
Console.WriteLine(m);  // output: 1500000
```

## <a name="conversions"></a>コンバージョン

浮動小数点数値型の間には、`float` から `double` に対する暗黙的な変換が 1 つだけあります。 ただし、[明示的なキャスト](../operators/type-testing-and-cast.md#cast-operator-)を使用して、任意の浮動小数点型を他の浮動小数点型に変換することはできます。 詳細については、「[Built-in numeric conversions](numeric-conversions.md)」(組み込みの数値変換) を参照してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [浮動小数点型](~/_csharplang/spec/types.md#floating-point-types)
- [decimal 型](~/_csharplang/spec/types.md#the-decimal-type)
- [実数リテラル](~/_csharplang/spec/lexical-structure.md#real-literals)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [値型](value-types.md)
- [整数型](integral-numeric-types.md)
- [標準の数値書式指定文字列](../../../standard/base-types/standard-numeric-format-strings.md)
- [.NET における数値](../../../standard/numerics.md)
- <xref:System.Numerics.Complex?displayProperty=nameWithType>
