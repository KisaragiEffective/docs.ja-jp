---
title: データ バインディングと LINQ to DataSet
ms.date: 03/30/2017
ms.assetid: 310bff4a-32dd-4f20-a271-6dbd82912631
ms.openlocfilehash: 2b49e2a3ff0d95dcbceff8099f51993c0f08d64e
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634873"
---
# <a name="data-binding-and-linq-to-dataset"></a>データ バインディングと LINQ to DataSet
*データバインディング*は、アプリケーション UI とビジネスロジックの間の接続を確立するプロセスです。 バインドが適切に設定され、データから適切な通知が提供される場合、データの値が変更されると、そのデータにバインドされている要素に変更が自動的に反映されます。 <xref:System.Data.DataSet> はメモリ内データ表現であり、含まれているデータ ソースとは関係なく、一貫性のあるリレーショナル プログラミング モデルを提供します。 ADO.NET 2.0 の <xref:System.Data.DataView> では、<xref:System.Data.DataTable> に格納されているデータの並べ替えとフィルター処理を行うことができます。 この機能は、データ バインディング アプリケーションで一般に使用されます。 <xref:System.Data.DataView> を使用すると、さまざまな並べ替え順序を使用してテーブルのデータを公開したり、行の状態やフィルター式に基づいてデータをフィルター処理したりできます。 <xref:System.Data.DataView> オブジェクトの詳細については、「 [Dataviews](./dataset-datatable-dataview/dataviews.md)」を参照してください。  
  
 LINQ to DataSet を使用すると、開発者は統合言語クエリ (LINQ) を使用して、<xref:System.Data.DataSet> に対して複雑で強力なクエリを作成できます。 ただし、LINQ to DataSet クエリでは <xref:System.Data.DataRow> オブジェクトの列挙型が返されますが、これはバインドシナリオでは簡単に使用できません。 バインドを容易にするために、LINQ to DataSet クエリから <xref:System.Data.DataView> を作成できます。 この <xref:System.Data.DataView> では、クエリで指定されたフィルター処理と並べ替えを使用しますが、データバインディングに適しています。 LINQ to DataSet は、LINQ 式ベースのフィルター処理と並べ替えを提供することによって、<xref:System.Data.DataView> の機能を拡張します。これにより、文字列ベースのフィルター処理や並べ替えよりもはるかに複雑で強力なフィルター処理と並べ替え操作が可能になります。  
  
 <xref:System.Data.DataView> はクエリ自体を表すもので、クエリに基づくビューではありません。 単純なデータ バインディング モデルを提供する場合、<xref:System.Data.DataView> は、<xref:System.Windows.Forms.DataGrid> や <xref:System.Windows.Forms.DataGridView> などの UI コントロールにバインドされます。 当該テーブルの既定のビューを提供する場合、<xref:System.Data.DataView> は、<xref:System.Data.DataTable> から作成することもできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [DataView オブジェクトの作成](creating-a-dataview-object-linq-to-dataset.md)  
 <xref:System.Data.DataView> の作成に関する情報を提供します。  
  
 [DataView によるフィルター処理](filtering-with-dataview-linq-to-dataset.md)  
 <xref:System.Data.DataView> を使用してフィルター処理する方法について説明します。  
  
 [DataView による並べ替え](sorting-with-dataview-linq-to-dataset.md)  
 <xref:System.Data.DataView> を使用して並べ替えを行う方法について説明します。  
  
 [DataView の DataRowView コレクションの照会](querying-the-datarowview-collection-in-a-dataview.md)  
 <xref:System.Data.DataRowView> が公開する <xref:System.Data.DataView> コレクションを照会する方法について説明します。  
  
 [DataView のパフォーマンス](dataview-performance.md)  
 <xref:System.Data.DataView> およびパフォーマンスに関する情報を提供します。  
  
 [方法 : DataView オブジェクトを Windows フォーム DataGridView コントロールにバインドする](how-to-bind-a-dataview-object-to-a-winforms-datagridview-control.md)  
 <xref:System.Data.DataView> オブジェクトを <xref:System.Windows.Forms.DataGridView> にバインドする方法について説明します。  
  
## <a name="see-also"></a>関連項目

- [プログラミング ガイド](programming-guide-linq-to-dataset.md)
