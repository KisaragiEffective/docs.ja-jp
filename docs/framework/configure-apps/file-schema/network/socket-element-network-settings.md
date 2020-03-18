---
title: <socket> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/settings/socket
- http://schemas.microsoft.com/.NetConfiguration/v2.0#socket
helpviewer_keywords:
- <socket> element
- socket element
ms.assetid: 366c634c-7d16-478f-aedf-053eda94a1a0
ms.openlocfilehash: 0e2b369eccfbc658a790ef61a961315a88361669
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089087"
---
# <a name="socket-element-network-settings"></a>\<socket> 要素 (ネットワーク設定)
ソケット操作が完了ポートを使用するかどうかを指定します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.net>** ](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<settings>** ](settings-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<socket>**

## <a name="syntax"></a>構文  
  
```xml  
<socket  
  alwaysUseCompletionPortsForConnect="true|false"  
  alwaysUseCompletionPortsForAccept="true|false"  
  ipProtectionLevel="EdgeRestricted|Restricted|Unrestricted|Unspecified"  
/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|**属性**|**説明**|  
|-------------------|---------------------|  
|`alwaysUseCompletionPortsForAccept`|Accept メソッド呼び出しに対して、ソケットが常に完了ポートを使用する必要があるかどうかを示します。 既定値は `false`です。|  
|`alwaysUseCompletionPortsForConnect`|接続メソッドの呼び出しで、ソケットが常に完了ポートを使用する必要があるかどうかを示します。 既定値は `false`です。|  
|`ipProtectionLevel`|ソケットに使用する既定の <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> を指定します。 既定値は、Windows のバージョンによって異なります。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|[settings](settings-element-network-settings.md)|<xref:System.Net> 名前空間の基本的なネットワーク オプションを構成します。|  
  
## <a name="remarks"></a>Remarks  
 `alwaysUseCompletionPortsForAccept` 属性と `alwaysUseCompletionPortsForConnect` 属性を使用して、<xref:System.Net.Sockets?displayProperty=nameWithType>のクラスによる完了ポートの使用に関する既定の動作を指定します。 ハイパフォーマンスサーバーアプリケーションでは、完了ポートをお勧めします。  
  
 `alwaysUseCompletionPortsForAccept` 属性と `alwaysUseCompletionPortsForConnect` 属性の既定値は**false**です。  
  
 <xref:System.Net.Configuration.SocketElement.AlwaysUseCompletionPortsForAccept%2A> を使用すると、適用可能な構成ファイルから `alwaysUseCompletionPortsForAccept` 属性の現在の値を取得できます。 <xref:System.Net.Configuration.SocketElement.AlwaysUseCompletionPortsForConnect%2A> を使用すると、適用可能な構成ファイルから `alwaysUseCompletionPortsForConnect` 属性の現在の値を取得できます。  
  
 `ipProtectionLevel` 属性は、ソケットに使用する既定の <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> を指定します。 <xref:System.Net.Configuration.SocketElement.IPProtectionLevel%2A> プロパティを使用すると、同じリンクローカルまたはサイトローカルプレフィックスを持つアドレスなど、指定されたスコープに IPv6 ソケットの制限を構成できます。 このオプションを使用すると、アプリケーションは IPv6 ソケットにアクセス制限を設けることができます。 この制限により、プライベート LAN で実行されるアプリケーションを外部からの攻撃に対して簡単かつ堅牢に強化できます。 このオプションは、リッスンしているソケットの範囲を拡大または縮小し、必要に応じてパブリックユーザーおよびプライベートユーザーからの無制限のアクセスを有効にするか、必要に応じて同じサイトへのアクセスのみを制限します。  
  
 この `ipProtectionLevel` 属性設定は、初期の受信トラフィックのみに影響します。  
  
- ソケットで着信接続をリッスンしている TCP サーバー。  
  
- ソケットでパケットを受信する UDP アプリケーション。  
  
 この構成設定は、既に確立されている TCP 接続には影響しません (トラフィックは両方向に制限されません)。また、UDP パケットを送信するアプリケーションには影響しません。  
  
 `ipProtectionLevel` 属性の設定に使用できる値は、次のように <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> 列挙で指定されている定義済みの保護レベルに対応しています。  
  
|**属性値**|**説明**|  
|-|-|  
|EdgeRestricted|IP 保護レベルは edge で制限されています。 この値は、インターネット経由で動作するように設計されたアプリケーションによって使用されます。 この設定では、Windows Teredo 実装を使用したネットワークアドレス変換 (NAT) トラバーサルは許可されません。 これらのアプリケーションは IPv4 ファイアウォールをバイパスする可能性があるため、開いているポートを使用しているインターネット攻撃に対してアプリケーションを強化する必要があります。 Windows Server 2003 および Windows XP では、ソケットの IP 保護レベルの既定値はエッジに制限されています。|  
|Restricted|IP 保護レベルは制限されています。 この値は、インターネットのシナリオを実装しないイントラネットアプリケーションで使用されます。 これらのアプリケーションは、通常、インターネットスタイルの攻撃に対してテストまたはセキュリティで保護されていません。 この設定により、受信したトラフィックがリンクローカルのみに制限されます。|  
|無制限|IP 保護レベルは無制限です。 この値は、インターネット経由で動作するように設計されたアプリケーション (たとえば、Windows に組み込まれた IPv6 NAT トラバーサル機能を利用するアプリケーションなど) によって使用されます。 これらのアプリケーションは IPv4 ファイアウォールをバイパスする可能性があるため、開いているポートを使用しているインターネット攻撃に対してアプリケーションを強化する必要があります。 Windows Server 2008 R2 および Windows Vista では、ソケットの IP 保護レベルの既定値は無制限です。|  
|指定されていません。|IP 保護レベルが指定されていません。 Windows 7 と Windows Server 2008 R2 では、ソケットの IP 保護レベルの既定値は指定されていません。|  
  
 `ipProtectionLevel` 属性の既定値は**指定**されていません。  
  
 <xref:System.Net.Configuration.SocketElement.IPProtectionLevel%2A> プロパティは、適用可能な構成ファイルから `ipProtectionLevel` 属性の現在の値を取得するために使用できます。  
  
## <a name="configuration-files"></a>構成ファイル  
 この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。  
  
## <a name="example"></a>例  
 次の例は、完了ポートを使用するように指定する方法と、既定の <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType> を制限しないようにする方法を示しています。  
  
```xml  
<configuration>  
  <system.net>  
    <settings>  
      <socket  
        alwaysUseCompletionPortsForAccept="true"  
        alwaysUseCompletionPortsForConnect="true"  
        ipProtectionLevel="Unrestricted"  
       />  
    </settings>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Net?displayProperty=nameWithType>
- <xref:System.Net.Configuration.SocketElement?displayProperty=nameWithType>
- <xref:System.Net.Sockets?displayProperty=nameWithType>
- <xref:System.Net.Sockets.IPProtectionLevel?displayProperty=nameWithType>
- <xref:System.Net.Sockets.SocketOptionName.IPProtectionLevel?displayProperty=nameWithType>
- [ネットワーク設定スキーマ](index.md)
