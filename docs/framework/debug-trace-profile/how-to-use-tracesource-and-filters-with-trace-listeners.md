---
title: '方法 : TraceSource とフィルターをトレース リスナーと共に使用する'
ms.date: 03/30/2017
helpviewer_keywords:
- initializing trace listeners
- configuration files [.NET Framework], trace listeners
- app.config files, trace listeners
- levels of writing trace messages
- trace listeners, TraceSource class
- application configuration files, trace listeners
- filters, trace listeners
- TraceSource class, trace listeners
- trace listeners, creating
- trace listeners, filters
- trace listeners, initializing
ms.assetid: 21dc2169-947d-453a-b0e2-3dac3ba0cc9f
ms.openlocfilehash: 53cdce767d437c47aab94e883381954f8cf70653
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215918"
---
# <a name="how-to-use-tracesource-and-filters-with-trace-listeners"></a>方法 : TraceSource とフィルターをトレース リスナーと共に使用する
.NET Framework Version 2.0 の新機能の 1 つは、強化されたトレース システムです。 基本的な前提は変更されていません。トレース メッセージはスイッチ経由でリスナーに送信され、関連付けられている出力メディアに、データを報告します。 バージョン 2.0 の主な違いは、<xref:System.Diagnostics.TraceSource> クラスのインスタンスを介してトレースを開始できることです。 <xref:System.Diagnostics.TraceSource> は、拡張されたトレース システムとして機能することを目的としており、以前の <xref:System.Diagnostics.Trace> トレース クラスと <xref:System.Diagnostics.Debug> トレース クラスの静的メソッドの代わりに使用できます。 使い慣れた <xref:System.Diagnostics.Trace> クラスと <xref:System.Diagnostics.Debug> クラスがまだ存在していますが、推奨される方法は、<xref:System.Diagnostics.TraceSource> クラスを使用したトレースです。  
  
 このトピックでは、アプリケーション構成ファイルと組み合わせた <xref:System.Diagnostics.TraceSource> の使用方法について説明します。  可能であれば、推奨されませんが、構成ファイルを使用せずに <xref:System.Diagnostics.TraceSource> を使用してトレースすることもできます。 構成ファイルを使わずにトレースする方法の詳細については、「[How to: Create and Initialize Trace Sources](how-to-create-and-initialize-trace-sources.md)」(方法: トレース ソースを作成し初期化する) を参照してください。  
  
### <a name="to-create-and-initialize-your-trace-source"></a>トレース ソースを作成して初期化するには  
  
1. トレースをアプリケーションにインストルメント化する最初の手順では、トレース ソースを作成します。 各種のコンポーネントを含む大規模なプロジェクトでは、コンポーネントごとに個別のトレース ソースを作成できます。 トレース ソース名にアプリケーション名を使用することをお勧めします。 これにより、別のトレースを容易に別々に保管できます。 次のコードでは、新しいトレース ソース (`mySource)`) を作成し、イベントをトレースするメソッド (`Activity1`) を呼び出します。  トレース メッセージは、既定のトレース リスナーによって書き込まれます。  
  
    ```csharp
    using System;  
    using System.Diagnostics;  
    using System.Threading;  
  
    namespace TraceSourceApp  
    {  
        class Program  
        {  
            private static TraceSource mySource =   
                new TraceSource("TraceSourceApp");  
            static void Main(string[] args)  
            {  
                Activity1();  
                mySource.Close();  
                return;  
            }  
            static void Activity1()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 1,   
                    "Error message.");  
                mySource.TraceEvent(TraceEventType.Warning, 2,   
                    "Warning message.");  
            }  
        }  
    }  
    ```  
  
### <a name="to-create-and-initialize-trace-listeners-and-filters"></a>トレース リスナーとフィルターを作成し初期化するには  
  
1. 最初の手順で示すコードは、トレース リスナーまたはフィルターをプログラム的に識別しません。 このコードだけでは、トレース メッセージが既定のトレース リスナーに書き込まれます。 特定のトレース リスナーと関連付けられているフィルターを構成するには、アプリケーションの名前に対応する構成ファイルを編集します。 このファイルで、リスナーの追加または削除、リスナーのプロパティとフィルターの設定、またはリスナーの削除を行うことができます。 次の構成ファイルの例は、コンソール トレース リスナーと、前の手順で作成されたトレース ソースのテキスト ライター トレース リスナーを初期化する方法を示しています。 この構成ファイルは、トレース リスナーを設定するだけでなく、両方のリスナーについてフィルターを作成し、トレース ソースのソース スイッチも作成します。 トレース リスナーを追加する 2 つの方法が示されています。1 つは、リスナーをトレース ソースに直接追加する方法で、もう 1 つは、リスナーを共有リスナー コレクションに追加したうえで、名前でリスナーをトレース ソースに追加する方法です。 2 つのリスナーに対して識別されたフィルターは、異なるソース レベルで初期化されます。 この結果、一部のメッセージが 2 つのリスナーのいずれか一方のみによって書き込まれます。  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
        <sources>  
          <source name="TraceSourceApp"   
            switchName="sourceSwitch"   
            switchType="System.Diagnostics.SourceSwitch">  
            <listeners>  
              <add name="console"   
                type="System.Diagnostics.ConsoleTraceListener">  
                <filter type="System.Diagnostics.EventTypeFilter"   
                  initializeData="Warning"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Warning"/>  
        </switches>  
        <sharedListeners>  
          <add name="myListener"   
            type="System.Diagnostics.TextWriterTraceListener"   
            initializeData="myListener.log">  
            <filter type="System.Diagnostics.EventTypeFilter"   
              initializeData="Error"/>  
          </add>  
        </sharedListeners>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
### <a name="to-change-the-level-at-which-a-listener-writes-a-trace-message"></a>リスナーが、トレース メッセージを書き込むレベルを変更するには  
  
1. 構成ファイルは、アプリケーションが初期化されるときにトレース ソースの設定を初期化します。 それらの設定を変更するには、構成ファイルを変更してアプリケーションを再起動するか、または <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=nameWithType> メソッドを使用して、プログラムによってアプリケーションを更新する必要があります。 アプリケーションは、構成ファイルによって設定されたプロパティを動的に変更し、ユーザーによって指定された設定値をオーバーライドできます。  たとえば、現在の構成設定とは無関係に、警告メッセージが常にテキスト ファイルに送られるようにすることもできます。  
  
    ```csharp
    using System;  
    using System.Diagnostics;  
    using System.Threading;  
  
    namespace TraceSourceApp  
    {  
        class Program  
        {  
            private static TraceSource mySource =   
                new TraceSource("TraceSourceApp");  
            static void Main(string[] args)  
            {  
                Activity1();  
  
                // Change the event type for which tracing occurs.  
                // The console trace listener must be specified   
                // in the configuration file. First, save the original  
                // settings from the configuration file.  
                EventTypeFilter configFilter =   
                    (EventTypeFilter)mySource.Listeners["console"].Filter;  
  
                // Then create a new event type filter that ensures   
                // critical messages will be written.  
                mySource.Listeners["console"].Filter =  
                    new EventTypeFilter(SourceLevels.Critical);  
                Activity2();  
  
                // Allow the trace source to send messages to listeners   
                // for all event types. This statement will override   
                // any settings in the configuration file.  
                mySource.Switch.Level = SourceLevels.All;  
  
                // Restore the original filter settings.  
                mySource.Listeners["console"].Filter = configFilter;  
                Activity3();  
                mySource.Close();  
                return;  
            }  
            static void Activity1()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 1,   
                    "Error message.");  
                mySource.TraceEvent(TraceEventType.Warning, 2,   
                    "Warning message.");  
            }  
            static void Activity2()  
            {  
                mySource.TraceEvent(TraceEventType.Critical, 3,   
                    "Critical message.");  
                mySource.TraceInformation("Informational message.");  
            }  
            static void Activity3()  
            {  
                mySource.TraceEvent(TraceEventType.Error, 4,   
                    "Error message.");  
                mySource.TraceInformation("Informational message.");  
            }  
        }  
    }  
    ```  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.EventTypeFilter>
- [方法: トレース ソースを作成し初期化する](how-to-create-and-initialize-trace-sources.md)
- [トレース リスナー](trace-listeners.md)
