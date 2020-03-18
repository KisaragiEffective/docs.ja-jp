---
title: プロキシの構成
ms.date: 06/18/2018
helpviewer_keywords:
- Networking
- adaptive proxies
- static proxies
- Network Resources
- Internet, proxy configuration
- proxies
- network, proxy configuration
- proxies, configuring
ms.assetid: 353c0a8b-4cee-44f6-8e65-60e286743df9
ms.openlocfilehash: 1fbfe25b90e810ff96924a2341582ff3f5ee5e5d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71047359"
---
# <a name="proxy-configuration"></a>プロキシの構成
プロキシ サーバーは、リソースに対するクライアント要求を処理します。 プロキシは、要求されたリソースをキャッシュから返したり、リソースが存在するサーバーに要求を転送したりできます。 プロキシは、リモート サーバーに送信された要求の数を減らすことで、ネットワークのパフォーマンスを向上できます。 プロキシを使用して、リソースへのアクセスを制限することもできます。  
  
## <a name="adaptive-proxies"></a>アダプティブ プロキシ  
 .NET Framework には、アダプティブと静的という 2 種類のプロキシがあります。 アダプティブ プロキシは、ネットワーク構成を変更するときに、その設定を調整します。 たとえば、ラップトップ ユーザーがダイヤルアップ ネットワーク接続を起動する場合、アダプティブ プロキシはこの変更を認識し、新しい構成スクリプトを検出して実行し、その設定を適切に調整します。  
  
 アダプティブ プロキシは、構成スクリプトによって構成されます (「[自動プロキシ検出](automatic-proxy-detection.md)」を参照)。 スクリプトは、一連のアプリケーション プロトコルと、各プロトコル用のプロキシを生成します。  
  
 ネットワーク環境での変更により、システムで新しい一連のプロキシを使用することが必要となる場合があります。 ネットワーク接続がダウンしたか、新しいネットワーク接続が開始された場合、システムは新しい環境で構成スクリプトの適切なソースを検出し、新しいスクリプトを実行する必要があります。  
  
 構成ファイル内の `usesystemdefault`[`<proxy>` 要素の ](../configure-apps/file-schema/network/proxy-element-network-settings.md) 属性を使用できます。 `usesystemdefault` 属性は、静的プロキシ設定 (プロキシ アドレス、バイパス リスト、およびローカルでのバイパス) をユーザーの Internet Explorer プロキシ設定から読み取るかどうかを制御します。 この値を `true` に設定した場合、Internet Explorer の静的プロキシ設定が使用されます。 この値が `false` であるか、または設定しない場合、静的プロキシ設定を構成で指定することができ、Internet Explorer のプロキシ設定をオーバーライドできます。 アダプティブ プロキシを有効にする場合も、この値を `false` に設定するか、または設定しないようにする必要があります。  
  
 一般的なアダプティブ プロキシの構成例を次に示します。  
  
```xml  
<system.net>  
    <defaultProxy>  
      <proxy usesystemdefault="false" />
    </defaultProxy>  
</system.net>  
```  
  
## <a name="static-proxies"></a>静的プロキシ  
 静的プロキシは、通常、アプリケーションによって明示的に構成されるか、アプリケーションまたはシステムによって構成ファイルが呼び出されたときに構成されます。 静的プロキシは、企業ネットワークに接続されているデスクトップ コンピューターなど、トポロジがあまり変更されないネットワークで役立ちます。  
  
 いくつかのオプションで静的プロキシの動作を制御します。 以下を指定できます。  
  
- プロキシのアドレス。  
  
- ローカル アドレスに対してプロキシをバイパスするかかどうか。  
  
- 一連のアドレスに対してプロキシをバイパスするかかどうか。  
  
 次の表に、静的プロキシの構成オプションを示します。  
  
|属性、プロパティ、または構成ファイルの設定|[説明]|  
|--------------------------------------------------------|-----------------|  
|`proxyaddress` または <xref:System.Net.WebProxy.Address>|使用するプロキシのアドレス。|  
|`bypassonlocal` または <xref:System.Net.WebProxy.BypassProxyOnLocal>|ローカル アドレスに対してプロキシをバイパスするかどうかを制御します。|  
|`bypasslist` または <xref:System.Net.WebProxy.BypassArrayList>|正規表現を使用して、プロキシをバイパスするアドレスのセットを記述します。|  
|`usesystemdefault`|静的プロキシ設定 (プロキシ アドレス、バイパス リスト、およびローカルでのバイパス) をユーザーの Internet Explorer プロキシ設定から読み取るかどうかを制御します。 この値を `true` に設定した場合、Internet Explorer の静的プロキシ設定が使用されます。 .NET Framework 2.0 でこの値を `true` に設定した場合、Internet Explorer のプロキシ設定は構成ファイル内の他のプロキシ設定でオーバーライドされません。 .NET Framework 1.1 では、構成ファイル内の他のプロキシ設定で Internet Explorer のプロキシ設定をオーバーライドできます。<br /><br /> この値が `false` であるか、または設定しない場合、静的プロキシ設定を構成で指定することができ、Internet Explorer のプロキシ設定をオーバーライドできます。 アダプティブ プロキシを有効にする場合も、この値を `false` に設定するか、または設定しないようにする必要があります。|  
  
 一般的な静的プロキシの構成例を次に示します。  
  
```xml  
<system.net>  
    <defaultProxy>  
        <proxy  proxyaddress="http://proxy.contoso.com:3128"  
                bypassonlocal="true"  
        />  
        <bypasslist>  
            <add address="[a-z]+.blueyonderairlines.com$" />  
        </bypasslist>  
    </defaultProxy>  
</system.net>  
```  
  
## <a name="see-also"></a>参照

- <xref:System.Net.WebProxy>
- <xref:System.Net.GlobalProxySelection>
- [自動プロキシ検出](automatic-proxy-detection.md)
