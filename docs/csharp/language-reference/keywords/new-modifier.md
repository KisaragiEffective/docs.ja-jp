---
title: new 修飾子 - C# リファレンス
ms.date: 07/20/2015
helpviewer_keywords:
- new modifier keyword [C#]
ms.assetid: a2e20856-33b9-4620-b535-a60dbce8349b
ms.openlocfilehash: 6c4fedd469efb79f91780dff26da89b586de2d1c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75713342"
---
# <a name="new-modifier-c-reference"></a>new 修飾子 (C# リファレンス)

`new` キーワードを宣言の修飾子として使用すると、基底クラスから継承されたメンバーを明示的に隠ぺいできます。 継承されたメンバーを隠ぺいすると、派生バージョンのメンバーで基底クラスのバージョンが置き換えられます。 `new` 修飾子を使わずにメンバーを隠ぺいすることもできますが、コンパイラ警告が表示されます。 メンバーを明示的に隠ぺいするために `new` を使用する場合は、この警告が抑制されます。

`new` キーワードは、[型のインスタンスの作成](../operators/new-operator.md)に使用することも、または[ジェネリック型制約](./new-constraint.md)として使用することもできます。

継承されたメンバーを隠ぺいするには、派生クラスで同じメンバー名を使用してメンバーを宣言し、`new` キーワードで修飾します。 次に例を示します。

[!code-csharp[csrefKeywordsOperator#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#8)]

この例では、`BaseC.Invoke` は `DerivedC.Invoke` で隠ぺいされます。 `x` フィールドは、似た名前によって隠ぺいされないため、影響を受けません。

継承による名前の隠ぺいは、次のいずれかの形式で行われます。

- 一般的に、定数、フィールド、プロパティ、型をクラスまたは構造体で使用すると、同じ名前を共有するすべての基底クラス メンバーが隠ぺいされます。 次のような特殊な状況があります。 たとえば、呼び出し可能ではない型を持つ `N` という名前の新しいフィールドを宣言し、基本型が `N` をメソッドとして宣言する場合は、新しいフィールドが呼び出し構文内で基本型の宣言を隠ぺいすることはありません。 詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md#member-lookup)の「[メンバー ルックアップ](~/_csharplang/spec/introduction.md)」セクションを参照してください。

- メソッドをクラスまたは構造体で使用すると、基底クラスで同じ名前を共有するプロパティ、フィールド、型が隠ぺいされます。 また、同じシグネチャを持つすべての基底クラス メソッドも隠ぺいされます。

- インデクサーをクラスまたは構造体で使用すると、同じシグネチャを持つすべての基底クラス インデクサーが隠ぺいされます。

`new` と [override](override.md) には相反する意味があるため、同じメンバーにこの 2 つの修飾子を使用するとエラーになります。 `new` 修飾子は、同じ名前で新しいメンバーを作成し、元のメンバーを隠ぺいします。 `override` 修飾子は、継承されたメンバーの実装を拡張します。

宣言で、継承されたメンバーを隠ぺいしない `new` 修飾子を使用すると、警告が出力されます。

## <a name="example"></a>例

この例では、基底クラス `BaseC` と派生クラス `DerivedC` が同じフィールド名 `x` を使用するため、継承されるフィールドの値が隠ぺいされます。 この例では、`new` 修飾子の使い方を示します。 また、基底クラスの隠ぺいされたメンバーに完全修飾名を使ってアクセスする方法も示します。

[!code-csharp[csrefKeywordsOperator#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#9)]

## <a name="example"></a>例

この例では、入れ子になったクラスが、基底クラスにある同名のクラスを隠ぺいします。 この例では、`new` 修飾子を使って警告メッセージが表示されないようにする方法に加えて、完全修飾名を使用してクラスの隠ぺいされたメンバーにアクセスする方法も示します。

[!code-csharp[csrefKeywordsOperator#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csrefKeywordsOperator/CS/csrefKeywordsOperators.cs#10)]

`new` 修飾子を削除すると、プログラムのコンパイルおよび実行は行われますが、次の警告が出力されます。

```text
The keyword new is required on 'MyDerivedC.x' because it hides inherited member 'MyBaseC.x'.
```

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/classes.md#the-new-modifier)の「[new 修飾子](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# リファレンス](../../language-reference/index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [修飾子](index.md)
- [Override キーワードと New キーワードによるバージョン管理](../../programming-guide/classes-and-structs/versioning-with-the-override-and-new-keywords.md)
- [Override キーワードと New キーワードを使用する場合について](../../programming-guide/classes-and-structs/knowing-when-to-use-override-and-new-keywords.md)
