---
title: <assert> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/assert
- http://schemas.microsoft.com/.NetConfiguration/v2.0#assert
helpviewer_keywords:
- <assert> element
- assert element
ms.assetid: ef4c3229-b151-4d85-8091-e6456af9b935
ms.openlocfilehash: f3c1a1670139a8262dea449bfff99c7c1c19f088
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74088949"
---
# <a name="assert-element"></a>\<assert > 要素
<xref:System.Diagnostics.Debug.Assert%2A?displayProperty=nameWithType> メソッドの呼び出し時にメッセージ ボックスを表示するかどうかを指定し、メッセージの書き込み先のファイルの名前も指定します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.diagnostics>** ](system-diagnostics-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<assert>**

## <a name="syntax"></a>構文  
  
```xml  
<assert assertuienabled="true|false" logfilename="file name"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`assertuienabled`|省略可能な属性です。<br /><br /> **デバッグの Assert**メソッドが**false**と評価されたときにメッセージボックスを表示するかどうかを指定します。|  
|`logfilename`|省略可能な属性です。<br /><br /> **デバッグ**が**false**と評価された場合にメッセージを書き込むファイルの名前を指定します。|  
  
## <a name="assertuienabled-attribute"></a>assertuienabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|`true`|メッセージボックスを表示します。 既定値です。|  
|`false`|では、メッセージボックスは表示されません。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`system.diagnostics`|メッセージを収集、格納、およびルーティングするトレース リスナーとトレース スイッチを設定するレベルを指定します。|  
  
## <a name="remarks"></a>Remarks  
 **\<assert >** 要素の両方の属性は省略可能です。 メッセージを書き込むファイルを指定せずにメッセージボックスを無効にすることも、メッセージを有効にしたままメッセージを書き込むファイルを指定することもできます。  
  
## <a name="example"></a>例  
 次の例は、 **Assert**を呼び出し、メッセージを `c:\log.txt`に書き込むときに、メッセージボックスの表示を無効にする方法を示しています。  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <assert assertuienabled="false" logfilename="c:\log.txt"/>  
   </system.diagnostics>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.Debug>
- [トレースおよびデバッグ設定のスキーマ](index.md)
