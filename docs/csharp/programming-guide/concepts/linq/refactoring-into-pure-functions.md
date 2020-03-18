---
title: 純粋関数へのリファクタリング (C#)
ms.date: 07/20/2015
ms.assetid: 2944a0d4-fd33-4e2e-badd-abb0f9be2fcc
ms.openlocfilehash: 4cf91ff078bd1c4582daa05475a91c4a4ecaba3e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70253107"
---
# <a name="refactoring-into-pure-functions-c"></a>純粋関数へのリファクタリング (C#)

純粋関数型変換で重要なのは、純粋関数を使用してコードをリファクターする方法を理解することです。  
  
> [!NOTE]
> 関数型プログラミングでの命名には、一般に純粋関数を使用してプログラムをリファクターする方法が使用されます。 Visual Basic および C++ では、この処理がそれぞれの言語の関数を使用して行われます。 ただし C# では、関数がメソッドと呼ばれます。 ここでは、純粋関数が C# のメソッドとして実装されます。  
  
 このセクションで既に説明したように、純粋関数には 2 つの実用的な特性があります。  
  
- 副作用がありません。 この関数は、関数の外部にある変数やあらゆる型のデータを一切変更しません。  
  
- 一貫性があります。 同じ入力データを与えられると、常に同じ出力値を返します。  
  
 関数型プログラミングに移行するには、既存のコードをリファクターして不要な副作用や外部依存関係を排除するのが 1 つの方法です。 この方法で、既存のコードの純粋関数バージョンを作成できます。  
  
 このトピックでは、純粋関数の特徴とそれ以外の関数の特徴について説明します。 「[チュートリアル:WordprocessingML ドキュメント内のコンテンツの操作 (C#)](./shape-of-wordprocessingml-documents.md)」チュートリアルでは、WordprocessingML ドキュメントの操作方法を説明し、純粋関数を使用してリファクターする方法を示す 2 つの例を紹介しています。  
  
## <a name="eliminating-side-effects-and-external-dependencies"></a>副作用と外部依存関係の排除  
 次の例に示す 2 つの非純粋関数と 1 つの純粋関数を参照して、その違いを確認してください。  
  
### <a name="non-pure-function-that-changes-a-class-member"></a>クラス メンバーを変更する非純粋関数  
 次のコードの `HyphenatedConcat` 関数は、クラス内の `aMember` データ メンバーを変更するため、純粋関数ではありません。  
  
```csharp  
public class Program  
{  
    private static string aMember = "StringOne";  
  
    public static void HyphenatedConcat(string appendStr)  
    {  
        aMember += '-' + appendStr;  
    }  
  
    public static void Main()  
    {  
        HyphenatedConcat("StringTwo");  
        Console.WriteLine(aMember);  
    }  
}  
```  
  
 このコードを実行すると、次の出力が生成されます。  
  
```output  
StringOne-StringTwo  
```  
  
 この場合、変更されるデータに `public` アクセスと `private` アクセスのどちらがあるか、またはこのデータが `static` メンバーとインスタンス メンバーのどちらであるかは関係ありません。 純粋関数は、関数の外部にあるデータを一切変更しません。  
  
### <a name="non-pure-function-that-changes-an-argument"></a>引数を変更する非純粋関数  
 同じ関数の次のバージョンは、そのパラメーターである `sb` の内容を変更するため、純粋関数ではありません。  
  
```csharp  
public class Program  
{  
    public static void HyphenatedConcat(StringBuilder sb, String appendStr)  
    {  
        sb.Append('-' + appendStr);  
    }  
  
    public static void Main()  
    {  
        StringBuilder sb1 = new StringBuilder("StringOne");  
        HyphenatedConcat(sb1, "StringTwo");  
        Console.WriteLine(sb1);  
    }  
}  
```  
  
 このバージョンのプログラムは、最初のバージョンと同じ出力を生成します。これは、`HyphenatedConcat` 関数が <xref:System.Text.StringBuilder.Append%2A> メンバー関数を呼び出して最初のパラメーターの値 (状態) を変更したためです。 `HyphenatedConcat` はパラメーターを値で渡しますが、それでもこの変更は行われるので注意してください。  
  
> [!IMPORTANT]
> 参照型の場合、パラメーターを値で渡すと、渡されるオブジェクトへの参照がコピーされて渡されます。 このコピーは、参照変数が新しいオブジェクトに割り当てられるまで、元の参照と同じインスタンス データに関連付けられたままとなります。 パラメーターを変更する場合に、必ずしも関数に参照を渡す必要はありません。  
  
### <a name="pure-function"></a>純粋関数  
次のバージョンのプログラムは、`HyphenatedConcat` 関数を純粋関数として実装する方法を示しています。  
  
```csharp  
class Program  
{  
    public static string HyphenatedConcat(string s, string appendStr)  
    {  
        return (s + '-' + appendStr);  
    }  
  
    public static void Main(string[] args)  
    {  
        string s1 = "StringOne";  
        string s2 = HyphenatedConcat(s1, "StringTwo");  
        Console.WriteLine(s2);  
    }  
}  
```  
  
 このバージョンも、同じ出力行 `StringOne-StringTwo` を生成します。 この連結された値を保持するために、中間変数 `s2` が使用されていることに注意してください。  
  
 ローカルには純粋ではないが (つまりローカル変数を宣言して変更する)、グローバルには純粋である関数を作成すると、非常に便利な場合があります。 このような関数は、必要に応じて構成できる特性を多く備えていますが、関数型プログラミングの複雑な表現方法の一部 (単純なループによる処理を行う場合は再帰を使用する必要があるなど) が省かれています。  
  
## <a name="standard-query-operators"></a>標準クエリ演算子  
 標準クエリ演算子の重要な特性は、純粋関数として実装される点です。  
  
 詳細については、「[標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [純粋関数型変換の概要 (C#)](./introduction-to-pure-functional-transformations.md)
- [関数型プログラミングと命令型プログラミング (C#)](./functional-programming-vs-imperative-programming.md)
