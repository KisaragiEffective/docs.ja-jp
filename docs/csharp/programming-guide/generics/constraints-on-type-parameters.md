---
title: 型パラメーターの制約 - C# プログラミング ガイド
ms.date: 04/12/2018
helpviewer_keywords:
- generics [C#], type constraints
- type constraints [C#]
- type parameters [C#], constraints
- unbound type parameter [C#]
ms.openlocfilehash: 76cd00b9c84f128d2a181115293df910d8deb6cb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398407"
---
# <a name="constraints-on-type-parameters-c-programming-guide"></a>型パラメーターの制約 (C# プログラミング ガイド)

制約では、型引数に必要な機能をコンパイラに通知します。 制約のない型引数は、任意の型にすることができます。 コンパイラは、.NET 型の最終的な基底クラスになる、<xref:System.Object?displayProperty=nameWithType> のメンバーを見なすことができるだけです。 詳細については、「[制約を使用する理由](#why-use-constraints)」を参照してください。 クライアント コードで、この制限で許可されていない型を使用してクラスをインスタンス化しようとすると、コンパイル時エラーが発生します。 制約を指定するには、`where` コンテキスト キーワードを使用します。 次の表では、7 種類の制約を一覧しています。

|制約|[説明]|
|----------------|-----------------|
|`where T : struct`|この型引数は null 非許容値型である必要があります。 null 許容値型の詳細については、「[null 許容値型](../../language-reference/builtin-types/nullable-value-types.md)」を参照してください。 すべての値の型にはアクセス可能なパラメーターなしのコンストラクターがあるため、`struct` 制約は `new()` 制約を意味し、`new()` 制約と組み合わせることはできません。 また、`struct` 制約を `unmanaged` 制約と組み合わせることもできません。|
|`where T : class`|この型引数は参照型である必要があります。 この制約は、任意のクラス、インターフェイス、デリゲート、または配列型にも適用されます。|
|`where T : notnull`|この型引数は null 非許容型である必要があります。 引数は、C# 8.0 以降では null 非許容参照型、または null 非許容値型にできます。 この制約は、任意のクラス、インターフェイス、デリゲート、または配列型にも適用されます。|
|`where T : unmanaged`|この型引数は null 非許容で[アンマネージド型](../../language-reference/builtin-types/unmanaged-types.md)である必要があります。 `unmanaged` 制約は `struct` 制約を意味し、`struct` 制約とも `new()` 制約とも組み合わせることはできません。|
|`where T : new()`|この型引数には、パラメーターなしのパブリック コンストラクターが必要です。 `new()` 制約を別の制約と併用する場合、この制約を最後に指定する必要があります。 `new()` 制約は、`struct` や `unmanaged` 制約と組み合わせることはできません。|
|`where T :` *\<base class name>*|この型引数は、指定された基底クラスであるか、そのクラスから派生している必要があります。|
|`where T :` *\<interface name>*|この型引数は、指定されたインターフェイスであるか、そのインターフェイスを実装している必要があります。 複数のインターフェイス制約を指定することができます。 制約のインターフェイスを汎用的にすることもできます。|
|`where T : U`|T に指定する型引数は、U に指定された引数であるか、その引数から派生している必要があります。|

## <a name="why-use-constraints"></a>制約を使用する理由

型パラメーターを制約すると、使用できる操作とメソッド呼び出しの数は、制約する型、およびその継承階層内のすべての型でサポートされている数まで増えます。 ジェネリック クラスまたはメソッドを設計するときに、単純な割り当てや、<xref:System.Object?displayProperty=nameWithType> でサポートされていない任意のメソッド呼び出しでジェネリック メンバーに対して任意の操作を実行する場合、型パラメーターに制約を適用する必要があります。 たとえば、この基底クラスの制約は、この型のオブジェクト、またはこの型から派生したオブジェクトのみを型引数として使用することをコンパイラに指示しています。 コンパイラがこの保証を獲得したら、その型のメソッドをジェネリック クラスで呼び出すことができるようになります。 基底クラスの制約を適用して `GenericList<T>` クラス (「[ジェネリックの概要](../../../standard/generics/index.md)」を参照) に追加できる機能を説明するコード例を次に示します。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#9)]

この制約ではジェネリック クラスで `Employee.Name` プロパティを使用できるようにします。 制約では、型 `T` のすべての項目が、`Employee` オブジェクトまたは `Employee` から継承するオブジェクトのいずれかになることが保証されることを指定します。

同じ型パラメーターに複数の制約を適用できます。また、制約自体をジェネリック型にすることもできます。次に例を示します。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#10)]

`where T : class` 制約を適用する場合は、型パラメーターに `==` および `!=` 演算子を使用しないでください。これらの演算子でテストされるのは、値の等価性ではなく、参照 ID についてのみです。 これらの演算子が、引数として使用されている型内でオーバーロードされている場合でも、この動作が発生します。 この点を説明するコードを次に示します。<xref:System.String> クラスが `==` 演算子をオーバーロードしている場合でも、出力は false です。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#11)]

コンパイラは `T` がコンパイル時に参照型であることしか認識しておらず、すべての参照型で有効な既定の演算子を使用する必要があります。 値の等価性をテストする必要がある場合は、`where T : IEquatable<T>` または `where T : IComparable<T>` 制約も適用し、ジェネリック クラスの制約に使用されるすべてのクラスでそのインターフェイスを実装することをお勧めします。

## <a name="constraining-multiple-parameters"></a>複数のパラメーターを制約する

複数のパラメーターに制約を適用できます。また、複数の制約を 1 つのパラメーターに適用することができます。次に例を示します。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#12)]

## <a name="unbounded-type-parameters"></a>非バインド型パラメーター

 パブリック クラス `SampleClass<T>{}` の T など、制約がない型パラメーターは、非バインド型パラメーターと呼ばれます。 非バインド型パラメーターには次の規則があります。

- `!=` および `==` 演算子は使用できません。これは、具象型引数によってこれらの演算子がサポートされるという保証がないためです。
- これらの演算子は `System.Object` との間で相互に変換できます。また、任意のインターフェイス型に明示的に変換できます。
- これらは [null](../../language-reference/keywords/null.md) と比較することができます。 非バインド型パラメーターと `null` を比較し、その型引数が値の型の場合、比較結果として常に false が返されます。

## <a name="type-parameters-as-constraints"></a>制約としての型パラメーター

制約としてジェネリック型パラメーターを使用する方法は、独自の型パラメーターがあるメンバー関数が、含まれる型の型パラメーターにそのパラメーターを制約する必要がある場合に便利です。次に例を示します。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#13)]

前の例の `T` は、`Add` メソッドのコンテキストでは型の制約ですが、`List` クラスのコンテキストでは非バインド型パラメーターです。

型パラメーターは、ジェネリック クラス定義の制約としても使用できます。 型パラメーターは、他の型パラメーターと共に山かっこ内で宣言する必要があります。

[!code-csharp[using the class and struct constraints](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#14)]

ジェネリック クラスで制約として型パラメーターを使用する方法が便利なのは、限られた場合のみです。コンパイラでは、`System.Object` から派生したことを除き、型パラメーターに関して何も仮定できないためです。 2 つの型パラメーター間に継承関係を適用するシナリオには、ジェネリック クラスの制約として型パラメーターを使用してください。

## <a name="notnull-constraint"></a>NotNull 制約

C# 8.0 以降では、`notnull` 制約を使用して、型引数が null 非許容値型または null 非許容参照型である必要があることを指定できます。 `notnull` 制約は、`nullable enable` コンテキストでのみ使用できます。 null 許容が未指定のコンテキストに `notnull` 制約を追加すると、コンパイラにより警告が生成されます。

他の制約とは異なり、型引数が `notnull` 制約に違反すると、そのコードが `nullable enable` コンテキストでコンパイルされるときにコンパイラにより警告が生成されます。 null 許容が未指定のコンテキストでコードがコンパイルされた場合、コンパイラによって警告やエラーは生成されません。

## <a name="unmanaged-constraint"></a>アンマネージド制約

C# 7.3 以降、`unmanaged` 制約を指定して、型パラメーターが null 非許容で[アンマネージド型](../../language-reference/builtin-types/unmanaged-types.md)である必要があることを指定できます。 `unmanaged` 制約では、次の例のように、メモリのブロックとして操作できる型を処理する再利用可能なルーチンを記述できます。

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#15)]

ビルトイン型ではない型で `unsafe` 演算子を使用するため、先行するメソッドは `sizeof` コンテキストでコンパイルされる必要があります。 `unmanaged` 制約なしで、`sizeof` 演算子を使用することはできません。

`unmanaged` 制約は `struct` 制約を意味するため、これと組み合わせることはできません。 `struct` 制約は `new()` 制約を意味するため、`unmanaged` 制約を `new()` 制約と組み合わせることもできません。

## <a name="delegate-constraints"></a>制約をデリゲートする

また、C# 7.3 以降、基底クラスの制約として <xref:System.Delegate?displayProperty=nameWithType> または <xref:System.MulticastDelegate?displayProperty=nameWithType> を使用することもできます。 CLR では常にこの制約を許可していますが、C# 言語では許可されていません。 `System.Delegate` 制約では、タイプ セーフな方法でデリゲートを処理するコードを記述できます。 次のコードでは、2 つのデリゲートが同じ型である場合にそれらを組み合わせる拡張メソッドを定義します。

[!code-csharp[using the delegate constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#16)]

上述のメソッドを使用して、同じ型のデリゲートを組み合わせることができます。

[!code-csharp[using the unmanaged constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#17)]

最後の行のコメントを解除した場合、コンパイルされません。 `first` と `test` は両方ともデリゲート型ですが、これらは異なるデリゲート型です。

## <a name="enum-constraints"></a>列挙の制約

C# 7.3 以降、基底クラスの制約として <xref:System.Enum?displayProperty=nameWithType> 型を指定することもできます。 CLR では常にこの制約を許可していますが、C# 言語では許可されていません。 `System.Enum` を使用するジェネリックは、`System.Enum` の静的メソッドの使用から結果をキャッシュするために、タイプ セーフのプログラミングを提供します。 次の例では、列挙型の有効な値をすべて見つけて、それらの値をその文字列表記にマップするディクショナリをビルドします。

[!code-csharp[using the enum constraint](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#18)]

メソッドはリフレクションを使用していました。これはパフォーマンスに影響します。 このメソッドを呼び出して、リフレクションを必要とする呼び出しを繰り返すことなく、キャッシュおよび再利用されるコレクションをビルドできます。

次の例で示すように、このメソッドを使用して、列挙を作成し、その値と名前のディクショナリをビルドできます。

[!code-csharp[enum definition](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#19)]

[!code-csharp[using the enum constrained method](~/samples/snippets/csharp/keywords/GenericWhereConstraints.cs#20)]

## <a name="see-also"></a>参照

- <xref:System.Collections.Generic>
- [C# プログラミングガイド](../index.md)
- [ジェネリックの概要](./index.md)
- [ジェネリック クラス](./generic-classes.md)
- [new 制約](../../language-reference/keywords/new-constraint.md)
