---
title: ジェネリック メソッド Field および SetField (LINQ to DataSet)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 1883365f-9d6c-4ccb-9187-df309f47706d
ms.openlocfilehash: 310eb3ccecc3159234ed362ed044be7ad704dde4
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75634834"
---
# <a name="generic-field-and-setfield-methods-linq-to-dataset"></a>ジェネリック メソッド Field および SetField (LINQ to DataSet)
LINQ to DataSet には、列の値にアクセスするための拡張メソッドが <xref:System.Data.DataRow> クラスに用意されています。 <xref:System.Data.DataRowExtensions.Field%2A> メソッドと <xref:System.Data.DataRowExtensions.SetField%2A> メソッドです。 開発者はこれらのメソッドを使用することで、列値に容易にアクセスできます。特に強化されている点は Null 値の扱いです。 <xref:System.Data.DataSet> は <xref:System.DBNull.Value?displayProperty=nameWithType> を使用して null 値を表しますが、LINQ では <xref:System.Nullable> と <xref:System.Nullable%601> の型が使用されます。 <xref:System.Data.DataRow> の既存の列アクセサーを使用するには、return オブジェクトを適切な型にキャストする必要があります。 <xref:System.Data.DataRow> 内の特定のフィールドを null にできる場合、null 値を明示的にチェックする必要があります。これは、<xref:System.DBNull.Value?displayProperty=nameWithType> を返し、暗黙的に別の型にキャストすると、<xref:System.InvalidCastException>がスローされるためです。 次の例では、<xref:System.Data.DataRow.IsNull%2A?displayProperty=nameWithType> メソッドを使用して null 値をチェックしなかった場合、インデクサーが <xref:System.DBNull.Value?displayProperty=nameWithType> を返し、<xref:System.String>にキャストしようとすると、例外がスローされます。  
  
 [!code-csharp[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#whereisnull)]
 [!code-vb[DP LINQ to DataSet Examples#WhereIsNull](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#whereisnull)]  
  
 <xref:System.Data.DataRowExtensions.Field%2A> は、<xref:System.Data.DataRow> の列値にアクセスするためのメソッドです。<xref:System.Data.DataRowExtensions.SetField%2A> は、<xref:System.Data.DataRow> の列値を設定するためのメソッドです。 <xref:System.Data.DataRowExtensions.Field%2A> メソッドも <xref:System.Data.DataRowExtensions.SetField%2A> メソッドも Null 許容型を扱うことができるため、前出の例のように Null 値を明示的にチェックする必要はありません。 また、どちらのメソッドもジェネリック メソッドであるため、戻り値の型をキャストする必要もありません。  
  
 次の例では、<xref:System.Data.DataRowExtensions.Field%2A> メソッドを使用します。  
  
 [!code-csharp[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/csharp/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/CS/Program.cs#where3)]
 [!code-vb[DP LINQ to DataSet Examples#Where3](../../../../samples/snippets/visualbasic/VS_Snippets_ADO.NET/DP LINQ to DataSet Examples/VB/Module1.vb#where3)]  
  
 `T` メソッドおよび <xref:System.Data.DataRowExtensions.Field%2A> メソッドのジェネリック パラメーター <xref:System.Data.DataRowExtensions.SetField%2A> に指定するデータ型は、基になる値の型と一致している必要があります。 それ以外の場合、<xref:System.InvalidCastException> 例外がスローされます。 指定する列の名前も <xref:System.Data.DataSet> 内の列名と一致している必要があります。一致していない場合、<xref:System.ArgumentException> がスローされます。 どちらの場合も、例外は、実行時にデータが列挙されて、クエリが実行されたときにスローされます。  
  
 <xref:System.Data.DataRowExtensions.SetField%2A> メソッド自体は、型変換を一切実行しません。 ただし、型変換がまったく発生しないということではありません。 <xref:System.Data.DataRowExtensions.SetField%2A> メソッドは、<xref:System.Data.DataRow> クラスの ADO.NET 動作を公開します。 <xref:System.Data.DataRow> オブジェクトが型変換を実行すると、変換後の値が <xref:System.Data.DataRow> オブジェクトに保存されます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Data.DataRowExtensions>
