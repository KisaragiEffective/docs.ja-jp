---
title: <appDomainManagerType> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- appDomainManagerType element
- <appDomainManagerType> element
ms.assetid: ae8d5a7e-e7f7-47f7-98d9-455cc243a322
ms.openlocfilehash: bae4aa39f9c43480ac2ef984f78834b68646742d
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73118239"
---
# <a name="appdomainmanagertype-element"></a>\<appDomainManagerType > 要素
既定のアプリケーション ドメインのアプリケーション ドメイン マネージャーの役割を果たす種類を指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<appDomainManagerType >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<appDomainManagerAssembly   
   value="type name" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`value`|必須の属性です。 プロセス内の既定のアプリケーションドメインのアプリケーションドメインマネージャーとして機能する、名前空間を含む型の名前を指定します。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>Remarks  
 アプリケーションドメインマネージャーの種類を指定するには、この要素と[\<appDomainManagerAssembly >](appdomainmanagerassembly-element.md)要素の両方を指定する必要があります。 これらの要素のいずれかが指定されていない場合、もう一方は無視されます。  
  
 既定のアプリケーションドメインが読み込まれると、 [\<appDomainManagerAssembly >](appdomainmanagerassembly-element.md)要素によって指定されたアセンブリ内に指定された型が存在しない場合に <xref:System.TypeLoadException> がスローされます。また、プロセスを開始できません。  
  
 既定のアプリケーションドメインに対してアプリケーションドメインマネージャーの種類を指定すると、既定のアプリケーションドメインから作成された他のアプリケーションドメインは、アプリケーションドメインマネージャーの種類を継承します。 <xref:System.AppDomainSetup.AppDomainManagerType%2A?displayProperty=nameWithType> と <xref:System.AppDomainSetup.AppDomainManagerAssembly%2A?displayProperty=nameWithType> のプロパティを使用して、新しいアプリケーションドメインに別のアプリケーションドメインマネージャーの種類を指定します。  
  
 アプリケーションドメインマネージャーの種類を指定するには、アプリケーションが完全に信頼されている必要があります。 (たとえば、デスクトップで実行されているアプリケーションには完全な信頼があります)。アプリケーションが完全に信頼されていない場合は、<xref:System.TypeLoadException> がスローされます。  
  
 型と名前空間の形式は、<xref:System.Type.FullName%2A?displayProperty=nameWithType> プロパティで使用される形式と同じです。  
  
 この構成要素は、.NET Framework 4 以降でのみ使用できます。  
  
## <a name="example"></a>例  
 次の例では、プロセスの既定のアプリケーションドメインのアプリケーションドメインマネージャーが `AdMgrExample` アセンブリの `MyMgr` の種類であることを指定する方法を示します。  
  
```xml  
<configuration>  
   <runtime>  
      <appDomainManagerType value="MyMgr" />  
      <appDomainManagerAssembly   
         value="AdMgrExample, Version=1.0.0.0, Culture=neutral, PublicKeyToken=6856bccf150f00b3" />  
   </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.AppDomainSetup.AppDomainManagerType%2A?displayProperty=nameWithType>
- <xref:System.AppDomainSetup.AppDomainManagerAssembly%2A?displayProperty=nameWithType>
- [\<appDomainManagerAssembly > 要素](appdomainmanagerassembly-element.md)
- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [SetAppDomainManagerType メソッド](../../../unmanaged-api/hosting/iclrcontrol-setappdomainmanagertype-method.md)
