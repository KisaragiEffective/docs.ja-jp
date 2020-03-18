---
title: 射影操作 (C#)
ms.date: 07/20/2015
ms.assetid: 98df573a-aad9-4b8c-9a71-844be2c4fb41
ms.openlocfilehash: f76eeeb779ab08a575e758a9d974573b700ae652
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79168337"
---
# <a name="projection-operations-c"></a>射影操作 (C#)
射影とは、オブジェクトを、必要なプロパティだけで構成された別の形式に変換する操作のことをいいます。 射影を使用することにより、個々のオブジェクトから構築された新しい型を作成できます。 プロパティを投影し、それに対して数値演算関数を実行できます。 また、元のオブジェクトを変更せずに射影することもできます。  
  
 次のセクションでは、射影を実行する標準クエリ演算子メソッドの一覧を示します。  
  
## <a name="methods"></a>メソッド  
  
|メソッド名|[説明]|C# のクエリ式の構文|説明|  
|-----------------|-----------------|---------------------------------|----------------------|  
|選択|変換関数に基づいて値を射影します。|`select`|<xref:System.Linq.Enumerable.Select%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.Select%2A?displayProperty=nameWithType>|  
|SelectMany|変換関数に基づいて値のシーケンスを射影し、それを 1 つのシーケンスに平坦化します。|複数の `from` 句を使用|<xref:System.Linq.Enumerable.SelectMany%2A?displayProperty=nameWithType><br /><br /> <xref:System.Linq.Queryable.SelectMany%2A?displayProperty=nameWithType>|  
  
## <a name="query-expression-syntax-examples"></a>クエリ式の構文例  
  
### <a name="select"></a>選択  
 次の例では、`select` 句を使って、文字列リストにある各文字列の最初の文字を射影します。  
  
```csharp  
List<string> words = new List<string>() { "an", "apple", "a", "day" };  
  
var query = from word in words  
            select word.Substring(0, 1);  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    a  
    a  
    a  
    d  
*/  
```  
  
### <a name="selectmany"></a>SelectMany  
 次の例では、`from` 句を複数使用して、文字列リストにある各文字列の各単語を射影します。  
  
```csharp  
List<string> phrases = new List<string>() { "an apple a day", "the quick brown fox" };  
  
var query = from phrase in phrases  
            from word in phrase.Split(' ')  
            select word;  
  
foreach (string s in query)  
    Console.WriteLine(s);  
  
/* This code produces the following output:  
  
    an  
    apple  
    a  
    day  
    the  
    quick  
    brown  
    fox  
*/  
```  
  
## <a name="select-versus-selectmany"></a>Select と SelectMany の比較  
 `Select()` と `SelectMany()` の機能はどちらも、ソース値から結果値 (複数も可) を生成することです。 `Select()` は、ソース値ごとに結果値を 1 つ生成します。 そのため、結果全体は、ソース コレクションと同じ数の要素を持つ 1 つのコレクションになります。 これに対し、`SelectMany()` は、各ソース値から、連結されたサブコレクションを含む 1 つの総合的な結果を生成します。 `SelectMany()` に引数として渡される変換関数は、ソース値ごとに列挙可能な値のシーケンスを返す必要があります。 この列挙可能なシーケンスは `SelectMany()` によって連結されて、1 つの大きなシーケンスが作成されます。  
  
 これら 2 つのメソッドのアクションの概念的な違いを次の 2 つの図に示します。 どちらも、セレクター (変換) 関数が各ソース値から花の配列を選択することを想定しています。  
  
 次の図は、`Select()` がソース コレクションと同じ数の要素を持つコレクションを返すしくみを示しています。  
  
 ![Select&#40;&#41; のアクションを示すグラフィック。](./media/projection-operations/select-action-graphic.png)  
  
 次の図は、`SelectMany()` が中間配列シーケンスを、各中間配列の値を含む最終的な結果値に連結するしくみを示しています。  
  
 ![SelectMany&#40;&#41; のアクションを示すグラフィック。](./media/projection-operations/select-many-action-graphic.png )  
  
### <a name="code-example"></a>コード例  
 次の例は、`Select()` と `SelectMany()` の動作を比較しています。 コードは、ソース コレクションの花の名前の各リストから最初の 2 つの項目を取って "花束" を作成します。 この例では、変換関数 <xref:System.Linq.Enumerable.Select%60%602%28System.Collections.Generic.IEnumerable%7B%60%600%7D%2CSystem.Func%7B%60%600%2C%60%601%7D%29> が使用する "単一の値" 自体が値のコレクションになっています。 各サブ シーケンスで各文字列を列挙するために追加の `foreach` ループを使用しています。  
  
```csharp  
class Bouquet  
{  
    public List<string> Flowers { get; set; }  
}  
  
static void SelectVsSelectMany()  
{  
    List<Bouquet> bouquets = new List<Bouquet>() {  
        new Bouquet { Flowers = new List<string> { "sunflower", "daisy", "daffodil", "larkspur" }},  
        new Bouquet{ Flowers = new List<string> { "tulip", "rose", "orchid" }},  
        new Bouquet{ Flowers = new List<string> { "gladiolis", "lily", "snapdragon", "aster", "protea" }},  
        new Bouquet{ Flowers = new List<string> { "larkspur", "lilac", "iris", "dahlia" }}  
    };  
  
    // *********** Select ***********
    IEnumerable<List<string>> query1 = bouquets.Select(bq => bq.Flowers);  
  
    // ********* SelectMany *********  
    IEnumerable<string> query2 = bouquets.SelectMany(bq => bq.Flowers);  
  
    Console.WriteLine("Results by using Select():");  
    // Note the extra foreach loop here.  
    foreach (IEnumerable<String> collection in query1)  
        foreach (string item in collection)  
            Console.WriteLine(item);  
  
    Console.WriteLine("\nResults by using SelectMany():");  
    foreach (string item in query2)  
        Console.WriteLine(item);  
  
    /* This code produces the following output:  
  
       Results by using Select():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
  
       Results by using SelectMany():  
        sunflower  
        daisy  
        daffodil  
        larkspur  
        tulip  
        rose  
        orchid  
        gladiolis  
        lily  
        snapdragon  
        aster  
        protea  
        larkspur  
        lilac  
        iris  
        dahlia  
    */  
  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Linq>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [select 句](../../../language-reference/keywords/select-clause.md)
- [複数のソースからオブジェクト コレクションにデータを設定する方法 (LINQ) (C#)](./how-to-populate-object-collections-from-multiple-sources-linq.md)
- [グループを使用して 1 つのファイルを複数のファイルに分割する方法 (LINQ) (C#)](./how-to-split-a-file-into-many-files-by-using-groups-linq.md)
