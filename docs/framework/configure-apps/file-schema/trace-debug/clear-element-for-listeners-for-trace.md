---
title: <trace> の <listeners> の <clear> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/trace/listeners/clear
helpviewer_keywords:
- clear element for <listeners> for <trace>
- <clear> element for <listeners> for <trace>
ms.assetid: b44732a8-271f-4a06-ba9e-fe3298d6f192
ms.openlocfilehash: 049b8e7ed17633c0f34b062915afaf719927dad6
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74088916"
---
# <a name="clear-element-for-listeners-for-trace"></a>\<トレース > の \<リスナー > のクリア > 要素を \<します
トレースの `Listeners` コレクションを削除します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<** ](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<[**トレース >** ](trace-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<[**リスナー >** ](listeners-element-for-trace.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**クリア >**

## <a name="syntax"></a>構文  
  
```xml  
<clear/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
 なし。  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`system.diagnostics`|メッセージを収集、格納、およびルーティングするトレース リスナーとトレース スイッチを設定するレベルを指定します。|  
|`trace`|トレース メッセージを収集、格納、およびルーティングするリスナーを保持します。|  
|`listeners`|メッセージを収集、格納、およびルーティングするリスナーを格納します。 リスナーは、適切なターゲットにトレース出力を送信します。|  
  
## <a name="remarks"></a>Remarks  
 `<clear>` 要素は、トレースの `Listeners` コレクションからすべてのリスナーを削除します。 `<add>` 要素を使用して、コレクション内に他のアクティブなリスナーが存在しないことを特定するには、`<clear>` 要素を使用します。  
  
 <xref:System.Diagnostics.Trace.Listeners%2A?displayProperty=nameWithType> プロパティ (`System.Diagnostics.Trace.Listeners.Clear()`) で <xref:System.Diagnostics.TraceListenerCollection.Clear%2A> メソッドを呼び出すことによって、`Listeners` コレクションをプログラムでクリアできます。  
  
 この要素は、コンピューターの構成ファイル (machine.config) とアプリケーション構成ファイルで使用できます。  
  
> [!NOTE]
> `<clear>` 要素は `Listeners` コレクションから <xref:System.Diagnostics.DefaultTraceListener> を削除し、<xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType>、<xref:System.Diagnostics.Trace.Assert%2A?displayProperty=nameWithType>、<xref:System.Diagnostics.Debug.Fail%2A?displayProperty=nameWithType>、および <xref:System.Diagnostics.Trace.Fail%2A?displayProperty=nameWithType> の各メソッドの動作を変更します。 `Assert` または `Fail` メソッドを呼び出すと、通常、メッセージボックスが表示されます。 ただし、<xref:System.Diagnostics.DefaultTraceListener> が `Listeners` コレクションに含まれていない場合、メッセージボックスは表示されません。  
  
## <a name="example"></a>例  
 次の例は、`<add>` 要素を使用してリスナー `console` をトレースの `Listeners` コレクションに追加する前に、`<clear>` 要素を使用する方法を示しています。  
  
```xml  
<configuration>  
  <system.diagnostics>  
    <trace autoflush="false" indentsize="4">  
      <listeners>  
        <clear/>  
        <add name="console"   
          type="System.Diagnostics.ConsoleTraceListener" >  
          <filter type="System.Diagnostics.EventTypeFilter"   
            initializeData="Error" />  
        </add>  
      </listeners>  
    </trace>  
  </system.diagnostics>  
</configuration>   
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.Trace.Listeners%2A>
- <xref:System.Diagnostics.Trace>
- <xref:System.Diagnostics.Debug>
- <xref:System.Diagnostics.TraceSource>
- [トレースおよびデバッグ設定のスキーマ](index.md)
- [\<remove>](remove-element-for-listeners-for-trace.md)
- [トレース リスナー](../../../debug-trace-profile/trace-listeners.md)
