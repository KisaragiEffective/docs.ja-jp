---
description: '詳細情報: 方法:データベース関数を呼び出す'
title: '方法: データベース関数を呼び出す'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 79038efa-15bf-464a-83e2-35fe145252ce
ms.openlocfilehash: d5c5854d375546d1604b6e27e6f757f1e126f9b4
ms.sourcegitcommit: ddf7edb67715a5b9a45e3dd44536dabc153c1de0
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/06/2021
ms.locfileid: "99724519"
---
# <a name="how-to-call-database-functions"></a>方法: データベース関数を呼び出す

<xref:System.Data.Objects.SqlClient.SqlFunctions> クラスには、LINQ to Entities クエリで使用する SQL Server 関数を公開するメソッドが含まれています。 LINQ to Entities クエリで <xref:System.Data.Objects.SqlClient.SqlFunctions> メソッドを使用すると、対応するデータベース関数がデータベースで実行されます。  
  
> [!NOTE]
> 値のセットに対して計算を実行して 1 つの値を返すデータベース関数 (集計データベース関数とも呼ばれる) は、直接呼び出すことができます。 他の正規関数は、LINQ to Entities クエリの一部としてしか呼び出すことができません。 集計関数を直接呼び出すには、その関数に <xref:System.Data.Objects.ObjectQuery%601> を渡す必要があります。 詳細については、以下の 2 番目の例を参照してください。  
  
> [!NOTE]
> <xref:System.Data.Objects.SqlClient.SqlFunctions> クラスのメソッドは、SQL Server 関数に固有です。 他のプロバイダーから、データベース関数を公開する同様のクラスを入手できることがあります。  
  
## <a name="example"></a>例  

 次の例では、[AdventureWorks Sales Model](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) を使用します。 この例では、<xref:System.Data.Objects.SqlClient.SqlFunctions.CharIndex%2A> メソッドを使用する LINQ to Entities クエリを実行して、姓が "Si" で始まる連絡先をすべて返します。  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#3](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#3)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#3](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#3)]  
  
## <a name="example"></a>例  

 次の例では、[AdventureWorks Sales Model](https://github.com/Microsoft/sql-server-samples/releases/tag/adventureworks) を使用します。 この例では、集計 <xref:System.Data.Objects.SqlClient.SqlFunctions.ChecksumAggregate%2A> メソッドを直接呼び出します。 <xref:System.Data.Objects.ObjectQuery%601> は関数に渡されているため、LINQ to Entities クエリに含まれていなくても呼び出すことができる点に注意してください。  
  
 [!code-csharp[DP L2E CanonicalAndStoreFunctions#4](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp l2e canonicalandstorefunctions/cs/program.cs#4)]
 [!code-vb[DP L2E CanonicalAndStoreFunctions#4](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/dp l2e canonicalandstorefunctions/vb/module1.vb#4)]  
  
## <a name="see-also"></a>関連項目

- [LINQ to Entities クエリ内の関数の呼び出し](calling-functions-in-linq-to-entities-queries.md)
- [LINQ to Entities でのクエリ](queries-in-linq-to-entities.md)
