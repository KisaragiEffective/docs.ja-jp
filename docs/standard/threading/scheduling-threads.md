---
title: スレッドのスケジューリング
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- threading [.NET Framework], scheduling
- scheduling threads
ms.assetid: 67e4a0eb-3095-4ea7-b20f-908faa476277
ms.openlocfilehash: abcdf56b90513b937adefc38583e0312fec69785
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73106223"
---
# <a name="scheduling-threads"></a>スレッドのスケジューリング

すべてのスレッドには、スレッド優先順位が割り当てられます。 共通言語ランタイム内で作成されたスレッドには、初期状態で <xref:System.Threading.ThreadPriority.Normal?displayProperty=nameWithType> の優先順位が割り当てられます。 ランタイムの外部で作成されたスレッドは、マネージド環境に入る前に設定されていた優先順位を維持します。 任意のスレッドの優先順位を取得または設定するには、<xref:System.Threading.Thread.Priority?displayProperty=nameWithType> プロパティを使用します。  
  
 スレッドの実行は、優先順位に基づいてスケジュールされます。 スレッドがランタイム内で実行されている場合でも、オペレーティング システムは、すべてのスレッドにプロセッサ タイム スライスを割り当てます。 スレッドの実行順序を決定するために使用されるスケジューリング アルゴリズムの詳細は、オペレーティング システムによって異なります。 オペレーティング システムによっては、実行可能なスレッドの中で最も優先順位の高いスレッドが常に最初に実行されるようにスケジュールされます。 同じ優先順位を持つ複数のスレッドがすべて実行可能である場合、スケジューラは、その優先順位にあるスレッド間を循環することで各スレッドに一定の実行用タイム スライスを与えます。 より優先順位の高いスレッドが実行可能である限り、それよりも優先順位の低いスレッドは実行されません。 特定の優先順位を持つ実行可能なスレッドがなくなると、スケジューラは次に低い優先順位に移り、その優先順位にあるスレッドの実行をスケジュールします。 より優先順位の高いスレッドが実行可能になった場合も、それよりも優先順位の低いスレッドの代わりに優先順位の高いスレッドが実行されます。 また、オペレーティング システムは、アプリケーションのユーザー インターフェイスをフォアグラウンドとバックグラウンド間で移動させながら、スレッドの優先順位を動的に調整することもできます。 オペレーティング システムによっては、別のスケジューリング アルゴリズムが使用される場合があります。  
  
## <a name="see-also"></a>参照

- [スレッドの使用とスレッド処理](../../../docs/standard/threading/using-threads-and-threading.md)
- [Windows でのマネージド スレッド処理とアンマネージド スレッド処理](../../../docs/standard/threading/managed-and-unmanaged-threading-in-windows.md)
