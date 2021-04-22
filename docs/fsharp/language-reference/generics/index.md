---
title: ジェネリック
description: F# のジェネリック関数とジェネリック型を使用する方法を説明します。これらを使用すると、コードを繰り返し記述しなくても、さまざまな型で動作するコードを記述できます。
ms.date: 05/16/2016
ms.openlocfilehash: 5d6e57762095e44836425f90d21a6c1dc71edaaa
ms.sourcegitcommit: cdf67135a98a5a51913dacddb58e004a3c867802
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/21/2019
ms.locfileid: "69666704"
---
# <a name="generics"></a>ジェネリック

F# の関数値、メソッド、プロパティ、および集約型 (クラス、レコード、判別共用体など) は、*ジェネリック* にすることができます。 ジェネリック コンストラクトには、少なくとも 1 つの型パラメーターが含まれます。この型は、通常、ジェネリック コンストラクトのユーザーによって指定されます。 ジェネリック関数とジェネリック型を使用すると、型ごとにコードを繰り返し記述しなくても、さまざまな型で動作するコードを記述できます。 多くの場合、F# のコードは、コンパイラの型推論と自動ジェネリック化メカニズムによって、暗黙的にジェネリックとして推論されるため、F# では、コードを簡単にジェネリックにできる可能性があります。

## <a name="syntax"></a>構文

```fsharp
// Explicitly generic function.
let function-name<type-parameters> parameter-list =
function-body

// Explicitly generic method.
[ static ] member object-identifer.method-name<type-parameters> parameter-list [ return-type ] =
method-body

// Explicitly generic class, record, interface, structure,
// or discriminated union.
type type-name<type-parameters> type-definition
```

## <a name="remarks"></a>解説

明示的なジェネリック関数やジェネリック型の宣言は、非ジェネリックの関数や型の宣言によく似ていますが、型パラメーターの指定 (および使用) 方法が異なり、型パラメーターは、関数名または型名の後ろに山かっこで囲んで指定します。

宣言は、多くの場合、暗黙的にジェネリックになります。 関数または型の作成に使用されるすべてのパラメーターの型が完全に指定されていない場合、コンパイラは、記述されたコードから、それぞれのパラメーター、値、および変数の型を推論しようと試みます。 詳細については、「[Type Inference](../type-inference.md)」を参照してください。 型や関数のコードによってパラメーターの型が特に制約されない場合、その関数または型は暗黙的にジェネリックとなります。 このプロセスは、*自動ジェネリック化* と呼ばれます。 自動ジェネリック化にいくつかの制限があります。 たとえば、F# コンパイラがジェネリック コンストラクトの型を推論できない場合、コンパイラは、*値の制限* と呼ばれる制約を示すエラーを報告します。 この場合、型の注釈の追加が必要になることがあります。 自動ジェネリック化と値の制限の詳細、およびコードを変更して問題に対処する方法については、「[自動ジェネリック化](automatic-generalization.md)」を参照してください。

前の構文の *type-parameters* は、未知の型を表すパラメーターのコンマ区切りのリストです。それぞれは単一引用符から開始し、必要に応じて、その型パラメーターで使用できる型をさらに制限する制約句を指定します。 制約句の構文および制約に関するその他の情報については、「[制約](constraints.md)」を参照してください。

構文の *type-definition* は、非ジェネリック型の型定義と同じです。 これには、クラス型のコンストラクター パラメーター、オプションの `as` 句、等号、レコード フィールド、`inherit` 句、判別共用体の選択、`let` バインドと `do` バインド、メンバー定義、および非ジェネリック型定義で許可されている他の任意の記述が含まれます。

その他の構文要素は、非ジェネリック関数および非ジェネリック型と同じです。 たとえば、*object-identifier* は、それを含むオブジェクト自体を表す識別子です。

プロパティ、フィールド、およびコンストラクターは、それを囲む型よりもジェネリックにすることはできません。 また、モジュール内の値をジェネリックにすることもできません。

## <a name="implicitly-generic-constructs"></a>暗黙的なジェネリック コンストラクト

F# コンパイラがコード内の型を推論するとき、ジェネリックにできる関数はすべて自動的にジェネリックとして扱われます。 パラメーター型など、型を明示的に指定すると、自動ジェネリック化を回避できます。

次のコード例では、`makeList` とそのパラメーターはいずれもジェネリックとして明示的に宣言されていませんが、makeList はジェネリックになります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1700.fs)]

関数のシグネチャは、`'a -> 'a -> 'a list` と推論されます。 この例で、`a` と `b` は同じ型と推論されることに注意してください。 これは、いずれも同じリストに含まれており、1 つのリスト内の要素はすべて同じ型でなければならないためです。

関数をジェネリックにするには、型の注釈で単一引用符の構文を使用して、パラメーター型がジェネリック型パラメーターであることを示す方法もあります。 次のコードでは、この方法でパラメーターが型パラメーターとして宣言されているため、`function1` はジェネリックになります。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1701.fs)]

## <a name="explicitly-generic-constructs"></a>明示的なジェネリック コンストラクト

山かっこ (`<type-parameter>`) 内で型パラメーターを明示的に宣言することで、関数をジェネリックにすることもできます。 次に例を示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1703.fs)]

## <a name="using-generic-constructs"></a>ジェネリック コンストラクトの使用

ジェネリック関数またはジェネリック メソッドを使用する場合、型引数を指定しなくてもよいことがあります。 コンパイラは、型推論を使用して適切な型引数を推論します。 あいまいさが残っている場合は、山かっこ内に型引数を指定できます。複数の型引数を指定する場合はコンマで区切ります。

次のコードでは、前のセクションで説明した関数の使用方法を示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1702.fs)]

> [!NOTE]
> 名前でジェネリック型を参照するには、2 つの方法があります。 たとえば、1 つの型引数 `list` を持つジェネリック型 `int` を参照する方法として、`list<int>` と `int list` の 2 つがあります 後者の形式は、通常、`list` や `option` などの F# の組み込み型でのみ使用されます。 複数の型引数がある場合、通常は `Dictionary<int, string>` 構文を使用しますが、`(int, string) Dictionary` 構文を使用することもできます。

## <a name="wildcards-as-type-arguments"></a>型引数としてのワイルドカード

引数がコンパイラによって推論される必要があることを指定するには、名前付き型引数の代わりに、アンダースコアまたはワイルドカード記号 (`_`) を使用できます。 これを次のコードに示します。

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1704.fs)]

## <a name="constraints-in-generic-types-and-functions"></a>ジェネリック型とジェネリック関数の制約

ジェネリック型またはジェネリック関数の定義では、ジェネリック型パラメーターで使用できることがわかっているコンストラクトのみを使用できます。 この制限は、コンパイル時に関数呼び出しやメソッド呼び出しを検証できるようにするために必要です。 型パラメーターを明示的に宣言する場合は、ジェネリック型パラメーターに明示的な制約を適用して、特定のメソッドおよび関数を使用できることをコンパイラに通知できます。 ただし、F# コンパイラがジェネリック パラメーターの型を推論できるようにすると、コンパイラが自動的に適切な制約を決定します。 詳細については、「[制約](constraints.md)」を参照してください。

## <a name="statically-resolved-type-parameters"></a>静的に解決される型パラメーター

F# プログラムで使用できる型パラメーターには、2 つの種類があります。 1 つ目は、前のセクションで説明した種類のジェネリック型パラメーターです。 1 つ目の種類の型パラメーターは、Visual Basic や C# などの言語で使用されるジェネリック型パラメーターと同等です。 もう 1 つの種類の型パラメーターは F# に固有のもので、*静的に解決される型パラメーター* と呼ばれます。 これらのコンストラクトの詳細については、「[Statically Resolved Type Parameters](statically-resolved-type-parameters.md)」を参照してください。

## <a name="examples"></a>例

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1705.fs)]

## <a name="see-also"></a>関連項目

- [言語リファレンス](../index.md)
- [型](../fsharp-types.md)
- [静的に解決される型パラメーター](statically-resolved-type-parameters.md)
- [ジェネリック](../../../standard/generics/index.md)
- [自動ジェネリック化](automatic-generalization.md)
- [制約](constraints.md)
