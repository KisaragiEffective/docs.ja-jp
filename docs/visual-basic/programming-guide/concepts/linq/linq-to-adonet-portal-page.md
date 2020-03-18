---
title: LINQ to ADO.NET (ポータル ページ)
ms.date: 07/20/2015
ms.assetid: bbbd7c76-2981-4b91-b8d2-437547181f52
ms.openlocfilehash: d5d7ba8230ae9737d4938c1c8c55f7cd4330b117
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636498"
---
# <a name="linq-to-adonet-portal-page"></a>LINQ to ADO.NET (ポータル ページ)
LINQ to ADO.NET を使用すると、統合言語クエリ (LINQ) プログラミングモデルを使用して、ADO.NET 内の列挙可能なオブジェクトに対してクエリを実行できます。  
  
> [!NOTE]
> LINQ to ADO.NET のドキュメントは、.NET Framework SDK: [LINQ と ADO.NET](../../../../framework/data/adonet/linq-and-ado-net.md)の ADO.NET セクションにあります。
  
 LINQ to DataSet、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)]、LINQ to Entities という3つの独立した統合言語クエリ (LINQ) テクノロジがあります。 LINQ to DataSet では、<xref:System.Data.DataSet> に対する高度で最適化されたクエリの実行が提供されます。[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] では、SQL Server データベース スキーマに対して直接クエリを実行できます。LINQ to Entities では、Entity Data Model のクエリを実行できます。  
  
## <a name="linq-to-dataset"></a>LINQ to DataSet  
 <xref:System.Data.DataSet> は、ADO.NET で最も幅広く使用されているコンポーネントの 1 つであり、ADO.NET の基礎である非接続型プログラミングの重要な要素です。 こうした突出した特長がある反面、<xref:System.Data.DataSet> のクエリ機能には制限もあります。  
  
 LINQ to DataSet では、他の多くのデータ ソースで使用できるのと同じクエリ機能を使用することで、豊富なクエリ機能を <xref:System.Data.DataSet> に組み込むことができます。  
  
 詳細については、「[LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)」を参照してください。  
  
## <a name="linq-to-sql"></a>LINQ to SQL  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] には、リレーショナル データをオブジェクトとして管理するためのランタイム インフラストラクチャが用意されています。 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] では、リレーショナル データベースのデータ モデルが、開発者のプログラミング言語で表されるオブジェクト モデルに対応付けられています。 アプリケーションを実行すると、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] は、オブジェクト モデルの統合言語クエリを SQL に変換し、それをデータベースに送信して実行します。 データベースから結果が返されると、[!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] で元の操作できるオブジェクトに変換されます。  
  
 [!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] には、データベースのストアド プロシージャ、ユーザー定義関数、オブジェクト モデルの継承のサポートが含まれています。  
  
 詳細については、「[LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)」を参照してください。  
  
## <a name="linq-to-entities"></a>LINQ to Entities  
 Entity Data Model では、リレーショナル データが .NET 環境にオブジェクトとして公開されます。 これにより、オブジェクトレイヤーは LINQ のサポートに最適なターゲットになり、開発者はビジネスロジックの構築に使用される言語からデータベースに対するクエリを作成できます。 この機能は、LINQ to Entities と呼ばれます。 LINQ の詳細については、「[LINQ to Entities](../../../../framework/data/adonet/ef/language-reference/linq-to-entities.md)」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [LINQ と ADO.NET](../../../../framework/data/adonet/linq-and-ado-net.md)
- [統合言語クエリ (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/index.md)
