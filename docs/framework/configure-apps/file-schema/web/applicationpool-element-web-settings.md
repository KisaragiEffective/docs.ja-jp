---
title: <applicationPool> 要素 (Web 設定)
ms.date: 03/30/2017
helpviewer_keywords:
- applicationPool element
- <applicationPool> element
ms.assetid: 46d1baaa-e343-4639-b70d-2a43a9f62b2a
ms.openlocfilehash: 9783844ff0fe719b0581c1c9e1fb96eb31933b89
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74801861"
---
# <a name="applicationpool-element-web-settings"></a>\<applicationPool > 要素 (Web 設定)
ASP.NET アプリケーションが IIS 7.0 以降のバージョンで統合モードで実行されている場合に、プロセス全体の動作を管理するために ASP.NET によって使用される構成設定を指定します。  
  
> [!IMPORTANT]
> この要素とサポートされる機能は、ASP.NET アプリケーションが IIS 7.0 以降のバージョンでホストされている場合にのみ機能します。  
  
[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<system.web>** ](system-web-element-web-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp; **\<applicationPool >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<applicationPool   
    maxConcurrentRequestsPerCPU="5000"   
    maxConcurrentThreadsPerCPU="0"   
    requestQueueLimit="5000" />  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  

次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`maxConcurrentRequestsPerCPU`|CPU ごとに ASP.NET が許可する同時要求の数を指定します。|  
|`maxConcurrentThreadsPerCPU`|CPU ごとにアプリケーションプールに対して同時に実行できるスレッドの数を指定します。 これにより、要求を処理するために CPU ごとに使用できるマネージスレッドの数を制限できるため、ASP.NET concurrency を制御する別の方法が提供されます。 既定では、この設定は0です。これは、ASP.NET が CPU ごとに作成できるスレッドの数を制限しないことを意味します。ただし、CLR スレッドプールでは、作成可能なスレッドの数も制限されます。|  
|`requestQueueLimit`|1つのプロセスで ASP.NET のキューに入れることができる要求の最大数を指定します。 複数の ASP.NET アプリケーションが1つのアプリケーションプールで実行される場合、アプリケーションプール内のアプリケーションに対して行われる要求の累積セットは、この設定の対象となります。|  
  
### <a name="child-elements"></a>子要素  
 ありません。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<system.web >](system-web-element-web-settings.md)|ASP.NET がホストアプリケーションと対話する方法について説明します。|  
  
## <a name="remarks"></a>Remarks  

IIS 7.0 以降のバージョンを統合モードで実行する場合、この要素の組み合わせを使用して、アプリケーションが IIS アプリケーションプールでホストされている場合に、ASP.NET がスレッドを管理し、要求をキューに配置する方法を構成できます。 IIS 6 を実行した場合、またはクラシックモードまたは ISAPI モードで IIS 7.0 を実行している場合、これらの設定は無視されます。  
  
`applicationPool` の設定は、.NET Framework の特定のバージョンで実行されるすべてのアプリケーションプールに適用されます。 設定は、aspnet ファイルに格納されています。 .NET Framework のバージョン2.0 および4.0 では、このファイルのバージョンがあります。 (.NET Framework のバージョン3.0 および3.5 は、aspnet .config ファイルをバージョン2.0 で共有します)。  
  
> [!IMPORTANT]
> Windows 7 で IIS 7.0 を実行している場合は、アプリケーションプールごとに個別の aspnet ファイルを構成できます。 これにより、各アプリケーションプールのスレッドのパフォーマンスを調整できます。  
  
`maxConcurrentRequestsPerCPU` 設定の場合、.NET Framework 4 の既定の設定 "5000" は、ASP.NET によって制御される要求の調整を無効にします (CPU あたりの要求が実際に5000以上の場合を除く)。 既定の設定は、CPU ごとの同時実行制御を自動的に管理する CLR スレッドプールに依存します。 非同期要求処理を広範囲にわたって使用するアプリケーションや、ネットワーク i/o でブロックされている実行時間の長い要求が多数あるアプリケーションでは、.NET Framework 4 の既定の制限値を増やすことができます。 `maxConcurrentRequestsPerCPU` を0に設定すると、ASP.NET 要求を処理するためのマネージスレッドの使用が無効になります。 アプリケーションが IIS アプリケーションプールで実行されている場合、要求は IIS の i/o スレッドにとどまります。そのため、同時実行は IIS スレッド設定によって制限されます。  
  
`requestQueueLimit` 設定は、ASP.NET アプリケーションの web.config ファイルで設定されている[processModel](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/7w2sway1(v=vs.100))要素の `requestQueueLimit` 属性と同じように動作します。 ただし、aspnet ファイルの `requestQueueLimit` 設定は、web.config ファイルの `requestQueueLimit` 設定よりも優先されます。 つまり、両方の属性が設定されている場合 (既定では true)、aspnet ファイルの `requestQueueLimit` 設定が優先されます。  
  
## <a name="example"></a>使用例  

次の例は、次のような場合に、ASP.NET ファイルでプロセス全体の動作を構成する方法を示しています。  
  
- アプリケーションは、IIS 7.0 アプリケーションプールでホストされます。  
  
- IIS 7.0 は統合モードで実行されています。  
  
- アプリケーションで .NET Framework 3.5 SP1 以降のバージョンが使用されています。  
  
この例の値は既定値です。  
  
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
|名前空間||  
|スキーマ名||  
|検証ファイル||  
|空にすることができます||  
  
## <a name="see-also"></a>参照

- [\<system.web> 要素 (Web 設定)](system-web-element-web-settings.md)
