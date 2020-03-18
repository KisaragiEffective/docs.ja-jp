---
title: '方法: トレース ソースを作成し初期化する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- trace sources
- initializing trace sources
- configuration files [.NET Framework], trace sources
ms.assetid: f88dda6f-5fda-45be-9b3c-745a9b708c4d
ms.openlocfilehash: cc2987499aa094960c08d220940fe1aed5440b2d
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449960"
---
# <a name="how-to-create-and-initialize-trace-sources"></a>方法: トレース ソースを作成し初期化する
<xref:System.Diagnostics.TraceSource> クラスをアプリケーションで使用すると、アプリケーションに関連付けることができるトレースを生成できます。 <xref:System.Diagnostics.TraceSource> は、イベントのトレース、データのトレース、および情報トレースの発行を簡単に実行できるトレース メソッドを提供します。 <xref:System.Diagnostics.TraceSource> からのトレース出力は、構成ファイルを使用してもしなくても作成および初期化できます。 このトピックでは、この両方の手順について説明しています。 ただし、実行時にトレース ソースによって作成されるトレースの再構成を容易にするために構成ファイルを使用することをお勧めします。  
  
### <a name="to-create-and-initialize-a-trace-source-using-a-configuration-file"></a>構成ファイルを使用してトレース ソースを作成し、初期化するには  
  
1. Visual Studio コンソールアプリケーションプロジェクト (.NET Framework) を作成し、指定したコードを次のコードに置き換えます。 このコードは、エラーや警告を記録し、コンソールと、構成ファイルのエントリによって作成された myListener ファイルにその一部をそれぞれ出力します。  
  
     [!code-csharp[TraceSourceExample1#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample1/cs/program.cs#1)]
     [!code-vb[TraceSourceExample1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample1/vb/program.vb#1)]  
  
2. アプリケーション構成ファイルが存在しない場合は、アプリケーション構成ファイルをプロジェクトに追加して、手順 1. のコード例にある `TraceSourceApp` というトレース ソースを初期化します。  
  
3. 既定の構成ファイルの内容を以下の設定に置き換えて、コンソール トレース リスナーと、手順 1. で作成されたトレース ソースのテキスト ライター トレース リスナーを初期化します。  
  
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
                  initializeData="Error"/>  
              </add>  
              <add name="myListener"/>  
              <remove name="Default"/>  
            </listeners>  
          </source>  
        </sources>  
        <switches>  
          <add name="sourceSwitch" value="Error"/>  
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
  
     この構成ファイルは、トレース リスナーを設定するだけでなく、両方のリスナーについてフィルターを作成し、トレース ソースのソース スイッチも作成します。 トレース リスナーを追加する 2 つの方法が示されています。1 つは、リスナーをトレース ソースに直接追加する方法で、もう 1 つは、リスナーを共有リスナー コレクションに追加したうえで、名前でリスナーをトレース ソースに追加する方法です。 2 つのリスナーに対して識別されたフィルターは、異なるソース レベルで初期化されます。 この結果、一部のメッセージが 2 つのリスナーのいずれか一方のみによって書き込まれます。  
  
     構成ファイルは、アプリケーションが初期化されるときにトレース ソースの設定を初期化します。 アプリケーションは、構成ファイルによって設定されたプロパティを動的に変更し、ユーザーによって指定された設定値をオーバーライドできます。 たとえば、現在の構成設定とは無関係に、警告メッセージが常にテキスト ファイルに送られるようにすることもできます。 このプログラム例は、構成ファイルの設定値をオーバーライドして、警告メッセージがトレース リスナーに出力されるようにする方法を示しています。  
  
     アプリケーションの実行中に構成ファイルの設定値を変更しても、初期設定は変更されません。 設定を変更するには、アプリケーションを再起動するか、または <xref:System.Diagnostics.Trace.Refresh%2A?displayProperty=nameWithType> メソッドを使用して、プログラムによってアプリケーションを更新する必要があります。  
  
### <a name="to-initialize-trace-sources-listeners-and-filters-without-a-configuration-file"></a>構成ファイルを使わずにトレース ソース、リスナー、およびフィルターを初期化するには  
  
- 次のプログラム例を使用すると、構成ファイルを使わずにトレース ソース全体のトレースが有効になります。 この方法はお勧めできませんが、構成ファイルに依存しないでトレース機能を確保する必要がある場合もあります。  
  
     [!code-csharp[TraceSourceExample2#1](../../../samples/snippets/csharp/VS_Snippets_CLR/tracesourceexample2/cs/program.cs#1)]
     [!code-vb[TraceSourceExample2#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/tracesourceexample2/vb/program.vb#1)]  
  
## <a name="see-also"></a>参照

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TextWriterTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.EventTypeFilter>
- [アプリケーションのトレースとインストルメント](tracing-and-instrumenting-applications.md)
