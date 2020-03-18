---
title: '方法: ネットワークのトレースを構成する'
ms.date: 03/30/2017
helpviewer_keywords:
- formatting [.NET Framework], network tracing
- network tracing, configuring
- level attribute
- app.config files, network tracing
- configuration files [.NET Framework], network tracing
- protocol-level trace output
- application configuration files, network tracing
- sockets, trace output
ms.assetid: 5ef9fe4b-8d3d-490e-9259-1d014b2181af
ms.openlocfilehash: 06132509860b16d1e22cfdf7e3226c968d16b7cf
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73040640"
---
# <a name="how-to-configure-network-tracing"></a>方法: ネットワークのトレースを構成する

アプリケーションまたはコンピューターの構成ファイルには、ネットワークのトレースの形式と内容を決定する設定が保持されます。 この手順に従う前に、トレースが有効になっていることを確認します。 詳細については、「[ネットワークのトレースの有効化](enabling-network-tracing.md)」を参照してください。

コンピューター構成ファイルの *machine.config* は、 *%windir%\Microsoft.NET\Framework* フォルダーに格納されています。 *machine.config* は、コンピューターにインストールされている .NET Framework のバージョンごとに、 *%windir%\Microsoft.NET\Framework* フォルダーの下に、次のように別ファイルで用意されています。

- *C:\WINDOWS\Microsoft.NET\Framework\v2.0.50727\Config\machine.config*
- *C:\WINDOWS\Microsoft.NET\Framework64\v4.0.30319\Config\machine.config*

これらの設定は、コンピューター構成ファイルよりも優先されるアプリケーション構成ファイルでも行うことができます。

## <a name="configure-network-tracing"></a>ネットワークのトレースを構成する

ネットワークのトレースを構成するには、適切な構成ファイルに次の行を追加します。 これらの設定の値とオプションを次の表に示します。

```xml
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.Net" tracemode="includehex" maxdatasize="1024">
        <listeners>
          <add name="System.Net"/>
        </listeners>
      </source>
      <source name="System.Net.Cache">
        <listeners>
          <add name="System.Net"/>
        </listeners>
      </source>
      <source name="System.Net.Http">
        <listeners>
          <add name="System.Net"/>
        </listeners>
      </source>
      <source name="System.Net.Sockets">
        <listeners>
          <add name="System.Net"/>
        </listeners>
      </source>
      <source name="System.Net.WebSockets">
        <listeners>
          <add name="System.Net"/>
        </listeners>
      </source>
   </sources>
    <switches>
      <add name="System.Net" value="Verbose"/>
      <add name="System.Net.Cache" value="Verbose"/>
      <add name="System.Net.Http" value="Verbose"/>
      <add name="System.Net.Sockets" value="Verbose"/>
      <add name="System.Net.WebSockets" value="Verbose"/>
    </switches>
    <sharedListeners>
      <add name="System.Net"
        type="System.Diagnostics.TextWriterTraceListener"
        initializeData="network.log"
        traceOutputOptions="ProcessId, DateTime"
      />
    </sharedListeners>
    <trace autoflush="true"/>
  </system.diagnostics>
</configuration>
```

### <a name="trace-output-from-methods"></a>メソッドからのトレース出力

`<switches>` ブロックに名前を追加すると、トレース出力にその名前に関連する一部のメソッドからの情報が含まれます。 次の表に出力を示します。

|名前|出力元|
|----------|-----------------|
|`System.Net.Sockets`|<xref:System.Net.Sockets.Socket>、<xref:System.Net.Sockets.TcpListener>、<xref:System.Net.Sockets.TcpClient>、および <xref:System.Net.Dns> クラスの一部のパブリック メソッド。|
|`System.Net`|<xref:System.Net.HttpWebRequest>、<xref:System.Net.HttpWebResponse>、<xref:System.Net.FtpWebRequest>、および <xref:System.Net.FtpWebResponse> クラスの一部のパブリック メソッド、および SSL デバッグ情報 (無効な証明書、不足している発行元一覧、およびクライアント証明書エラー)。|
|`System.Net.HttpListener`|<xref:System.Net.HttpListener>、<xref:System.Net.HttpListenerRequest>、および <xref:System.Net.HttpListenerResponse> クラスの一部のパブリック メソッド。|
|`System.Net.Cache`|`System.Net.Cache` 内の一部のプライベート メソッドと内部メソッド。|
|`System.Net.Http`|<xref:System.Net.Http.HttpClient>、<xref:System.Net.Http.DelegatingHandler>、<xref:System.Net.Http.HttpClientHandler>、<xref:System.Net.Http.HttpMessageHandler>、<xref:System.Net.Http.MessageProcessingHandler>、および <xref:System.Net.Http.WebRequestHandler> クラスの一部のパブリック メソッド。|
|`System.Net.WebSockets.WebSocket`|<xref:System.Net.WebSockets.ClientWebSocket> および <xref:System.Net.WebSockets.WebSocket> クラスの一部のパブリック メソッド。|

### <a name="trace-output-attributes"></a>トレースの出力属性

次の表に示した属性で、トレース出力を構成できます。

|属性名|属性値|
|--------------------|---------------------|
|`value`|必須の <xref:System.String> 属性です。 出力の詳細度を設定します。 有効な値は、`Critical`、`Error`、`Verbose`、`Warning`、および `Information` です。<br /><br />この属性は、**switches** 要素の **add** 要素に設定する必要があります。 この属性を **source** 要素に設定すると、例外がスローされます。<br/><br/>例 : `<add name="System.Net" value="Verbose"/>`|
|`maxdatasize`|省略可能な <xref:System.Int32> 属性です。 各行トレースに含まれるネットワーク データの最大バイト数を設定します。 既定値は 1024 です。<br /><br />この属性は、**source** 要素に設定する必要があります。 この属性を **switches** 要素の下の要素に設定すると、例外がスローされます。<br/><br/>例 : `<source name="System.Net" tracemode="includehex" maxdatasize="1024">`|
|`tracemode`|省略可能な <xref:System.String> 属性です。 プロトコル トレースを 16 進数およびテキストの形式で表示するには、`includehex` に設定します。 テキストのみを表示するには、`protocolonly` に設定します。 既定値は `includehex` です。<br /><br />この属性は、**source** 要素に設定する必要があります。 この属性を **switches** 要素の下の要素に設定すると、例外がスローされます。<br/><br/>例 : `<source name="System.Net" tracemode="includehex" maxdatasize="1024">`|

## <a name="see-also"></a>関連項目

- [ネットワークのトレースの解釈](interpreting-network-tracing.md)
- [.NET Framework のネットワークのトレース](network-tracing.md)
- [ネットワークのトレースの有効化](enabling-network-tracing.md)
- [アプリケーションのトレースとインストルメント](../debug-trace-profile/tracing-and-instrumenting-applications.md)
