---
title: SpinLock
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- synchronization primitives, SpinLock
ms.assetid: f9af93bb-7a0d-4ba5-afe8-74f48b6b6958
ms.openlocfilehash: eac9a1be38ea81e8ccee1d05d9061ceeb597627f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73106166"
---
# <a name="spinlock"></a>SpinLock
<xref:System.Threading.SpinLock> 構造体は低レベルで相互排他的な同期プリミティブであり、ロックの取得を待機する間にスピンします。 マルチコア コンピューターでは、待機時間が短いことが予測され、競合を最小限に抑えられる場合、パフォーマンスは他の種類のロックよりも <xref:System.Threading.SpinLock> の方が優れています。 ただし、プロファイルにより、<xref:System.Threading.SpinLock> メソッドまたは <xref:System.Threading.Monitor?displayProperty=nameWithType> メソッドがプログラムのパフォーマンスを大幅に低下させていることがわかった場合にのみ、<xref:System.Threading.Interlocked> を使用することをお勧めします。  
  
 <xref:System.Threading.SpinLock> では、ロックをまだ取得していない場合でも、スレッドのタイム スライスが生成される可能性があります。 スレッドの優先順位の逆転を避ける場合と、ガベージ コレクターを進める場合にこのようになります。 <xref:System.Threading.SpinLock> を使用する場合は、スレッドが一定時間ロックを保持できないことと、スレッドがロックを保持している間はブロックできないことを確認してください。  
  
 SpinLock は値の型であるため、2 つのコピーで同じロックを参照する場合は、参照渡しで明示的に渡す必要があります。  
  
 この型の使用方法の詳細については、「<xref:System.Threading.SpinLock?displayProperty=nameWithType>」を参照してください。 例については、「[方法: 下位レベルの同期に SpinLock を使用する](../../../docs/standard/threading/how-to-use-spinlock-for-low-level-synchronization.md)」を参照してください。  
  
 <xref:System.Threading.SpinLock> では*スレッド*-*追跡* モードがサポートされ、開発フェーズ中に使用することができ、特定の時間にロックを保持しているスレッドの追跡に役立ちます。 スレッド追跡モードはデバッグに非常に役立ちますが、パフォーマンスが低下する可能性があるため、リリース バージョンのプログラムでは無効にすることをお勧めします。 詳細については、「[方法: SpinLock のスレッド追跡モードを有効にする](../../../docs/standard/threading/how-to-enable-thread-tracking-mode-in-spinlock.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [スレッド処理オブジェクトと機能](../../../docs/standard/threading/threading-objects-and-features.md)
