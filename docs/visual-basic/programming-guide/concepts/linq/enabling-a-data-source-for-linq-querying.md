---
title: LINQ Querying2 のデータソースを有効にする
ms.date: 07/20/2015
ms.assetid: c412f0cf-ff0e-4993-ab3d-1b49e23f00f8
ms.openlocfilehash: ecd43b371a8e907e9bcfc8687c04bdd0350235ac
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636888"
---
# <a name="enabling-a-data-source-for-linq-querying"></a>データ ソースの LINQ クエリの有効化

Linq を拡張して linq パターンで任意のデータソースを照会できるようにするには、さまざまな方法があります。 データ ソースは、いくつか例を挙げると、データ構造体、Web サービス、ファイル システム、またはデータベースの場合があります。 LINQ パターンを使用すると、クエリの構文とパターンが変更されないため、LINQ クエリが有効になっているデータソースをクライアントが簡単に照会できるようになります。 LINQ をこれらのデータソースに拡張するには、次の方法があります。

- 型の <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装して、その型の LINQ to Objects クエリを有効にします。

- 型を拡張する <xref:System.Linq.Enumerable.Where%2A> や <xref:System.Linq.Enumerable.Select%2A> などの標準クエリ演算子メソッドを作成し、その型のカスタム LINQ クエリを有効にします。

- データ ソース用に、<xref:System.Linq.IQueryable%601> インターフェイスを実装したプロバイダーを作成する。 このインターフェイスを実装するプロバイダーは、式ツリーの形式で LINQ クエリを受け取ります。これは、たとえばリモートでカスタムの方法で実行できます。

- 既存の LINQ テクノロジを活用したデータソースのプロバイダーを作成する。 そのようなプロバイダーは、クエリだけでなく、挿入、更新、および削除などの操作や、ユーザー定義型のマッピングも有効にします。

このトピックでは、これらのオプションについて説明します。

## <a name="how-to-enable-linq-querying-of-your-data-source"></a>データ ソースの LINQ クエリを有効にする方法

### <a name="in-memory-data"></a>インメモリ データ
 メモリ内データの LINQ クエリを有効にするには、2つの方法があります。 データが <xref:System.Collections.Generic.IEnumerable%601>を実装する型の場合、LINQ to Objects を使用してデータのクエリを実行できます。 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスを実装して型の列挙を有効にしない場合は、その型の LINQ 標準クエリ演算子メソッドを定義するか、型を拡張する LINQ 標準クエリ演算子メソッドを作成できます。 標準クエリ演算子のカスタム実装は、結果を返すために遅延実行を使用する必要があります。

### <a name="remote-data"></a>リモート データ
 リモートデータソースの LINQ クエリを有効にするための最適なオプションは、<xref:System.Linq.IQueryable%601> インターフェイスを実装することです。 しかしこれは、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] などのプロバイダーをデータ ソースに対して拡張することとは別です。 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]などの既存の LINQ テクノロジを他の種類のデータソースに拡張するためのプロバイダーモデルは、Visual Studio 2008 では使用できません。

## <a name="iqueryable-linq-providers"></a>IQueryable LINQ プロバイダー
 <xref:System.Linq.IQueryable%601> を実装する LINQ プロバイダーは、複雑さが大きく異なる場合があります。 このセクションでは、さまざまなレベルの複雑度について説明します。

 複雑度が低めの `IQueryable` プロバイダーは、多くの場合、Web サービスの単一のメソッドとやり取りします。 この種のプロバイダーは処理するクエリに特定の情報を受け取るので非常に高い固有性を持ちます。 クローズされた型システムを持ち、おそらく 1 つの結果型を公開します。 クエリはほとんどの場合、標準クエリ演算子の <xref:System.Linq.Enumerable> 実装などを使用して、ローカルで実行されます。 複雑度が低めのプロバイダーは、クエリを表す式ツリーのメソッド呼び出し式を 1 つだけ調べ、残りのクエリのロジックは他の場所で処理されるようにします。

 複雑度が中レベルの `IQueryable` プロバイダーは、部分的に表現力が豊かなクエリ言語を持つデータ ソースを対象とします。 そのプロバイダーが Web サービスを対象とする場合、Web サービスの複数のメソッドとやり取りし、クエリが提示する問題に基づいて呼び出すメソッドを選択します。 中レベルの複雑度のプロバイダーは、簡単なプロバイダーより型システムが豊富ですが、それでもその種類は限られています。 たとえば、このレベルのプロバイダーは走査できる一対多リレーションシップを持つ型を公開する場合がありますが、ユーザー定義型のマッピング テクノロジは提供しません。

 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] プロバイダーなどの複雑な `IQueryable` プロバイダーは、完全な LINQ クエリを、SQL などの表現力が豊かなクエリ言語に変換する場合があります。 複雑なプロバイダーは、それほど複雑でないプロバイダーより一般的です。これは、より多様な質問をクエリで処理できるためです。 さらに、オープンな型システムを持つため、ユーザー定義型をマップするために広範なインフラストラクチャを含める必要があります。 複雑なプロバイダーの開発には、多大な労力を要します。

## <a name="see-also"></a>関連項目

- <xref:System.Linq.IQueryable%601>
- <xref:System.Collections.Generic.IEnumerable%601>
- <xref:System.Linq.Enumerable>
- [標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)
- [LINQ to Objects (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)
