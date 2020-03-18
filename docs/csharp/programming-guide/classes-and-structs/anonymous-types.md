---
title: 匿名型 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- anonymous types [C#]
- C# Language, anonymous types
ms.assetid: 59c9d7a4-3b0e-475e-b620-0ab86c088e9b
ms.openlocfilehash: 63bc5560ba19ff36764465a6b89b81c13beec97a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79170339"
---
# <a name="anonymous-types-c-programming-guide"></a>匿名型 (C# プログラミング ガイド)

匿名型を使用すると、あらかじめ明示的に型を定義することなく、一連の読み取り専用プロパティを単一のオブジェクトにカプセル化できるので便利です。 型の名前はコンパイラにより生成され、ソース コード レベルでは使用できません。 各プロパティの型はコンパイラにより推測されます。  
  
 匿名型を作成するには、[new](../../language-reference/operators/new-operator.md) 演算子をオブジェクト初期化子と一緒に使用します。 オブジェクト初期化子の詳細については、「[オブジェクト初期化子とコレクション初期化子](./object-and-collection-initializers.md)」を参照してください。  
  
 次の例では、`Amount` および `Message` という名前の 2 つのプロパティがある、初期化される匿名型を示します。  
  
```csharp  
var v = new { Amount = 108, Message = "Hello" };  
  
// Rest the mouse pointer over v.Amount and v.Message in the following  
// statement to verify that their inferred types are int and string.  
Console.WriteLine(v.Amount + v.Message);  
```  
  
 通常、匿名型はクエリ式の [select](../../language-reference/keywords/select-clause.md) 句で使用され、ソース シーケンスの各オブジェクトからプロパティのサブセットを返します。 クエリの詳細については、「[C# での LINQ](../../linq/index.md)」を参照してください。  
  
 匿名型には、読み取り専用パブリック プロパティが 1 つ以上含まれます。 それ以外のクラス メンバー (メソッドやイベントなど) は無効です。 プロパティの初期化に使用される式に、`null`、匿名関数、ポインター型を指定することはできません。  
  
 最も一般的な用例は、別の型のプロパティを使用して匿名型を初期化することです。 次の例では、`Product` という名前のクラスが存在すると仮定します。 `Product` クラスには、さまざまなプロパティが含まれますが、ここで注目するのは `Color` と `Price` プロパティです。 変数 `products` は、`Product` オブジェクトのコレクションです。 匿名型の宣言は、`new` キーワードで始まります。 この宣言により、`Product` の 2 つのプロパティだけを使用する新しい型が初期化されます。 この結果、クエリに返されるデータの量が少なくなります。  
  
 匿名型のメンバー名を指定しない場合、コンパイラは初期化に使用されるプロパティと同じ名前を匿名型メンバーに付けます。 前の例で示されているように、式を使用して初期化されるプロパティの名前を指定する必要があります。 次の例では、`Color` と `Price` が匿名型のプロパティの名前になっています。  
  
 [!code-csharp[csRef30Features#81](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csRef30Features/CS/csref30.cs#81)]  
  
 通常、変数の初期化に匿名型を使用する場合は、[var](../../language-reference/keywords/var.md) を使用することにより、変数を暗黙的に型指定したローカル変数として宣言します。 コンパイラだけが匿名型の基になる名前にアクセスできるため、変数宣言では型の名前を指定できません。 `var` の詳細については、「[暗黙的に型指定されたローカル変数](./implicitly-typed-local-variables.md)」を参照してください。  
  
 次の例に示すように、暗黙的に型指定されたローカル変数と暗黙的に型指定された配列を組み合わせることにより、匿名型の要素の配列を作成できます。  
  
```csharp  
var anonArray = new[] { new { name = "apple", diam = 4 }, new { name = "grape", diam = 1 }};  
```  
  
## <a name="remarks"></a>解説  
 匿名型は [object](../../language-reference/keywords/class.md) から直接派生した [class](../../language-reference/builtin-types/reference-types.md) 型であり、[object](../../language-reference/builtin-types/reference-types.md) 以外の型にキャストできません。 コンパイラは各匿名型に名前を付けますが、この名前にアプリケーションはアクセスできません。 共通言語ランタイムから見た場合、匿名型と他の参照型に違いはありません。  
  
 アセンブリ内の複数の匿名オブジェクト初期化子が、同じ順序で同じ名前や型を持つプロパティのシーケンスを指定する場合、コンパイラはそれらのオブジェクトを同じ型のインスタンスとして処理します。 これらのオブジェクトは、コンパイラで生成された同一の型情報を共有します。  
  
 フィールド、プロパティ、イベント、またはメソッドの戻り値の型は、匿名型を持つものとして宣言できません。 同様に、メソッドの仮パラメーター、プロパティ、コンストラクター、またはインデクサーも、匿名型を持つものとして宣言できません。 メソッドの引数として匿名型または匿名型を含むコレクションを渡すために、パラメーターを型オブジェクトとして宣言できます。 ただし、これにより厳密な型指定が無効になります。 クエリ結果をメソッドの境界を越えて格納したり渡したりする必要がある場合、匿名型の代わりに、通常の名前の構造体またはクラスの使用を検討してください。  
  
 匿名型の <xref:System.Object.Equals%2A> メソッドと <xref:System.Object.GetHashCode%2A> メソッドは、プロパティの `Equals` メソッドと `GetHashCode` メソッドとして定義されています。このため、同じ匿名型の 2 つのインスタンスは、すべてのプロパティが等しい場合のみ等しいとみなされます。  
  
## <a name="see-also"></a>参照

- [C# プログラミングガイド](../index.md)
- [オブジェクト初期化子とコレクション初期化子](./object-and-collection-initializers.md)
- [C# の LINQ の概要](../concepts/linq/index.md)
- [C# での LINQ](../../linq/index.md)
