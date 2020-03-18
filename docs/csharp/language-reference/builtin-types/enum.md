---
title: 列挙型 - C# リファレンス
description: 選択肢または選択肢の組み合わせを表す C# 列挙型について説明します
ms.date: 12/13/2019
f1_keywords:
- enum
- enum_CSharpKeyword
helpviewer_keywords:
- enum keyword [C#]
- enum type [C#]
- enumeration type [C#]
- bit flags [C#]
ms.assetid: bbeb9a0f-e9b3-41ab-b0a6-c41b1a08974c
ms.openlocfilehash: ab5eb1679f846bf0e25d90a4d0e0a71f0bdb0096
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847711"
---
# <a name="enumeration-types-c-reference"></a>列挙型 (C# リファレンス)

"*列挙型*" は、基になる[整数値](integral-numeric-types.md)型の一連の名前付き定数によって定義された[値の型](value-types.md)です。 列挙型を定義するには、`enum` キーワードを使用して "*列挙型メンバー*" の名前を指定します。

```csharp
enum Season
{
    Spring,
    Summer,
    Autumn,
    Winter
}
```

既定では、列挙型メンバーの関連する定数値の型は `int` で、0 から始まり、定義テキストの順序に従って 1 ずつ増加します。 他の任意の[整数値](integral-numeric-types.md)型を、列挙型の基になる型として明示的に指定できます。 また、次の例に示すように、関連する定数値を明示的に指定することもできます。

```csharp
enum ErrorCode : ushort
{
    None = 0,
    Unknown = 1,
    ConnectionLost = 100,
    OutlierReading = 200
}
```

列挙型の定義内でメソッドを定義することはできません。 列挙型に機能を追加するには、[拡張メソッド](../../programming-guide/classes-and-structs/extension-methods.md)を作成します。

列挙型 `E` の既定値は、式 `(E)0` によって生成される値です。この値は、ゼロに対応する列挙メンバーがなくても生成されます。

列挙型を使用して、相互に排他的な一連の値の選択肢、または選択肢の組み合わせを表します。 選択肢の組み合わせを表すには、列挙型をビット フラグとして定義します。

## <a name="enumeration-types-as-bit-flags"></a>ビット フラグとしての列挙型

列挙型で選択肢の組み合わせを表したいときは、個々の選択肢がビット フィールドになるように、列挙型メンバーをそれらの選択肢に対して定義します。 つまり、これらの列挙型メンバーの関連する値は、2 の累乗である必要があります。 次に、[ビットごとの論理演算子 `|` または `&`](../operators/bitwise-and-shift-operators.md#enumeration-logical-operators) を使用し、選択肢を組み合わせたり、選択肢の組み合わせを交差させたりすることができます。 列挙型によってビット フィールドが宣言されていることを示すには、[フラグ](xref:System.FlagsAttribute)属性を適用します。 次の例に示すように、列挙型の定義に一般的な組み合わせをいくつか含めることもできます。

[!code-csharp[enum flags](snippets/EnumType.cs#Flags)]

詳細と例については、<xref:System.FlagsAttribute?displayProperty=nameWithType> API リファレンス ページ、および <xref:System.Enum?displayProperty=nameWithType> API リファレンス ページの「[非排他的メンバーと Flags 属性](/dotnet/api/system.enum#non-exclusive-members-and-the-flags-attribute)」セクションを参照してください。

## <a name="the-systemenum-type-and-enum-constraint"></a>System.Enum 型と列挙型定数

<xref:System.Enum?displayProperty=nameWithType> 型は、すべての列挙型の抽象基底クラスです。 この型には、列挙型とその値に関する情報を取得するためのメソッドがいくつか用意されています。 詳細と例については、<xref:System.Enum?displayProperty=nameWithType> API リファレンス ページを参照してください。

C# 7.3 以降、基底クラス制約 ([列挙の制約](../../programming-guide/generics/constraints-on-type-parameters.md#enum-constraints)と呼ばれます) で `System.Enum` を使用して、型パラメーターが列挙型であることを指定できます。

## <a name="conversions"></a>変換

列挙型については、列挙型とその基になる整数型との間に明示的な変換が存在します。 列挙値をその基になる型に[キャスト](../operators/type-testing-and-cast.md#cast-operator-)すると、結果は列挙メンバーの関連する整数値になります。

[!code-csharp[enum conversions](snippets/EnumType.cs#Conversions)]

<xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> メソッドを使用して、列挙型に、関連する特定の値を持つ列挙型メンバーが含まれているかどうかを確認します。

列挙型については、<xref:System.Enum?displayProperty=nameWithType> 型との間に[ボックス化とボックス化解除](../../programming-guide/types/boxing-and-unboxing.md)変換が存在します。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [列挙型](~/_csharplang/spec/enums.md)
- [列挙型の値と演算子](~/_csharplang/spec/enums.md#enum-values-and-operations)
- [列挙論理演算子](~/_csharplang/spec/expressions.md#enumeration-logical-operators)
- [列挙型比較演算子](~/_csharplang/spec/expressions.md#enumeration-comparison-operators)
- [明示的な列挙変換](~/_csharplang/spec/conversions.md#explicit-enumeration-conversions)
- [暗黙的な列挙変換](~/_csharplang/spec/conversions.md#implicit-enumeration-conversions)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [列挙型書式指定文字列](../../../standard/base-types/enumeration-format-strings.md)
- [設計ガイドライン - 列挙型の設計](../../../standard/design-guidelines/enum.md)
- [設計ガイドライン - 列挙型の名前付け規則](../../../standard/design-guidelines/names-of-classes-structs-and-interfaces.md#naming-enumerations)
- [switch ステートメント](../keywords/switch.md)
