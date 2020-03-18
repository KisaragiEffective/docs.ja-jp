---
ms.openlocfilehash: 3b7309347c643d89a28331c6ef3cac36085a969a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858649"
---
### <a name="wpf-windows-are-rendered-without-clipping-when-extending-outside-a-single-monitor"></a>1 つのモニターの外部に拡張すると、クリッピングなしで WPF ウィンドウがレンダリングされる

|   |   |
|---|---|
|説明|Windows 8 以上で実行している .NET Framework 4.6 では、マルチ モニターのシナリオで 1 つのディスプレイの外部にウィンドウを拡張すると、ウィンドウ全体がクリッピングなしでレンダリングされます。 これは、1 つのディスプレイを超える WPF ウィンドウをクリッピングする、以前のバージョンの .NET Framework とは異なります。|
|提案される解決策|この動作 (クリッピングするかどうか) は、アプリケーションの構成ファイルの <code>&lt;EnableMultiMonitorDisplayClipping&gt;</code> で <code>&lt;appSettings&gt;</code> 要素を使用するか、アプリの起動時に <code>EnableMultiMonitorDisplayClipping</code> プロパティを設定することで明示的に設定できます。|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|ランタイム|
