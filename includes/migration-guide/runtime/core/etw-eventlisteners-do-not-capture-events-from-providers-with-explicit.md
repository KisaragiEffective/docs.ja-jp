---
ms.openlocfilehash: 1ef31202d7c072ca27c21fc22db102aafa6b8de7
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858375"
---
### <a name="etw-eventlisteners-do-not-capture-events-from-providers-with-explicit-keywords-like-the-tpl-provider"></a>ETW EventListeners は、明示的なキーワードを持つプロバイダーからのイベント (TPL プロバイダーなど) をキャプチャしません

|   |   |
|---|---|
|説明|空白のキーワード マスクを持つ ETW EventListeners は、明示的なキーワードを持つプロバイダーからのイベントを正しくキャプチャしません。 .NET Framework 4.5 では、TPL プロバイダーは、明示的なキーワードを提供するようになり、この問題が発生しました。 .NET Framework 4.6 では、EventListeners が更新され、この問題は修正されました。|
|提案される解決策|この問題を回避するには、<xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)> の呼び出しを、使用する &quot;任意のキーワード&quot; マスクを明示的に指定する EnableEvents オーバーロード (<code>EnableEvents(eventSource, level, unchecked((EventKeywords)0xFFFFffffFFFFffff))</code>) の呼び出しに置換します。または、この問題は .NET Framework 4.6 で修正されたため、このバージョンの .NET Framework にアップグレードすることによって対処できます。|
|スコープ|エッジ|
|バージョン|4.5|
|[種類]|ランタイム|
|影響を受ける API|<ul><li><xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType></li></ul>|
