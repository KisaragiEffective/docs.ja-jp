---
title: 作業の開始
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: db8a557a-fef8-4f4f-bb91-8cff7250ee25
ms.openlocfilehash: 3bff4e9f268e9eac84c244cb58eed8b4384e717d
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634692"
---
# <a name="getting-started"></a>作業の開始
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]を使用すると、メモリ内コレクションにアクセスする場合と同様に、LINQ テクノロジを使用して SQL データベースにアクセスできます。  
  
 たとえば、次のコードでは、`nw` データベースを表す `Northwind` オブジェクトが作成され、`Customers` テーブルが対象とされ、`Customers` からの `London` を選択するように行がフィルター処理され、取得する `CompanyName` の文字列が選択されます。  
  
 ループが実行されると、`CompanyName` の値のコレクションが取得されます。  
  
 [!code-csharp[DLinqGettingStarted#1](../../../../../../samples/snippets/csharp/VS_Snippets_Data/DLinqGettingStarted/cs/Program.cs#1)]
 [!code-vb[DLinqGettingStarted#1](../../../../../../samples/snippets/visualbasic/VS_Snippets_Data/DLinqGettingStarted/vb/Module1.vb#1)]  
  
## <a name="next-steps"></a>次のステップ  
 挿入や更新など、いくつかの追加の例については、「 [LINQ to SQL でできること](what-you-can-do-with-linq-to-sql.md)」を参照してください。  
  
 次に、チュートリアルで [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] の使用を実際に体験できます。 「[チュートリアルによる学習」を](learning-by-walkthroughs.md)参照してください。  
  
 最後に、 [LINQ to SQL を使用するための一般的な手順](typical-steps-for-using-linq-to-sql.md)を読んで、独自の [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] プロジェクトを開始する方法について説明します。  
  
## <a name="see-also"></a>関連項目

- [LINQ to SQL](index.md)
- [LINQ (C#) の概要](../../../../../csharp/programming-guide/concepts/linq/index.md)
- [LINQ の概要 (Visual Basic)](../../../../../visual-basic/programming-guide/concepts/linq/introduction-to-linq.md)
- [LINQ to SQL オブジェクト モデル](the-linq-to-sql-object-model.md)
