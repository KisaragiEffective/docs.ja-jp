---
title: LINQ の概要
ms.date: 07/20/2015
ms.assetid: c6339c12-9b2d-433e-961c-0d2b7f0091c2
ms.openlocfilehash: add442583bd81665533b704c0c9721b111cddd78
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74338904"
---
# <a name="introduction-to-linq-visual-basic"></a>LINQ の概要 (Visual Basic)
統合言語クエリ (LINQ) は、.NET Framework Version 3.5 で導入された、オブジェクトとデータの溝を埋める画期的な手法です。  
  
 これまでは、データに対するクエリは、コンパイル時の型チェックや IntelliSense のサポートなしに、単純な文字列として表現されてきました。 さらに、SQL データベース、XML ドキュメント、さまざまな Web サービスといったデータ ソースの種類ごとに、異なるクエリ言語を習得する必要がありました。 LINQ は、*クエリ*を Visual Basic 内のファーストクラスの言語コンストラクトにします。 厳密に型指定されているオブジェクトのコレクションに対して、言語キーワードと使い慣れた演算子を用いてクエリを記述します。  
  
 LINQ クエリは、SQL Server データベース、XML ドキュメント、ADO.NET データセット、および <xref:System.Collections.IEnumerable> または汎用 <xref:System.Collections.Generic.IEnumerable%601> インターフェイスをサポートするオブジェクトの任意のコレクションに対して Visual Basic で記述できます。 サード パーティからも、多くの Web サービスとその他のデータベース実装に対する LINQ のサポートが提供されます。  
  
 新しいプロジェクトで LINQ クエリを使用できるほか、既存のプロジェクトで LINQ 以外のクエリを使用することもできます。 唯一の要件は、プロジェクトが .NET Framework 3.5 以降をターゲットとしていることです。  
  
 次の Visual Studio の図は、SQL Server データベースに対して部分的に完了した LINQ クエリを示しています。このクエリは C# および Visual Basic の両方で記述されており、完全な型チェックと IntelliSense に対応しています。  
  
 ![Intellisense を使用する LINQ クエリを示す図。](./media/introduction-to-linq/linq-query-intellisense.png)  
  
## <a name="next-steps"></a>次の手順  
 LINQ の詳細については、まず、 [Visual Basic の linq ではじめに](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)はじめにセクションの基本的な概念を理解してから、関心のある linq テクノロジのドキュメントを参照してください。  
  
- SQL Server データベース: [LINQ to SQL](../../../../framework/data/adonet/sql/linq/index.md)  
  
- XML ドキュメント: [LINQ to XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml.md)  
  
- ADO.NET データセット: [LINQ to DataSet](../../../../framework/data/adonet/linq-to-dataset.md)  
  
- .NET のコレクション、ファイル、文字列など: [LINQ to Objects (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md)  
  
## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/index.md)
