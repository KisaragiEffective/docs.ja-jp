---
title: Entity Framework 用 SqlClient の型
ms.date: 03/30/2017
ms.assetid: f2a95ead-c845-4e97-9fb3-04b444f7ed81
ms.openlocfilehash: d132583bba2520d37693be6c4b085cfa514003e0
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73737845"
---
# <a name="sqlclient-for-entity-frameworktypes"></a>Entity Framework 用 SqlClient の型
.NET Framework Data Provider for SQL Server (SqlClient) プロバイダー マニフェスト ファイルには、プロバイダー プリミティブ型のリスト、それぞれの型のファセット、概念モデルとストレージ モデルのプリミティブ型とのマッピング、および概念モデルとストレージ モデルのプリミティブ型間での昇格と変換の規則が含まれています。  
  
 次の表では、SQL Server 2008、SQL Server 2005、および SQL Server 2000 データベースの型と、これらの型を概念モデルの型にマップする方法について説明します。 いくつかの新しい型が SQL Server の新しいバージョンで導入されており、これらの型は SQL Server の古いバージョンではサポートされていません。 これらの型については次の表で説明します。  
  
|プロバイダー型の<br /><br /> name|プロバイダー型の<br /><br /> 属性|`EDMSimpleType`<br /><br /> name|ファセット|  
|----------------------------|----------------------------------|------------------------------|------------|  
|`bit`|N/A|`Edm.Boolean`|N/A|  
|`tinyint`|N/A|`Edm.Byte`|N/A|  
|`smallint`|N/A|`Edm.Int16`|N/A|  
|`int`|N/A|`Edm.Int32`|N/A|  
|`bigint`|N/A|`Edm.Int64`|N/A|  
|`float`|N/A|`Edm.Double`|N/A|  
|`real`|N/A|`Edm.Double`|N/A|  
|`decimal`|N/A|`Edm.Decimal`|精度<br /><br /> -最小: 1<br /><br /> -最大:38<br /><br /> -既定値:18<br /><br /> -Constant: False<br /><br /> 段階<br /><br /> -最小: 0<br /><br /> -最大:38<br /><br /> -既定値: 0<br /><br /> -Constant: False|  
|`numeric`|N/A|`Edm.Decimal`|精度<br /><br /> -最小: 1<br /><br /> -最大:38<br /><br /> -既定値:18<br /><br /> -Constant: False<br /><br /> 段階<br /><br /> -最小: 0<br /><br /> -最大:38<br /><br /> -既定値: 0<br /><br /> -Constant: False|  
|`smallmoney`|N/A|`Edm.Decimal`|精度<br /><br /> -既定値:10<br /><br /> -Constant: True<br /><br /> 段階<br /><br /> -既定値: 4<br /><br /> -Constant: True|  
|`money`|N/A|`Edm.Decimal`|精度<br /><br /> -既定値:19<br /><br /> -Constant: True<br /><br /> 段階<br /><br /> -既定値: 4<br /><br /> -Constant: True|  
|`binary`|N/A|`Edm.Binary`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 8000<br /><br /> -既定値: 8000<br /><br /> -Constant: False<br /><br /> FixedLength<br /><br /> -既定値: True<br /><br /> -Constant: True|  
|`varbinary`|N/A|`Edm.Binary`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 8000<br /><br /> -既定値: 8000<br /><br /> -Constant: False<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`varbinary(max)`<br /><br /> 注: この型は SQL Server 2000 ではサポートされていません。|N/A|`Edm.Binary`|MaxLength<br /><br /> -既定値: 214748364780<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`image`|N/A|`Edm.Binary`|MaxLength<br /><br /> -既定値: 2147483647<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`timestamp`|N/A|`Edm.Binary`|MaxLength<br /><br /> -既定値: 8<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: True<br /><br /> -Constant: True|  
|`rowversion`|N/A|`Edm.Binary`|MaxLength<br /><br /> -既定値: 8<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: True<br /><br /> -Constant: True|  
|`smalldatetime`|N/A|`Edm.DateTime`|精度<br /><br /> -既定値: 0<br /><br /> -Constant: True|  
|`datetime`|N/A|`Edm.DateTime`|精度<br /><br /> -既定値: 3<br /><br /> -Constant: True|  
|`date`<br /><br /> 注: この型は SQL Server 2005 および SQL Server 2000 ではサポートされていません。|N/A|`Edm.DateTime`|精度<br /><br /> -既定値: 0<br /><br /> -Constant: False|  
|`time`<br /><br /> 注: この型は SQL Server 2005 および SQL Server 2000 ではサポートされていません。|N/A|`Edm.Time`|精度<br /><br /> -既定値: 7<br /><br /> -Constant: False|  
|`datetime2`<br /><br /> 注: この型は SQL Server 2005 および SQL Server 2000 ではサポートされていません。|N/A|`Edm.DateTime`|精度<br /><br /> -既定値: 7<br /><br /> -Constant: False|  
|`datetimeoffset`<br /><br /> 注: この型は SQL Server 2005 および SQL Server 2000 ではサポートされていません。|N/A|`Edm.DateTimeOffset`|精度<br /><br /> -既定値: 7<br /><br /> -Constant: False|  
|`nvarchar`<br /><br /> 注: この型は SQL Server 2000 ではサポートされていません。|N/A|`Edm.String`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 4000<br /><br /> -既定値: 4000<br /><br /> -Constant: False<br /><br /> Unicode:<br /><br /> -既定値: True<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`varchar`<br /><br /> 注: この型は SQL Server 2000 ではサポートされていません。|N/A|`Edm.String`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 8000<br /><br /> -既定値: 8000<br /><br /> -Constant: False<br /><br /> Unicode:<br /><br /> -既定値: False<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`char`|N/A|`Edm.String`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 8000<br /><br /> -既定値: 8000<br /><br /> -Constant: False<br /><br /> Unicode:<br /><br /> -既定値: False<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: True<br /><br /> -Constant: True|  
|`nchar`|N/A|`Edm.String`|MaxLength<br /><br /> -最小: 1<br /><br /> -最大: 4000<br /><br /> -既定値: 4000<br /><br /> -Constant: False<br /><br /> Unicode:<br /><br /> -既定値: True<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: True<br /><br /> -Constant: True|  
|`varchar`(`max`)|N/A|`Edm.String`|MaxLength<br /><br /> -既定値: 2147483647<br /><br /> -Constant: True<br /><br /> Unicode:<br /><br /> -既定値: False<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`nvarchar`(`max`)|N/A|`Edm.String`|MaxLength<br /><br /> -既定値: 1073741823<br /><br /> -Constant: True<br /><br /> Unicode:<br /><br /> -既定値: True<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`ntext`|等しい比較可能: False<br /><br /> 順序を比較する: False|`Edm.String`|MaxLength<br /><br /> -既定値: 1073741823<br /><br /> -Constant: True<br /><br /> Unicode:<br /><br /> -既定値: False<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`text`|等しい比較可能: False<br /><br /> 順序を比較する: False|`Edm.String`|MaxLength<br /><br /> -既定値: 2147483647<br /><br /> -Constant: True<br /><br /> Unicode:<br /><br /> -既定値: False<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
|`Unique`<br /><br /> `identifier`|等しい比較可能: True<br /><br /> 順序を比較できる: True|`Edm.Guid`|N/A|  
|`xml`|等しい比較可能: False<br /><br /> 順序を比較する: False|`Edm.String`|MaxLength<br /><br /> -既定値: 1073741823<br /><br /> -Constant: True<br /><br /> Unicode:<br /><br /> -既定値: True<br /><br /> -Constant: True<br /><br /> FixedLength<br /><br /> -既定値: False<br /><br /> -Constant: True|  
  
## <a name="see-also"></a>関連項目

- [CSDL、SSDL、および MSL 仕様](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
