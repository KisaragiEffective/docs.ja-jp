---
ms.openlocfilehash: 22f8e3bb1ba72379b3f5fc87a077e5fe57f89bf8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "67858604"
---
### <a name="null-coalescer-values-are-not-visible-in-debugger-until-one-step-later"></a>デバッガーですぐに null コアレッサー値が表示されない

|   |   |
|---|---|
|説明|.NET Framework 4.5 のバグにより、64 ビット版の Framework で実行中に代入演算が実行された直後に、デバッガーで null 合体演算で設定された値が表示されません。|
|提案される解決策|デバッガーでの操作に時間がかかると、ローカル/フィールドの値が正しく更新されなくなります。 この問題は .NET Framework 4.6 で修正されました。このバージョンの .NET Framework にアップグレードすれば、問題は解決します。|
|スコープ|エッジ|
|バージョン|4.5|
|[種類]|ランタイム|
