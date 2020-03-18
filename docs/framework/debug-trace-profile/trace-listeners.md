---
title: トレース リスナー
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Listener object types
- listeners
- Trace class, listeners
- trace listeners, about trace listeners
- Listeners collection
- trace listeners
- tracing [.NET Framework], trace listeners
- logs, trace listeners
ms.assetid: 444b0d33-67ea-4c36-9e94-79c50f839025
ms.openlocfilehash: a51c046a296fbb62d21c7784cf7c1e78b700f3e9
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216144"
---
# <a name="trace-listeners"></a>トレース リスナー
**Trace**、**Debug**、および <xref:System.Diagnostics.TraceSource> を使用するときには、送信されたメッセージを収集して記録するための機構が必要です。 トレース メッセージは*リスナー*によって受け取られます。 リスナーの目的は、トレース メッセージを収集し、保存し、ルーティングすることです。 リスナーは、ログ、ウィンドウ、テキスト ファイルなどの適切なターゲットにトレース出力を送ります。  
  
 リスナーは、**Debug**、**Trace**、および <xref:System.Diagnostics.TraceSource> の各クラスで使用できます。それらのクラスからも出力を各種のリスナー オブジェクトに送ることができます。 一般的に使用される定義済みのリスナーは、次のとおりです。  
  
- <xref:System.Diagnostics.TextWriterTraceListener> は、<xref:System.IO.TextWriter> クラスのインスタンスまたは <xref:System.IO.Stream> クラスの任意の対象に出力をリダイレクトします。 また、コンソールまたはファイルに書き込むこともできます (これらは <xref:System.IO.Stream> クラスなので)。  
  
- <xref:System.Diagnostics.EventLogTraceListener> は、出力をイベント ログにリダイレクトします。  
  
- <xref:System.Diagnostics.DefaultTraceListener> は、**Write** メッセージと **WriteLine** メッセージを **OutputDebugString** と **Debugger.Log** メソッドに出力します。 Visual Studio では、これによってデバッグ メッセージが [出力] ウィンドウに表示されます。 **Fail** メッセージと、失敗した **Assert** メッセージも **OutputDebugString** Windows API と **Debugger.Log** メソッドに出力され、これらはメッセージ ボックスとしても表示されます。 **DefaultTraceListener** はすべての **コレクションに自動的に取り込まれる唯一のリスナーであるため、この動作は**Debug**メッセージと**Trace`Listeners` メッセージの既定の動作になります。  
  
- <xref:System.Diagnostics.ConsoleTraceListener> は、トレース出力またはデバッグ出力を、標準出力と標準エラー出力ストリームのいずれかに転送します。  
  
- <xref:System.Diagnostics.DelimitedListTraceListener> は、トレース出力またはデバッグ出力を、テキスト ライター (ストリーム ライターなど) またはストリーム (ファイル ストリームなど) に送ります。 トレース出力は、<xref:System.Diagnostics.DelimitedListTraceListener.Delimiter%2A> プロパティによって指定された区切り記号を使用する区切りテキスト形式になっています。  
  
- <xref:System.Diagnostics.XmlWriterTraceListener> は、トレース出力またはデバッグ出力を、XML でエンコードされたデータとして <xref:System.IO.TextWriter> または <xref:System.IO.Stream> (<xref:System.IO.FileStream> など) に送ります。  
  
 <xref:System.Diagnostics.DefaultTraceListener> 以外のリスナーが **Debug** 出力、**Trace** 出力、および <xref:System.Diagnostics.TraceSource> 出力を受け取るようにする場合は、目的のリスナーを `Listeners` コレクションに追加する必要があります。 詳細については、「[方法 : トレース リスナーを作成し初期化する](how-to-create-and-initialize-trace-listeners.md)」と「[方法 : TraceSource とフィルターをトレース リスナーと共に使用する](how-to-use-tracesource-and-filters-with-trace-listeners.md)」を参照してください。 **Listeners** コレクションのすべてのリスナーは、トレース出力のメソッドから同じメッセージを受け取ります。 たとえば、**TextWriterTraceListener** と **EventLogTraceListener** という 2 つのリスナーを設定したとします。 各リスナーは同一のメッセージを受け取ります。 この場合、**TextWriterTraceListener** はストリームに出力を送り、**EventLogTraceListener** はイベント ログに出力を送るなどの動作が可能です。  
  
 出力を **Listeners** コレクションに送る方法を次の例に示します。  
  
```vb  
' Use this example when debugging.  
Debug.WriteLine("Error in Widget 42")  
' Use this example when tracing.  
Trace.WriteLine("Error in Widget 42")  
```  
  
```csharp  
// Use this example when debugging.  
System.Diagnostics.Debug.WriteLine("Error in Widget 42");  
// Use this example when tracing.  
System.Diagnostics.Trace.WriteLine("Error in Widget 42");  
```  
  
 デバッグとトレースは、同じ **Listeners** コレクションを共有しています。したがって、リスナー オブジェクトをアプリケーションの **Debug.Listeners** コレクションに追加した場合、そのリスナー オブジェクトは **Trace.Listeners** コレクションにも追加されます。  
  
 トレース情報をコンソールに送る場合のリスナーの使用方法を次の例に示します。  
  
```vb  
Trace.Listeners.Clear()  
Trace.Listeners.Add(New TextWriterTraceListener(Console.Out))  
```  
  
```csharp  
System.Diagnostics.Trace.Listeners.Clear();  
System.Diagnostics.Trace.Listeners.Add(  
   new System.Diagnostics.TextWriterTraceListener(Console.Out));  
```  
  
## <a name="developer-defined-listeners"></a>開発者が定義するリスナー  
 **TraceListener** 基底クラスから継承することにより、独自のリスナーを定義し、カスタマイズされたメソッドで既存のメソッドをオーバーライドできます。 開発者が定義するリスナーの作成については、「.NET Framework リファレンス」の「<xref:System.Diagnostics.TraceListener>」を参照してください。  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.DefaultTraceListener>
- <xref:System.Diagnostics.TraceListener>
- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
- [トレース スイッチ](trace-switches.md)
