---
ms.openlocfilehash: e7154919d6a09a04e650d5546feb2ae6c6cc912f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67859179"
---
### <a name="httpruntimeappdomainapppath-throws-a-nullreferenceexception"></a>HttpRuntime.AppDomainAppPath で NullReferenceException がスローされる

|   |   |
|---|---|
|説明|ランタイムによってスローされる、.NET Framework 4.6.2、<code>T:System.NullReferenceException</code>取得するときに、 <code>P:System.Web.HttpRuntime.AppDomainAppPath</code> null 文字を含む値です。 .NET Framework 4.6.1 と以前のバージョンでは、ランタイムによってスローされる、<code>T:System.ArgumentNullException</code>です。|
|提案される解決策|次のいずれかでこの変更に対応できます。<ul><li>アプリケーションが .NET Framework 4.6.2 で実行されている場合には <code>T:System.NullReferenceException</code> を処理します。</li><li>.NET Framework 4.7 にアップグレードします。以前の動作が復元され、<code>T:System.ArgumentNullException</code> がスローされます。</li></ul>|
|スコープ|エッジ|
|バージョン|4.6.2|
|[種類]|再ターゲット中|
|影響を受ける API|<ul><li><xref:System.Web.HttpRuntime.AppDomainAppPath?displayProperty=nameWithType></li></ul>|
