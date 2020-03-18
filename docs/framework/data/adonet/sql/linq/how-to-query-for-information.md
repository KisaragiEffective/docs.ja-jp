---
title: '方法 : クエリで情報を取得する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e538d288-2070-40ca-9da6-4fbc68cd6ad0
ms.openlocfilehash: 3380b486da33a5dc083ed51f6705e666978df197
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634652"
---
# <a name="how-to-query-for-information"></a>方法 : クエリで情報を取得する
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] のクエリでは、LINQ のクエリと同じ構文を使用します。 唯一の違いは、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] クエリで参照されるオブジェクトがデータベース内の要素にマップされる点です。 詳細については、「[LINQ クエリの概要 (C#)](../../../../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md)」を参照してください。  
  
 [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] は、作成したクエリを同等の SQL クエリに変換し、それをサーバーに送って処理します。  
  
 LINQ クエリの機能の中には、[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] アプリケーションで特に注意が必要なものもあります。 詳細については、「[クエリの概念](query-concepts.md)」を参照してください。  
  
## <a name="example"></a>使用例  
 次のクエリは、ロンドンからの顧客のリストを取得します。 この例の `Customers` は、Northwind サンプル データベース内のテーブルです。  
  
 [!code-csharp[DLinqQuerying#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqQuerying/cs/Program.cs#1)]
 [!code-vb[DLinqQuerying#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqQuerying/vb/Module1.vb#1)]  
  
## <a name="see-also"></a>関連項目

- [オブジェクト モデルの作成](creating-the-object-model.md)
- [サンプル データベースのダウンロード](downloading-sample-databases.md)
- [データベースに対するクエリの実行](querying-the-database.md)
