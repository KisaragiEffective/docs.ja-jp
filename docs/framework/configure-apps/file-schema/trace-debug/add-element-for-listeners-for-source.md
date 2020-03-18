---
title: <source> の <listeners> の <add> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/sources/source/listeners/add
helpviewer_keywords:
- initializeData attribute
- add element for <listeners> for <source>
- <add> element for <listeners> for <source>
ms.assetid: 4ce36ac1-81ef-48e8-b8b2-b5a5b0e2adcb
ms.openlocfilehash: c32205310f9fc451a5a55a943f925ee52f65c8a8
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089000"
---
# <a name="add-element-for-listeners-for-source"></a>\<source> の \<listeners> の \<add> 要素
トレース ソースの `Listeners` コレクションにリスナーを追加します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.diagnostics>** ](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<sources>** ](sources-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<sources>** ](source-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<listeners>** ](listeners-element-for-source.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<add>**

## <a name="syntax"></a>構文  
  
```xml  
<add name="name"   
  type="TraceListenerClassName, Version, Culture, PublicKeyToken"  
  initializeData="data"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`type`|必須属性。 `sharedListeners` コレクション内のリスナーを参照している場合を除き、名前で参照する必要があります ([例](#example)を参照)。<br /><br /> リスナーの種類を指定します。 [「完全修飾型名の指定](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)」で指定した要件を満たす文字列を使用する必要があります。|  
|`initializeData`|省略可能な属性です。<br /><br /> 指定したクラスのコンストラクターに渡される文字列。 クラスに文字列を受け取るコンストラクターがない場合、<xref:System.Configuration.ConfigurationException> がスローされます。|  
|`name`|省略可能な属性です。<br /><br /> リスナーの名前を指定します。|  
|`traceOutputOptions`|省略可能な属性です。<br /><br /> トレースリスナーの <xref:System.Diagnostics.TraceListener.TraceOutputOptions%2A> プロパティ値を指定します。|  
|[カスタム属性]|省略可能な属性。<br /><br /> リスナーの <xref:System.Diagnostics.TraceListener.GetSupportedAttributes%2A> メソッドによって識別されるリスナー固有の属性の値を指定します。 <xref:System.Diagnostics.DelimitedListTraceListener.Delimiter%2A> は、<xref:System.Diagnostics.DelimitedListTraceListener> クラスに固有の追加属性の例です。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<filter>](filter-element-for-add-for-listeners-for-source.md)|トレース ソースの `Listeners` コレクション内のリスナーにフィルターを追加します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`system.diagnostics`|メッセージを収集、格納、およびルーティングするトレース リスナーとトレース スイッチを設定するレベルを指定します。|  
|`sources`|トレース メッセージを開始するトレース ソースを保持します。|  
|`source`|トレース メッセージを開始するトレース ソースを指定します。|  
|`listeners`|メッセージを収集、格納、およびルーティングするリスナーを指定します。|  
  
## <a name="remarks"></a>Remarks  
 .NET Framework に付属しているリスナークラスは、<xref:System.Diagnostics.TraceListener> クラスから派生します。  
  
 トレースリスナーの `name` 属性を指定しない場合、トレースリスナーの <xref:System.Diagnostics.TraceListener.Name%2A> プロパティの既定値は空の文字列 ("") になります。 アプリケーションにリスナーが1つしかない場合は、名前を指定せずに追加することができます。名前に空の文字列を指定することで削除できます。 ただし、アプリケーションに複数のリスナーがある場合は、トレースリスナーごとに一意の名前を指定する必要があります。これにより、<xref:System.Diagnostics.TraceSource.Listeners%2A?displayProperty=nameWithType> コレクション内の個々のトレースリスナーを識別および管理できます。  
  
> [!NOTE]
> 同じ種類の複数のトレースリスナーを同じ名前で追加すると、その型と名前のトレースリスナーが1つだけ `Listeners` コレクションに追加されます。 ただし、複数の同じリスナーを `Listeners` コレクションにプログラムで追加することができます。  
  
 `initializeData` 属性の値は、作成するリスナーの種類によって異なります。 すべてのトレースリスナーで `initializeData`を指定する必要はありません。  
  
> [!NOTE]
> `initializeData` 属性を使用すると、"initializeData" 属性が宣言されていないことを示すコンパイラの警告が表示されることがあります。 この警告は、`initializeData` 属性を認識しない抽象基本クラス <xref:System.Diagnostics.TraceListener>に対して構成設定が検証されるために発生します。 通常、パラメーターを受け取るコンストラクターを持つトレースリスナーの実装では、この警告を無視できます。  
  
 次の表に、.NET Framework に含まれているトレースリスナーと、それらの `initializeData` 属性の値を示します。  
  
|トレースリスナークラス|initializeData 属性値|  
|--------------------------|------------------------------------|  
|<xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.ConsoleTraceListener.%23ctor%2A> コンストラクターの `useErrorStream` 値。  トレース出力とデバッグ出力を標準エラーストリームに書き込むには、`initializeData` 属性を "`true`" に設定します。標準出力ストリームに書き込むには、"`false`" に設定します。|  
|<xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.DelimitedListTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType>|既存のイベントログソースの名前。|  
|<xref:System.Diagnostics.EventSchemaTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.EventSchemaTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.TextWriterTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.TextWriterTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.XmlWriterTraceListener> が書き込むファイルの名前。|  
  
## <a name="configuration-file"></a>構成ファイル  
 この要素は、コンピューターの構成ファイル (machine.config) とアプリケーション構成ファイルで使用できます。  
  
## <a name="example"></a>例  
 次の例では、`<add>` 要素を使用して、トレースソース `TraceSourceApp`の `Listeners` コレクションにリスナー `console` と `textListener` を追加する方法を示します。 `textListener` リスナーは、トレース出力をファイル myListener .log に書き込みます。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <sources>  
      <source name="TraceSourceApp" switchName="sourceSwitch"   
        switchType="System.Diagnostics.SourceSwitch">  
        <listeners>  
          <add name="console"   
            type="System.Diagnostics.ConsoleTraceListener"/>  
          <add name="textListener"/>  
          <remove name="Default"/>  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add name="textListener"   
        type="System.Diagnostics.TextWriterTraceListener"   
        initializeData="myListener.log"/>  
    </sharedListeners>  
    <switches>  
      <add name="sourceSwitch" value="Warning"/>  
    </switches>  
  </system.diagnostics>  
</configuration>   
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.TraceSource>
- <xref:System.Diagnostics.TraceListener>
- [トレースおよびデバッグ設定のスキーマ](index.md)
- [トレース リスナー](../../../debug-trace-profile/trace-listeners.md)
