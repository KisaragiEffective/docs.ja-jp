---
title: releaseHandleFailed MDA
ms.date: 03/30/2017
helpviewer_keywords:
- managed debugging assistants (MDAs), handles
- release handle failed
- CriticalHandle class, run-time errors
- releaseHandleFailed MDA
- ReleaseHandle method
- SafeHandle class, run-time errors
- MDAs (managed debugging assistants), handles
ms.assetid: 44cd98ba-95e5-40a1-874d-e8e163612c51
ms.openlocfilehash: 265344cb100a41cde5443cd0914dc66271aabf93
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216122"
---
# <a name="releasehandlefailed-mda"></a>releaseHandleFailed MDA
`releaseHandleFailed` マネージド デバッグ アシスタント (MDA) は、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> または <xref:System.Runtime.InteropServices.SafeHandle> から派生するクラスの <xref:System.Runtime.InteropServices.CriticalHandle> メソッドが `false` を返すときに、開発者に通知するためにアクティブ化されます。  
  
## <a name="symptoms"></a>現象  
 リソースまたはメモリのリーク。  <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> または <xref:System.Runtime.InteropServices.SafeHandle> から派生するクラスの <xref:System.Runtime.InteropServices.CriticalHandle> メソッドでエラーが発生する場合、クラスによってカプセル化されたリソースが、解放またはクリーンアップされていない可能性があります。  
  
## <a name="cause"></a>原因  
 ユーザーは、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> または <xref:System.Runtime.InteropServices.SafeHandle> から派生するクラスを作成している場合に、<xref:System.Runtime.InteropServices.CriticalHandle> メソッドの実装を行う必要があります。このため、状況は個別のリソースに固有です。 ただし、要件は次のとおりです。  
  
- <xref:System.Runtime.InteropServices.SafeHandle> 型および <xref:System.Runtime.InteropServices.CriticalHandle> 型は、プロセスの重要なリソースのラッパーを表します。 メモリ リークは、時間の経過と共にプロセスを使用不能にしてしまいます。  
  
- <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> メソッドは、その機能を実行しないことがあってはなりません。 プロセスがこのようなリソースを取得したら、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> が解放する唯一の方法です。 そのため、実行しないとリソースのリークになってしまいます。  
  
- <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> の実行中に発生する、リソースの解放を妨げるエラーは、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> メソッド自体の実装のバグです。 そのコードが、機能を実行するために他のプログラマが作成したコードを呼び出している場合でも、この規定を実行するようにすることがプログラマの責任です。  
  
## <a name="resolution"></a>解決策  
 MDA 通知を発生させた特定の <xref:System.Runtime.InteropServices.SafeHandle> (または <xref:System.Runtime.InteropServices.CriticalHandle>) 型を使用するコードをレビューし、未処理のハンドル値が<xref:System.Runtime.InteropServices.SafeHandle> から抽出されて、別の場所にコピーされている場所を探します。 これは、未処理のハンドル値の使用がランタイムにより追跡できなくなっているため、<xref:System.Runtime.InteropServices.SafeHandle> または <xref:System.Runtime.InteropServices.CriticalHandle> の実装内のエラーの一般的な原因です。 未処理のハンドルのコピーがその後閉じられると、同じハンドルを閉じようとして無効になるため、後で <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> 呼び出しがエラーになる可能性があります。  
  
 不適切なハンドルの重複は、様々な方法で発生します。  
  
- <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A> メソッドの呼び出しを探します。 このメソッドへの呼び出しは、ごく限られた場合に行い、実行する場合は、<xref:System.Runtime.InteropServices.SafeHandle.DangerousAddRef%2A> メソッドと <xref:System.Runtime.InteropServices.SafeHandle.DangerousRelease%2A> メソッドの呼び出しで囲む必要があります。 後者のメソッドは、未処理のハンドル値を安全に使用できるコードの領域を指定します。 この領域の外、または参照カウントが最初にインクリメントされない場合は、別のスレッドで <xref:System.Runtime.InteropServices.SafeHandle.Dispose%2A> または <xref:System.Runtime.InteropServices.SafeHandle.Close%2A> を呼び出して、いつでもハンドル値を無効化できます。 <xref:System.Runtime.InteropServices.SafeHandle.DangerousGetHandle%2A> のすべての利用を追跡したら、未処理のハンドルが取得するパスに従い、ハンドルを解放する `CloseHandle` または別の低レベル ネイティブ メソッドを最終的に呼び出すコンポーネントに渡されないようにします。  
  
- 未処理の有効なハンドル値を使用して <xref:System.Runtime.InteropServices.SafeHandle> を初期化するために使用されるコードがハンドルを所有していることを確認します。 基本コンストラクターで <xref:System.Runtime.InteropServices.SafeHandle> パラメーターを `ownsHandle` に設定せずに、コードで所有していないハンドルの周囲に `false` を作成する場合、<xref:System.Runtime.InteropServices.SafeHandle> と実際のハンドルの所有者の両方がハンドルを閉じようとすることがあり、<xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> が閉じることができないと <xref:System.Runtime.InteropServices.SafeHandle> でエラーが発生します。  
  
- <xref:System.Runtime.InteropServices.SafeHandle> がアプリケーション ドメイン間でマーシャリングされる場合、使用されている <xref:System.Runtime.InteropServices.SafeHandle> の派生がシリアル可能とマークされていることを確認してください。 <xref:System.Runtime.InteropServices.SafeHandle> から派生したクラスがシリアル化されているまれなケースでは、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装するか、シリアル化と逆シリアル化のプロセスを手動で管理するその他のテクニックのいずれかを使用する必要があります。 これが必要なのは、既定のシリアル化アクションでは、その中の未処理のハンドル値のビットごとのクローンを作成し、結果として 2 つの <xref:System.Runtime.InteropServices.SafeHandle> のインスタンスが同じハンドルを所有すると認識してしまうためです。 両方とも、同じ地点で同じハンドルの <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> を呼び出そうとします。 2 番目の <xref:System.Runtime.InteropServices.SafeHandle> がこれを行おうとすると、エラーが発生します。 <xref:System.Runtime.InteropServices.SafeHandle> をシリアル化するときの適切な一連のアクションは、ネイティブのハンドル型に適した `DuplicateHandle` 関数、または同様の関数を呼び出し、有効なハンドルのコピーを明示的に作成することです。 ハンドル型がこれをサポートしない場合、それをラップしている <xref:System.Runtime.InteropServices.SafeHandle> 型をシリアル化可能にすることはできません。  
  
- ハンドルが早い段階で閉じられ、その結果 <xref:System.Runtime.InteropServices.SafeHandle.ReleaseHandle%2A> メソッドが最終的に呼び出されたときにエラーが発生した場所を、`CloseHandle` 関数など、ハンドルの解放に使用されるネイティブ ルーチンにデバッガーのブレークポイントを配置することで追跡することができます。 このようなルーチンは高トラフィックを処理することが多いため、負荷の高いシナリオや、中サイズの機能テストでも、これを実行することができない場合があります。 呼び出し元、または場合によっては完全なスタック トレースの ID と解放されるハンドルの値をキャプチャするために、ネイティブの解放メソッドを呼び出すコードをインストルメント化することが役に立つ場合があります。  ハンドル値は、この MDA から報告された値と比較できます。  
  
- Win32 ハンドルなど、`CloseHandle` 関数により解放される可能性があるネイティブのハンドル型の一部は、同じハンドル名前空間を共有します。 1 つのハンドル型の解放でエラーが発生すると、別のハンドル型でも問題が発生する可能性があります。 たとえば、誤って Win32 イベント ハンドルを 2 回閉じると、一見関連のないファイルのハンドルが早く閉じられてしまう可能性があります。 これは、ハンドルが解放され、ハンドル値が別のリソース、おそらく別の型のものを追跡するために利用できるようになったときに発生します。 この状態が発生し、2 番目の間違った解放が続くと、関連のないスレッドのハンドルが無効になる可能性があります。  
  
## <a name="effect-on-the-runtime"></a>ランタイムへの影響  
 この MDA は CLR に影響しません。  
  
## <a name="output"></a>出力  
 <xref:System.Runtime.InteropServices.SafeHandle> または <xref:System.Runtime.InteropServices.CriticalHandle> でエラーが発生し、ハンドルを適切に解放できないことを示すメッセージ。 例 :  
  
```output
"A SafeHandle or CriticalHandle of type 'MyBrokenSafeHandle'   
failed to properly release the handle with value 0x0000BEEF. This   
usually indicates that the handle was released incorrectly via   
another means (such as extracting the handle using DangerousGetHandle   
and closing it directly or building another SafeHandle around it."  
```  
  
## <a name="configuration"></a>構成  
  
```xml  
<mdaConfig>  
  <assistants>  
    <releaseHandleFailed/>  
  </assistants>  
</mdaConfig>  
```  
  
## <a name="example"></a>例  
 `releaseHandleFailed` MDA をアクティブ化することができるコードの例を、次に示します。  
  
```csharp
bool ReleaseHandle()  
{  
    // Calling the Win32 CloseHandle function to release the   
    // native handle wrapped by this SafeHandle. This method returns   
    // false on failure, but should only fail if the input is invalid   
    // (which should not happen here). The method specifically must not   
    // fail simply because of lack of resources or other transient   
    // failures beyond the user’s control. That would make it unacceptable   
    // to call CloseHandle as part of the implementation of this method.  
    return CloseHandle(handle);  
}  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [マネージド デバッグ アシスタントによるエラーの診断](diagnosing-errors-with-managed-debugging-assistants.md)
- [相互運用マーシャリング](../interop/interop-marshaling.md)
