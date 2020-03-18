---
title: LINQ と ADO.NET
titleSuffix: ''
ms.date: 03/30/2017
ms.assetid: bf0c8f93-3ff7-49f3-8aed-f2b7ac938dec
ms.openlocfilehash: c5b56fa78ce0276953597d63b3d6e2f45d88c8ab
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094437"
---
# <a name="linq-and-adonet"></a>LINQ と ADO.NET

現在、多くのビジネス開発者は、2つ (またはそれ以上) のプログラミング言語を使用する必要があります。ビジネスロジックとC#プレゼンテーション層の高水準言語 (Visual や Visual Basic など) と、データベースと対話するためのクエリ言語 (transact-sql など) です。 開発者は実質的に複数の言語に精通していることが要求され、開発環境における言語の不整合が生じる原因にもなっています。 たとえば、データ アクセス API を使用してデータベースを照会するアプリケーションでは、クエリは文字列リテラルとして引用符で囲んで指定する必要があります。 このクエリ文字列は、無効な構文、参照先の列または行が実際に存在するかどうかなど、コンパイラに対して読み取ることができず、エラーをチェックしません。 クエリ パラメーターの型チェックや `IntelliSense` のサポートもありません。  
  
 統合言語クエリ (LINQ) を使用すると、開発者は、個別のクエリ言語を使用しなくても、アプリケーションコードにセットベースのクエリを作成できます。 LINQ クエリは、メモリ内のデータ構造、XML ドキュメント、SQL データベース、<xref:System.Data.DataSet> オブジェクトなど、さまざまな列挙可能なデータソース (<xref:System.Collections.IEnumerable> インターフェイスを実装するデータソース) に対して記述できます。 実装方法には違いがありますが、こうした列挙可能なデータ ソースはすべて同じ構文および言語構造を公開しています。 クエリはプログラミング言語のみで作成できるため、コンパイラによる認識も検証もできない文字列リテラルとしてクエリ言語を記述する必要はありません。 クエリをプログラミング言語に統合することにより、コンパイル時の型と構文チェックを提供し、`IntelliSense`することで、Visual Studio プログラマの生産性を向上させることができます。 クエリのデバッグやエラーの修正に伴う手間は、これらの機能によって軽減されます。  
  
 SQL テーブルのデータをメモリ内のオブジェクトに転送することは、面倒であるだけでなく間違いの元にもなります。 LINQ to DataSet および [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] によって実装される LINQ プロバイダーは、ソースデータを <xref:System.Collections.IEnumerable>ベースのオブジェクトコレクションに変換します。 プログラマからは、クエリの実行時も更新時も常にデータが <xref:System.Collections.IEnumerable> コレクションとして見えます。 これらのコレクションに対するクエリを記述する際には、`IntelliSense` の機能を完全に利用できます。  
  
 LINQ to DataSet、[!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)]、LINQ to Entities という3つの独立した統合言語クエリ (LINQ) テクノロジがあります。 LINQ to DataSet は、<xref:System.Data.DataSet> に対してより高度で最適化されたクエリを提供し [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] SQL Server データベーススキーマに対して直接クエリを実行できるようにします。また、LINQ to Entities を使用して Entity Data Model に対してクエリを実行できます。  
  
 次の図は、ADO.NET LINQ テクノロジが、高級プログラミング言語および LINQ 対応のデータ ソースとどのように関係しているかを簡単に表したものです。  
  
 ![LINQ to ADO.NET の概要](./media/dpue-linqtoadonetoverview-bpuedev11.gif "DPUE_LinqToAdoNetOverview_bpuedev11")  
  
 LINQ の詳細については、「[統合言語クエリ (linq)](../../../csharp/programming-guide/concepts/linq/index.md)」を参照してください。
  
 次のセクションでは、LINQ to DataSet、[!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)]、および LINQ to Entities の詳細について説明します。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 <xref:System.Data.DataSet> は、ADO.NET が構築される非接続型プログラミングモデルの重要な要素であり、広く使用されています。 LINQ to DataSet を使用すると、開発者は、他の多くのデータソースで使用できるのと同じクエリ定式化メカニズムを使用して、より豊富なクエリ機能を <xref:System.Data.DataSet> に組み込むことができます。 詳細については、「[LINQ to DataSet](linq-to-dataset.md)」を参照してください。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] は、概念モデルへのマッピングを必要としない開発者にとって有用なツールです。 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)]を使用すると、既存のデータベーススキーマに対して直接 LINQ プログラミングモデルを使用できます。 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)] を使用すると、開発者はデータを表す .NET Framework クラスを生成できます。 こうして生成されたクラスは、概念データ モデルではなく、直接データベースのテーブル、ビュー、ストアド プロシージャ、およびユーザー定義関数にマッピングされます。  
  
 [!INCLUDE[vbtecdlinq](../../../../includes/vbtecdlinq-md.md)]を使用すると、開発者は、XML などの他のデータソースに加えて、メモリ内コレクションや <xref:System.Data.DataSet>と同じ LINQ プログラミングパターンを使用して、ストレージスキーマに対して直接コードを記述できます。 詳細については、「 [LINQ to SQL](./sql/linq/index.md)」を参照してください。  
  
## <a name="linq-to-entities"></a>LINQ to Entities  
 現在、多くのアプリケーションが、リレーショナル データベースを基にして作成されています。 ある時点で、これらのアプリケーションにはリレーショナル形式で表されるデータと対話する必要が生じます。 データベース スキーマはアプリケーションの構築に理想的であるとは限らず、アプリケーションの概念モデルはデータベースの論理モデルと同じではありません。 Entity Data Model は、アプリケーションがデータをオブジェクトとして操作できるように、特定のドメインのデータをモデル化するために使用できる概念データモデルです。 詳細については、「 [ADO.NET Entity Framework](./ef/index.md)」を参照してください。  
  
 Entity Data Model では、リレーショナル データが .NET 環境にオブジェクトとして公開されます。 これにより、LINQ の利用に最適なオブジェクト レイヤーが実現されます。開発者は、ビジネス ロジックの構築に使用する言語で、データベースを照会するクエリを作成できます。 この機能は、LINQ to Entities と呼ばれます。 詳細については、「 [LINQ to Entities](./ef/language-reference/linq-to-entities.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [LINQ to DataSet](linq-to-dataset.md)
- [LINQ to SQL](./sql/linq/index.md)
- [LINQ to Entities](./ef/language-reference/linq-to-entities.md)
- [統合言語クエリ (LINQ)](../../../csharp/programming-guide/concepts/linq/index.md)
- [ADO.NET の概要](ado-net-overview.md)
