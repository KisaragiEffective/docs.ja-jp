---
title: <relativeBindForResources> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- RelativeBindForResources element
- <relativeBindForResources> element
ms.assetid: 846ffa47-7257-4ce3-8cac-7ff627e0e34f
ms.openlocfilehash: 6a418fc546313b74bb965a0b223eca9c2e5acc08
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73115801"
---
# <a name="relativebindforresources-element"></a>\<Relativebindforresources> > 要素
サテライト アセンブリのプローブを最適化します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<relativebindforresources> >**  
  
## <a name="syntax"></a>構文  
  
```xml
<relativeBindForResources    
   enabled="true|false" />  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`enabled`|必須の属性です。<br /><br /> 共通言語ランタイムがサテライトアセンブリのプローブを最適化するかどうかを指定します。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|`false`|ランタイムは、サテライトアセンブリのプローブを最適化しません。 これは既定値です。|  
|`true`|ランタイムは、サテライトアセンブリのプローブを最適化します。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|ランタイム初期化オプションに関する情報を含んでいます。|  
  
## <a name="remarks"></a>Remarks  
 一般に、リソースの[パッケージ化とデプロイ](../../../resources/packaging-and-deploying-resources-in-desktop-apps.md)に関するトピックで説明されているように、Resource Manager はリソースをプローブします。 これは、resource Manager が特定のローカライズされたバージョンのリソースをプローブするときに、グローバルアセンブリキャッシュを検索し、アプリケーションのコードベースでカルチャ固有のフォルダーを検索し、サテライトアセンブリに対してクエリ Windows インストーラーを実行し、<xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベント。 `<relativeBindForResources>` 要素は、リソースマネージャーがサテライトアセンブリをプローブする方法を最適化します。 次の条件下でリソースを調査するときにパフォーマンスを向上させることができます。  
  
- サテライトアセンブリがコードアセンブリと同じ場所に配置されている場合。 つまり、コードアセンブリがグローバルアセンブリキャッシュにインストールされている場合は、サテライトアセンブリもインストールする必要があります。 コードアセンブリがアプリケーションのコードベースにインストールされている場合は、サテライトアセンブリをコードベースのカルチャ固有のフォルダーにもインストールする必要があります。  
  
- Windows インストーラーが使用されていない場合、またはサテライトアセンブリのオンデマンドインストールではあまり使用されない場合。  
  
- アプリケーションコードが <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントを処理しない場合。  
  
 `<relativeBindForResources>` 要素の `enabled` 属性を `true` に設定すると、次のように、サテライトアセンブリのリソースマネージャーのプローブが最適化されます。  
  
- 親コードアセンブリの場所を使用して、サテライトアセンブリをプローブします。  
  
- サテライトアセンブリの Windows インストーラーに対してはクエリを実行しません。  
  
- <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントは発生しません。  
  
## <a name="see-also"></a>関連項目

- [リソースのパッケージ化と配置](../../../resources/packaging-and-deploying-resources-in-desktop-apps.md)
- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
