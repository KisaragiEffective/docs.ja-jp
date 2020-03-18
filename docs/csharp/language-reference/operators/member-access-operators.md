---
title: メンバー アクセス演算子 - C# リファレンス
description: 型のメンバーにアクセスするために使用できる C# 演算子について説明します。
ms.date: 09/18/2019
author: pkulikov
f1_keywords:
- ._CSharpKeyword
- '[]_CSharpKeyword'
- ()_CSharpKeyword
- ^_CSharpKeyword
- .._CSharpKeyword
helpviewer_keywords:
- member access operators [C#]
- member access operator [C#]
- dot operator [C#]
- . operator [C#]
- subscript operator [C#]
- square brackets [] operator [C#]
- indexer operator [C#]
- '[] operator [C#]'
- null-conditional operators [C#]
- Elvis operator [C#]
- ?. operator [C#]
- ?[] operator [C#]
- invocation operator [C#]
- method call [C#]
- method invocation [C#]
- delegate invocation [C#]
- () operator [C#]
- ^ operator [C#]
- index from end operator [C#]
- hat operator [C#]
- .. operator [C#]
- range operator [C#]
ms.openlocfilehash: 4d4bc0c192912b5fa87a8e91bc5ba0e1d4ce3598
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398209"
---
# <a name="member-access-operators-c-reference"></a>メンバー アクセス演算子 (C# リファレンス)

型のメンバーにアクセスするときは、次の演算子を使用できます。

- [`.` (メンバー アクセス) ](#member-access-operator-): 名前空間または型のメンバーにアクセスします
- [`[]` (配列要素またはインデクサー アクセス) ](#indexer-operator-): 配列要素または型のインデクサーにアクセスします
- [`?.` および `?[]` (null 条件演算子)](#null-conditional-operators--and-): オペランドが null でない場合にのみ、メンバーまたは要素へのアクセス操作を実行します
- [`()` (呼び出し)](#invocation-operator-): アクセスしたメソッドを呼び出すか、デリゲートを呼び出します
- [`^` (末尾からのインデックス)](#index-from-end-operator-): 要素の位置がシーケンスの末尾からであることを示します
- [`..` (範囲)](#range-operator-): シーケンス要素の範囲を取得するために使用できるインデックスの範囲を指定します

## <a name="member-access-operator-"></a>メンバー アクセス演算子 .

以下の例に示すように、名前空間のメンバーまたは型にアクセスするために `.` トークンを使います。

- 次の [`using` ディレクティブ](../keywords/using-directive.md)の例に示すように、`.` を使って、名前空間内の入れ子になった名前空間にアクセスします。

  [!code-csharp[nested namespaces](snippets/MemberAccessOperators.cs#NestedNamespace)]

- 次のコードに示すように、`.` を使って "*修飾名*" を作成して名前空間内の型にアクセスします。

  [!code-csharp[qualified name](snippets/MemberAccessOperators.cs#QualifiedName)]

  [`using` ディレクティブ](../keywords/using-directive.md)を使い、必要に応じて修飾名を利用します。

- 次のコードに示すように、`.` を使って、[型のメンバー](../../programming-guide/classes-and-structs/index.md#members) (静的および非静的) にアクセスします。

  [!code-csharp-interactive[type members](snippets/MemberAccessOperators.cs#TypeMemberAccess)]

また、`.` を使って[拡張メソッド](../../programming-guide/classes-and-structs/extension-methods.md)にアクセスすることもできます。

## <a name="indexer-operator-"></a>インデクサー演算子 []

通常、角かっこ `[]` は、配列、インデクサー、またはポインター要素へのアクセスに使用されます。

### <a name="array-access"></a>配列へのアクセス

次の例は、配列要素へのアクセス方法を示しています。

[!code-csharp-interactive[array access](snippets/MemberAccessOperators.cs#Arrays)]

配列インデックスが配列の対応するディメンションの範囲に含まれない場合、<xref:System.IndexOutOfRangeException> がスローされます。

前述の例が示すように、配列型の宣言と配列インスタンスのインスタンス化にも角かっこを使用します。

配列の詳細については、「[配列](../../programming-guide/arrays/index.md)」を参照してください。

### <a name="indexer-access"></a>インデクサーへのアクセス

次の例では、インデクサーへのアクセスを示すために .NET <xref:System.Collections.Generic.Dictionary%602> 型を使用します。

[!code-csharp-interactive[indexer access](snippets/MemberAccessOperators.cs#Indexers)]

インデクサーを使用すると、配列のインデックス作成と同様の方法でユーザー定義型のインスタンスのインデックスを作成することができます。 整数である必要がある配列インデックスとは異なり、任意の型を持つインデクサー パラメーターを宣言できます。

インデクサーの詳細については、「[インデクサー](../../programming-guide/indexers/index.md)」を参照してください。

### <a name="other-usages-of-"></a>[] の他の使用方法

ポインター要素へのアクセスの詳細については、[ポインターに関連する演算子](pointer-related-operators.md)に関する記事の「[ポインター要素アクセス演算子 []](pointer-related-operators.md#pointer-element-access-operator-)」セクションを参照してください。

角かっこは、[属性](../../programming-guide/concepts/attributes/index.md)を指定するためにも使用されます。

```csharp
[System.Diagnostics.Conditional("DEBUG")]
void TraceMethod() {}
```

## <a name="null-conditional-operators--and-"></a>Null 条件演算子 ?. および ?[]

C# 6 以降で使用できる Null 条件付き演算子は、そのオペランドが null 以外と評価された場合にのみ、オペランドに[メンバー アクセス](#member-access-operator-)操作 (`?.`) または[要素アクセス](#indexer-operator-)操作 (`?[]`) を適用し、それ以外の場合は、`null` を返します。 つまり、以下のようになります。

- `a` が `null` と評価された場合、`a?.x` または `a?[x]` の結果は `null` です。
- `a` が null 以外と評価された場合、`a?.x` または `a?[x]` の結果は、`a.x` または `a[x]` の結果とそれぞれ同じです。

  > [!NOTE]
  > `a.x` または `a[x]` が例外をスローした場合は、`a?.x` または `a?[x]` が、null 以外の `a` に対して同じ例外をスローします。 たとえば、`a` が null 以外の配列インスタンスで、`x` が `a`の範囲外にある場合、`a?[x]` は <xref:System.IndexOutOfRangeException> をスローします。

Null 条件演算子はショートサーキットです。 つまり、条件付きのメンバーまたは要素アクセス操作のチェーン内にある 1 つの操作から `null` が返された場合、残りのチェーンは実行されません。 次の例では、`A` が `null` と評価されると `B` は評価されず、`A` または `B` が `null` と評価されると `C` は評価されません。

```csharp
A?.B?.Do(C);
A?.B?[C];
```

`?.` および `?[]` 演算子の使用例を次に示します。

[!code-csharp-interactive[null-conditional operators](snippets/MemberAccessOperators.cs#NullConditional)]

前の例では、null 条件演算の結果が `null` の場合に評価する代替の式を指定するために、[null 合体演算子 `??`](null-coalescing-operator.md) も使用しています。

Null 条件メンバー アクセス演算子 `?.` は Elvis 演算子とも呼ばれます。

### <a name="thread-safe-delegate-invocation"></a>スレッドセーフなデリゲートの呼び出し

次のコードに示すように、`?.` 演算子を使用してデリゲートが null 以外かどうかを確認し、それをスレッドセーフな方法で呼び出します (たとえば、[イベントを発生させる](../../../standard/events/how-to-raise-and-consume-events.md)場合)。

```csharp
PropertyChanged?.Invoke(…)
```

このコードは、C# 5 以前で使用する次のコードと同等です。

```csharp
var handler = this.PropertyChanged;
if (handler != null)
{
    handler(…);
}
```

## <a name="invocation-operator-"></a>呼び出し演算子 ()

かっこ `()` は、[メソッド](../../programming-guide/classes-and-structs/methods.md)を呼び出すとき、または[デリゲート](../../programming-guide/delegates/index.md)を呼び出すときに使用します。

メソッドを呼び出す方法 (引数を指定した場合と指定しない場合) とデリゲートを呼び出す方法の例を次に示します。

[!code-csharp-interactive[invocation with ()](snippets/MemberAccessOperators.cs#Invocation)]

かっこは、[`new`](new-operator.md) 演算子を使用して[コンストラクター](../../programming-guide/classes-and-structs/constructors.md)を呼び出すときにも使用します。

### <a name="other-usages-of-"></a>() の他の使用方法

式に含まれる演算を評価する順序を調整する場合にもかっこを使用します。 詳細については、[C# 演算子](index.md)に関するページを参照してください。

明示的な型変換を実行する[キャスト式](type-testing-and-cast.md#cast-operator-)でも、かっこが使われます。

## <a name="index-from-end-operator-"></a>末尾からのインデックス演算子 ^

`^` 演算子は C# 8.0 以降で使用することができ、要素の位置がシーケンスの末尾からであることを示します。 長さが `length` のシーケンスの場合、`^n` は、シーケンスの先頭からのオフセットが `length - n` である要素を指します。 たとえば、`^1` は、シーケンスの最後の要素を指し、`^length` は、シーケンスの最初の要素を指します。

[!code-csharp[index from end](snippets/MemberAccessOperators.cs#IndexFromEnd)]

前の例で示すように、式 `^e` は <xref:System.Index?displayProperty=nameWithType> 型です。 式 `^e`で、`e` の結果は `int` に暗黙に変換される必要があります。

さらに、`^` 演算子を[範囲演算子 ](#range-operator-) と組み合わせて使用してインデックスの範囲を作成することもできます。 詳細については、「[インデックスと範囲](../../tutorials/ranges-indexes.md)」を参照してください。

## <a name="range-operator-"></a>範囲演算子 .

`..` 演算子は C# 8.0 以降で使用することができ、インデックスの範囲の先頭と末尾をオペランドとして指定します。 左側のオペランドは "*包含的*" で、範囲の先頭を含みます。 右側のオペランドは "*排他的*" で、範囲の末尾を含みません。 次の例で示すように、どちらのオペランドであっても、シーケンスの先頭または末尾からのインデックスとすることができます。

[!code-csharp[range examples](snippets/MemberAccessOperators.cs#Ranges)]

前の例で示すように、式 `a..b` は <xref:System.Range?displayProperty=nameWithType> 型です。 式 `a..b` で、`a` および `b` の結果は暗黙に `int` または <xref:System.Index> に変換される必要があります。

`..` 演算子のオペランドのいずれかを省略して、変更可能な範囲を取得することができます。

- `a..` は `a..^0` と同じです。
- `..b` は `0..b` と同じです。
- `..` は `0..^0` と同じです。

[!code-csharp[ranges with omitted operands](snippets/MemberAccessOperators.cs#RangesOptional)]

詳細については、「[インデックスと範囲](../../tutorials/ranges-indexes.md)」を参照してください。

## <a name="operator-overloadability"></a>演算子のオーバーロード可/不可

`.`、`()`、`^`、および `..` の各演算子はオーバーロードできません。 `[]` 演算子も、オーバーロードできない演算子と見なされます。 ユーザー定義型を使用したインデックス作成をサポートするには、[インデクサー](../../programming-guide/indexers/index.md)を使用してください。

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、「[C# 言語仕様](~/_csharplang/spec/introduction.md)」の次のセクションを参照してください。

- [メンバー アクセス](~/_csharplang/spec/expressions.md#member-access)
- [要素アクセス](~/_csharplang/spec/expressions.md#element-access)
- [NULL 条件演算子](~/_csharplang/spec/expressions.md#null-conditional-operator)
- [呼び出し式](~/_csharplang/spec/expressions.md#invocation-expressions)

インデックスと範囲について詳しくは、[機能提案メモ](~/_csharplang/proposals/csharp-8.0/ranges.md)を参照してください。

## <a name="see-also"></a>関連項目

- [C# リファレンス](../index.md)
- [C# 演算子](index.md)
- [?? (Null 合体演算子)](null-coalescing-operator.md)
- [:: 演算子](namespace-alias-qualifier.md)
