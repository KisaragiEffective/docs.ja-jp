---
title: 値型 - C# リファレンス
ms.date: 01/22/2020
f1_keywords:
- cs.valuetypes
helpviewer_keywords:
- value types [C#]
- types [C#], value types
- C# language, value types
ms.assetid: 471eb994-2958-49d5-a6be-19b4313f80a3
ms.openlocfilehash: 406e5b8bbe0802146a65bb4b9a053e753a7827ee
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398269"
---
# <a name="value-types-c-reference"></a>値型 (C# リファレンス)

C# 型の 2 つの主なカテゴリは、*値型*と[参照型](../keywords/reference-types.md)です。 値型の変数には、その型のインスタンスが含まれます。 これは、その型のインスタンスへの参照を含む参照型の変数とは異なります。 既定では、[代入](../operators/assignment-operator.md)時、引数がメソッドに渡され、メソッドの結果が返され、変数値がコピーされます。 値型の変数の場合、対応する型のインスタンスがコピーされます。 次の例は、その動作を示します。

[!code-csharp[copy of values](snippets/ValueTypes.cs#ValueTypeCopied)]

前の例のとおり、値型変数に対する操作は、その変数に格納されている値型のインスタンスのみに影響します。

値型に参照型のデータ メンバーが含まれている場合は、値型のインスタンスがコピーされるとき、その参照型のインスタンスへの参照のみがコピーされます。 コピーと元の値型のインスタンスの両方が、同じ参照型のインスタンスにアクセスできます。 次の例は、その動作を示します。

[!code-csharp[shallow copy](snippets/ValueTypes.cs#ShallowCopy)]

> [!NOTE]
> ご自分のコードをエラーがより発生しにくく、より堅牢にするには、変更できない値型を定義して使用します。 この記事では、デモンストレーションの目的でのみ、変更可能な値型を使用します。

## <a name="kinds-of-value-types"></a>値型の種類

値型には、次の 2 種類のいずれかを指定できます。

- データと関連機能をカプセル化する[構造体の型](struct.md)
- 名前付き定数のセットによって定義され、選択肢または選択肢の組み合わせを表す、[列挙型](enum.md)

[null 許容値型](nullable-value-types.md) `T?` は、基になる値型のすべての値 `T` と、追加の [null](../keywords/null.md) 値を表します。 値型の変数には、Null 許容値型でない限り `null` を割り当てることはできません。

## <a name="built-in-value-types"></a>組み込みの値型

C# には、*単純型*とも呼ばれる次の組み込み値型が用意されています。

- [整数数値型](integral-numeric-types.md)
- [浮動小数点数値型](floating-point-numeric-types.md)
- ブール値を表す [bool](bool.md)
- Unicode UTF-16 文字を表す [char](char.md)

すべての単純型は構造体型で、それらには他の構造体型とは異なる特定の追加操作があります。

- 単純型の値の指定には、リテラルを使用できます。 たとえば、`'A'` は `char` 型のリテラルで、`2001` は `int` 型のリテラルです。

- 単純型の定数は、[const](../keywords/const.md) キーワードを使用して宣言できます。 他の構造体型の定数は使用できません。

- オペランドがすべて単純型の定数の定数式は、コンパイル時に評価されます。

C# は C# 7.0 以降で、[値のタプル](../../tuples.md)をサポートしています。 値のタプルは単純型ではない値型です。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [値型](~/_csharplang/spec/types.md#value-types)
- [単純型](~/_csharplang/spec/types.md#simple-types)
- [変数](~/_csharplang/spec/variables.md)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- <xref:System.ValueType?displayProperty=nameWithType>
- [参照型](../keywords/reference-types.md)
