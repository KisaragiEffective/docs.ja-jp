---
ms.openlocfilehash: 09e3f0e168e0dcbe79d8ee7216f2671c67bfb87e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67802965"
---
### <a name="wpf-spell-checking-fails-in-unexpected-ways"></a>WPF スペル チェックが予期しない方法で失敗する

|   |   |
|---|---|
|説明|これには、次の WPF スペル チェックのいくつかの問題が含まれます。<ul><li>WPF スペル チェックで <xref:System.Runtime.InteropServices.COMException?displayProperty=name> がスローされる場合がある。</li><li>"別のユーザーとして実行" を使用してアプリケーションが起動された場合、WPF スペル チェックが <xref:System.UnauthorizedAccessException> で失敗する。</li><li>WPF スペル チェックで、ドイツ語の "Hausnummer" などの複合語のスペル ミスが正しく識別されない。</li></ul>|
|提案される解決策|問題 1 - これは .NET Framework 4.6.2 で修正されました。問題 2 - "別のユーザーとして実行" を使用してアプリケーションが起動された場合、WPF スペル チェックがサポートされなくなりました。 .NET Framework 4.6.2 以降では、このように起動されたアプリケーションが予期せずクラッシュしなくなります。代わりに、スペル チェックが自動的に無効になります。 問題 3 - これは .NET Framework 4.6.2 で修正されました。|
|スコープ|エッジ|
|バージョン|4.6.1|
|[種類]|ランタイム|
