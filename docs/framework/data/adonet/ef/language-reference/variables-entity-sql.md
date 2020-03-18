---
title: 変数 (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 3eed222a-f8f6-46b6-9cd5-220cc0e4e5d8
ms.openlocfilehash: bb8e6ebe81dacc7ec0f45fdde65b9c18cfd76789
ms.sourcegitcommit: 628e8147ca10187488e6407dab4c4e6ebe0cac47
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/15/2019
ms.locfileid: "72319199"
---
# <a name="variables-entity-sql"></a>変数 (Entity SQL)
## <a name="variable"></a>[Variable]  
 変数式は、現在のスコープで定義されている名前付きの式への参照です。 変数参照は、[識別子](identifiers-entity-sql.md)で定義されている有効な [!INCLUDE[esql](../../../../../../includes/esql-md.md)] 識別子である必要があります。  
  
 次の例は、式における変数の使用方法を示しています。 FROM 句の `c` は変数の定義です。 SELECT 句内で使用された `c` は、変数参照を表します。  
  
```sql  
select c   
from LOB.customers as c  
```  
  
## <a name="see-also"></a>参照

- [識別子](identifiers-entity-sql.md)
- [パラメーター](parameters-entity-sql.md)
- [Entity SQL の概要](entity-sql-overview.md)
