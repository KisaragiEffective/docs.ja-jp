---
title: CLR ストアド プロシージャ
ms.date: 03/30/2017
ms.assetid: fd7eea9b-218a-4988-8c9a-8abcc6031c66
ms.openlocfilehash: ca0fd2d33f0ad56c96fb63530e6ba3f7f7890f81
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77450363"
---
# <a name="clr-stored-procedures"></a>CLR ストアド プロシージャ
ストアド プロシージャはスカラー式では使用できないルーチンです。 ストアド プロシージャは、表形式の結果とメッセージをクライアントに返したり、データ定義言語 (DDL) ステートメントおよびデータ操作言語 (DML) ステートメントを呼び出したり、出力パラメーターを返したりすることができます。  
  
> [!NOTE]
> Microsoft Visual Basic では、出力パラメーターのサポートが Microsoft Visual C# と異なります。 次に示すように、パラメーターを参照によって渡し、\<Out () > 属性を適用して出力パラメーターを表す必要があります。  
  
```vb
Public Shared Sub ExecuteToClient( <Out()> ByRef number As Integer)  
```
  
詳細については、使用している SQL Server バージョンの[SQL Server ドキュメント](/sql)のバージョンを参照してください。
  
 **SQL Server のドキュメント**

1. [CLR ストアド プロシージャ](https://docs.microsoft.com/previous-versions/sql/sql-server-2008/ms131094(v=sql.100))  
  
## <a name="see-also"></a>参照

- [マネージコードでの SQL Server 2005 オブジェクトの作成](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/6s0s2at1(v=vs.90))
- [ADO.NET の概要](../ado-net-overview.md)
