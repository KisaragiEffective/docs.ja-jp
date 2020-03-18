---
title: 式 - C# プログラミング ガイド
ms.date: 05/11/2017
helpviewer_keywords:
- expressions [C#]
- C# language, expressions
ms.assetid: c7d8feb0-0e58-4f94-8bf6-4d070550a832
ms.openlocfilehash: 4bbee8f15c2591e8b172df9a6759449d48697804
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75699094"
---
# <a name="expressions-c-programming-guide"></a>式 (C# プログラミング ガイド)

"*式*" とは、1 つの値、オブジェクト、メソッド、または名前空間に評価できる、1 つ以上のオペランドと 0 個以上の[演算子](../../language-reference/operators/index.md)のシーケンスです。 式には、リテラル値、メソッドの呼び出し、演算子とそのオペランド、または*簡易名*を含めることができます。 単純な名前には、変数、型メンバー、メソッド パラメーター、名前空間、または型の名前を指定できます。  
  
 式では、他の式をパラメーターとして使用する演算子、またはパラメーターが他のメソッド呼び出しであるメソッド呼び出しを使用できます。そのため、式には単純なものもあれば、非常に複雑なものもあります。 次に式の例を 2 つ示します。  
  
```csharp  
((x < 10) && ( x > 5)) || ((x > 20) && (x < 25));

System.Convert.ToInt32("35");  
```  
  
## <a name="expression-values"></a>式の値

 ステートメントやメソッド パラメーターなど、式が使用されるコンテキストの大部分では、式はなんらかの値に評価されることが期待されます。 たとえば、x と y が整数である場合、式 `x + y` は数値に評価されます。 式 `new MyClass()` は、`MyClass` クラスの新しいインスタンスへの参照に評価されます。 式 `myClass.ToString()` は、メソッドの戻り値の型である文字列に評価されます。 ただし、名前空間名については、分類上は式として扱われますが、値には評価されないため、式の最終結果になることはありません。 名前空間名は、メソッド パラメーターに渡すことはできません。また、新しい式で使用したり、変数に割り当てたりすることもできません。 名前空間名は、より大きい式の部分式としてのみ使用できます。 同じことが、型 (<xref:System.Type?displayProperty=nameWithType> オブジェクトとは異なります)、メソッド グループ名 (特定のメソッドとは異なります)、およびイベント アクセサーである [add](../../language-reference/keywords/add.md) と [remove](../../language-reference/keywords/remove.md) にも当てはまります。  
  
 すべての値には、型が関連付けられています。 たとえば、x と y が両方とも型 `int` の変数である場合、式 `x + y` の値も `int` として型指定されます。 異なる型の変数に値が割り当てられた場合や、x と y の型が異なる場合は、型変換の規則が適用されます。 このような変換の動作方法の詳細については、「[キャストと型変換](../types/casting-and-type-conversions.md)」を参照してください。  
  
## <a name="overflows"></a>オーバーフロー

 値の型の最大値よりも値が大きくなると、数式でオーバーフローが発生することがあります。 詳細については、[Checked と Unchecked](../../language-reference/keywords/checked-and-unchecked.md) に関する記事と[組み込みの数値変換](../../language-reference/builtin-types/numeric-conversions.md#explicit-numeric-conversions)に関する記事の「[明示的な数値変換](../../language-reference/builtin-types/numeric-conversions.md)」セクションを参照してください。
  
## <a name="operator-precedence-and-associativity"></a>演算子の優先順位と結合規則

 式の評価方法は、結合規則と演算子の優先順位によって決まります。 詳細については、「 [オペレーター](../../language-reference/operators/index.md)」を参照してください。  
  
 代入式とメソッドの呼び出し式を除き、ほとんどの式はステートメントに埋め込む必要があります。 詳細については、「[ステートメント](./statements.md)」を参照してください。  
  
## <a name="literals-and-simple-names"></a>リテラルと簡易名

 式の中で最も単純なものはリテラルと簡易名です。 リテラルは、名前を持たない定数値です。 たとえば、次のコード例の `5` と `"Hello World"` は共にリテラル値です。  
  
 [!code-csharp[csProgGuideStatements#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#2)]  
  
 リテラルの詳細については、「[型](/dotnet/csharp/language-reference/keywords)」を参照してください。  
  
 前の例の `i` と `s` は、どちらもローカル変数を識別する簡易名です。 これらの変数を式で使用すると、変数名は、変数のメモリ位置に現在格納されている値に評価されます。 以下の例を参照してください。  
  
 [!code-csharp[csProgGuideStatements#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideStatements/CS/Statements.cs#3)]

## <a name="invocation-expressions"></a>Invocation 式

 次のコード例では、`DoWork` の呼び出しが Invocation 式です。  
  
```csharp
DoWork();  
```  
  
 メソッドの呼び出しでは、メソッドの名前 (上の例のような名前、または別の式の結果としての名前) を使用し、その後にかっことメソッド パラメーターを記述する必要があります。 詳細については、「[メソッド](../classes-and-structs/methods.md)」を参照してください。 デリゲートの呼び出しでは、デリゲートの名前とメソッド パラメーターをかっこで囲んで使用します。 詳細については、「 [デリゲート](../delegates/index.md)で定義されているインターフェイスのプライベート C++ 固有の実装です。 メソッドの呼び出しとデリゲートの呼び出しは、メソッドが値を返す場合、メソッドの戻り値に評価されます。 void を返すメソッドは、式の値の代わりに使用できません。  

## <a name="query-expressions"></a>クエリ式

 一般的な式の規則と同じ規則がクエリ式に適用されます。 詳細については、[LINQ](../../linq/index.md)に関するページを参照してください。  
  
## <a name="lambda-expressions"></a>ラムダ式

 ラムダ式は、名前がないが入力パラメーターおよび複数のステートメントを持つことができる "インライン メソッド" を表します。 LINQ では、メソッドに引数を渡すために広く使用されています。 ラムダ式は、使用されるコンテキストに応じて、デリゲートまたは式ツリーにコンパイルされます。 詳しくは、「[ラムダ式](lambda-expressions.md)」をご覧ください。  
  
## <a name="expression-trees"></a>式ツリー

式ツリーを使用すると、式をデータ構造体として表すことができます。 クエリ式を、SQL データベースなどの他のコンテキストで意味を持つコードに変換するために、LINQ プロバイダーにより広く使用されています。 詳しくは、「[式ツリー (C#)](../concepts/expression-trees/index.md)」をご覧ください。
  
## <a name="expression-body-definitions"></a>式本体の定義

C# は*式形式のメンバー*をサポートしています。式形式のメンバーを使用すると、メソッド、コンストラクター、ファイナライザー、プロパティ、およびインデクサー用に簡潔な式本体の定義を指定できます。 詳細については、「[式形式のメンバー](expression-bodied-members.md)」を参照してください。

## <a name="remarks"></a>解説

 変数、オブジェクト プロパティ、またはオブジェクトのインデクサー アクセスが式から識別されると、その項目の値が式の値として使用されます。 C# の式は、式が最終的に必要な型に評価される限り、値やオブジェクトが必要とされる任意の位置に配置できます。  

## <a name="c-language-specification"></a>C# 言語仕様

詳細については、[C# 言語仕様](~/_csharplang/spec/expressions.md)の「[式](~/_csharplang/spec/introduction.md)」セクションを参照してください。

## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [オペレーター](../../language-reference/operators/index.md)
- [メソッド](../classes-and-structs/methods.md)
- [デリゲート](../delegates/index.md)
- [型](../types/index.md)
- [LINQ](../../linq/index.md)
