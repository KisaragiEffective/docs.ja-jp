---
ms.openlocfilehash: 65d9edc1e7377a86f8185ebf28bb5bee3a3f887d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67803259"
---
### <a name="sqlconnectionopen-fails-on-windows-7-with-non-ifs-winsock-bsp-or-lsp-present"></a>IFS 以外の Winsock BSP または LSP が存在する Windows 7 で SqlConnection.Open が失敗する

|   |   |
|---|---|
|説明|<xref:System.Data.SqlClient.SqlConnection.Open> および <xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)> は、コンピューター上に IFS 以外の Winsock BSP または LSP が存在する Windows 7 コンピューターで実行している場合、.NET Framework 4.5 では失敗します。IFS 以外の BSP または LSP がインストールされているかどうかを確認するには、<code>netsh WinSock Show Catalog</code> コマンドを使用して、返される <code>Winsock Catalog Provider Entry</code> 項目をすべて調べてください。 Service Flags 値の <code>0x20000</code> ビットがセットされている場合、プロバイダーは IFS ハンドルを使用していて、正しく機能します。 <code>0x20000</code> ビットがクリアされている (セットされていない) 場合は、IFS 以外の BSP または LSP です。|
|提案される解決策|このバグは .NET framework 4.5.2 で修正されたため、.NET Framework をアップグレードすることによって回避できます。 または、インストールされている IFS 以外の Winsock LSP を削除することによって回避できます。|
|スコープ|Minor|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Data.SqlClient.SqlConnection.Open?displayProperty=nameWithType></li><li><xref:System.Data.SqlClient.SqlConnection.OpenAsync(System.Threading.CancellationToken)?displayProperty=nameWithType></li></ul>|
