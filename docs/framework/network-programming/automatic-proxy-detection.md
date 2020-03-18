---
title: 自動プロキシ検出
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- automatic proxy detections
- Web Proxy Auto-Discovery
- Web proxy
- detecting proxies automatically
- WebProxy class, automatic proxy detections
- proxies, automatically detecting
- network
- WPAD (Web Proxy Auto-Discovery)
ms.assetid: fcd9c3bd-93de-4c92-8ff3-837327ad18de
ms.openlocfilehash: 4c5bc9e0efb39032d388d141e8bccf3e520ebd45
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79180891"
---
# <a name="automatic-proxy-detection"></a>自動プロキシ検出
自動プロキシ検出は、Web プロキシ サーバーがシステムによって確認され、クライアントに代わって要求を送信する際に使用されるプロセスです。 この機能は、Web プロキシの自動検出 (WPAD) とも呼ばれます。 自動プロキシ検出を有効にすると、システムは、要求に使用できるプロキシのセットを返すプロキシ構成スクリプトを検索しようとします。 プロキシ構成スクリプトが見つかった場合、プロキシ情報、要求ストリーム、または <xref:System.Net.WebProxy> インスタンスを使用する要求に対する応答が取得されたときに、ローカル コンピューター上でスクリプトがダウンロード、コンパイル、および実行されます。  
  
 自動プロキシ検出は <xref:System.Net.WebProxy> クラスによって実行され、要求レベルの設定、構成ファイルの設定、および Internet Explorer の**ローカル エリア ネットワーク (LAN)** ダイアログ ボックスを使用して指定された設定を使用できます。  
  
> [!NOTE]
> Internet Explorer の **[ローカル エリア ネットワーク (LAN) の設定]** ダイアログ ボックスを表示するには、Internet Explorer のメイン メニューで **[ツール]** を選択してから **[インターネット オプション]** を選択します。 次に、 **[接続]** タブを選択して、 **[LAN の設定]** をクリックします。  
  
 自動プロキシ検出を有効にすると、<xref:System.Net.WebProxy> クラスは次のようにプロキシ構成スクリプトを検索しようとします。  
  
1. WinINet の `InternetQueryOption` 関数を使用して、Internet Explorer によって最後に検出されたプロキシ構成スクリプトを検索します。  
  
2. スクリプトが見つからない場合、<xref:System.Net.WebProxy> クラスは動的ホスト構成プロトコル (DHCP) を使用してスクリプトを検索します。 DHCP サーバーは、スクリプトの場所 (ホスト名)、またはスクリプトの完全な URL で応答できます。  
  
3. DHCP で WPAD ホストが識別されない場合は、名前またはエイリアスとして WPAD を使用して、ホストについて DNS が照会されます。  
  
4. ホストが識別されず、プロキシ構成スクリプトの場所が Internet Explorer の LAN の設定または構成ファイルで指定されている場合は、この場所が使用されます。  
  
> [!NOTE]
> NT サービスまたは ASP.NET の一部として実行されているアプリケーションでは、呼び出しユーザーの Internet Explorer のプロキシ サーバー設定が使用されます (使用可能な場合)。 これらの設定が、すべてのサービス アプリケーションで使用できるとは限りません。  
  
 プロキシは、connectoid ごとに構成されます。 connectoid は、ネットワーク接続ダイアログにある項目で、物理ネットワーク デバイス (モデムまたはイーサネット カード) または仮想インターフェイス (ネットワーク デバイス経由で実行されている VPN 接続など) を指定することができます。 connectoid が変更された場合 (ワイヤレス接続でアクセス ポイントが変更された場合や、VPN が有効になった場合など)、プロキシ検出アルゴリズムが再度実行されます。  
  
 既定では、プロキシの検出には Internet Explorer のプロキシ設定が使用されます。 アプリケーションが、対話型ではないアカウント (つまり、IE のプロキシ設定を構成するための便利な手段を持たないアカウント) で実行されている場合、または IE の設定とは異なるプロキシ設定を使用する必要がある場合は、[\<defaultProxy> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/defaultproxy-element-network-settings.md) と [\<proxy> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/proxy-element-network-settings.md) 要素が定義された構成ファイルを作成することで、プロキシを構成できます。  
  
 要求を自分で作成する場合は、次のコード例に示すように、null の <xref:System.Net.WebRequest.Proxy%2A> を要求で使用することにより、要求レベルで自動プロキシ検出を無効にできます。  
  
```csharp  
public static void DisableForMyRequest (Uri resource)  
{  
    WebRequest request = WebRequest.Create (resource);  
    request.Proxy = null;  
    WebResponse response = request.GetResponse ();  
}  
```  
  
```vb  
Public Shared Sub DisableForMyRequest(ByVal resource As Uri)  
    Dim request As WebRequest = WebRequest.Create(resource)  
    request.Proxy = Nothing  
    Dim response As WebResponse = request.GetResponse()  
    End Sub
```  
  
 プロキシがない要求では、アプリケーション ドメインの既定のプロキシが使用されます。この既定のプロキシは <xref:System.Net.WebRequest.DefaultWebProxy%2A> プロパティで使用できます。  
  
## <a name="see-also"></a>参照

- <xref:System.Net.WebProxy>
- <xref:System.Net.WebRequest>
- [\<system.Net> 要素 (ネットワーク設定)](../configure-apps/file-schema/network/system-net-element-network-settings.md)
