---
title: SpinWait
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- synchronization primitives, SpinWait
ms.assetid: 36012f42-34e5-4f86-adf4-973f433ed6c6
ms.openlocfilehash: 91588fc6e9c3c8e85de6a315c0743efb0137ecd5
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73128983"
---
# <a name="spinwait"></a>SpinWait
<xref:System.Threading.SpinWait?displayProperty=nameWithType> は軽量な同期型であり、負荷が高いコンテキスト スイッチとカーネル イベントに必要なカーネル遷移を避けるために低レベルのシナリオで使用できます。 マルチコア コンピューターでは、リソースの保持期間が長くならないと予測される場合、待機中のスレッドを数十または数百サイクルの間ユーザー モードでスピンさせてから、リソースの取得を再試行した方が効率的です。 スピン後にリソースを使用できる場合は、数千サイクルを節約したことになります。 リソースをまだ使用できない場合でも、数サイクルを消費しただけであり、カーネル ベースの待機に移行できます。 スピン後に待機というこの組み合わせは、*2 フェーズ待機操作* と呼ばれることがあります。  
  
 <xref:System.Threading.SpinWait> は、<xref:System.Threading.ManualResetEvent> などのカーネル イベントをラップする .NET Framework 型と共に使用するように設計されています。 また、<xref:System.Threading.SpinWait> は、1 つのプログラムのみで基本的なスピン機能のために単独で使用することもできます。  
  
 <xref:System.Threading.SpinWait> は単なる空のループではありません。 一般的に、適切なスピン動作を提供するためには慎重に実装する必要があり、長時間 (おおよそ、カーネル遷移に要する時間) スピンすると、コンテキスト スイッチが開始されます。 たとえば、シングルコア コンピューターでは、スピンはすべてのスレッドの進行をブロックするため、<xref:System.Threading.SpinWait> によって、スレッドのタイム スライスがすぐに生成されます。 マルチコア コンピューターでも、待機中のスレッドがより優先順位の高いスレッドやガベージ コレクターをブロックしないように、<xref:System.Threading.SpinWait> によってタイム スライスが生成されます。 したがって、<xref:System.Threading.SpinWait> を 2 フェーズ待機操作で使用する場合は、<xref:System.Threading.SpinWait> 自体がコンテキスト スイッチを開始する前にカーネル待機を呼び出すことをお勧めします。 <xref:System.Threading.SpinWait> では <xref:System.Threading.SpinWait.NextSpinWillYield%2A> プロパティが提供され、<xref:System.Threading.SpinWait.SpinOnce%2A> のすべての呼び出しの前に確認できます。 プロパティが `true` を返したときに、独自の待機操作を開始します。 例については、「[方法: SpinWait を使用して 2 フェーズ待機操作を実装する](../../../docs/standard/threading/how-to-use-spinwait-to-implement-a-two-phase-wait-operation.md)」を参照してください。  
  
 2 フェーズ待機操作は実行せずに、特定の条件が満たされるまでスピンを行うだけの場合は、Windows オペレーティング システム環境で問題を発生させることなく、<xref:System.Threading.SpinWait> を有効にしてコンテキスト スイッチを実行できます。 ロックされないスタックの <xref:System.Threading.SpinWait> を次の基本的な例に示します。 高パフォーマンスのスレッド セーフ スタックが必要な場合は、<xref:System.Collections.Concurrent.ConcurrentStack%601?displayProperty=nameWithType> の使用を検討してください。  
  
 [!code-csharp[CDS_SpinWait#05](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait.cs#05)]
 [!code-vb[CDS_SpinWait#05](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/cds_spinwait1.vb#05)]  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.Thread.SpinWait%2A>
- [スレッド処理オブジェクトと機能](../../../docs/standard/threading/threading-objects-and-features.md)
