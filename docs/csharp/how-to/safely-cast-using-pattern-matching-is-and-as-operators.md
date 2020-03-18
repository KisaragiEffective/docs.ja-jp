---
title: パターン マッチング、is 演算子、as 演算子を使用して安全にキャストする方法
description: パターン マッチングの手法を利用し、変数を別の型に安全にキャストする方法について説明します。 パターン マッチング、is 演算子、as 演算子を利用し、型を安全に変換できます。
ms.date: 09/05/2018
helpviewer_keywords:
- cast operators [C#], as and is operators
- as operator [C#]
- is operator [C#]
ms.openlocfilehash: 762f8135063f7256ce7a167c65013703d9249039
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73973094"
---
# <a name="how-to-safely-cast-by-using-pattern-matching-and-the-is-and-as-operators"></a>パターン マッチング、is 演算子、as 演算子を使用して安全にキャストする方法

オブジェクトはポリモーフィックであるため、基本クラス型の変数で派生[型](../programming-guide/types/index.md)を保持できます。 派生型のインスタンス メンバーにアクセスするには、値を[キャスト](../programming-guide/types/casting-and-type-conversions.md)して派生型に戻す必要があります。 ただし、キャストでは、<xref:System.InvalidCastException> がスローされるリスクが生まれます。 C# には、[パターン マッチング](../pattern-matching.md) ステートメントがあります。これは成功する場合のみという条件でキャストを実行します。 C# には、値が特定の型であることをテストする [is](../language-reference/operators/type-testing-and-cast.md#is-operator) 演算子と [as](../language-reference/operators/type-testing-and-cast.md#as-operator) 演算子もあります。

次のコードは、パターン マッチング `is` ステートメントを示しています。 メソッド引数をテストし、それが可能な派生型セットの 1 つであるかどうかを判断するメソッドが含まれています。

[!code-csharp[Pattern matching is statement](../../../samples/snippets/csharp/how-to/safelycast/patternmatching/Program.cs#PatternMatchingIs)]

上記のサンプルでは、パターン マッチング構文のいくつかの機能が示されています。 `if (a is Mammal m)` ステートメントと `if (o is Mammal m)` ステートメントにより、初期化が割り当てられたテストが結合されます。 この割り当ては、テストに成功した場合にのみ行われます。 変数 `m` は、それが割り当てられている埋め込み `if` ステートメントでのみ範囲に入ります。 後で同じメソッドで `m` にアクセスすることはできません。 対話型ウィンドウで試してください。

次のサンプル コードに示されているように、[null 許容値型](../language-reference/builtin-types/nullable-value-types.md)に値があるかどうかをテストする目的で同じ構文を使用することもできます。

[!code-csharp[Pattern matching with nullable types](../../../samples/snippets/csharp/how-to/safelycast/nullablepatternmatching/Program.cs#PatternMatchingNullable)]

上記のサンプルでは、変換で使用するパターン マッチング構文の他の機能が示されています。 `null` 値を具体的に探すことで null パターンの変数をテストできます。 変数のランタイム値が `null` のとき、`is` ステートメントで型を確認すると必ず `false` が返されます。 パターン マッチング `is` ステートメントでは、`int?` や `Nullable<int>` など、null 許容値型が許可されませんが、他の値の型についてはテストできます。 前の例の `is` パターンは null 許容値型に限定されません。 また、これらのパターンを使用して、参照型の変数に値があるか、`null` であるかをテストすることもできます。

上記のサンプルでは、変数がさまざまな型の 1 つになる `is` ステートメントでパターン マッチング `switch` 式を使用する方法も確認できます。

変数が指定の型かどうかをテストしても、それを新しい変数に割り当てない場合、参照型と null 許容型に対して `is` 演算子と `as` 演算子を使用できます。 次のコードでは、パターン マッチングが導入される前に C# 言語に含まれていた `is` ステートメントと `as` ステートメントを使用し、変数が指定の型かどうかをテストする方法が示されています。

[!code-csharp[testing variable types with the is and as statements](../../../samples/snippets/csharp/how-to/safelycast/asandis/Program.cs#IsAndAs)]

ご覧のとおり、このコードとパターン マッチング コードを比較することで、テストと割り当てが 1 回のステートメントで組み合わされ、パターン マッチング構文がより強固になります。 可能な限り、パターン マッチングを使用してください。

[GitHub リポジトリ](https://github.com/dotnet/samples/tree/master/snippets/csharp/how-to/safelycast)のコードを見て、これらのサンプルを試すことができます。 または、サンプルを [zip ファイルとして](https://github.com/dotnet/samples/raw/master/snippets/csharp/how-to/safelycast.zip)ダウンロードすることができます。
