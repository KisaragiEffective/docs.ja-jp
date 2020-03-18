---
title: C# の型と変数 - C# 言語のツアー
description: C# における型の定義と変数の宣言について説明します
ms.date: 02/25/2020
ms.assetid: f8a8051e-0049-43f1-b594-9c84cc7b1224
ms.openlocfilehash: b2a5255a243c12543a1cd59b5724b6c826306e04
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78159092"
---
# <a name="types-and-variables"></a>型と変数

C# には、*値型*と*参照型*という 2 種類の型があります。 値型の変数が直接データを格納するのに対して、参照型の変数はデータへの参照を格納し、後者はオブジェクトとして知られています。 参照型を使用すると 2 つの変数が同じオブジェクトを参照できるため、1 つの変数に対する演算によって、もう一方の変数によって参照されるオブジェクトに影響を与えることができます。 値型の場合、各変数によって独自のデータ コピーが保持され、1 つの変数に対する演算によって別の変数に影響を与えることはできません (`ref` と `out` のパラメーターの変数を除く)。

C# の値型はさらに、*単純型*、*列挙型*、*構造体型*、および *null 許容値型* に分けられます。 C# の参照型はさらに、*クラス型*、*インターフェイス型*、*配列型*、および*デリゲート型*に分けられます。

以下は、C# の型システムの概要です。

- [値型][ValueTypes]
  - [単純型][SimpleTypes]
    - 符号付きの整数: `sbyte`、`short`、`int`、`long`
    - 符号なしの整数: `byte`、`ushort`、`uint`、`ulong`
    - Unicode 文字: `char`
    - IEEE バイナリ浮動小数点数: `float`、`double`
    - 高精度 10 進浮動小数点数: `decimal`
    - ブール値: `bool`
  - [列挙型][EnumTypes]
    - `enum E {...}` 形式のユーザー定義型
  - [構造体の型][StructTypes]
    - `struct S {...}` 形式のユーザー定義型
  - [null 許容値型][NullableTypes]
    - `null` 値を持つその他すべての値型の拡張子
- [参照型][ReferenceTypes]
  - [クラス型][ClassTypes]
    - その他すべての型の最終的な基底クラス: `object`
    - Unicode 文字列: `string`
    - `class C {...}` 形式のユーザー定義型
  - [インターフェイス型][InterfaceTypes]
    - `interface I {...}` 形式のユーザー定義型
  - [配列型][ArrayTypes]
    - 1 次元または多次元、たとえば `int[]` および `int[,]`
  - [デリゲート型][DelegateTypes]
    - `delegate int D(...)` 形式のユーザー定義型

[ValueTypes]: ../language-reference/builtin-types/value-types.md
[SimpleTypes]: ../language-reference/builtin-types/value-types.md#built-in-value-types
[EnumTypes]: ../language-reference/builtin-types/enum.md
[StructTypes]: ../language-reference/builtin-types/struct.md
[NullableTypes]: ../language-reference/builtin-types/nullable-value-types.md
[ReferenceTypes]: ../language-reference/keywords/reference-types.md
[ClassTypes]: ../language-reference/keywords/class.md
[InterfaceTypes]: ../language-reference/keywords/interface.md
[DelegateTypes]: ../language-reference/keywords/delegate.md
[ArrayTypes]: ../programming-guide/arrays/index.md

数値型について詳しくは、[整数型](../language-reference/builtin-types/integral-numeric-types.md)および[浮動小数点型の一覧表](../language-reference/builtin-types/floating-point-numeric-types.md)に関するページをご覧ください。

C# の `bool` 型はブール値を表すのに使用します。値は `true` か `false` のどちらかです。

C# における文字および文字列の処理では、Unicode エンコーディングを使用します。 `char` 型は UTF-16 コード単位を表し、`string` 型は一連の UTF-16 コード単位を表します。

C# プログラムでは*型宣言*を使用して新しい型を作成します。 型宣言は、新しい型の名前とメンバーを指定します。 C# の型カテゴリのうち 5 つはユーザー定義が可能です。クラス型、構造体型、インターフェイス型、列挙型、そしてデリゲート型です。

`class` 型は、データ メンバー (フィールド) と関数メンバー (メソッド、プロパティ、その他) を含むデータ構造を定義します。 クラス型では、単一継承とポリモーフィズムをサポートします。このメカニズムによって派生クラスが基底クラスを拡張して特殊化できます。

`struct` 型は、データ メンバーおよび関数メンバーで構造体を表す点において、クラス型に似ています。 ただしクラスと異なり、構造体は値型で、通常はヒープ割り当てが不要です。 構造体型ではユーザー指定の継承がサポートされず、すべての構造体型によって暗黙的に `object` 型が継承されます。

`interface` 型は、パブリック関数メンバーの名前付きセットとしてコントラクトを定義します。 `interface` を実装する `class` または `struct` は、インターフェイスの関数メンバーの実装を提供する必要があります。 `interface` は複数の基底インターフェイスから継承することがあり、`class` または `struct` は複数のインターフェイスを実装することがあります。

`delegate` 型は、特定のパラメーター リストおよび戻り値を使用してメソッドへの参照を表します。 デリゲートを使用すると、変数に割り当ててパラメーターとして渡すことのできるエンティティとして、メソッドを処理できます。 デリゲートは、関数型言語で提供される関数の型に似ています。 また、他のいくつかの言語で見られる関数ポインターの概念に似ています。 ただし、関数ポインターとは異なり、デリゲートはオブジェクト指向で、タイプ セーフです。

`class`、`struct`、`interface` および `delegate` の型はすべてジェネリックをサポートし、他の型と共にパラメーター化できます。

`enum` 型は、名前付き定数を持つ固有の型です。 `enum` 型にはそれぞれ基になる型があり、これは 8 つの整数型のいずれかでなければいけません。 `enum` 型の値のセットは、その基になる型の値のセットと同じです。

C# は、あらゆる型の 1 次元および多次元の配列をサポートしています。 上記の型とは異なり、配列型は使用前に宣言する必要がありません。 代わりに配列型は、角かっこで囲んだ型名を後に付けることにより構成されます。 たとえば、`int[]` は `int` の 1 次元配列で、`int[,]` は `int` の 2 次元配列、そして `int[][]` は `int` の 1 次元配列の 1 次元配列です。

null 許容値型もまた、使用前に宣言する必要がありません。 null 非許容値型 `T` のそれぞれについて、対応する null 許容値型 `T?` があり、これは追加値 `null` を保持することができます。 たとえば、`int?` は任意の 32 ビット整数または `null` 値を保持できる型です。

C# の型システムは、任意の型の値を `object` として扱うように統一されています。 C# における型はすべて、直接的または間接的に `object` クラス型から派生し、`object` はすべての型の究極の基底クラスです。 参照型の値は、値を単純に `object` 型としてみなすことによってオブジェクトとして扱われます。 値型の値は、*ボックス化*と*ボックス化解除操作*を実行することによって、オブジェクトとして扱われます。 次の例では、`int` 値は `object` 値に変換され、また `int` に戻されます。

[!code-csharp[Boxing](../../../samples/snippets/csharp/tour/types-and-variables/Program.cs#L1-L10)]

値型の値を `object` 型に変換すると、"ボックス" とも呼ばれる `object` インスタンスが値を保持するために割り当てられ、値がそのボックスにコピーされます。 逆に、`object` 参照が値型にキャストされると、参照先の `object` が適切な値型のボックスかどうかが確認され、確認が成功すると、ボックスの値がコピーされます。

C# の型システムが統一されたということは、実質的には値型が “オンデマンドで” オブジェクトになることができるということです。 こうした統一性があるため、`object` 型を使用する汎用的なライブラリは、参照型と値型の両方で使用できます。

C# には、フィールド、配列要素、ローカル変数、パラメーターなどの、いくつかの種類の*変数*があります。 変数は記憶域の場所を表し、次のように、すべての変数には、その変数に格納できる値を指定する型があります。

- null 非許容値型
  - 型そのものの値
- null 許容値型
  - `null` 値、またはその型そのものの値
- object
  - `null` 参照、任意の参照型のオブジェクトへの参照、または任意の値型のボックス化された値への参照
- クラス型
  - `null` 参照、そのクラス型のインスタンスへの参照、またはそのクラス型から派生したクラスのインスタンスへの参照
- インターフェイスの型
  - `null` 参照、そのインターフェイスの型を実装するクラス型のインスタンスへの参照、またはそのインターフェイス型を実装する値型のボックス化された値への参照
- 配列型
  - `null` 参照、その配列型のインスタンスへの参照、または互換性のある配列型のインスタンスへの参照
- デリゲート型
  - `null` 参照、またはそのデリゲート型と互換性のあるインスタンスへの参照

> [!div class="step-by-step"]
> [前へ](program-structure.md)
> [次へ](expressions.md)
