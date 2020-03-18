---
title: DataRelation の移動
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: e5e673f4-9b44-45ae-aaea-c504d1cc5d3e
ms.openlocfilehash: 73523297454be37716acedad13498954ef9a89a0
ms.sourcegitcommit: ad800f019ac976cb669e635fb0ea49db740e6890
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/29/2019
ms.locfileid: "73040344"
---
# <a name="navigating-datarelations"></a>DataRelation の移動
<xref:System.Data.DataRelation> の主な機能の 1 つは、<xref:System.Data.DataTable> の 1 つの <xref:System.Data.DataSet> から別の  を移動できることです。 これにより、関連する**datatable**から1つの**DataRow**が渡されたときに、1つの**datatable**内の関連するすべての <xref:System.Data.DataRow> オブジェクトを取得できます。 たとえば、customers テーブルと orders テーブルの間に**DataRelation**を確立した後、 **GetChildRows**を使用して特定の顧客行のすべての注文行を取得できます。  
  
 次のコード例では、 **Customers**テーブルと**データセット**の**Orders**テーブルの間に**DataRelation**を作成し、各顧客のすべての注文を返します。  
  
 [!code-csharp[DataWorks Data.DataTableRelation#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.DataTableRelation/CS/source.cs#1)]
 [!code-vb[DataWorks Data.DataTableRelation#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.DataTableRelation/VB/source.vb#1)]  
  
 上記の例に基づいて、4 つのテーブルを相互に関連付け、そのテーブルのリレーションシップ間を移動する例を次に示します。 前の例と同様に、 **CustomerID**は**Customers**テーブルを**Orders**テーブルに関連付けます。 **Customers**テーブルの顧客ごとに、 **orders**テーブル内のすべての子行が決定され、特定の顧客が持つ注文の数とその**OrderID**値が返されます。  
  
 展開された例では、 **OrderDetails**テーブルと**Products**テーブルからも値が返されます。 **Orders**テーブルは、 **OrderID**を使用して**OrderDetails**テーブルに関連付けられており、顧客の注文ごとに、どの製品と数量が注文されたかを判断します。 **OrderDetails**テーブルに含まれるのは注文済み製品の**productid**のみであるため、 **OrderDetails**は ProductName**を使用する** **製品**に関連付けられ、 **ProductName**が返されます。 このリレーションシップでは、 **Products**テーブルが親であり、 **Order Details**テーブルが子です。 その結果、 **OrderDetails**テーブルを反復処理するときに、 **GetParentRow**が呼び出され、関連する**ProductName**の値が取得されます。  
  
 **Customers**テーブルと**Orders**テーブルに対して**DataRelation**が作成された場合、 **createConstraints**フラグに値が指定されていないことに注意してください (既定値は**true**です)。 これは、 **Orders**テーブルのすべての行に、親**Customers**テーブルに存在する**CustomerID**値があることを前提としています。 **Customers**テーブルに存在しない**Orders**テーブルに**CustomerID**が存在する場合、<xref:System.Data.ForeignKeyConstraint> によって例外がスローされます。  
  
 子列に親列に含まれていない値が含まれている可能性がある場合は、 **DataRelation**を追加するときに**createConstraints**フラグを**false**に設定します。 この例では、 **Orders**テーブルと**OrderDetails**テーブルの間の**DataRelation**に対して**createConstraints**フラグが**false**に設定されています。 これにより、アプリケーションは**OrderDetails**テーブルからすべてのレコードを返すことができ、実行時の例外を生成することなく**Orders**テーブルのレコードのサブセットのみを返すことができます。 展開された例では、次の形式で出力が生成されます。  
  
```output  
Customer ID: NORTS  
  Order ID: 10517  
        Order Date: 4/24/1997 12:00:00 AM  
           Product: Filo Mix  
          Quantity: 6  
           Product: Raclette Courdavault  
          Quantity: 4  
           Product: Outback Lager  
          Quantity: 6  
  Order ID: 11057  
        Order Date: 4/29/1998 12:00:00 AM  
           Product: Outback Lager  
          Quantity: 3  
```  
  
 次のコード例は、返される**Orders**テーブル内のレコードのサブセットだけを使用して、 **OrderDetails**テーブルと**Products**テーブルの値が返される、拡張されたサンプルです。  
  
 [!code-csharp[DataWorks Data.DataTableNavigation#1](../../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DataWorks Data.DataTableNavigation/CS/source.cs#1)]
 [!code-vb[DataWorks Data.DataTableNavigation#1](../../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DataWorks Data.DataTableNavigation/VB/source.vb#1)]  
  
## <a name="see-also"></a>関連項目

- [DataSet、DataTable、および DataView](index.md)
- [ADO.NET の概要](../ado-net-overview.md)
