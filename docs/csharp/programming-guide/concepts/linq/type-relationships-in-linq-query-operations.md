---
title: LINQ クエリ操作での型の関係 (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- inferring type information [LINQ in C#]
- data sources [LINQ in C#], type relationships
- queries [LINQ in C#], type relationships
- relationships [LINQ in C#]
- type relationships [LINQ in C#]
- variable relationships [LINQ in C#]
- type information inferred [LINQ in C#]
- data transformations [LINQ in C#]
- LINQ [C#], type relationships
ms.assetid: 99118938-d47c-4d7e-bb22-2657a9f95268
ms.openlocfilehash: 41853e6858fae9e8d449aeed95a6a84f343d5874
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635614"
---
# <a name="type-relationships-in-linq-query-operations-c"></a>LINQ クエリ操作での型の関係 (C#)
クエリを効果的に記述するには、クエリ操作全体における変数の型の相互関係を理解する必要があります。 これらの関係を理解しておくと、このドキュメント内の LINQ のサンプルやコード例を理解しやすくなります。 また、`var` を使用して変数を暗黙的に型指定した場合に、背後でどのような処理が行われるかを理解することもできます。  
  
 LINQ クエリ操作は、データ ソース、クエリ自体、およびクエリの実行において厳密に型指定されます。 クエリの変数の型には、データ ソース内の要素の型および `foreach` ステートメントの反復変数の型との互換性が必要です。 この厳密な型指定により、コンパイル時に型のエラーが検出され、実際にエラーが発生する前にそのエラーを修正できます。  
  
 これらの型の関係を示すために、後の例の大部分では、すべての変数に明示的な型指定を使用しています。 最後の例では、[var](../../../language-reference/keywords/var.md) を使用して暗黙的な型指定を行う場合でも、同じ基本原則が適用されることを示します。  
  
## <a name="queries-that-do-not-transform-the-source-data"></a>ソース データを変換しないクエリ  
 次の図は、データの変換を行わない LINQ to Objects クエリ操作を示しています。 ソースには文字列のシーケンスが含まれているので、クエリ出力も文字列のシーケンスです。  
  
 ![LINQ クエリ内のデータ型の関係を示す図。](./media/type-relationships-in-linq-query-operations/linq-query-data-type-relation.png)  
  
1. データ ソースの型引数によって、範囲変数の型が決まります。  
  
2. 選択したオブジェクトの型によって、クエリ変数の型が決まります。 ここでは、`name` は文字列です。 したがって、クエリ変数は `IEnumerable<string>` になります。  
  
3. クエリ変数は、`foreach` ステートメントで反復処理されます。 クエリ変数は文字列のシーケンスなので、反復変数も文字列です。  
  
## <a name="queries-that-transform-the-source-data"></a>ソース データを変換するクエリ  
 次の図は、単純なデータ変換を行う [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] クエリ操作を示しています。 このクエリは、`Customer` オブジェクトのシーケンスを入力として受け取り、`Name` プロパティのみを結果に選択します。 `Name` は文字列なので、クエリは出力として文字列のシーケンスを作成します。  
  
 ![データ型を変換するクエリを示す図。](./media/type-relationships-in-linq-query-operations/linq-query-transform-data-type.png)  
  
1. データ ソースの型引数によって、範囲変数の型が決まります。  
  
2. `select` ステートメントは、完全な `Name` オブジェクトではなく、`Customer` プロパティを返します。 `Name` は文字列なので、`custNameQuery` の型引数は `string` ではなく `Customer` になります。  
  
3. `custNameQuery` は文字列のシーケンスなので、`foreach` ループの反復変数も `string` です。  
  
 次の図は、もう少し複雑な変換を示しています。 `select` ステートメントは、元の `Customer` オブジェクトのメンバーを 2 つだけ取り込む匿名型を返します。  
  
 ![データ型を変換するより複雑なクエリを示す図。](./media/type-relationships-in-linq-query-operations/linq-complex-query-transform-data-type.png)  
  
1. データ ソースの型引数は、常にクエリの範囲変数の型です。  
  
2. `select` ステートメントによって匿名型が生成されるため、クエリ変数は `var` を使用して暗黙的に型指定する必要があります。  
  
3. クエリ変数の型が暗黙的なので、`foreach` ループの反復変数も暗黙的にする必要があります。  
  
## <a name="letting-the-compiler-infer-type-information"></a>コンパイラによる型情報の推論  
 クエリ操作における変数の関係を理解することは大切ですが、この処理をコンパイラで自動的に行う方法もあります。 [var](../../../language-reference/keywords/var.md) キーワードは、クエリ操作の任意のローカル変数に使用できます。 次の図は、前に説明した例 2 と類似しています。 ここでは、コンパイラがクエリ操作の各変数について、厳密な型を指定します。  
  
 ![暗黙的な入力による型フローを示す図。](./media/type-relationships-in-linq-query-operations/linq-type-flow-implicit-typing.png)  
  
 `var` の詳細については、「[暗黙的に型指定されたローカル変数](../../classes-and-structs/implicitly-typed-local-variables.md)」を参照してください。  
