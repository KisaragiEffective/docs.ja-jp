---
title: 組み込みの数値変換 - C# リファレンス
ms.date: 10/22/2019
helpviewer_keywords:
- implicit numeric conversions [C#]
- explicit numeric conversion [C#]
- numeric conversions [C#], implicit
- numeric conversions [C#], explicit
- conversions [C#], implicit numeric
- conversions [C#], explicit numeric
ms.openlocfilehash: 5380e8480c39d1940df13b2ecb50a0f394367388
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398287"
---
# <a name="built-in-numeric-conversions-c-reference"></a>組み込みの数値変換 (C# リファレンス)

C# では、[整数](integral-numeric-types.md)数値型と[浮動小数点](floating-point-numeric-types.md)数値型のセットを提供します。 任意の 2 つの数値型の間で、暗黙的または明示的のいずれかの変換が存在します。 明示的な変換を呼び出すには、[キャスト演算子 `()`](../operators/type-testing-and-cast.md#cast-operator-) を使用する必要があります。

## <a name="implicit-numeric-conversions"></a>暗黙の数値変換

組み込みの数値型間の定義済みの暗黙的な変換を次の表に示します。

|ソース|ターゲット|
|----------|--------|
|[sbyte](integral-numeric-types.md)|`short`、`int`、`long`、`float`、`double`、または `decimal`|
|[byte](integral-numeric-types.md)|`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、`double`、または `decimal`|
|[short](integral-numeric-types.md)|`int`、`long`、`float`、`double`、または `decimal`|
|[ushort](integral-numeric-types.md)|`int`、`uint`、`long`、`ulong`、`float`、`double`、または `decimal`|
|[int](integral-numeric-types.md)|`long`、`float`、`double`、または `decimal`|
|[uint](integral-numeric-types.md)|`long`、`ulong`、`float`、`double`、または `decimal`|
|[long](integral-numeric-types.md)|`float`、`double`、または `decimal`|
|[ulong](integral-numeric-types.md)|`float`、`double`、または `decimal`|
|[float](floating-point-numeric-types.md)|`double`|

> [!NOTE]
> `int`、`uint`、`long`、または `ulong` から `float` および `long` または `ulong` から `double` への暗黙的な変換では、精度が失われる可能性がありますが、桁違いの損失は発生しません。 その他の暗黙的な数値変換では、情報が失われることはありません。

次の点にも注意してください。

- [整数数値型](integral-numeric-types.md)はすべて、あらゆる[浮動小数点数値型](floating-point-numeric-types.md)に暗黙的に変換できます。

- `byte` および `sbyte` 型への暗黙的な変換はありません。 `double` および `decimal` 型からの暗黙的な変換はありません。

- `decimal` 型と `float` 型または `double` 型の間に暗黙的な変換はありません。

- 型 `int` の定数式の値 (整数リテラルで表される値など) は、それが変換先の型の範囲内にある場合、`sbyte`、`byte`、`short`、`ushort`、`uint`、または `ulong` に暗黙的に変換できます。

  ```csharp
  byte a = 13;
  byte b = 300;  // CS0031: Constant value '300' cannot be converted to a 'byte'
  ```

  前の例で示したように、定数値が変換先の型の範囲内にない場合、コンパイラ エラー [CS0031](../../misc/cs0031.md) が発生します。

## <a name="explicit-numeric-conversions"></a>明示的な数値変換

次の表では、[暗黙的な変換](#implicit-numeric-conversions)がない組み込みの数値型間で事前定義されている明示的変換を示しています。

|ソース|ターゲット|
|----------|--------|
|[sbyte](integral-numeric-types.md)|`byte`、`ushort`、`uint`、または `ulong`|
|[byte](integral-numeric-types.md)|`sbyte`|
|[short](integral-numeric-types.md)|`sbyte`、`byte`、`ushort`、`uint`、または `ulong`|
|[ushort](integral-numeric-types.md)|`sbyte`、`byte`、または `short`|
|[int](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`uint`、または `ulong`|
|[uint](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、または `int`|
|[long](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、または `ulong`|
|[ulong](integral-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、または `long`|
|[float](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、または `decimal`|
|[double](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、または `decimal`|
|[decimal](floating-point-numeric-types.md)|`sbyte`、`byte`、`short`、`ushort`、`int`、`uint`、`long`、`ulong`、`float`、または `double`|

> [!NOTE]
> 明示的な数値変換によって、データが失われたり、例外がスローされたりすることがあります (通常は <xref:System.OverflowException>)。

次の点にも注意してください。

- ある整数型の値を別の整数型に変換するとき、その結果は、オーバーフロー [チェック コンテキスト](../keywords/checked-and-unchecked.md)によって変わります。 checked コンテキストでは、変換元の値が変換先の型の範囲内にあるとき、変換に成功します。 それ以外の場合は、<xref:System.OverflowException> がスローされます。 unchecked コンテキストでは、変換は常に成功し、次のように続行されます。

  - 変換元の型が変換先の型より大きい場合、変換元の値はその "余分な" 最上位ビットを破棄することで切り詰められます。 結果は変換先の型の値として扱われます。

  - 変換元の型が変換先の型より小さい場合、変換元の値は変換先の型と同じサイズになるように、符号拡張またはゼロ拡張されます。 変換元の型に符号が付いている場合は符号拡張が利用され、符号が付いていない場合はゼロ拡張が利用されます。 結果は変換先の型の値として扱われます。

  - 変換元の型が変換先の型と同じサイズの場合、変換元の値は変換先の型の値として扱われます。

- `decimal` 値を整数型に変換するとき、この値は 0 方向に最も近い整数値に丸められます。 結果的に生成される整数値が変換先の型の範囲外になった場合、<xref:System.OverflowException> がスローされます。

- `double` または `float` 値を整数型に変換するとき、この値は 0 方向に最も近い整数値に丸められます。 結果的に生成される整数値が変換先の型の範囲外になる場合、結果はオーバーフロー [チェック コンテキスト](../keywords/checked-and-unchecked.md)によって変わります。 チェック済みコンテキストの場合、<xref:System.OverflowException> がスローされます。未チェック コンテキストの場合、結果は変換先の型の未指定値になります。

- `double` を `float` に変換すると、`double` 値は最も近い `float` 値に丸められます。 `double` 値が小さすぎるか、大きすぎて `float` 型に合わない場合、結果は 0 か無限になります。

- `float` または `double` を `decimal` に変換するとき、変換元の値は `decimal` 表現に変換され、必要であれば、28 番目の小数位の後に最も近い数字に丸められます。 変換元の値によっては、結果は次のいずれかになります。

  - 変換元の値が小さすぎて `decimal` として表現できない場合、結果は 0 になります。

  - 変換元の値が NaN (Not a Number/数字ではない) か、無限か、大きすぎて `decimal` として表現できない場合、<xref:System.OverflowException> がスローされます。

- `decimal` を `float` または `double` に変換すると、変換元の値はそれぞれ最も近い `float` または `double` 値に丸められます。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [暗黙の数値変換](~/_csharplang/spec/conversions.md#implicit-numeric-conversions)
- [明示的な数値変換](~/_csharplang/spec/conversions.md#explicit-numeric-conversions)

## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [キャストと型変換](../../programming-guide/types/casting-and-type-conversions.md)
