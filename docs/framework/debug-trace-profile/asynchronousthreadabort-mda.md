---
title: asynchronousThreadAbort MDA
ms.date: 03/30/2017
helpviewer_keywords:
- asynchronous thread aborts
- AsynchronousThreadAbort MDA
- managed debugging assistants (MDAs), asynchronous thread aborts
- threading [.NET Framework], managed debugging assistants
- MDAs (managed debugging assistants), asynchronous thread aborts
ms.assetid: 9ebe40b2-d703-421e-8660-984acc42bfe0
ms.openlocfilehash: d0c78e6d52ae4a5b3a24e0bb4278b2e8a1b98751
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217588"
---
# <a name="asynchronousthreadabort-mda"></a>asynchronousThreadAbort MDA
`asynchronousThreadAbort` マネージド デバッグ アシスタント (MDA) は、スレッドが別のスレッドに非同期の中止処理を適用しようとするとアクティブになります。 同期のスレッド中止では、`asynchronousThreadAbort` MDA はアクティブになりません。

## <a name="symptoms"></a>現象
 メインのアプリケーション スレッドが中止されると、アプリケーションは未処理の <xref:System.Threading.ThreadAbortException> でクラッシュします。 アプリケーションが実行を続けると、アプリケーションがクラッシュした場合よりも悪い結果が生じ、さらにデータが破損する可能性があります。

 不可分であるべき操作が部分的に完了した後で中断された可能性があり、アプリケーション データは予測不能な状態のままになっています。 <xref:System.Threading.ThreadAbortException> は、コードの実行中に見かけ上はランダムなポイントから生成でき、例外の発生が予期されていない場所で生成されることもよくあります。 コードはこのような例外を処理できない場合があるため、破損した状態になります。

 問題に伴うランダム性により、症状は大きく異なる場合があります。

## <a name="cause"></a>原因
 あるスレッドのコードが、対象スレッドで <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドを呼び出したため、非同期のスレッド中止が発生しました。 <xref:System.Threading.Thread.Abort%2A> への呼び出しを実行するコードは、中止操作の対象スレッドとは異なるため、スレッド中止は非同期になります。 同期のスレッド中止の場合、<xref:System.Threading.Thread.Abort%2A> を実行するスレッドがこの操作を実行するのは、アプリケーション状態に一貫性がある安全なチェックポイントのみであるため、問題になりません。

 非同期のスレッド中止は、対象スレッドの実行中に予期しないポイントで処理されるため、問題になります。 これを回避するには、この方法で中止される可能性のあるスレッドで実行するように作成されたコードでは、ほとんどすべてのコード行で <xref:System.Threading.ThreadAbortException> を処理し、アプリケーション データを一貫性のある状態に戻せるように気を付ける必要があります。 コードがこの問題を考慮して作成されることを想定したり、起こりうるすべての状況に対処するコードを作成したりすることは、現実的ではありません。

 アンマネージ コードの呼び出しと `finally` ブロックは非同期に中止されませんが、これらのカテゴリのいずれかから終了するとすぐに中止されます。

 問題に伴うランダム性により、原因を特定することが困難な場合があります。

## <a name="resolution"></a>解決策
 非同期のスレッド中止を使用する必要があるコード設計を避けます。 <xref:System.Threading.Thread.Abort%2A> の呼び出しを必要としない対象スレッドを中断するのにより適した方法はいくつかあります。 最も安全な方法は、対象スレッドの中断要求をシグナル通知する、共通プロパティなどの機構を導入することです。 対象スレッドは、特定の安全なチェックポイントでシグナルをチェックします。 中断が要求されたことが示されている場合は、適切にシャットダウンできます。

## <a name="effect-on-the-runtime"></a>ランタイムへの影響
 この MDA は CLR に影響しません。 非同期のスレッド中止に関するデータを報告するだけです。

## <a name="output"></a>出力
 MDA は、中止を実行するスレッドの ID、および中止の対象となるスレッドの ID を報告します。 これは非同期の中止に限られるため、これらが同じになることはありません。

## <a name="configuration"></a>構成

```xml
<mdaConfig>
  <assistants>
    <asynchronousThreadAbort />
  </assistants>
</mdaConfig>
```

## <a name="example"></a>例
 `asynchronousThreadAbort` MDA をアクティブにする場合、単独の実行スレッドで <xref:System.Threading.Thread.Abort%2A> を呼び出すだけです。 スレッドの開始関数の内容が、中止によって任意の時点に中断される可能性のある、より複雑な一連の操作である場合は、その結果を考慮してください。

```csharp
using System.Threading;
void FireMda()
{
    Thread t = new Thread(delegate() { Thread.Sleep(1000); });
    t.Start();
    // The following line activates the MDA.
    t.Abort();
    t.Join();
}
```

## <a name="see-also"></a>参照

- <xref:System.Threading.Thread>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
