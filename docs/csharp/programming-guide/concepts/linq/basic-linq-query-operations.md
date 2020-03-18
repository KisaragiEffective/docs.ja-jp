---
title: LINQ クエリの基本操作 (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- orderby clause [LINQ in C#]
- ordering data [LINQ in C#]
- selecting data [LINQ in C#]
- queries [LINQ in C#], basic operations
- grouping data [LINQ in C#]
- data sources [LINQ in C#]
- sorting data [LINQ in C#]
- projections [LINQ in C#]
- filtering data [LINQ in C#]
- joining data [LINQ in C#]
- select clause [LINQ in C#]
- LINQ [C#], basic query operations
- join clause [LINQ in C#]
- group clause [LINQ in C#]
ms.assetid: a7ea3421-1cf4-4df7-832a-aa22fe6379e9
ms.openlocfilehash: 91c038303c1ad7c2530964d3102aae49090c4c2a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635939"
---
# <a name="basic-linq-query-operations-c"></a>LINQ クエリの基本操作 (C#)
このトピックでは、LINQ クエリ式とクエリで実行する一般的な操作について、簡単に説明します。 詳細については、以下のトピックを参照してください。  
  
 [LINQ クエリ式](../../../linq/index.md)  
  
 [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)  
  
 [チュートリアル: C# でのクエリの作成](./walkthrough-writing-queries-linq.md)  
  
> [!NOTE]
> 既に SQL や XQuery などのクエリ言語に精通している場合は、このトピックの大部分を省略できます。 LINQ クエリ式における句の順序について理解するには、次のセクションの "`from` 句" を参照してください。  
  
## <a name="obtaining-a-data-source"></a>データ ソースの取得  
 LINQ クエリで必要な最初の手順は、データ ソースを指定することです。 ほとんどのプログラミング言語と同じように、C# でも、変数を使用する前に宣言しておく必要があります。 LINQ クエリでは、データ ソース (`from`) および "`customers`範囲変数 *" (* ) を導入するために `cust` 句が最初に使用されます。  
  
 [!code-csharp[csLINQGettingStarted#23](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#23)]  
  
 範囲変数は、`foreach`ループの反復変数と似ていますが、クエリ式では実際の反復は発生しません。 クエリが実行されると、範囲変数は `customers` の連続する各要素への参照として機能します。 `cust` の型はコンパイラで推論できるため、明示的に指定する必要はありません。 追加の範囲変数は、`let` 句で導入できます。 詳細については、「[let 句](../../../language-reference/keywords/let-clause.md)」を参照してください。  
  
> [!NOTE]
> <xref:System.Collections.ArrayList> などの非ジェネリック データ ソースの場合は、範囲変数を明示的に型指定する必要があります。 詳細については、「[LINQ を使用して ArrayList にクエリを実行する方法 (C#)](./how-to-query-an-arraylist-with-linq.md)」および「[from 句](../../../language-reference/keywords/from-clause.md)」を参照してください。  
  
## <a name="filtering"></a>Filtering  
 最も一般的なクエリ操作は、ブール式の形式でフィルターを適用することです。 クエリにフィルターを使用すると、式の条件に該当する要素だけがクエリから返されます。 結果は、`where` 句を使って生成されます。 フィルターは、実質的にはソース シーケンスから除外する要素を指定します。 次の例では、住所がロンドンにある `customers` だけが返されます。  
  
 [!code-csharp[csLINQGettingStarted#24](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#24)]  
  
 使い慣れた C# 論理 `AND` 演算子と論理 `OR` 演算子を使用すると、必要なだけのフィルター式を `where` 句に適用できます。 たとえば、住所が "London" にあり、かつ (`AND`) 名前が "Devon" の顧客だけを返すには、次のコードを記述します。  
  
 [!code-csharp[csLINQGettingStarted#25](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#25)]  
  
 住所がロンドンまたはパリにある顧客を返すには、次のコードを記述します。  
  
 [!code-csharp[csLINQGettingStarted#26](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#26)]  
  
 詳細については、「[where 句](../../../language-reference/keywords/where-clause.md)」を参照してください。  
  
## <a name="ordering"></a>並べ替え  
 返されたデータを並べ替えると便利なことがよくあります。 `orderby` 句を使用すると、並べ替える型の既定の比較子に従って、返されたシーケンスの要素が並べ替えられます。 たとえば、次のクエリは `Name` プロパティに基づいて結果を並び替えるように拡張できます。 `Name` は文字列であるため、既定の比較子によって、アルファベット順 (A から Z) で並べ替えられます。  
  
 [!code-csharp[csLINQGettingStarted#27](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#27)]  
  
 結果を逆の順序 (Z から A) で並び替えるには、`orderby…descending` 句を使用します。  
  
 詳細については、「[orderby 句](../../../language-reference/keywords/orderby-clause.md)」を参照してください。  
  
## <a name="grouping"></a>グループ化  
 指定したキーに基づいて結果をグループ化するには、`group` 句を使用します。 たとえば、結果を `City` 別にグループ化するように指定して、住所がロンドンまたはパリにあるすべての顧客を個々のグループに分けることができます。 この場合は、`cust.City` がキーになります。  
  
 [!code-csharp[csLINQGettingStarted#28](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#28)]  
  
 `group` 句を使用したクエリが終了すると、結果は複数リストのリストという形式になります。 リストの各要素はオブジェクトであり、そのオブジェクトには、`Key` メンバーとそのキーに基づいてグループ化された要素のリストが含まれます。 グループのシーケンスを生成するクエリを反復処理する場合は、入れ子になった `foreach` ループを使用する必要があります。 外側のループは各グループを反復処理し、内側のループは各グループのメンバーを反復処理します。  
  
 グループ操作の結果を参照する必要がある場合は、`into` キーワードを使用して、さらに照会可能な識別子を作成します。 次のクエリでは、顧客が 2 人より多いグループだけが返されます。  
  
 [!code-csharp[csLINQGettingStarted#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#29)]  
  
 詳細については、「[group 句](../../../language-reference/keywords/group-clause.md)」を参照してください。  
  
## <a name="joining"></a>参加  
 結合操作は、データ ソースで明示的にモデル化されていないシーケンス間に関連付けを作成します。 たとえば、結合を実行して、住所地が同じすべての顧客と販売業者を検索することができます。 LINQ では、`join` 句はデータベース テーブルを直接の対象とするのではなく、オブジェクトのコレクションを対象として機能します。  
  
 [!code-csharp[csLINQGettingStarted#36](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#36)]  
  
 LINQ では、SQL ほど頻繁に `join` を使用する必要はありません。これは、オブジェクト モデルでは、LINQ の外部キーが項目のコレクションを保持するプロパティとして表されるためです。 たとえば、`Customer` オブジェクトには `Order` オブジェクトのコレクションが含まれます。 結合を実行しなくても、ドット表記を使用して注文にアクセスできます。  
  
```csharp
from order in Customer.Orders...  
```  
  
 詳しくは、「[join 句](../../../language-reference/keywords/join-clause.md)」をご覧ください。  
  
## <a name="selecting-projections"></a>選択 (投影)  
 `select` 句はクエリの結果を生成し、返される各要素の "シェイプ" つまり型を指定します。 たとえば、完全な `Customer` オブジェクト、１ つのメンバーのみ、メンバーのサブセット、または計算や新しいオブジェクトの作成に基づいた、まったく異なる種類の結果のいずれで結果が構成されるかを指定できます。 `select` 句でソース要素のコピー以外のものを生成する場合、その操作は*投影*と呼ばれます。 投影を使用したデータの変換は、LINQ クエリ式の強力な機能です。 詳細については、「[LINQ によるデータ変換 (C#)](./data-transformations-with-linq.md)」と「[select 句](../../../language-reference/keywords/select-clause.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [LINQ クエリ式](../../../linq/index.md)
- [チュートリアル: C# でのクエリの作成](./walkthrough-writing-queries-linq.md)
- [クエリ キーワード (LINQ)](../../../language-reference/keywords/query-keywords.md)
- [匿名型](../../classes-and-structs/anonymous-types.md)
