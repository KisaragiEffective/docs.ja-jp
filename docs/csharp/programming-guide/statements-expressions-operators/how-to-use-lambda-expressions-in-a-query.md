---
title: クエリでラムダ式を使用する方法 - C# プログラミング ガイド
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [C#], in LINQ
ms.assetid: 3cac4d25-d11f-4abd-9e7c-0f02e97ae06d
ms.openlocfilehash: 92bdbf842c5c30b2f32e06f622f3e08f3c7a878f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75711962"
---
# <a name="how-to-use-lambda-expressions-in-a-query-c-programming-guide"></a>クエリでラムダ式を使用する方法 (C# プログラミング ガイド)
クエリ構文でラムダ式を直接使うことはありませんが、メソッドの呼び出しで使い、クエリ式はメソッドの呼び出しを含むことができます。 実際、一部のクエリ操作はメソッド構文でのみ表現できます。 クエリ構文とメソッド構文の違いについて詳しくは、「[LINQ でのクエリ構文とメソッド構文](../concepts/linq/query-syntax-and-method-syntax-in-linq.md)」をご覧ください。  
  
## <a name="example"></a>例  
 次の例を見ると、<xref:System.Linq.Enumerable.Where%2A?displayProperty=nameWithType> 標準クエリ演算子を使用することにより、メソッド ベースのクエリでラムダ式を使用する方法がわかります。 この例の <xref:System.Linq.Enumerable.Where%2A> メソッドにはデリゲート型 <xref:System.Func%602> の入力パラメーターがあり、そのデリゲートは入力として整数を受け取ってブール値を返すことに注意してください。 ラムダ式は、そのデリゲートに変換できます。 これが [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] メソッドを使用する <xref:System.Linq.Queryable.Where%2A?displayProperty=nameWithType> のクエリであったなら、パラメーターの型は `Expression<Func<int,bool>>` になりますが、ラムダ式の表現はまったく同じです。 式の型の詳細については、<xref:System.Linq.Expressions.Expression?displayProperty=nameWithType> に関する記事をご覧ください。  
  
 [!code-csharp[csProgGuideLINQ#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csrefLINQHowTos.cs#1)]  
  
## <a name="example"></a>例  
 次の例では、クエリ式のメソッド呼び出しでラムダ式を使う方法を示します。 <xref:System.Linq.Enumerable.Sum%2A> 標準クエリ演算子はクエリ構文を使って呼び出すことができないため、ラムダが必要です。  
  
 このクエリは最初に、`GradeLevel` 列挙型で定義されている成績レベルに従って、学生をグループ分けします。 その後、各グループについて、各学生の合計点数を追加します。 これには、2 つの `Sum` 演算が必要です。 内側の `Sum` は各学生の合計点数を計算し、外側の `Sum` はグループ内のすべての学生の集計中の合計を保持します。  
  
 [!code-csharp[csProgGuideLINQ#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideLINQ/CS/csrefLINQHowTos.cs#2)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 このコードを実行するには、メソッドをコピーして、「`StudentClass`オブジェクトのコレクションにクエリを実行する[」で提供されている ](../../linq/query-a-collection-of-objects.md) に貼り付けた後、`Main` メソッドからそれを呼び出します。
  
## <a name="see-also"></a>参照

- [ラムダ式](./lambda-expressions.md)
- [式ツリー (C#)](../concepts/expression-trees/index.md)
