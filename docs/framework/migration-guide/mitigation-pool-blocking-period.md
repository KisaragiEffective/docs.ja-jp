---
title: '軽減策: プールのロック期間'
ms.date: 03/30/2017
ms.assetid: 92d2de20-79be-4df1-b182-144143a8866a
ms.openlocfilehash: 98396d4254975d1806a8477cbcd2380cb52ceaf3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73457844"
---
# <a name="mitigation-pool-blocking-period"></a>軽減策: プールのロック期間
Azure SQL データベースへの接続に関して、接続プールのブロック期間が削除されました。  
  
## <a name="additional-description"></a>その他の説明  
 .NET Framework 4.6.1 以前のバージョンでは、データベースへの接続時にアプリで一時的な接続エラーが発生した場合、接続はすぐに再試行されません。接続プールがエラーをキャッシュし、5 秒間から 1 分間エラーを再スローするためです。詳細については、「[SQL Server の接続プール (ADO.NET)](../data/adonet/sql-server-connection-pooling.md)」を参照してください。 この動作は、Azure SQL データベースへの接続時に問題となります。多くの場合、一時的なエラーが発生し、通常数秒内に回復します。 接続プールのブロック機能を使用すると、データベースが使用可能な場合でも、長い期間にわたってアプリがデータベースに接続できなくなるということです。 この動作が特に問題となるのは、Azure SQL データベースに接続し、数秒内でレンダリングする必要がある Web アプリの場合です。  
  
 .NET Framework 4.6.2 以降では、既知の Azure SQL データベースへの接続確立要求 (*.database.windows.net、\*.database.chinacloudapi.cn、\*.database.usgovcloudapi.net、\*.database.cloudapi.de) の場合、接続確立のエラーはキャッシュされません。 他のすべての接続を試みる場合は、接続プールのブロック期間が引き続き適用されます。  
  
## <a name="impact"></a>影響  
 この変更により、Azure SQL データベースへの接続確立がすぐに再試行するされるため、クラウド対応アプリケーションのパフォーマンスが向上します。  
  
## <a name="mitigation"></a>対応策  
 この変更から悪影響を受けるアプリの場合、新しい <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A?displayProperty=nameWithType> プロパティを設定することで、接続プールのブロック期間を構成できます。  プロパティ値が <xref:System.Data.SqlClient.PoolBlockingPeriod?displayProperty=nameWithType> 列挙型のメンバーである場合、次の 3 つの値のいずれかを使用できます。  
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock?displayProperty=nameWithType>
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.Auto?displayProperty=nameWithType>
  
- <xref:System.Data.SqlClient.PoolBlockingPeriod.NeverBlock?displayProperty=nameWithType>
  
 <xref:System.Data.SqlClient.SqlConnectionStringBuilder.PoolBlockingPeriod%2A> プロパティを <xref:System.Data.SqlClient.PoolBlockingPeriod.AlwaysBlock?displayProperty=nameWithType> に設定して、以前の動作を復元することができます。  
  
## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
