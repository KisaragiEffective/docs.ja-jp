---
title: '方法: 下位レベルの同期に SpinLock を使用する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinLock, how to use
ms.assetid: a9ed3e4e-4f29-4207-b730-ed0a51ecbc19
ms.openlocfilehash: ad254cb6208bff868e5fc689c502b7ddcc175ad5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73137963"
---
# <a name="how-to-use-spinlock-for-low-level-synchronization"></a>方法: 下位レベルの同期に SpinLock を使用する

<xref:System.Threading.SpinLock> の使用例を以下に示します。 この例では、クリティカル セクションが実行する作業量は最小限であるため、<xref:System.Threading.SpinLock> に適しています。 作業量を少し増やすと、標準ロックと比較して <xref:System.Threading.SpinLock> のパフォーマンスは高まります。 ただし、SpinLock が標準ロックよりも高負荷になるポイントがあります。 プロファイリング ツールでコンカレンシープロファイリング機能を使用して、どのタイプのロックを使用すればプログラムのパフォーマンスが高まるかを確認できます。 詳細については、「[コンカレンシー ビジュアライザー](/visualstudio/profiling/concurrency-visualizer)」を参照してください。  
  
 [!code-csharp[CDS_SpinLock#02](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinlock/cs/spinlockdemo.cs#02)]
 [!code-vb[CDS_SpinLock#02](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinlock/vb/spinlock_vb.vb#02)]  
  
 <xref:System.Threading.SpinLock> は、共有リソースのロックが非常に長い期間使用されない場合に有用なことがあります。 そのような場合、マルチコア コンピューターでは、ロックが解除されるまで数回のサイクルの間、ブロックされたスレッドをスピンさせると効率が高まることがあります。 スピンするとスレッドはブロックされなくなりますが、これは CPU 負荷の高いプロセスです。 ハイパースレッディングを使用するシステムでは、<xref:System.Threading.SpinLock> は特定の状況でスピンを停止して、論理プロセッサの不足や優先順位の逆転が発生するのを回避します。  
  
 この例では、<xref:System.Collections.Generic.Queue%601?displayProperty=nameWithType> クラスを使用するため、マルチスレッド アクセスにはユーザーによる同期が必要になります。 .NET Framework Version 4 をターゲットにしているアプリケーションでは、ユーザーのロックが不要な <xref:System.Collections.Concurrent.ConcurrentQueue%601?displayProperty=nameWithType> を使用することもできます。  
  
 `false` の呼び出しに `False` (Visual Basic では <xref:System.Threading.SpinLock.Exit%2A?displayProperty=nameWithType>) が使用されていることに注目してください。 これにより、最適なパフォーマンスを得られます。 メモリ フェンスを使用するには、IA64 アーキテクチャで `true` (Visual Basic では `True`) を指定します。これにより、書き込みバッファーがフラッシュされるので、ロックを使用して他のスレッドを終了できるようになります。  
  
## <a name="see-also"></a>参照

- [スレッド処理オブジェクトと機能](threading-objects-and-features.md)
- [lock ステートメント (C#)](../../csharp/language-reference/keywords/lock-statement.md)
- [SyncLock ステートメント (Visual Basic)](../../visual-basic/language-reference/statements/synclock-statement.md)
