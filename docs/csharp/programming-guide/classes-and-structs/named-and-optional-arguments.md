---
title: 名前付き引数と省略可能な引数 - C# プログラミング ガイド
ms.date: 07/20/2015
f1_keywords:
- namedParameter_CSharpKeyword
- cs_namedParameter
helpviewer_keywords:
- parameters [C#], named
- named arguments [C#]
- arguments [C#], named
- optional arguments [C#]
- arguments [C#], optional
- parameters [C#], optional
- named and optional arguments [C#]
ms.assetid: 839c960c-c2dc-4d05-af4d-ca5428e54008
ms.openlocfilehash: 15b685248730c1f742035612a201d97d180bbc41
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398419"
---
# <a name="named-and-optional-arguments-c-programming-guide"></a>名前付き引数と省略可能な引数 (C# プログラミング ガイド)
C# 4 では、名前付き引数と省略可能な引数が導入されています。 *名前付き引数*を使用すると、パラメーター リストのパラメーターの位置ではなく、パラメーター名に引数を関連付けることによって、特定のパラメーターの引数を指定できます。 *省略可能な引数*を使用すると、一部のパラメーターの引数を省略できます。 両方の手法をメソッド、インデクサー、コンストラクター、デリゲートで使用できます。  
  
 名前付き引数と省略可能な引数を使用すると、引数は、パラメーター リストではなく、引数リストに記述されている順に評価されます。  
  
 名前付きパラメーターと省略可能なパラメーターを併用する場合、省略可能なパラメーターのリストにあるパラメーターのうち、ごく一部のパラメーターに対してのみ引数を指定できます。 この機能により、Microsoft Office オートメーション API などの COM インターフェイスの呼び出しが大幅に円滑化します。  
  
## <a name="named-arguments"></a>名前付き引数  
 名前付き引数を使用すると、呼び出されたメソッドのパラメーター リストに記述されているパラメーターの順序を記憶したり、検索したりする必要がなくなります。 各引数のパラメーターはパラメーター名で指定できます。 たとえば、注文の詳細 (販売者の名前、注文番号、製品名など) を出力する関数は標準的な方法で呼び出すことができます。その関数で定義されている順序で、引数を位置によって渡します。
  
 `PrintOrderDetails("Gift Shop", 31, "Red Mug");`
  
 パラメーターの順序を覚えていなくても、パラメーターの名前がわかっていれば、任意の順序で引数を渡すことができます。  
  
 `PrintOrderDetails(orderNum: 31, productName: "Red Mug", sellerName: "Gift Shop");`
  
 `PrintOrderDetails(productName: "Red Mug", sellerName: "Gift Shop", orderNum: 31);`
  
 また、名前付き引数を使用すると、各引数が表すものが識別しやすくなり、コードが読みやすくなります。 次のメソッド例では、`sellerName` は null にしたり、空白にしたりできません。 `sellerName` と `productName` はいずれも文字列型であり、引数を位置によって渡すより、名前付き引数を使用するほうが合理的です。2 つのあいまいさが取り除かれ、コードを読む人の混乱が少なくなります。
  
 名前付き引数は、位置引数と共に使用するとき、次の場合において有効となります

- 後ろに位置引数が続かない。

 `PrintOrderDetails("Gift Shop", 31, productName: "Red Mug");`

- _C# 7.2 以降_。正しい位置で使用されます。 下の例では、パラメーター `orderNum` は正しい位置にありますが、明示的に名前が付けられていません。

 `PrintOrderDetails(sellerName: "Gift Shop", 31, productName: "Red Mug");`
  
 後に位置引数が続く場合、順序が正しくない名前付き引数は無効になります。

 ```csharp
 // This generates CS1738: Named argument specifications must appear after all fixed arguments have been specified.
 PrintOrderDetails(productName: "Red Mug", 31, "Gift Shop");
 ```
  
## <a name="example"></a>例  
 次のコードでは、このセクションの例の実装し、さらにいくつかのプログラミングを追加しています。  
  
 [!code-csharp[csProgGuideNamedAndOptional#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/program.cs#1)]  
  
## <a name="optional-arguments"></a>省略可能な引数  
 メソッド、コンストラクター、インデクサー、デリゲートの定義では、そのパラメーターが必須であるか、省略可能であるかを指定できます。 どの呼び出しでも、すべての必須パラメーターに対して引数を指定する必要があります。ただし、省略可能なパラメーターの引数は省略できます。  
  
 省略可能な各パラメーターには、パラメーターの定義に既定値が含まれています。 そのパラメーターの引数を渡さない場合、既定値が使用されます。 既定値には、次のいずれかの種類の式を指定できます。  
  
- 定数式  
  
- `new ValType()` 形式の式です。`ValType` は、[enum](../../language-reference/builtin-types/enum.md) や [struct](../../language-reference/builtin-types/struct.md) のような値型になります。  
  
- [default(ValType)](../../language-reference/operators/default.md) 形式の式です。`ValType` は値型です。  
  
 省略可能なパラメーターは、パラメーター リストの末尾で必須パラメーターの後に定義されます。 呼び出し元は、連続する省略可能なパラメーターのいずれか 1 つに対して引数を指定する場合、先行するすべての省略可能なパラメーターに引数を指定する必要があります。 引数リストでは、スペースをコンマで区切ることはできません。 たとえば、次のコードでは、1 つの必須パラメーターと 2 つの省略可能パラメーターでインスタンス メソッド `ExampleMethod` が定義されます。  
  
 [!code-csharp[csProgGuideNamedAndOptional#15](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#15)]  
  
 次に示す `ExampleMethod` の呼び出しでは、3 つ目のパラメーターに引数が指定されていますが、2 つ目のパラメーターには指定されていないため、コンパイル エラーが発生します。  
  
 `//anExample.ExampleMethod(3, ,4);`  
  
 ただし、3 つ目のパラメーターの名前がわかっている場合、名前付き引数を使用してタスクを実行できます。  
  
 `anExample.ExampleMethod(3, optionalint: 4);`  
  
 次の例に示すように、IntelliSense では、省略可能なパラメーターを角かっこで示します。  
  
 ![ExampleMethod メソッドの IntelliSense クイック インフォを示すスクリーンショット。](./media/named-and-optional-arguments/optional-examplemethod-parameters.png)  
  
> [!NOTE]
> また、.NET <xref:System.Runtime.InteropServices.OptionalAttribute> クラスを使用して省略可能なパラメーターを宣言することもできます。 `OptionalAttribute` パラメーターに既定値は必要ありません。  
  
## <a name="example"></a>例  
 次の例では、`ExampleClass` のコンストラクターに省略可能なパラメーターが 1 つあります。 `ExampleMethod` インスタンス メソッドには、`required` という 1 つの必須パラメーターと、`optionalstr` と `optionalint` という 2 つの省略可能なパラメーターがあります。 `Main` のコードに示すように、いくつかの異なる方法でコンストラクターとメソッドを呼び出すことができます。  
  
 [!code-csharp[csProgGuideNamedAndOptional#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/optional.cs#2)]  
  
## <a name="com-interfaces"></a>COM インターフェイス  
 名前付き引数と省略可能な引数を動的オブジェクトやその他の拡張機能のサポートと併用すると、Office オートメーション API などの COM API との相互運用性が大幅に向上します。  
  
 たとえば、Microsoft Office Excel <xref:Microsoft.Office.Interop.Excel.Range.AutoFormat%2A> インターフェイスの <xref:Microsoft.Office.Interop.Excel.Range> メソッドには 7 つのパラメーターがあります。それらはすべて省略可能です。 これらのパラメーターを次の例に示します。  
  
 ![AutoFormat メソッドの IntelliSense クイック インフォを示すスクリーンショット。](./media/named-and-optional-arguments/autoformat-method-parameters.png)  
  
 C# 3.0 以前のバージョンの C# では、次の例に示すように、各パラメーターの引数が必要です。  
  
 [!code-csharp[csProgGuideNamedAndOptional#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#3)]  
  
 ただし、C# 4.0 の導入により、名前付き引数と省略可能な引数を使用すると、`AutoFormat` の呼び出しを大幅に簡略化できます。 名前付き引数と省略可能な引数を使用すると、パラメーターの既定値の変更を望まない場合に、省略可能なパラメーターの引数を省略できます。 次の呼び出しでは、7 つのパラメーターのうちの 1 つのパラメーターのみに値を指定しています。  
  
 [!code-csharp[csProgGuideNamedAndOptional#13](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidenamedandoptional/cs/namedandoptcom.cs#13)]  
  
 詳細と例については、「[Office プログラミングで名前付き引数と省略可能な引数を使用する方法](./how-to-use-named-and-optional-arguments-in-office-programming.md)」と「[C# の機能を使用して Office 相互運用オブジェクトにアクセスする方法](../interop/how-to-access-office-onterop-objects.md)」を参照してください。  
  
## <a name="overload-resolution"></a>Overload Resolution  
 名前付き引数と省略可能な引数を使用すると、オーバーロードの解決に次のように影響します。  
  
- メソッド、インデクサー、コンストラクターのパラメーターのそれぞれが任意であるか、名前か位置により、呼び出しステートメントの 1 つの引数に対応するとき、その引数がパラメーターの型に変換できる場合、メソッド、インデクサー、コンストラクターが実行の候補になります。  
  
- 複数の候補が見つかった場合、明示的に指定される引数には、優先変換に関するオーバーロード解決の規則が適用されます。 任意のパラメーターの省略された引数は無視されます。  
  
- 2 つの候補が等しく良好であると判断された場合、呼び出しで引数が省略された任意のパラメーターのない候補が優先されます。 これはパラメーターの少ない候補に関するオーバーロード解決の一般優先設定の結果です。  
  
## <a name="c-language-specification"></a>C# 言語仕様  
 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>参照

- [Office プログラミングで名前付き引数と省略可能な引数を使用する方法](./how-to-use-named-and-optional-arguments-in-office-programming.md)
- [dynamic 型の使用](../types/using-type-dynamic.md)
- [コンストラクターの使用](./using-constructors.md)
- [インデクサーの使用](../indexers/using-indexers.md)
