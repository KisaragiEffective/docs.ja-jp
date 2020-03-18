---
title: <PreferComInsteadOfManagedRemoting> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- <PreferComInsteadOfManagedRemoting> element
- PreferComInsteadOfManagedRemoting element
ms.assetid: a279a42a-c415-4e79-88cf-64244ebda613
ms.openlocfilehash: 1376df4efd56734f2b8da9bd76033afcce8a285b
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452254"
---
# <a name="prefercominsteadofmanagedremoting-element"></a>\<PreferComInsteadOfManagedRemoting > 要素
アプリケーションドメインの境界を越えたすべての呼び出しに対して、ランタイムがリモート処理の代わりに COM 相互運用を使用するかどうかを指定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<PreferComInsteadOfManagedRemoting >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<PreferComInsteadOfManagedRemoting enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>属性と要素  
 次のセクションでは、属性、子要素、親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`enabled`|必須の属性です。<br /><br /> アプリケーションドメインの境界を越えて、ランタイムがリモート処理ではなく COM 相互運用を使用するかどうかを示します。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|値|説明|  
|-----------|-----------------|  
|`false`|ランタイムは、アプリケーションドメインの境界を越えてリモート処理を使用します。 これは既定値です。|  
|`true`|ランタイムは、アプリケーションドメインの境界を越えて COM 相互運用を使用します。|  
  
### <a name="child-elements"></a>子要素  
 [なし]。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>コメント  
 `enabled` 属性を `true`に設定すると、ランタイムは次のように動作します。  
  
- [Iunknown](/windows/win32/api/unknwn/nn-unknwn-iunknown)インターフェイスが COM インターフェイス経由でドメインに入るとき、ランタイムは[Imanagedobject](../../../unmanaged-api/hosting/imanagedobject-interface.md)インターフェイスに対して[iunknown:: QueryInterface](/windows/win32/api/unknwn/nf-unknwn-iunknown-queryinterface(q))を呼び出しません。 代わりに、オブジェクトをラップする[ランタイム呼び出し可能ラッパー](../../../../standard/native-interop/runtime-callable-wrapper.md) (RCW) を構築します。  
  
- ランタイムは、このドメインで作成されたすべての[COM 呼び出し可能ラッパー](../../../../standard/native-interop/com-callable-wrapper.md) (CCW) に対して[Imanagedobject](../../../unmanaged-api/hosting/imanagedobject-interface.md)インターフェイスの `QueryInterface` 呼び出しを受け取ったときに、E_NOINTERFACE を返します。  
  
 これらの2つの動作により、アプリケーションドメインの境界を越えたマネージオブジェクト間の COM インターフェイス経由のすべての呼び出しで、リモート処理ではなく COM と COM の相互運用が使用されるようになります。  
  
## <a name="example"></a>例  
 次の例では、ランタイムが分離の境界を越えて COM 相互運用機能を使用するように指定する方法を示します。  
  
```xml  
<configuration>  
  <runtime>  
    <PreferComInsteadOfManagedRemoting enabled="true"/>  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>参照

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
