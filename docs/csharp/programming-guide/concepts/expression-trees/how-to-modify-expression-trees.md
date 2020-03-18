---
title: 式ツリーを変更する方法 (C#)
ms.date: 07/20/2015
ms.assetid: 9b0cd8c2-457e-4833-9e36-31e79545f442
ms.openlocfilehash: e921c594497d02f5eb16cc60294e947e83636d7a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73969901"
---
# <a name="how-to-modify-expression-trees-c"></a>式ツリーを変更する方法 (C#)
このトピックでは、式ツリーを変更する方法について説明します。 式ツリーは変更不可であるため、直接変更を加えることができません。 式ツリーを変更するには、既存の式ツリーのコピーを作成する必要があります。コピーを作成する際に、必要な変更を加えます。 <xref:System.Linq.Expressions.ExpressionVisitor> クラスを使用して、既存の式ツリーを走査し、走査した各ノードをコピーすることができます。  
  
### <a name="to-modify-an-expression-tree"></a>式ツリーを変更するには  
  
1. 新しい**コンソール アプリケーション** プロジェクトを作成します。  
  
2. ファイルに `using` 名前空間の `System.Linq.Expressions` ディレクティブを 追加します。  
  
3. `AndAlsoModifier` クラスをプロジェクトに追加します。  
  
    ```csharp  
    public class AndAlsoModifier : ExpressionVisitor  
    {  
        public Expression Modify(Expression expression)  
        {  
            return Visit(expression);  
        }  
  
        protected override Expression VisitBinary(BinaryExpression b)  
        {  
            if (b.NodeType == ExpressionType.AndAlso)  
            {  
                Expression left = this.Visit(b.Left);  
                Expression right = this.Visit(b.Right);  
  
                // Make this binary expression an OrElse operation instead of an AndAlso operation.  
                return Expression.MakeBinary(ExpressionType.OrElse, left, right, b.IsLiftedToNull, b.Method);  
            }  
  
            return base.VisitBinary(b);  
        }  
    }  
    ```  
  
     このクラスは、<xref:System.Linq.Expressions.ExpressionVisitor> 条件演算を表す式を変更するための特別なクラスで、`AND` クラスを継承します。 このクラスによって条件 `AND` が条件 `OR` に変更されます。 そのために、クラスは基本データ型の <xref:System.Linq.Expressions.ExpressionVisitor.VisitBinary%2A> メソッドをオーバーライドします。`AND` 条件式は二項式で表されるためです。 `VisitBinary` メソッドでは、渡される式が `AND` 条件演算を表す場合、`OR` 条件演算子ではなく `AND` 条件演算子を含む新しい式がコードによって作成されます。 `VisitBinary` に渡される式が `AND` 条件演算を表さない場合は、基底クラスの実装が延期されます。 基底クラスのメソッドによって、渡された式ツリーに似たノードが作成されますが、そのノードのサブツリーは、ビジターによって再帰的に作成される式ツリーに置き換えられます。  
  
4. ファイルに `using` 名前空間の `System.Linq.Expressions` ディレクティブを 追加します。  
  
5. 式ツリーを作成し、それをメソッドに渡して変更するコードを、Program.cs ファイルの `Main` メソッドに追加します。  
  
    ```csharp  
    Expression<Func<string, bool>> expr = name => name.Length > 10 && name.StartsWith("G");  
    Console.WriteLine(expr);  
  
    AndAlsoModifier treeModifier = new AndAlsoModifier();  
    Expression modifiedExpr = treeModifier.Modify((Expression) expr);  
  
    Console.WriteLine(modifiedExpr);  
  
    /*  This code produces the following output:  
  
        name => ((name.Length > 10) && name.StartsWith("G"))  
        name => ((name.Length > 10) || name.StartsWith("G"))  
    */  
    ```  
  
     次のコードは、`AND` 条件演算を含む式を作成し、 `AndAlsoModifier` クラスのインスタンスを作成して、このクラスの `Modify` メソッドにその式を渡します。 元の式ツリーと変更された式ツリーの両方が出力され、変更内容が表示されます。  
  
6. アプリケーションをコンパイルして実行します。  
  
## <a name="see-also"></a>参照

- [式ツリーを実行する方法 (C#)](./how-to-execute-expression-trees.md)
- [式ツリー (C#)](./index.md)
