---
title: 破棄 - C# ガイド
description: C# の破棄のサポートについて説明します。破棄は、未割り当てで破棄可能な変数です。また、破棄の使用例についても説明します。
ms.technology: csharp-fundamentals
ms.date: 07/21/2017
ms.openlocfilehash: a76e7fc13f92ec0de87153bb35eb3924bb317616
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73100639"
---
# <a name="discards---c-guide"></a>破棄 - C# ガイド

C# 7.0 以降、C# は破棄をサポートしています。破棄は、アプリケーション コードで意図的に使用しない一時的なダミー変数です。 破棄は、未割り当ての変数と同等です。つまり、値がありません。 破棄変数は 1 つのみであり、破棄変数には記憶域も割り当てられないため、破棄を使用するとメモリの割り当てを減らすことができます。 また、コードの意図がわかりやすくなるため、読みやすさと保守性が向上します。

変数を破棄と指定するには、変数名にアンダースコア (`_`) を指定します。 たとえば、次のメソッド呼び出しは 3 つのタプルを返します。この 1 つ目と 2 つ目の値が破棄されます。*area* は以前に宣言した変数であり、*GetCityInformation* から返された、対応する 3 つ目のコンポーネントに設定されます。

```csharp
(_, _, area) = city.GetCityInformation(cityName);
```

C# 7.0 では、破棄は次のコンテキストの割り当てでサポートされます。

- タプルとオブジェクトの[分解](deconstruct.md)。
- [is](language-reference/keywords/is.md) と [switch](language-reference/keywords/switch.md) によるパターン マッチング。
- `out` パラメーターを使用したメソッドの呼び出し。
- `_` がスコープ内にない場合のスタンドアロンの `_`。

`_` が有効な破棄の場合、その値を取得しようとすると、または代入演算で使用しようとすると、"名前 '\_' は、現在のコンテキストに存在していません" というコンパイラ エラー CS0301 が生成されます。 これは、`_` に値が割り当てられておらず、記憶域の場所も割り当てることができないためです。 実際の変数であれば、前の例のように、複数の値を破棄できません。

## <a name="tuple-and-object-deconstruction"></a>タプルとオブジェクトの分解

分解は、複数のタプルがあり、アプリケーション コードで一部のタプル要素を使用し、その他の要素を無視する場合に特に便利です。 たとえば、次の `QueryCityDataForYears` メソッドは、市区町村名、その地域、年、市区町村のその年の人口、2 つ目の年、市区町村のその 2 つ目の年の人口という 6 つのタプルを返します。 この例は、2 つの年の間に変化した人口数を示しています。 タプルから使用できるデータのうち、市区町村の地域は使用しません。また、指定時に市区町村名と 2 つの日付はわかっています。 そのため、タプルに格納されている 2 つの人口値のみが必要であり、残りの値は破棄対象として処理できます。  

[!code-csharp[Tuple-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/discard-tuple1.cs)]

破棄を使用したタプルの分解の詳細については、「[タプルとその他の型の分解](deconstruct.md#deconstructing-tuple-elements-with-discards)」を参照してください。

また、クラス、構造体、またはインターフェイスの `Deconstruct` メソッドを使用すると、オブジェクトから特定セットのデータを取得し、分解することもできます。 分解された値の一部のみが必要な場合は、破棄を使用できます。 `Person` オブジェクトを 4 つの文字列 (名、姓、市区町村、州) に分解し、姓と州を破棄する例を次に示します。

[!code-csharp[Class-discard](../../samples/snippets/csharp/programming-guide/deconstructing-tuples/class-discard1.cs)]

破棄を使用したユーザー定義型の分解の詳細については、「[タプルとその他の型の分解](deconstruct.md#deconstructing-a-user-defined-type-with-discards)」を参照してください。

## <a name="pattern-matching-with-switch-and-is"></a>`switch` と `is` を使用したパターン マッチング

*破棄パターン*は、[is](language-reference/keywords/is.md) キーワードと [switch](language-reference/keywords/switch.md) キーワードを使用したパターン マッチングで使用できます。 各式は常に破棄パターンと一致します。

`ProvidesFormatInfo`is[ ステートメントを使用して、オブジェクトが ](language-reference/keywords/is.md) 実装を提供しているかどうかを判断し、オブジェクトが <xref:System.IFormatProvider> かどうかをテストする `null` メソッドの定義例を次に示します。 また、破棄パターンを使用して、その他の任意の型の null 以外のオブジェクトを処理します。

[!code-csharp[discard-pattern](../../samples/snippets/csharp/programming-guide/discards/discard-pattern2.cs)]

## <a name="calls-to-methods-with-out-parameters"></a>out パラメーターを使用したメソッドの呼び出し

`Deconstruct` メソッドを呼び出してユーザー定義型 (クラス、構造体、またはインターフェイスのインスタンス) を分解する場合、個々の `out` 引数の値を破棄できます。 また、out パラメーターを指定して任意のメソッドを呼び出すときに、`out` 引数の値を破棄することもできます。

次の例では、[DateTime.TryParse(String, out DateTime)](<xref:System.DateTime.TryParse(System.String,System.DateTime@)>) メソッドを呼び出して、現在のカルチャで日付の文字列表現が有効かどうかを判断します。 この例では、日付文字列の検証のみが目的で、解析して日付を抽出する処理は行わないため、メソッドの `out` 引数は破棄されます。

[!code-csharp[discard-with-out](../../samples/snippets/csharp/programming-guide/discards/discard-out1.cs)]

## <a name="a-standalone-discard"></a>スタンドアロンの破棄

スタンドアロンの破棄を使用して、無視対象として任意の変数を指定できます。 次の例では、スタンドアロンの破棄を使用して、非同期操作で返される <xref:System.Threading.Tasks.Task> オブジェクトを無視します。 この結果、この処理が完了するときにスローされる例外が抑制される効果があります。

[!code-csharp[standalone-discard](../../samples/snippets/csharp/programming-guide/discards/standalone-discard1.cs)]

`_` は有効な識別子でもある点に注意してください。 サポートされるコンテキスト以外で `_` を使用すると、破棄対象ではなく、有効な変数として扱われます。 `_` という識別子が既にスコープ内にある場合、スタンドアロンの破棄として `_` を使用すると、次のような結果になります。

- 意図した破棄の値を割り当てることで、スコープ内の `_` 変数の値が誤って変更される。 次に例を示します。

   [!code-csharp[standalone-discard](../../samples/snippets/csharp/programming-guide/discards/standalone-discard2.cs#1)]

- タイプ セーフに違反するコンパイラ エラーが発生する。 次に例を示します。

   [!code-csharp[standalone-discard](../../samples/snippets/csharp/programming-guide/discards/standalone-discard2.cs#2)]

- コンパイラ エラー CS0136 "ローカルまたはパラメーター '\_' は、その名前が外側のローカルのスコープでローカルやパラメーターの定義に使用されているため、このスコープでは宣言できません" が発生する。 次に例を示します。

   [!code-csharp[standalone-discard](../../samples/snippets/csharp/programming-guide/discards/standalone-discard2.cs#3)]

## <a name="see-also"></a>参照

- [タプルおよびその他の型の分解](deconstruct.md)
- [`is` キーワード](language-reference/keywords/is.md)
- [`switch` キーワード](language-reference/keywords/switch.md)
