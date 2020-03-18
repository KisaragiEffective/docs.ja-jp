---
title: <system.web> 要素 (Web 設定)
ms.date: 03/30/2017
helpviewer_keywords:
- Web.config configuration file [ASP.NET]
- system.Web element
- <system.Web> element
- ASP.NET configuration system
- configuration files [ASP.NET]
ms.assetid: 24c4cf4f-ad32-42b2-b040-8e4549e2855e
ms.openlocfilehash: 5c5c857d4494b6d78b819e56bae4213abc5e2035
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71699091"
---
# <a name="systemweb-element-web-settings"></a>\<system.web > 要素 (Web 設定)
ASP.NET ホスティングレイヤーがプロセス全体の動作をどのように管理するかについて説明します。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp; **\<system.web >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<system.web>  
</system.web>  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  

次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  

[なし]。  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<applicationPool>](applicationpool-element-web-settings.md)|Aspnet ファイル内の IIS アプリケーションプールの構成設定を指定します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<configuration>](../configuration-element.md)|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素を指定します。|  
  
## <a name="remarks"></a>コメント  

`system.web` 要素とその子 `applicationPool` 要素が .NET Framework 3.5 SP1 の .NET Framework に追加されました。 IIS 7.0 以降のバージョンを統合モードで実行すると、この要素の組み合わせによって、ASP.NET がどのようにスレッドを管理し、ASP.NET が IIS アプリケーションプールでホストされている場合の要求をキューに配置するかを構成できます。 IIS 7.0 以降のバージョンをクラシックモードまたは ISAPI モードで実行した場合、これらの設定は無視されます。  
  
## <a name="example"></a>例  

次の例は、ASP.NET が IIS アプリケーションプールでホストされている場合に、ASP.NET ファイルでプロセス全体の動作を構成する方法を示しています。 この例では、IIS が統合モードで実行されており、アプリケーションが .NET Framework 3.5 SP1 以降のバージョンを使用していることを前提としています。 この動作は、.NET Framework 3.5 SP1 より前のバージョンの .NET Framework では発生しません。 この例の値は既定値です。  
  
```xml  
<configuration>  
  <system.web>  
    <applicationPool   
        maxConcurrentRequestsPerCPU="5000"   
        maxConcurrentThreadsPerCPU="0"   
        requestQueueLimit="5000" />  
  </system.web>  
</configuration>  
```  
  
## <a name="element-information"></a>要素情報  
  
|||  
|-|-|  
|Namespace||  
|スキーマ名||  
|検証ファイル||  
|空にすることができます||  
  
## <a name="see-also"></a>参照

- [\<applicationPool> 要素 (Web 設定)](applicationpool-element-web-settings.md)
