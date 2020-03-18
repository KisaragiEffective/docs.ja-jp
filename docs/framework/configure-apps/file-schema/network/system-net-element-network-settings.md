---
title: <system.Net> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#system.Net
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.Net
helpviewer_keywords:
- system.Net element
- <system.Net> element
ms.assetid: 52de4d6c-b24d-44aa-ba7d-6b5061f1357e
ms.openlocfilehash: 810e942394c75c192e4423afe4c674ef3a2b9900
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697503"
---
# <a name="systemnet-element-network-settings"></a>\<system.Net> 要素 (ネットワーク設定)
.NET Framework がネットワークに接続する方法を指定するための設定が含まれています。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp; **\<system. net >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<system.net>   
</system.net>  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
 [なし]。  
  
### <a name="child-elements"></a>子要素  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|[authenticationModules](authenticationmodules-element-network-settings.md)|インターネット要求を認証するために使用するモジュールを指定します。|  
|[connectionManagement](connectionmanagement-element-network-settings.md)|インターネットホストへの接続の最大数を指定します。|  
|[defaultProxy](defaultproxy-element-network-settings.md)|ハイパーテキスト転送プロトコル (HTTP: Hypertext Transfer Protocol) プロキシ サーバーを構成します。|  
|[mailSettings](mailsettings-element-network-settings.md)|簡易メール転送プロトコル (SMTP) 電子メールの送信オプションを構成します。|  
|[requestCaching](requestcaching-element-network-settings.md)|ネットワーク要求のキャッシュメカニズムを制御します。|  
|[settings](settings-element-network-settings.md)|<xref:System.Net> および関連する子名前空間のクラスの基本的なネットワークオプションを構成します。|  
|[webRequestModules](webrequestmodules-element-network-settings.md)|インターネットホストから情報を要求するために使用するモジュールを指定します。|  
  
### <a name="parent-elements"></a>親要素  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|[configuration](../configuration-element.md)|すべての名前空間の設定が含まれます。|  
  
## <a name="remarks"></a>コメント  
 [\<の >](system-net-element-network-settings.md)要素には、<xref:System.Net> および関連する子名前空間のクラスの設定が含まれています。 この設定では、インターネットホストから情報を受信するための認証モジュール、接続管理、メール設定、プロキシサーバー、およびインターネット要求モジュールを構成します。  
  
## <a name="example"></a>例  
 次の例は <xref:System.Net> クラスで使用される一般的な構成を示しています。  
  
```xml  
<configuration>  
  <system.net>  
    <authenticationModules>  
      <add type="System.Net.DigestClient" />  
      <add type="System.Net.NegotiateClient" />  
      <add type="System.Net.KerberosClient" />  
      <add type="System.Net.NtlmClient" />  
      <add type="System.Net.BasicClient" />  
    </authenticationModules>  
    <connectionManagement>  
      <add address="*" maxconnection="2" />  
    </connectionManagement>  
    <defaultProxy>  
      <proxy  
        usesystemdefault="true"  
        bypassonlocal="true"  
      />  
    </defaultProxy>  
    <webRequestModules>  
      <add prefix="http"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="https"  
           type="System.Net.HttpRequestCreator"  
      />  
      <add prefix="file"  
           type="System.Net.FileWebRequestCreator"  
      />  
    </webRequestModules>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>参照

- [ネットワーク設定スキーマ](index.md)
