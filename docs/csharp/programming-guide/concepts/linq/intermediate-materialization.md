---
title: 中間結果の具体化 (C#)
ms.date: 07/20/2015
ms.assetid: 7922d38f-5044-41cf-8e17-7173d6553a5e
ms.openlocfilehash: af1eb7df7da02d8e72fc102cda4ee5f329dc7974
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "70253159"
---
# <a name="intermediate-materialization-c"></a>中間結果の具体化 (C#)
不注意なユーザーは、クエリ内でコレクションを早期に具体化して、アプリケーションのメモリおよびパフォーマンスのプロファイルを大幅に変更してしまうことがあります。 一部の標準クエリ演算子では、単一の要素を生成する前に、ソース コレクションを具体化する場合があります。 たとえば、<xref:System.Linq.Enumerable.OrderBy%2A?displayProperty=nameWithType> は最初にソース コレクション全体を反復処理し、次にすべての項目を並べ替えて、最後に最初の項目を生成します。 これは、順序付けられたコレクションの最初の項目の取得が高コストであることを意味しています。これ以降の各項目の取得は高コストではありません。 このクエリ演算子は他の方法では処理を実行できないため、これは処理方法として適切なものです。  
  
## <a name="example"></a>例  
 この例では、前の例を変更します。 `AppendString` メソッドは、ソースを反復処理する前に、<xref:System.Linq.Enumerable.ToList%2A> ‚ðŒÄ‚Ño‚µ‚Ü‚·B これにより、具体化が行われます。  
  
```csharp  
public static class LocalExtensions  
{  
    public static IEnumerable<string>  
      ConvertCollectionToUpperCase(this IEnumerable<string> source)  
    {  
        foreach (string str in source)  
        {  
            Console.WriteLine("ToUpper: source >{0}<", str);  
            yield return str.ToUpper();  
        }  
    }  
  
    public static IEnumerable<string>  
      AppendString(this IEnumerable<string> source, string stringToAppend)  
    {  
        // the following statement materializes the source collection in a List<T>  
        // before iterating through it  
        foreach (string str in source.ToList())  
        {  
            Console.WriteLine("AppendString: source >{0}<", str);  
            yield return str + stringToAppend;  
        }  
    }  
}  
  
class Program  
{  
    static void Main(string[] args)  
    {  
        string[] stringArray = { "abc", "def", "ghi" };  
  
        IEnumerable<string> q1 =  
            from s in stringArray.ConvertCollectionToUpperCase()  
            select s;  
  
        IEnumerable<string> q2 =  
            from s in q1.AppendString("!!!")  
            select s;  
  
        foreach (string str in q2)  
        {  
            Console.WriteLine("Main: str >{0}<", str);  
            Console.WriteLine();  
        }  
    }  
}  
```  
  
 この例を実行すると、次の出力が生成されます。  
  
```output  
ToUpper: source >abc<  
ToUpper: source >def<  
ToUpper: source >ghi<  
AppendString: source >ABC<  
Main: str >ABC!!!<  
  
AppendString: source >DEF<  
Main: str >DEF!!!<  
  
AppendString: source >GHI<  
Main: str >GHI!!!<  
```  
  
 この例では、<xref:System.Linq.Enumerable.ToList%2A> の呼び出しにより、`AppendString` が、最初の項目を生成する前にソース全体を列挙することがわかります。 ソースが大きな配列である場合は、これによってアプリケーションのメモリ プロファイルが大幅に変更されます。  
  
 標準クエリ演算子も連結することができます。 これについては、このチュートリアルの最後のトピックで説明します。  
  
- [標準クエリ演算子の連結 (C#)](./chaining-standard-query-operators-together.md)  
  
## <a name="see-also"></a>参照

- [チュートリアル: クエリの連結 (C#)](./deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)
