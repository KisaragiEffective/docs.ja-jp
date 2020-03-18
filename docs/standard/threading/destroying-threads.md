---
title: スレッドの破棄
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- destroying threads
- threading [.NET Framework], destroying threads
ms.assetid: df54e648-c5d1-47c9-bd29-8e4438c1db6d
ms.openlocfilehash: 842ca4ff17f9cbab3a1518d2dea37436c9b23f9d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78155933"
---
# <a name="destroying-threads"></a>スレッドの破棄

スレッドの実行を終了するには、通常、[協調的なキャンセル モデル](cancellation-in-managed-threads.md)を使用します。 協調的なキャンセルを行うように設計されていないサード パーティのコードがスレッドで実行されているために、スレッドを協調的に停止できない場合があります。 .NET Framework の <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドを使用すると、マネージド スレッドを強制的に終了できます。 <xref:System.Threading.Thread.Abort%2A> を呼び出すと、共通言語ランタイムにより対象スレッド内で <xref:System.Threading.ThreadAbortException> がスローされます。対象スレッドはこれをキャッチできます。 詳細については、<xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> を参照してください。 <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> メソッドは、.NET Core ではサポートされていません。 .NET Core でサード パーティのコードの実行を強制的に終了する必要がある場合は、それを別のプロセスで実行し、<xref:System.Diagnostics.Process.Kill%2A?displayProperty=nameWithType> を使用します。

> [!NOTE]
> スレッドが <xref:System.Threading.Thread.Abort%2A> メソッドの呼び出し時にアンマネージ コードを実行する場合、ランタイムはそれを <xref:System.Threading.ThreadState.AbortRequested?displayProperty=nameWithType> としてマークします。 スレッドがマネージド コードに戻ると、例外がスローされます。  
  
 スレッドが中止されると、再起動することはできません。  
  
 対象スレッドが <xref:System.Threading.Thread.Abort%2A> をキャッチし、<xref:System.Threading.ThreadAbortException> ブロック内の任意の量のコードを実行できるため、`finally` メソッドにより、スレッドがすぐに中止されることはありません。 スレッドが終了するまで待機する必要がある場合は、<xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> を呼び出すことができます。 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> は、スレッドが実際に実行を停止するか、オプションのタイムアウト間隔が経過するまで返されないブロック呼び出しです。 中止されたスレッドは <xref:System.Threading.Thread.ResetAbort%2A> メソッドを呼び出したり、`finally` ブロックで無制限処理を実行したりすることができるため、タイムアウトを指定しない場合、終了するまで待機するとは限りません。  
  
 <xref:System.Threading.Thread.Join%2A?displayProperty=nameWithType> メソッドへの呼び出しを待機しているスレッドは、<xref:System.Threading.Thread.Interrupt%2A?displayProperty=nameWithType> を呼び出す他のスレッドで中断することができます。  
  
## <a name="handling-threadabortexception"></a>ThreadAbortException の処理  
 独自のコードからの <xref:System.Threading.Thread.Abort%2A> の呼び出しの結果、またはスレッドが実行中のアプリケーション ドメインのアンロード (<xref:System.AppDomain.Unload%2A?displayProperty=nameWithType> が <xref:System.Threading.Thread.Abort%2A?displayProperty=nameWithType> を使用してスレッドを終了する) の結果として、スレッドが中止されることが予想される場合、スレッドは <xref:System.Threading.ThreadAbortException> を処理し、以下のコードに示すように、`finally` 句で最終処理を実行する必要があります。  
  
```vb  
Try  
    ' Code that is executing when the thread is aborted.  
Catch ex As ThreadAbortException  
    ' Clean-up code can go here.  
    ' If there is no Finally clause, ThreadAbortException is  
    ' re-thrown by the system at the end of the Catch clause.
Finally  
    ' Clean-up code can go here.  
End Try  
' Do not put clean-up code here, because the exception
' is rethrown at the end of the Finally clause.  
```  
  
```csharp  
try
{  
    // Code that is executing when the thread is aborted.  
}
catch (ThreadAbortException ex)
{  
    // Clean-up code can go here.  
    // If there is no Finally clause, ThreadAbortException is  
    // re-thrown by the system at the end of the Catch clause.
}  
// Do not put clean-up code here, because the exception
// is rethrown at the end of the Finally clause.  
```  
  
 クリーンアップ コードは `catch` 句または `finally` 句にある必要があります。これは、<xref:System.Threading.ThreadAbortException> が `finally` 句の最後、または `catch` 句の最後 (`finally` 句がない場合) にシステムによって再スローされるためです。  
  
 <xref:System.Threading.Thread.ResetAbort%2A?displayProperty=nameWithType> メソッドを呼び出して、システムが例外を再スローしないようにすることができます。 ただし、この操作は独自のコードにより <xref:System.Threading.ThreadAbortException> が発生した場合にのみ行ってください。  
  
## <a name="see-also"></a>参照

- <xref:System.Threading.ThreadAbortException>
- <xref:System.Threading.Thread>
- [スレッドの使用とスレッド処理](../../../docs/standard/threading/using-threads-and-threading.md)
