---
title: LINQ to ADO.NET (ポータル ページ)
ms.date: 07/20/2015
ms.assetid: 6bd269b4-3509-4688-b672-836008704182
ms.openlocfilehash: 84412e43a9d6b1e256e4ac8306a94126a3eaaaf4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635549"
---
# <a name="linq-to-adonet-portal-page"></a>LINQ to ADO.NET (ポータル ページ)
LINQ to ADO.NET では、統合言語クエリ (LINQ) プログラミング モデルを使用して ADO.NET 内の列挙可能なオブジェクトに対してクエリを実行できます。  
  
> [!NOTE]
> LINQ to ADO.NET のドキュメントは、.NET Framework SDK の ADO.NET のセクションにあります。「[LINQ と ADO.NET](../../../../framework/data/adonet/linq-and-ado-net.md)」。  
  
 ADO.NET 統合言語クエリ (LINQ) には、3 つのテクノロジがあります。LINQ to DataSet、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]、LINQ to Entities です。 LINQ to DataSet では、<xref:System.Data.DataSet> に対する高度で最適化されたクエリの実行が提供されます。[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] では、SQL Server データベース スキーマに対して直接クエリを実行できます。LINQ to Entities では、Entity Data Model のクエリを実行できます。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 <xref:System.Data.DataSet> は、ADO.NET で最も幅広く使用されているコンポーネントの 1 つであり、ADO.NET の基礎である非接続型プログラミングの重要な要素です。 こうした突出した特長がある反面、<xref:System.Data.DataSet> のクエリ機能には制限もあります。  
  
 LINQ to DataSet では、他の多くのデータ ソースで使用できるのと同じクエリ機能を使用することで、豊富なクエリ機能を <xref:System.Data.DataSet> に組み込むことができます。  
  
 詳細については、「[LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)」を参照してください。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] には、リレーショナル データをオブジェクトとして管理するためのランタイム インフラストラクチャが用意されています。 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] では、リレーショナル データベースのデータ モデルが、開発者のプログラミング言語で表されるオブジェクト モデルに対応付けられています。 アプリケーションを実行すると、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] は、オブジェクト モデルの統合言語クエリを SQL に変換し、それをデータベースに送信して実行します。 データベースから結果が返されると、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] で元の操作できるオブジェクトに変換されます。  
  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] には、データベースのストアド プロシージャ、ユーザー定義関数、オブジェクト モデルの継承のサポートが含まれています。  
  
 詳細については、「[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)」を参照してください。  
  
## <a name="linq-to-entities"></a>LINQ to Entities  
 Entity Data Model では、リレーショナル データが .NET 環境にオブジェクトとして公開されます。 これにより、LINQ の利用に最適なオブジェクト レイヤーが実現されます。開発者は、ビジネス ロジックの構築に使用する言語で、データベースを照会するクエリを作成できます。 この機能は、LINQ to Entities と呼ばれます。 LINQ の詳細については、「[LINQ to Entities](../../../../framework/data/adonet/ef/language-reference/linq-to-entities.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [LINQ と ADO.NET](../../../../framework/data/adonet/linq-and-ado-net.md)
- [統合言語クエリ (LINQ) (C#)](./index.md)
