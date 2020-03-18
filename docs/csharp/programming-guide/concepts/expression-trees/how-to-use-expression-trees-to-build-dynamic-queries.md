---
title: 式ツリーを使用して動的クエリをビルドする方法 (C#)
ms.date: 07/20/2015
ms.assetid: 52cd44dd-a3ec-441e-b93a-4eca388119c7
ms.openlocfilehash: 6114ec13dd43a7df146b87dda00fba06d6eb870c
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635900"
---
# <a name="how-to-use-expression-trees-to-build-dynamic-queries-c"></a>式ツリーを使用して動的クエリをビルドする方法 (C#)
LINQ では、<xref:System.Linq.IQueryable%601> を実装するデータ ソースをターゲットとする構造化されたクエリを表すために、式ツリーが使われます。 たとえば、LINQ プロバイダーは、リレーショナル データ ストアのクエリを行うために、<xref:System.Linq.IQueryable%601> インターフェイスを実装します。 C# コンパイラは、このようなデータ ソースをターゲットとするクエリをコンパイルして、実行時に式ツリーを作成するコードを生成します。 クエリ プロバイダーは式ツリー データ構造を走査して、データ ソースに適したクエリ言語に変換できます。  
  
 LINQ では、<xref:System.Linq.Expressions.Expression%601> 型の変数に代入されるラムダ式を表すためにも、式ツリーが使われます。  
  
 このトピックでは、式ツリーを使って動的な LINQ クエリを作成する方法について説明します。 動的クエリは、コンパイル時にクエリの詳細がわからない場合に便利です。 たとえば、データをフィルター処理するための述語をエンド ユーザーが指定できるユーザー インターフェイスをアプリケーションで提供することがあります。 クエリに LINQ を使うには、このようなアプリケーションでは式ツリーを使って実行時に LINQ クエリを作成する必要があります。  
  
## <a name="example"></a>例  
 次の例では、式ツリーを使って `IQueryable` データ ソースに対するクエリを作成して実行する方法を示します。 このコードは、次のクエリを表す式ツリーを作成します。  
  
 ```csharp
 companies.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))
          .OrderBy(company => company)
 ```
  
 クエリ全体を構成する式を表す式ツリーの作成には、<xref:System.Linq.Expressions> 名前空間のファクトリ メソッドが使われます。 標準クエリ演算子メソッドの呼び出しを表す式は、これらのメソッドの <xref:System.Linq.Queryable> の実装を参照します。 最終的な式ツリーが、<xref:System.Linq.IQueryProvider.CreateQuery%60%601%28System.Linq.Expressions.Expression%29> データ ソースのプロバイダーの `IQueryable` 実装に渡されて、`IQueryable` 型の実行可能なクエリが作成されます。 結果は、そのクエリ変数を列挙することにより取得されます。  
  
```csharp  
// Add a using directive for System.Linq.Expressions.  
  
string[] companies = { "Consolidated Messenger", "Alpine Ski House", "Southridge Video", "City Power & Light",  
                   "Coho Winery", "Wide World Importers", "Graphic Design Institute", "Adventure Works",  
                   "Humongous Insurance", "Woodgrove Bank", "Margie's Travel", "Northwind Traders",  
                   "Blue Yonder Airlines", "Trey Research", "The Phone Company",  
                   "Wingtip Toys", "Lucerne Publishing", "Fourth Coffee" };  
  
// The IQueryable data to query.  
IQueryable<String> queryableData = companies.AsQueryable<string>();  
  
// Compose the expression tree that represents the parameter to the predicate.  
ParameterExpression pe = Expression.Parameter(typeof(string), "company");  
  
// ***** Where(company => (company.ToLower() == "coho winery" || company.Length > 16)) *****  
// Create an expression tree that represents the expression 'company.ToLower() == "coho winery"'.  
Expression left = Expression.Call(pe, typeof(string).GetMethod("ToLower", System.Type.EmptyTypes));  
Expression right = Expression.Constant("coho winery");  
Expression e1 = Expression.Equal(left, right);  
  
// Create an expression tree that represents the expression 'company.Length > 16'.  
left = Expression.Property(pe, typeof(string).GetProperty("Length"));  
right = Expression.Constant(16, typeof(int));  
Expression e2 = Expression.GreaterThan(left, right);  
  
// Combine the expression trees to create an expression tree that represents the  
// expression '(company.ToLower() == "coho winery" || company.Length > 16)'.  
Expression predicateBody = Expression.OrElse(e1, e2);  
  
// Create an expression tree that represents the expression  
// 'queryableData.Where(company => (company.ToLower() == "coho winery" || company.Length > 16))'  
MethodCallExpression whereCallExpression = Expression.Call(  
    typeof(Queryable),  
    "Where",  
    new Type[] { queryableData.ElementType },  
    queryableData.Expression,  
    Expression.Lambda<Func<string, bool>>(predicateBody, new ParameterExpression[] { pe }));  
// ***** End Where *****  
  
// ***** OrderBy(company => company) *****  
// Create an expression tree that represents the expression  
// 'whereCallExpression.OrderBy(company => company)'  
MethodCallExpression orderByCallExpression = Expression.Call(  
    typeof(Queryable),  
    "OrderBy",  
    new Type[] { queryableData.ElementType, queryableData.ElementType },  
    whereCallExpression,  
    Expression.Lambda<Func<string, string>>(pe, new ParameterExpression[] { pe }));  
// ***** End OrderBy *****  
  
// Create an executable query from the expression tree.  
IQueryable<string> results = queryableData.Provider.CreateQuery<string>(orderByCallExpression);  
  
// Enumerate the results.  
foreach (string company in results)  
    Console.WriteLine(company);  
  
/*  This code produces the following output:  
  
    Blue Yonder Airlines  
    City Power & Light  
    Coho Winery  
    Consolidated Messenger  
    Graphic Design Institute  
    Humongous Insurance  
    Lucerne Publishing  
    Northwind Traders  
    The Phone Company  
    Wide World Importers  
*/  
```  
  
 このコードでは、`Queryable.Where` メソッドに渡される述語で固定数の式を使います。 ただし、ユーザー入力に依存する可変個の述語式を結合するアプリケーションを作成することもできます。 また、ユーザーからの入力に応じて、クエリで呼び出される標準クエリ演算子を変えることもできます。  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- System.Linq.Expressions 名前空間をインクルードします。  
  
## <a name="see-also"></a>参照

- [式ツリー (C#)](./index.md)
- [式ツリーを実行する方法 (C#)](./how-to-execute-expression-trees.md)
- [実行時における述語フィルターの動的指定](../../../linq/dynamically-specify-predicate-filters-at-runtime.md)
