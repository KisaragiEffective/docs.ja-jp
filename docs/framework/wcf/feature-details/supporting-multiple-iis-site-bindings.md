---
title: 複数の IIS サイト バインディングのサポート
ms.date: 03/30/2017
ms.assetid: 40440495-254d-45c8-a8c6-b29f364892ba
ms.openlocfilehash: e364be55687323d3059c4a7e084818e3f7d54d5f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743443"
---
# <a name="supporting-multiple-iis-site-bindings"></a>複数の IIS サイト バインディングのサポート
インターネットインフォメーションサービス (IIS) 7.0 で Windows Communication Foundation (WCF) サービスをホストする場合、同じサイトで同じプロトコルを使用する複数のベースアドレスを指定することができます。 これにより、同じサービスで多数の異なる URI に応答できます。 これは、`http://www.contoso.com` と `http://contoso.com`でリッスンするサービスをホストする場合に便利です。 また、内部ユーザー用に 1 つのベース アドレスを持ち、外部ユーザー用に別のベース アドレスを持つサービスを作成するのにも役立ちます。 たとえば、`http://internal.contoso.com` と `http://www.contoso.com` などです。  
  
> [!NOTE]
> この機能は、HTTP プロトコルを使用してのみ、使用可能です。  
  
## <a name="multiple-base-addresses"></a>複数のベース アドレス  
 この機能は、IIS でホストされている WCF サービスでのみ使用できます。 既定では、この機能は無効になっています。 これを有効にするには、次の例に示すように、web.config ファイルの <`serviceHostingEnvironment`> 要素に `multipleSiteBindingsEnabled` 属性を追加し、それを `true`に設定する必要があります。  
  
```xml  
<serviceHostingEnvironment multipleSiteBindingsEnabled="true"/>  
```  
  
 IIS で WCF サービスをホストする場合、IIS は、アプリケーションを含む仮想ディレクトリへの URI に基づいて、1つのベースアドレスを作成します。 インターネット インフォメーション サービス マネージャーを使用して、同じプロトコルを使用するベース アドレスを追加し、1 つ以上のバインディングを Web サイトに追加できます。 それぞれのバインディングに対して、プロトコル (HTTP または HTTPS)、IP アドレス、ポート、およびホスト名を指定します。 インターネットインフォメーションサービス Manager の使用方法の詳細については、「 [Iis マネージャー (iis 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753842(v=ws.10))」を参照してください。 サイトにバインドを追加する方法の詳細については、「 [Web サイトを作成する (IIS 7)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772350(v=ws.10)) 」を参照してください。  
  
 同じサイトに複数のベースアドレスを指定すると、WCF ヘルプページの内容、インポートスキーマ、およびサービスによって生成される WSDL/MEX 情報が影響を受けます。 WCF ヘルプページには、サービスと通信できる WCF クライアントを生成するために使用するコマンドラインが表示されます。 このコマンド ラインには、その Web サイト用の IIS バインディングで指定された最初のアドレスだけが含まれています。 同様に、スキーマをインポートするときには、IIS バインディングで指定された最初のベース アドレスだけが使用されます。 WSDL および MEX データには、IIS バインディングで指定されたすべてのベース アドレスが含まれています。  
  
> [!WARNING]
> つまり、サービスに 2 つのベース アドレス (1 つは内部ユーザー用、もう 1 つは外部ユーザー用) がある場合、サービスによって生成される WSDL/MEX 情報では、両方のベース アドレスが指定されます。
