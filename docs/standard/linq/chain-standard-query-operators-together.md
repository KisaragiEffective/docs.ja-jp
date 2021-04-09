---
title: 標準クエリ演算子を連結する (C#) - LINQ to XML
description: クエリ演算子を連結する方法について説明します。
ms.date: 07/20/2015
dev_langs:
- csharp
ms.assetid: 66f2b0a9-2c23-4735-988e-bbc9dfb55c7b
ms.openlocfilehash: ed3ca44c853ebbeee690cb31232a13e35b383d1b
ms.sourcegitcommit: 0c3ce6d2e7586d925a30f231f32046b7b3934acb
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/08/2020
ms.locfileid: "103411950"
---
# <a name="chain-standard-query-operators-together-c-linq-to-xml"></a>標準クエリ演算子を連結する (C#) (LINQ to XML)

標準クエリ演算子を連結できます。 たとえば、<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType> 演算子 (`where` 句によって呼び出される) を挿入すると、レイジー演算となります。つまり、中間的な成果が具体化されることがありません。

## <a name="example-interject-a-where-clause"></a>例: where 句を挿入する

この例では、<xref:System.Linq.Enumerable.Where%2A> の前に `ConvertCollectionToUpperCase` メソッドが呼び出されます。 <xref:System.Linq.Enumerable.Where%2A> メソッドは、このチュートリアルの前の例で使用したレイジー メソッド (`ConvertCollectionToUpperCase` および `AppendString`) とほぼ同様に動作しますが、この例では異なる点もあります。

異なる点とは、この場合の <xref:System.Linq.Enumerable.Where%2A> メソッドではソース コレクションを反復処理し、最初の項目を述語に渡さないことを決定してから、述語に渡す次の項目を取得します。 その後、2 番目の項目を生成します。

ただし、基本的な考え方は同じです。つまり、中間コレクションは、必要がない限り具体化されません。

クエリ式が使用されている場合、そのクエリ式は標準クエリ演算子への呼び出しに変換され、同じ原則が適用されます。

Office Open XML ドキュメントに対してクエリを実行するこのセクションの例はすべて、同じ原則を使用します。 遅延実行およびレイジー評価は、LINQ (および LINQ to XML) を効果的に使用するために理解しておく必要がある基本的概念です。

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
        foreach (string str in source)
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
            where s.CompareTo("D") >= 0
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
AppendString: source >DEF<
Main: str >DEF!!!<

ToUpper: source >ghi<
AppendString: source >GHI<
Main: str >GHI!!!<
```

これは、「[チュートリアル: クエリを連結する (C#)](chain-queries-example.md)」の最後の記事です。