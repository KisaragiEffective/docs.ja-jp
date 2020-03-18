---
title: readonly キーワード - C# リファレンス
ms.date: 06/21/2018
f1_keywords:
- readonly_CSharpKeyword
- readonly
helpviewer_keywords:
- readonly keyword [C#]
ms.assetid: 2f8081f6-0de2-4903-898d-99696c48d2f4
ms.openlocfilehash: 165b6287e1610e013b289601e1535a08fdd3b5c9
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398125"
---
# <a name="readonly-c-reference"></a>readonly (C# リファレンス)

`readonly` キーワードは、次の 4 つのコンテキストで使用できる修飾子です。

- [フィールドの宣言](#readonly-field-example)では、`readonly` は、フィールドへの割り当てが、宣言の一部として、または同じクラスのコンストラクター内でのみ可能であることを示します。 readonly フィールドは、フィールドの宣言とコンストラクターで複数回割り当ておよび再割り当てを行うことができます。
  
  `readonly` フィールドは、コンストラクターが終了した後で割り当てることはできません。 この規則は、値型と参照型では意味が異なります。
  
  - 値型にはそのデータが直接含まれるため、`readonly` 値型のフィールドは変更できません。
  - 参照型にはそのデータへの参照が含まれるため、`readonly` 参照型のフィールドは、常に同じオブジェクトを参照する必要があります。 そのオブジェクトは不変ではありません。 `readonly` 修飾子があると、フィールドを参照型の別のインスタンスで置き換えることはできません。 ただし、フィールドのインスタンス データを読み取り専用フィールドで変更することは禁止されません。

  > [!WARNING]
  > 変更可能な参照型である外部から参照できる読み取り専用フィールドを含む外部から参照できる型はセキュリティの脆弱性があり、警告 [CA2104](/visualstudio/code-quality/ca2104) がトリガーされる可能性があります: "読み取り専用の変更可能な参照型を宣言しません"。

- [`readonly struct` の定義](#readonly-struct-example)では、`readonly` は `struct` が変更不可であることを示します。
- [`readonly` メンバー定義](#readonly-member-examples)では、`readonly`は、`struct` のメンバーが構造体の内部状態を変更しないことを示します。
- [`ref readonly` メソッドの戻り値](#ref-readonly-return-example)では、`readonly` 修飾子は、メソッドが参照を返し、その参照への書き込みが許可されないことを示します。

`readonly struct` と `ref readonly` のコンテキストは、C# 7.2 で追加されました。 `readonly` 構造体メンバーは、C# 8.0 で追加されました。

## <a name="readonly-field-example"></a>読み取り専用フィールドの例

この例では、`year` フィールドの値は、クラス コンストラクターで値が割り当てられていても `ChangeYear` メソッドでは変更できません。

[!code-csharp[Readonly Field example](~/samples/snippets/csharp/keywords/ReadonlyKeywordExamples.cs#ReadonlyField)]

`readonly` のフィールドに値を割り当てることができるのは、次のコンテキスト内に限られます。

- 値が宣言で初期化される場合。次に例を示します。

  ```csharp
  public readonly int y = 5;
  ```

- インスタンス フィールド宣言を含むクラスのインスタンス コンストラクター内。
- 静的フィールド宣言を含むクラスの静的コンストラクター内。

また、これらのコンストラクター コンテキスト内でのみ、`readonly` フィールドを [out](out-parameter-modifier.md) パラメーターまたは [ref](ref.md) パラメーターとして渡すことができます。

> [!NOTE]
> `readonly` キーワードは [const](const.md) キーワードとは異なります。 `const` フィールドは、フィールドの宣言でしか初期化できません。 `readonly` フィールドは、フィールドの宣言と任意のコンストラクターで複数回割り当てることができます。 このため、`readonly` フィールドは、使用するコンストラクターに応じて異なる値を持つことができます。 また、次の例のように、`const` フィールドがコンパイル時定数であるのに対し、`readonly` フィールドは実行時定数として使用できます。
>
> ```csharp
> public static readonly uint timeStamp = (uint)DateTime.Now.Ticks;
> ```

[!code-csharp[Initialize readonly Field example](~/samples/snippets/csharp/keywords/ReadonlyKeywordExamples.cs#InitReadonlyField)]

上の例で、次の例のようなステートメントを使うものとします。

```csharp
p2.y = 66;        // Error
```

この場合、次のコンパイル エラー メッセージが表示されます。

**読み取り専用フィールドに割り当てることはできません (コンストラクター、変数初期化子では可)**

## <a name="readonly-struct-example"></a>読み取り専用の構造体の例

`struct` 定義での `readonly` 修飾子は、構造体が**変更不可**であることを宣言します。 次の例のように、`struct` のすべてのインスタンス フィールドを `readonly` とマークする必要があります。

[!code-csharp[readonly struct example](~/samples/snippets/csharp/keywords/ReadonlyKeywordExamples.cs#ReadonlyStruct)]

前の例では、[読み取り専用の自動プロパティ](../../properties.md#read-only)を使ってその記憶域を宣言しています。 これは、これらのプロパティに対して `readonly` バッキング フィールドを作成するようコンパイラに指示します。 `readonly` フィールドを直接宣言することもできます。

```csharp
public readonly struct Point
{
    public readonly double X;
    public readonly double Y;

    public Point(double x, double y) => (X, Y) = (x, y);

    public override string ToString() => $"({X}, {Y})";
}
```

`readonly` に指定されていないフィールドを追加すると、コンパイラ エラー `CS8340`:"読み取り専用の構造体のインスタンス フィールドは、読み取り専用である必要があります" が生成されます。

## <a name="readonly-member-examples"></a>読み取り専用メンバーの例

また、変更をサポートする構造体を作成する場合もあります。 このような場合、一部のインスタンス メンバーは、構造体の内部状態を変更しない可能性があります。 `readonly` を使用すると、これらのインスタンス メンバーを宣言できます。 コンパイラによって意図が適用されます。 そのメンバーが状態を直接変更したり、`readonly` 修飾子で宣言されていないメンバーにもアクセスしたりすると、コンパイル時エラーが発生します。 `readonly` 修飾子は、`class` メンバー宣言または `interface` メンバー宣言ではなく `struct` メンバーで有効です。

`readonly` 修飾子を適用可能な `struct` メソッドに適用すると、2 つの利点が得られます。 最も重要なのは、コンパイラによって意図が適用されることです。 状態を変更するコードは、`readonly` では有効ではありません。 コンパイラは、`readonly` 修飾子を使用してパフォーマンスの最適化を有効にすることもできます。 大きな `struct` 型が `in` 参照によって渡された場合、構造体の状態が変更される可能性があれば、コンパイルは防御用のコピーを生成する必要があります。 `readonly` メンバーのみがアクセスされる場合、コンパイラは防御用のコピーを作成しない可能性があります。

`readonly` 修飾子は、<xref:System.Object?displayProperty=nameWithType> で宣言されたメソッドをオーバーライドするメソッドを含めて、`struct` のほとんどのメンバーで有効です。 いくつかの制限があります。

- `readonly` 静的メソッドまたはプロパティを宣言することはできません。
- `readonly` コンストラクターを宣言することはできません。

`readonly` をプロパティ宣言またはインデクサー宣言に追加できます。

```csharp
readonly public int Counter
{
  get { return 0; }
  set {} // not useful, but legal
}
```

また、`readonly` 修飾子をプロパティまたはインデクサーの個々の `get` アクセサーまたは `set` アクセサーに追加することもできます。

```csharp
public int Counter
{
  readonly get { return _counter; }
  set { _counter = value; }
}
int _counter;
```

`readonly` 修飾子をプロパティと、その同じプロパティの 1 つ以上のアクセサーの両方に追加することはできません。 同じ制限がインデクサーにも適用されます。

コンパイラによって実装されたコードが状態を変更しない場合、コンパイラは `readonly` 修飾子を自動実装プロパティに暗黙的に適用します。 これは次の宣言と同等です。

```csharp
public readonly int Index { get; }
// Or:
public int Number { readonly get; }
public string Message { readonly get; set; }
```

これらの場所に `readonly` 修飾子を追加できますが、有意義な効果はありません。 `readonly` 修飾子を自動実装プロパティのセッター、または読み取り/書き込み自動実装プロパティに追加することはできません。

## <a name="ref-readonly-return-example"></a>ref readonly の戻り値の例

`ref return`での `readonly` 修飾子は、返される参照を変更できないことを示します。 次の例は、origin に参照を返します。 `readonly` 修飾子を使用して、呼び出し元が origin を変更できないことを示しています。

[!code-csharp[readonly struct example](~/samples/snippets/csharp/keywords/ReadonlyKeywordExamples.cs#ReadonlyReturn)]
返される型を `readonly struct` にする必要はありません。 `ref` で返すことができる任意の型を、`ref readonly` で返すことができます。

## <a name="c-language-specification"></a>C# 言語仕様

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

言語仕様の提案を参照することもできます。

- [readonly ref と readonly struct](~/_csharplang/proposals/csharp-7.2/readonly-ref.md)
- [readonly 構造体メンバー](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# プログラミング ガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [修飾子](index.md)
- [const](const.md)
- [フィールド](../../programming-guide/classes-and-structs/fields.md)
