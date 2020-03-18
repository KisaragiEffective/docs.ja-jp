---
title: 構造体型 - C# リファレンス
ms.date: 02/24/2020
f1_keywords:
- struct_CSharpKeyword
helpviewer_keywords:
- struct keyword [C#]
- struct type [C#]
- structure type [C#]
ms.assetid: ff3dd9b7-dc93-4720-8855-ef5558f65c7c
ms.openlocfilehash: b85d0df086f3ca65ed995594dd374286e1c3ba5c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78847730"
---
# <a name="structure-types-c-reference"></a>構造体型 (C# リファレンス)

"*構造体型*" (または "*構造体型*") とは、データおよび関連する機能をカプセル化できる[値の型](value-types.md)です。 構造体型を定義するには、`struct` キーワードを使用します。

[!code-csharp[struct example](snippets/StructType.cs#StructExample)]

構造体型には、"*値のセマンティクス*" があります。 つまり、構造体型の変数には、型のインスタンスが含まれます。 既定では、変数値が代入時にコピーされ、引数がメソッドに渡され、メソッドの結果が返されます。 構造体型の変数の場合は、型のインスタンスがコピーされます。 詳細については、[値の型](value-types.md)に関するページを参照してください。

通常は、構造体型を使用して、ほとんどまたはまったく動作を提供しない小さなデータ中心型を設計します。 たとえば、.NET では、構造体型を使用して数値 ([整数](integral-numeric-types.md)と[実数](floating-point-numeric-types.md)の両方)、[ブール値](bool.md)、[Unicode 文字](char.md)、[時刻インスタンス](xref:System.DateTime)が表現されます。 型の動作に重点を置いている場合は、[class](../keywords/class.md) を定義することを検討してください。 クラス型には "*参照セマンティクス*" があります。 つまり、クラス型の変数には、インスタンス自体ではなく、型のインスタンスへの参照が含まれています。

## <a name="limitations-with-the-design-of-a-structure-type"></a>構造体型の設計に関する制限事項

構造体型を設計する場合は、[class](../keywords/class.md) 型と同じ機能を使用できますが、次の例外があります。

- パラメーターなしのコンストラクターを宣言することはできません。 すべての構造体型には、型の[既定値](default-values.md)を生成する暗黙的なパラメーターなしのコンストラクターが既に備わっています。

- インスタンス フィールドまたはプロパティを、それらの宣言で初期化することはできません。 ただし、[static](../keywords/static.md) または [const](../keywords/const.md) フィールド、あるいは静的プロパティについては、それらの宣言で初期化することができます。

- 構造体型のコンストラクターでは、型のすべてのインスタンス フィールドを初期化する必要があります。

- 構造体型は、他のクラスまたは構造体型から継承することができないほか、クラスのベースとすることもできません。 ただし、構造体型では [interfaces](../keywords/interface.md) を実装することができます。

- 構造体型内で[ファイナライザー](../../programming-guide/classes-and-structs/destructors.md)を宣言することはできません。

## <a name="instantiation-of-a-structure-type"></a>構造体型のインスタンス化

C# では、宣言された変数を使用するには、事前にこれを初期化する必要があります。 構造体型の変数は `null` とすることができないため (それが [null 許容値型](nullable-value-types.md)の変数でない限り)、対応する型のインスタンスをインスタンス化する必要があります。 それにはいくつかの方法があります。

通常は、[`new`](../operators/new-operator.md) 演算子を使用して適切なコンストラクターを呼び出すことによって、構造体型をインスタンス化します。 すべての構造体型に、少なくとも 1 つのコンストラクターがあります。 それは暗黙的なパラメーターなしのコンストラクターであり、型の[既定値](default-values.md)を生成するものです。 また、[default](../operators/default.md) 演算子またはリテラルを使用して、型の既定値を作成することもできます。

構造体型のすべてのインスタンス フィールドにアクセスできる場合は、それを `new` 演算子なしでインスタンス化することもできます。 その場合は、インスタンスを初めて使用する前に、すべてのインスタンス フィールドを初期化する必要があります。 その方法を次の例に示します。

[!code-csharp[without new](snippets/StructType.cs#WithoutNew)]

[組み込みの値型](value-types.md#built-in-value-types)の場合は、対応するリテラルを使用して型の値を指定します。

## <a name="passing-structure-type-variables-by-reference"></a>構造体型の変数を参照渡しする

構造体型の変数を引数としてメソッドに渡す場合、またはメソッドから構造体型の値を返す場合は、構造体型のインスタンス全体がコピーされます。 これは、大規模な構造体型を必要とするハイパフォーマンスのシナリオの場合、ご利用のコードのパフォーマンスに影響を与える可能性があります。 値のコピーを回避するには、構造体型の変数を参照渡しします。 引数を参照渡しする必要があることを示すには、[`ref`](../keywords/ref.md#passing-an-argument-by-reference)、[`out`](../keywords/out-parameter-modifier.md)、または [`in`](../keywords/in-parameter-modifier.md) のメソッド パラメーター修飾子を使用します。 メソッドの結果を参照渡しによって返すには、[ref 戻り値](../../programming-guide/classes-and-structs/ref-returns.md)を使用します。 詳細については、「[安全で効率的な C# コードを記述する](../../write-safe-efficient-code.md)」をご覧ください。

## <a name="conversions"></a>変換

どの構造体型にも、<xref:System.ValueType?displayProperty=nameWithType> 型と <xref:System.Object?displayProperty=nameWithType> 型の間に[ボックス化およびボックス化解除](../../programming-guide/types/boxing-and-unboxing.md)の変換が存在します。 また、構造体型と、これによって実装されるインターフェイスとの間にも、ボックス化とボックス化解除の変換が存在します。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/introduction.md)の「[構造体](~/_csharplang/spec/structs.md)」セクションを参照してください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [デザインのガイドライン - クラスまたは構造体の選択](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [デザインのガイドライン - 構造体のデザイン](../../../standard/design-guidelines/struct.md)
- [クラスと構造体](../../programming-guide/classes-and-structs/index.md)
