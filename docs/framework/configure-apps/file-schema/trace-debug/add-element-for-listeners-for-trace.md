---
title: <trace> の <listeners> の <add> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners/add
helpviewer_keywords:
- initializeData attribute
- <add> element for <listeners>
- add element for <listeners>
ms.assetid: 81e804a3-ef11-4d39-bbde-bfa012c179e2
ms.openlocfilehash: eb3e9cf4a6d138998cfde865cda8ed4146be26d0
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74088984"
---
# <a name="add-element-for-listeners-for-trace"></a>\<リスナー > の > 要素を追加 \<\<トレース >
リスナーを**リスナー**コレクションに追加します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<** ](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<[**トレース >** ](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**リスナー >** ](listeners-element-for-trace.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**追加 >**

## <a name="syntax"></a>構文  
  
```xml  
<add name="name"   
     type="trace listener class name, Version, Culture, PublicKeyToken"  
     initializeData="data"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|**type**|必須の属性です。<br /><br /> リスナーの種類を指定します。 [「完全修飾型名の指定](../../../reflection-and-codedom/specifying-fully-qualified-type-names.md)」で指定した要件を満たす文字列を使用する必要があります。|  
|**initializeData**|省略可能な属性です。<br /><br /> 指定したクラスのコンストラクターに渡される文字列。|  
|**name**|省略可能な属性です。<br /><br /> リスナーの名前を指定します。|  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<filter>](filter-element-for-add-for-listeners-for-trace.md)|トレースの `Listeners` コレクション内のリスナーにフィルターを追加します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`listeners`|メッセージを収集、格納、およびルーティングするリスナーを指定します。 リスナーは、適切なターゲットにトレース出力を送信します。|  
|`system.diagnostics`|ASP.NET 構成セクションのルート要素を指定します。|  
|`trace`|トレース メッセージを収集、格納、およびルーティングするリスナーを保持します。|  
  
## <a name="remarks"></a>Remarks  
 <xref:System.Diagnostics.Debug> クラスと <xref:System.Diagnostics.Trace> クラスは、同じ**Listeners**コレクションを共有します。 これらのクラスのいずれかのコレクションにリスナーオブジェクトを追加すると、他のクラスは同じリスナーを使用します。 リスナークラスは、<xref:System.Diagnostics.TraceListener>から派生します。  
  
 トレースリスナーの `name` 属性を指定しない場合、トレースリスナーの <xref:System.Diagnostics.TraceListener.Name%2A> は既定で空の文字列 ("") になります。 アプリケーションにリスナーが1つしかない場合は、名前を指定せずにリスナーを追加し、名前に空の文字列を指定することで削除できます。 ただし、アプリケーションに複数のリスナーがある場合は、トレースリスナーごとに一意の名前を指定する必要があります。これにより、<xref:System.Diagnostics.Debug.Listeners%2A> コレクションと <xref:System.Diagnostics.Trace.Listeners%2A> コレクション内の個々のトレースリスナーを識別して管理できます。  
  
> [!NOTE]
> 同じ種類の複数のトレースリスナーを同じ名前で追加すると、その型と名前のトレースリスナーが1つだけ `Listeners` コレクションに追加されます。 ただし、複数の同じリスナーを `Listeners` コレクションにプログラムで追加することができます。  
  
 **Initializedata**属性の値は、作成するリスナーの種類によって異なります。 すべてのトレースリスナーで**Initializedata**を指定する必要はありません。  
  
> [!NOTE]
> `initializeData` 属性を使用すると、"initializeData" 属性が宣言されていないことを示すコンパイラの警告が表示されることがあります。 この警告は、`initializeData` 属性を認識しない抽象基本クラス <xref:System.Diagnostics.TraceListener>に対して構成設定が検証されるために発生します。 通常、パラメーターを受け取るコンストラクターを持つトレースリスナーの実装では、この警告を無視できます。  
  
 次の表に、.NET Framework に含まれるトレースリスナーと、 **Initializedata**属性の値について説明します。  
  
|トレースリスナークラス|initializeData 属性値|  
|--------------------------|------------------------------------|  
|<xref:System.Diagnostics.ConsoleTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.ConsoleTraceListener.%23ctor%2A> コンストラクターの `useErrorStream` 値。  `initializeData` 属性を "`true`" に設定して、トレース出力とデバッグ出力を <xref:System.Console.Error%2A?displayProperty=nameWithType>に書き込みます。 "`false`" を <xref:System.Console.Out%2A?displayProperty=nameWithType>に書き込みます。|  
|<xref:System.Diagnostics.DelimitedListTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.DelimitedListTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.EventLogTraceListener?displayProperty=nameWithType>|既存のイベントログソースの名前。|  
|<xref:System.Diagnostics.EventSchemaTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.EventSchemaTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.TextWriterTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.TextWriterTraceListener> が書き込むファイルの名前。|  
|<xref:System.Diagnostics.XmlWriterTraceListener?displayProperty=nameWithType>|<xref:System.Diagnostics.XmlWriterTraceListener> が書き込むファイルの名前。|  
  
## <a name="example"></a>例  
 次の例では、> 要素を追加して**リスナーコレクションにリスナー `MyListener`** および `MyEventListener` を追加 **\<** 方法を示します。 `MyListener` によって `MyListener.log` というファイルが作成され、出力がファイルに書き込まれます。 `MyEventListener` は、イベントログにエントリを作成します。  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <trace autoflush="true" indentsize="0">  
         <listeners>  
            <add name="myListener" type="System.Diagnostics.TextWriterTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089" initializeData="c:\myListener.log" />  
            <add name="MyEventListener"  
                 type="System.Diagnostics.EventLogTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"                 initializeData="MyConfigEventLog"/>  
            <add name="configConsoleListener"  
                 type="System.Diagnostics.ConsoleTraceListener, system, version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>  
         </listeners>  
      </trace>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.Debug>
- <xref:System.Diagnostics.EventLogTraceListener>
- <xref:System.Diagnostics.ConsoleTraceListener>
- <xref:System.Diagnostics.TextWriterTraceListener>
- [トレースおよびデバッグ設定のスキーマ](index.md)
- [トレース リスナー](../../../debug-trace-profile/trace-listeners.md)
