---
title: '方法: SpinWait を使用して 2 フェーズ待機操作を実装する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- SpinWait, how to synchronize two-phase wait
ms.assetid: b2ac4e4a-051a-4f65-b4b9-f8e103aff195
ms.openlocfilehash: 5bac174660177fd47e1f345e64581e35ae4c0ffc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73137948"
---
# <a name="how-to-use-spinwait-to-implement-a-two-phase-wait-operation"></a>方法: SpinWait を使用して 2 フェーズ待機操作を実装する
次の例では、<xref:System.Threading.SpinWait?displayProperty=nameWithType> オブジェクトを使用して、2 フェーズ待機操作を実装する方法を示します。 最初のフェーズでは、同期オブジェクトである `Latch` は、ロックが使用可能になったかどうかを確認しながら、数回のサイクルの間スピンします。 2 番目のフェーズでは、ロックが使用可能になった場合に、`Wait` メソッドは待機を実行するために <xref:System.Threading.ManualResetEvent?displayProperty=nameWithType> を使用せずに制御を返します (それ以外の場合、`Wait` は待機を実行します)。  
  
## <a name="example"></a>例  
 この例では、ラッチ同期プリミティブの非常に基本的な実装を示します。 待機時間が非常に短くなると予測される場合は、このデータ構造を使用することができます。 この例は、デモンストレーション目的のみで提供されます。 プログラムでラッチ型機能が必要な場合は、<xref:System.Threading.ManualResetEventSlim?displayProperty=nameWithType> の使用を検討してください。  
  
 [!code-csharp[CDS_SpinWait#03](../../../samples/snippets/csharp/VS_Snippets_Misc/cds_spinwait/cs/spinwait03.cs#03)]
 [!code-vb[CDS_SpinWait#03](../../../samples/snippets/visualbasic/VS_Snippets_Misc/cds_spinwait/vb/spinwait2.vb#03)]  
  
 ラッチでは <xref:System.Threading.SpinWait> オブジェクトを使用して、次の `SpinOnce` の呼び出しで <xref:System.Threading.SpinWait> がスレッドのタイム スライスを生成するまでの間のみ、スピンが発生するようにします。 その時点で、<xref:System.Threading.WaitHandle.WaitOne%2A> で <xref:System.Threading.ManualResetEvent> を呼び出し、残りのタイムアウト値を渡すことで、ラッチにより独自のコンテキスト切り替えが発生します。  
  
 ログ出力には、<xref:System.Threading.ManualResetEvent> を使用せずにロックを取得することで、ラッチでパフォーマンスを向上させることができた頻度が示されます。  
  
## <a name="see-also"></a>参照

- [SpinWait](../../../docs/standard/threading/spinwait.md)
- [スレッド処理オブジェクトと機能](../../../docs/standard/threading/threading-objects-and-features.md)
