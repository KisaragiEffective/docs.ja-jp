---
title: in パラメーター修飾子 - C# リファレンス
ms.date: 03/26/2019
helpviewer_keywords:
- parameters [C#], in
- in parameters [C#]
ms.openlocfilehash: cbde7a571fb71ed7577077c77a5c61db553ec859
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173615"
---
# <a name="in-parameter-modifier-c-reference"></a>in パラメーター修飾子 (C# リファレンス)

`in` キーワードによって、参照により引数が渡されます。 仮パラメーターを引数 (変数にする必要があります) の別名にします。 つまり、パラメーターに対するすべての操作は引数に対して行われます。 これは、[ref](ref.md) または [out](out-parameter-modifier.md) キーワードと似ています。ただし、呼び出されたメソッドで `in` 引数を変更することはできません。 `ref` 引数には変更が許される一方で、`out` 引数の場合、呼び出されたメソッドによって変更される必要があります。そのような変更は、呼び出し元のコンテキストで観察できます。

[!code-csharp-interactive[cs-in-keyword](../../../../samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/InParameterModifier.cs#1)]  

前の例は、通常、`in` 修飾子が呼び出しサイトでは不要であることを示しています。 これはメソッド宣言でのみ必要です。

> [!NOTE]
> `in` ステートメントの一部、または LINQ クエリの `foreach` 句の一部として、型パラメーターが反変であることを示すために `join` キーワードをジェネリック型パラメーターで使用することもできます。 これらのコンテキストでの `in` キーワードの使用の詳細については、これらすべての使用に関するリンクを提供する「[in](in.md)」を参照してください。
  
`in` 引数として渡される変数は、メソッド呼び出しで渡される前に初期化する必要があります。 ただし、呼び出されたメソッドでは値の割り当てや、引数の変更を行うことはできません。  

`in` パラメーター修飾子は C# 7.2 以降で使用可能です。 以前のバージョンでは、コンパイラ エラー `CS8107` ("Feature 'readonly references' is not available in C# 7.0. Please use language version 7.2 or greater." ('読み取り専用参照' 機能は C# 7.0 では使用できません。7.2 以上の言語バージョンを使用してください)) が生成されました。コンパイラ言語のバージョンを構成するには、「[C# 言語のバージョンの選択](../configure-language-version.md)」をご覧ください。

`in`、`ref`、および `out` キーワードは、オーバーロード解決のためのメソッド シグネチャの一部とは見なされません。 したがって、唯一の違いが、1 つのメソッドは `ref` または `in` 引数を受け取り、もう一方のメソッドは `out` 引数を受け取ることである場合、メソッドはオーバーロードできません。 たとえば、次のコードはコンパイルされません。  
  
```csharp
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on in, ref and out".
    public void SampleMethod(in int i) { }
    public void SampleMethod(ref int i) { }
}
```
  
`in` の有無に基づくオーバーロードが可能です。  
  
```csharp
class InOverloads
{
    public void SampleMethod(in int i) { }
    public void SampleMethod(int i) { }
}
```

## <a name="overload-resolution-rules"></a>オーバーロードの解決ルール

`in` 引数の意図を理解して、値と `in` 引数の比較によって、メソッドに対するオーバーロードの解決ルールを理解できます。 `in` パラメーターを使用してメソッドを定義すると、パフォーマンスを最適化できる可能性があります。 一部の `struct` 型引数は、サイズが大きくなる可能性があり、メソッドが短いループまたは重要なコード パスで呼び出される場合、その構造のコピーのコストが重要になります。 呼び出されたメソッドは引数の状態を変更しないため、メソッドで `in` パラメーターを宣言して、参照で安全に渡すことができる引数を指定します。 参照によりこれらの引数を渡して、(可能性がある) 高額なコピーを回避します。

呼び出しサイトで引数に `in` を指定することは、通常は省略可能です。 値で引数を渡す場合と `in` 修飾子を使用して参照で渡す場合の間に、セマンティックの相違点はありません。 引数の値が変更される可能性があることを示す必要はないため、呼び出しサイトの `in` 修飾子は省略可能です。 呼び出しサイトで `in` 修飾子を明示的に追加して、引数が値ではなく、参照で渡されるようにします。 明示的に `in` を使用すると、次の 2 つの効果があります。

1 つ目は、呼び出しサイトで `in` を指定すると、一致する `in` パラメーターで定義されたメソッドがコンパイラで強制的に選択されます。 それ以外の場合は、2 つのメソッドで `in` の有無のみが異なるときは、値によるオーバーロードの方が適しています。

2 つ目は、`in` を指定して、参照で引数を渡す意図を宣言します。 `in` で使用される引数では、直接参照できる場所を表す必要があります。 `out` および `ref` 引数と同じ一般ルールが適用されます。定数、通常のプロパティ、または値を生成するその他の式を使用することはできません。 それ以外の場合は、呼び出しサイトで `in` を省略すると、メソッドに読み取り専用の参照で渡すために、一時変数の作成を許可することが、コンパイラに通知されます。 コンパイラでは、一時変数を作成して、`in` 引数でのいくつかの制限に対処します。

- 一時変数では、`in` パラメーターとしてコンパイル時の定数を許可します。
- 一時変数では、プロパティ、または `in` パラメーターのその他の式を許可します。
- 一時変数では、引数型からパラメーターの型への暗黙の変換がある引数を許可します。

先のすべてのインスタンスで、コンパイラは定数、プロパティ、またはその他の式の値を格納する一時変数を作成します。

これらのルールが発生するコード例を次に示します。

```csharp
static void Method(in int argument)
{
    // implementation removed
}

Method(5); // OK, temporary variable created.
Method(5L); // CS1503: no implicit conversion from long to int
short s = 0;
Method(s); // OK, temporary int created with the value 0
Method(in s); // CS1503: cannot convert from in short to in int
int i = 42;
Method(i); // passed by readonly reference
Method(in i); // passed by readonly reference, explicitly using `in`
```

ここでは、値の引数で使用する別のメソッドが使用できたと想定します。 次のコードに示すように、結果が変更されます。

```csharp
static void Method(int argument)
{
    // implementation removed
}

static void Method(in int argument)
{
    // implementation removed
}

Method(5); // Calls overload passed by value
Method(5L); // CS1503: no implicit conversion from long to int
short s = 0;
Method(s); // Calls overload passed by value.
Method(in s); // CS1503: cannot convert from in short to in int
int i = 42;
Method(i); // Calls overload passed by value
Method(in i); // passed by readonly reference, explicitly using `in`
```

引数が参照で渡されるメソッドの呼び出しは、最後のメソッドのみです。

> [!NOTE]
> 先のコードでは、わかりやすくするために `int` を引数型として使用します。 `int` は最新のマシンの参照より大きくなることはないため、読み取り専用の参照として単一の `int` を渡すメリットはありません。

## <a name="limitations-on-in-parameters"></a>`in` パラメーターの制限

次の種類のメソッドには、`in`、`ref`、`out` キーワードを使用することはできません。  
  
- [async](async.md) 修飾子を使用して定義した Async メソッド。  
- [yield return](yield.md) または `yield break` ステートメントを含む Iterator メソッド。  

## <a name="c-language-specification"></a>C# 言語仕様  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [C# リファレンス](../index.md)
- [C# プログラミングガイド](../../programming-guide/index.md)
- [C# のキーワード](index.md)
- [メソッド パラメーター](method-parameters.md)
- [安全で効率的なコードを記述する](../../write-safe-efficient-code.md)
