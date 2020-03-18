---
title: <webRequestModules> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/webRequestModules
- http://schemas.microsoft.com/.NetConfiguration/v2.0#webRequestModules
helpviewer_keywords:
- webRequestModules element
- <webRequestModules> element
ms.assetid: 1263de11-3e0a-4f94-97c9-710b2ae53817
ms.openlocfilehash: e119d9ce1f8bb6f07f8050612550db459a2f065c
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697461"
---
# <a name="webrequestmodules-element-network-settings"></a>\<webRequestModules> 要素 (ネットワーク設定)
ネットワークホストから情報を要求するために使用するモジュールを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<system.net>** ](system-net-element-network-settings.md)  
&nbsp; @ no__t-1 @ no__t @ no__t @ no__t-4webRequestModules >  
  
## <a name="syntax"></a>構文  
  
```xml  
<webRequestModules>   
</webRequestModules>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
 [なし] :  
  
### <a name="child-elements"></a>子要素  
  
|**要素**|**[説明]**|  
|-----------------|---------------------|  
|[add](add-element-for-webrequestmodules-network-settings.md)|アプリケーションにカスタム Web 要求モジュールを追加します。|  
|[clear](clear-element-for-webrequestmodules-network-settings.md)|アプリケーションから、登録されているすべての Web 要求モジュールを削除します。|  
|[remove](remove-element-for-webrequestmodules-network-settings.md)|アプリケーションからカスタム Web 要求モジュールを削除します。|  
  
### <a name="parent-elements"></a>親要素  
  
|**要素**|**[説明]**|  
|-----------------|---------------------|  
|[system.net](system-net-element-network-settings.md)|.NET Framework がネットワークに接続する方法を指定するための設定が含まれています。|  
  
## <a name="remarks"></a>コメント  
 @No__t-0 要素は、ネットワークホストへの情報要求を処理するために、<xref:System.Net.WebRequest> クラスの子孫を登録します。 Web 要求モジュールは @no__t 0 インターフェイスを実装する必要があります。  
  
 .NET Framework には、`http://`、`https://`、および `file://` で始まる Uri の Web 要求モジュールが含まれています。 既定のモジュールをオーバーライドするには、構成ファイルにカスタムモジュールを登録する必要があります。  
  
## <a name="configuration-files"></a>構成ファイル  
 この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。  
  
## <a name="example"></a>例  
 次の例では、既定の HTTP モジュールを登録します。 Version および PublicKeyToken の値は、指定されたモジュールの正しい値に置き換える必要があります。  
  
```xml  
<configuration>  
  <system.net>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator, System, Version=2.0.3600.0,  
           Culture=neutral, PublicKeyToken=b77a5c561934e089"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Net.WebRequest>
- <xref:System.Net.IWebRequestCreate>
- [ネットワーク設定スキーマ](index.md)
