---
ms.openlocfilehash: 71c81cf188fa4c2300661f10eb87e7ae00e031f6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67804417"
---
### <a name="etw-event-names-cannot-differ-only-by-a-start-or-stop-suffix"></a>サフィックスの "Start" または "Stop" のみで ETW イベント名を使い分けることができない

|   |   |
|---|---|
|説明|.NET Framework 4.6 と 4.6.1 では、2 つの ETW (Windows イベント トレーシング) イベント名の違いがサフィックスの <xref:System.ArgumentException>Start&quot; または &quot;Stop&quot; のみのとき (たとえば、あるイベントの名前が &quot; で、別のイベントの名前が <code>LogUser</code> のとき)、ランタイムによって <code>LogUserStart</code> がスローされます。 この場合、ランタイムはイベント ソースを作成できないため、ログ記録は生成できません。|
|提案される解決策|この例外を回避するには、サフィックスの &quot;Start&quot; または &quot;Stop&quot; でしか違いのないイベント名が存在しないようにします。この要件は .NET Framework 4.6.2 以降で削除されています。サフィックスの &quot;Start&quot; と &quot;Stop&quot; のみが異なるイベント名がランタイムによって区別されます。|
|スコープ|エッジ|
|バージョン|4.6|
|[種類]|再ターゲット中|
