---
title: invalidCERCall MDA
ms.date: 03/30/2017
helpviewer_keywords:
- invalid CER calls
- InvalidCERCall MDA
- MDAs (managed debugging assistants), CER calls
- constrained execution regions
- CER calls
- managed debugging assistants (MDAs), CER calls
ms.assetid: c4577410-602e-44e5-9dab-fea7c55bcdfe
ms.openlocfilehash: f8e467401f7c50898613c7cf6eca68a8a705431a
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77217394"
---
# <a name="invalidcercall-mda"></a>invalidCERCall MDA
`invalidCERCall` マネージド デバッグ アシスタント (MDA) は、制約された実行領域 (CER) グラフ内で信頼契約がないかまたは過度に脆弱な契約を持つメソッドの呼び出しがある場合に、アクティブ化されます。 脆弱な契約とは、最悪のケースの状態の破損が、呼び出しに渡されるインスタンスよりも大きい範囲であることを宣言する契約です。つまり、<xref:System.AppDomain> またはプロセスの状態が破損するか、または CER 内で呼び出されたときにその結果を常に確定的に計算できるとは限りません。  
  
## <a name="symptoms"></a>現象  
 CER でコードを実行するときの予期しない結果。 この現象は固有ではありません。 ランタイムがメソッドを事前に準備しないか、実行時に <xref:System.OutOfMemoryException> 例外から保護しないため、信頼できないメソッドの呼び出し時に予期しない <xref:System.Threading.ThreadAbortException>、<xref:System.Threading.ThreadAbortException>、または他の例外が発生する可能性があります。 より大きな脅威は、実行時にメソッドから結果として発生する例外が <xref:System.AppDomain> またはプロセスを CER の目的とは反対の不安定な状態にする可能性があることです。 CER が作成される理由は、このような状態の破損を避けるためです。 一貫性のある状態の定義がアプリケーション間で異なるために、破損状態の現象はアプリケーションに固有です。  
  
## <a name="cause"></a>原因  
 CER 内のコードが、<xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> を持たない関数、または CER での実行と互換性のない脆弱な <xref:System.Runtime.ConstrainedExecution.ReliabilityContractAttribute> を持つ関数を呼び出しています。  
  
 信頼契約の構文の観点から、脆弱な契約とは、<xref:System.Runtime.ConstrainedExecution.Consistency> 列挙値を指定しない契約、または <xref:System.Runtime.ConstrainedExecution.Consistency>、<xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptProcess>、<xref:System.Runtime.ConstrainedExecution.Consistency.MayCorruptAppDomain> の <xref:System.Runtime.ConstrainedExecution.Cer.None> 値を指定する契約です。 これらのどの状態も、呼び出されるコードが一貫性のある状態を維持するための CER の他のコードの処理を妨げる可能性があることを示します。  CER では、エラーを非常に明確な方法で処理し、アプリケーションにとって重要な内部の不変性を維持し、メモリ不足の例外などの一時的なエラーが発生したときにアプリケーションの実行を継続することができます。  
  
 この MDA がアクティブ化は、CER で呼び出されるメソッドが、呼び出し元が予期しない方法で失敗する可能性があるか、<xref:System.AppDomain> またはプロセスの状態が破損または回復不能な状態になることを示します。 当然ながら、呼び出されたコードが正しく実行され、問題が単純な契約の不足である場合があります。 ただし、信頼できるコードの記述に関連する問題は微妙であり、契約がない状況は、コードが正しく実行できないことを示す有効な指標です。 契約は、プログラマが確実にコードを記述したことを示すインジケーターであり、コードの将来のリビジョンで保証が変更されないことを約束します。  つまり、契約は、単なる実装の詳細ではなく、意図の宣言です。  
  
 弱い契約または存在しない契約を持つメソッドは、予期しない方法で失敗する可能性があるため、ランタイムは、レイジーな JIT コンパイル、ジェネリック ディクショナリの読み込み、スレッドの中断などによって発生する、メソッドからの独自の予期しないエラーを削除しようとしません。 つまり、この MDA がアクティブになっているときは、ランタイムが、定義されている CER に呼び出されたメソッドを含めなかったことを意味します。引き続きこのサブツリーを準備すると潜在的なエラーが隠される可能性があるので、呼び出し先はこのノードで終了しました。  
  
## <a name="resolution"></a>解決策  
 有効な信頼契約を関数に追加するか、その関数呼び出しを使用しないでください。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 CER から脆弱な契約を呼び出そうとする、CER でその操作を完了できない可能性があります。 これにより、<xref:System.AppDomain> プロセス ツリーが破損する可能性があります。  
  
## <a name="output"></a>出力  
 この MDA の出力サンプルを次に示します。  
  
 `Method 'MethodWithCer', while executing within a constrained execution region, makes a call at IL offset 0x000C to 'MethodWithWeakContract', which does not have a sufficiently strong reliability contract and might cause non-deterministic results.`  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <invalidCERCall />  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.CompilerServices.RuntimeHelpers.PrepareMethod%2A>
- <xref:System.Runtime.ConstrainedExecution>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
