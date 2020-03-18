---
title: LINQ をサポートする C# の機能
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], features supporting LINQ
ms.assetid: 524b0078-ebfd-45a7-b390-f2ceb9d84797
ms.openlocfilehash: 9fc8adaa49d02f8b69c2db6e94a28b9fab36b3b0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635796"
---
# <a name="c-features-that-support-linq"></a>LINQ をサポートする C# の機能

このセクションでは、C# 3.0 で導入された新しい言語構成要素について説明します。 これらの新機能はすべてある程度まで LINQ クエリで使用されていますが、LINQ だけでなく、これらの機能が役立つと思われるあらゆる状況で使用できます。

## <a name="query-expressions"></a>クエリ式

クエリ式は、SQL や XQuery に似た宣言型構文を使用して、IEnumerable コレクションを照会します。 クエリ構文は、コンパイル時に、LINQ プロバイダーの標準クエリ演算子拡張メソッドの実装に対するメソッド呼び出しに変換されます。 アプリケーションは、`using` ディレクティブを使用して適切な名前空間を指定することにより、スコープ内の標準クエリ演算子を制御します。 次のクエリ式では、文字列の配列を受け取り、文字列の最初の文字を基に文字列をグループ化し、グループを並び替えています。

```csharp
var query = from str in stringArray
            group str by str[0] into stringGroup
            orderby stringGroup.Key
            select stringGroup;
```

詳細については、「[LINQ クエリ式](../../../linq/index.md)」を参照してください。

## <a name="implicitly-typed-variables-var"></a>暗黙的に型指定された変数 (var)

変数を宣言して初期化するときに、型を明示的に指定する代わりに、次に示すように [var](../../../language-reference/keywords/var.md) 修飾子を使用すると、コンパイラが型を推論して代入するように指示できます。

```csharp
var number = 5;
var name = "Virginia";
var query = from str in stringArray
            where str[0] == 'm'
            select str;
```

`var` として宣言された変数は、明示的に型を指定した変数とまったく同じように厳密に型指定されます。 `var` を使用すると匿名型を作成できますが、ローカル変数にのみ使用できます。 また、暗黙的な型指定を使用して、配列を宣言することもできます。

詳細については、「[暗黙的に型指定されたローカル変数](../../classes-and-structs/implicitly-typed-local-variables.md)」を参照してください。

## <a name="object-and-collection-initializers"></a>オブジェクト初期化子とコレクション初期化子

オブジェクト初期化子とコレクション初期化子を使用すると、オブジェクトのコンストラクターを明示的に呼び出さなくても、オブジェクトを初期化できます。 通常、初期化子は、ソース データを新しいデータ型に投影するクエリ式で使用されます。 パブリックな `Customer` プロパティと `Name` プロパティを持つ `Phone` という名前のクラスがある場合、オブジェクト初期化子は次のコードのように使用できます。

```csharp
var cust = new Customer { Name = "Mike", Phone = "555-1212" };
```

引き続き `Customer` クラスの例を進めます。`IncomingOrders` というデータ ソースがあり、`OrderSize` が大きな注文ごとに、その注文に基づいて新しい `Customer` を作成するとします。 LINQ クエリをこのデータ ソースで実行し、オブジェクト初期化を使用してコレクションを満たすことができます。

```csharp
var newLargeOrderCustomers = from o in IncomingOrders
                            where o.OrderSize > 5
                            select new Customer { Name = o.Name, Phone = o.Phone };
```

データ ソースには、`Customer` など、`OrderSize` クラス以外にもプロパティが存在する可能性がありますが、オブジェクト初期化を利用することで、クエリから返されるデータは目的のデータ型に形成されます。今回のクラスに関連するデータを選択します。 その結果、必要とする新しい `IEnumerable` が `Customer` に入力されました。 上記は次のような LINQ のメソッド構文でも記述できます。

```csharp
var newLargeOrderCustomers = IncomingOrders.Where(x => x.OrderSize > 5).Select(y => new Customer { Name = y.Name, Phone = y.Phone });
```

詳細については次を参照してください:

- [オブジェクト初期化子とコレクション初期化子](../../classes-and-structs/object-and-collection-initializers.md)

- [標準クエリ演算子のクエリ式構文](./query-expression-syntax-for-standard-query-operators.md)

## <a name="anonymous-types"></a>匿名型

匿名型はコンパイラによって作成され、型名はコンパイラにしかわかりません。 匿名型を使用すると、個別に名前付き型を定義しなくても、クエリ結果内のプロパティのセットを一時的にグループ化できるため便利です。 次に示すように、匿名型は new 式とオブジェクト初期化子を使用して初期化されます。

```csharp
select new {name = cust.Name, phone = cust.Phone};
```

詳細については、「[匿名型](../../classes-and-structs/anonymous-types.md)」を参照してください。

## <a name="extension-methods"></a>拡張メソッド

拡張メソッドは型に関連付けることができる静的メソッドであるため、その型のインスタンス メソッドと同じように呼び出すことができます。 この機能を使用すると、既存の型を実際に変更しなくても、その型に新しいメソッドを実質的に "追加" できます。 標準クエリ演算子は、<xref:System.Collections.Generic.IEnumerable%601> を実装する任意の型で LINQ クエリ機能を実現する拡張メソッドのセットです。

詳細については、「[拡張メソッド](../../classes-and-structs/extension-methods.md)」を参照してください。

## <a name="lambda-expressions"></a>ラムダ式

ラムダ式は、=> 演算子を使用して関数本体からパラメーター入力を分離するインライン関数で、コンパイル時にデリゲートまたは式ツリーに変換されます。 LINQ プログラミングでは、標準クエリ演算子に対する直接メソッド呼び出しを行う場合にラムダ式を使用します。

詳細については次を参照してください:

- [匿名関数](../../statements-expressions-operators/anonymous-functions.md)

- [ラムダ式](../../statements-expressions-operators/lambda-expressions.md)

- [式ツリー (C#)](../expression-trees/index.md)

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ) (C#)](./index.md)
