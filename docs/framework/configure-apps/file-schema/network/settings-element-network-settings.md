---
title: <settings> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#settings
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings
helpviewer_keywords:
- settings element
- <settings> element
ms.assetid: 189ce989-c39b-427d-b004-6b82a668b931
ms.openlocfilehash: d510c445c585a36005ed415b14188efc4be03984
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089119"
---
# <a name="settings-element-network-settings"></a>\<settings> 要素 (ネットワーク設定)
<xref:System.Net?displayProperty=nameWithType> 名前空間の基本的なネットワーク オプションを構成します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.net>** ](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<settings>**

## <a name="syntax"></a>構文  
  
```xml  
<settings>  
  <httpListener>...</httpListener>  
  <httpWebRequest>...</httpWebRequest>  
  <ipv6>...</ipv6>  
  <performanceCounters>...</performanceCounters>  
  <servicePointManager>...</servicePointManager>  
  <socket>...</socket>  
  <webProxyScript>...</webProxyScript>  
</settings>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
 なし。  
  
### <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[httpListener](httplistener-element-network-settings.md)|<xref:System.Net.HttpListener> クラスによって使用されるパラメーターをカスタマイズします。|  
|[httpWebRequest](httpwebrequest-element-network-settings.md)|Web 要求パラメーターをカスタマイズします。|  
|[ipv6](ipv6-element-network-settings.md)|インターネットプロトコルバージョン 6 (IPv6) のサポートを有効にします。|  
|[\<performanceCounter > 要素 (ネットワーク設定)](performancecounter-element-network-settings.md)|ネットワークパフォーマンスカウンターを有効にします。|  
|[servicePointManager](servicepointmanager-element-network-settings.md)|ネットワークリソースへの接続を構成します。|  
|[socket](socket-element-network-settings.md)|ソケット操作が完了ポートを使用するかどうかを指定します。|  
|[\<webProxyScript > 要素 (ネットワーク設定)](webproxyscript-element-network-settings.md)|Web プロキシを検出するために使用するスクリプトの特性を構成します。|  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[system.net](system-net-element-network-settings.md)|.NET Framework がネットワークに接続する方法を指定するための設定が含まれています。|  
  
## <a name="remarks"></a>Remarks  
  
## <a name="configuration-files"></a>構成ファイル  
 この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Net?displayProperty=nameWithType>
- [ネットワーク設定スキーマ](index.md)
