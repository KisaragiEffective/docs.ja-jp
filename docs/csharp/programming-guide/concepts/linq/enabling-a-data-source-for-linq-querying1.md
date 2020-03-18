---
title: データ ソースの LINQ クエリの有効化
ms.date: 07/20/2015
ms.assetid: d2ef04a5-31a6-45cb-af9a-a5ce7732662c
ms.openlocfilehash: 9a143f0da74d4e91ef697f468d7fda225e75245b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635770"
---
# <a name="enabling-a-data-source-for-linq-querying"></a>データ ソースの LINQ クエリの有効化
LINQ を拡張して、データ ソースを LINQ パターンでクエリできるようにする方法はいくつかあります。 データ ソースは、いくつか例を挙げると、データ構造体、Web サービス、ファイル システム、またはデータベースの場合があります。 クエリの構文とパターンは変わらないため、LINQ パターンを使用すると、クライアントは LINQ クエリが有効になっているデータ ソースを簡単にクエリできます。 LINQ は、次の方法によってこれらのデータ ソースに拡張することができます。  
  
- 型に <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装し、その型の LINQ to Objects クエリを実行できるようにする。  
  
- 型を拡張する <xref:System.Linq.Enumerable.Where%2A> や <xref:System.Linq.Enumerable.Select%2A> などの標準クエリ演算子メソッドを作成し、その型のカスタム LINQ クエリを有効にする。  
  
- データ ソース用に、<xref:System.Linq.IQueryable%601> インターフェイスを実装したプロバイダーを作成する。 このインターフェイスを実装したプロバイダーは、式ツリーの形式で LINQ クエリを受け取ります。プロバイダーはこれをカスタマイズされた方法 (たとえばリモート) で実行できます。  
  
- データ ソース用に、既存の LINQ テクノロジを利用するプロバイダーを作成する。 そのようなプロバイダーは、クエリだけでなく、挿入、更新、および削除などの操作や、ユーザー定義型のマッピングも有効にします。  
  
 このトピックでは、これらのオプションについて説明します。  
  
## <a name="how-to-enable-linq-querying-of-your-data-source"></a>データ ソースの LINQ クエリを有効にする方法  
  
### <a name="in-memory-data"></a>インメモリ データ  
 インメモリ データの LINQ クエリを有効にする方法は 2 つあります。 データ型が <xref:System.Collections.Generic.IEnumerable%601> を実装する型の場合、LINQ to Objects を使用してデータをクエリすることができます。 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装して型の列挙体を有効にしても意味がない場合は、その型の LINQ 標準クエリ演算子メソッドを定義するか、または型を拡張する LINQ 標準クエリ演算子メソッドを作成することができます。 標準クエリ演算子のカスタム実装は、結果を返すために遅延実行を使用する必要があります。  
  
### <a name="remote-data"></a>リモート データ  
 リモート データ ソースの LINQ クエリを有効にするための最善の選択肢は、<xref:System.Linq.IQueryable%601> インターフェイスを実装することです。 しかしこれは、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] などのプロバイダーをデータ ソースに対して拡張することとは別です。 Visual Studio 2008 では、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] などの既存の LINQ テクノロジを別の型のデータ ソースに拡張するためにプロバイダー モデルを使用することができません。
  
## <a name="iqueryable-linq-providers"></a>IQueryable LINQ プロバイダー  
 <xref:System.Linq.IQueryable%601> を実装する LINQ プロバイダーの複雑度にはかなりのばらつきがあります。 このセクションでは、さまざまなレベルの複雑度について説明します。  
  
 複雑度が低めの `IQueryable` プロバイダーは、多くの場合、Web サービスの単一のメソッドとやり取りします。 この種のプロバイダーは処理するクエリに特定の情報を受け取るので非常に高い固有性を持ちます。 クローズされた型システムを持ち、おそらく 1 つの結果型を公開します。 クエリはほとんどの場合、標準クエリ演算子の <xref:System.Linq.Enumerable> 実装などを使用して、ローカルで実行されます。 複雑度が低めのプロバイダーは、クエリを表す式ツリーのメソッド呼び出し式を 1 つだけ調べ、残りのクエリのロジックは他の場所で処理されるようにします。  
  
 複雑度が中レベルの `IQueryable` プロバイダーは、部分的に表現力が豊かなクエリ言語を持つデータ ソースを対象とします。 そのプロバイダーが Web サービスを対象とする場合、Web サービスの複数のメソッドとやり取りし、クエリが提示する問題に基づいて呼び出すメソッドを選択します。 中レベルの複雑度のプロバイダーは、簡単なプロバイダーより型システムが豊富ですが、それでもその種類は限られています。 たとえば、このレベルのプロバイダーは走査できる一対多リレーションシップを持つ型を公開する場合がありますが、ユーザー定義型のマッピング テクノロジは提供しません。  
  
 `IQueryable` プロバイダーなどの複雑な [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] プロバイダーは LINQ クエリ一式を、SQL などの表現力が豊かなクエリ言語に変換する場合があります。 複雑なプロバイダーは、それほど複雑でないプロバイダーより一般的です。これは、より多様な質問をクエリで処理できるためです。 さらに、オープンな型システムを持つため、ユーザー定義型をマップするために広範なインフラストラクチャを含める必要があります。 複雑なプロバイダーの開発には、多大な労力を要します。  
  
## <a name="see-also"></a>参照

- <xref:System.Linq.IQueryable%601>
- <xref:System.Collections.Generic.IEnumerable%601>
- <xref:System.Linq.Enumerable>
- [標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)
- [LINQ to Objects (C#)](./linq-to-objects.md)
