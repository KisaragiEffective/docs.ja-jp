---
title: <Directives> 要素 (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: 444846f3-48d5-4341-a43e-69f7221389eb
ms.openlocfilehash: abe2e7221e0afb984a6178b12fabc36ea24deb35
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73128465"
---
# <a name="directives-element-net-native"></a>\<ディレクティブ > 要素 (.NET ネイティブ)
.NET ネイティブのすべてのランタイムディレクティブファイルのルート要素。  
  
 `<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">` 
  
## <a name="syntax"></a>構文  
  
```xml  
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">  
   <!-- child elements -->   
</Directives>  
```  
  
## <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`xmlns`|XML 名前空間。 値は常に **"http://schemas.microsoft.com/netfx/2013/01/metadata"** です。|  
  
## <a name="child-elements"></a>子要素  
  
|要素|説明|  
|-------------|-----------------|  
|[\<Application>](application-element-net-native.md)|リフレクションで使用可能なメタデータを持つアプリケーション全体の型と型のメンバーのコンテナーとして機能します。|  
|[\<Library>](library-element-net-native.md)|実行時にメタデータを必要とする子型と型のメンバーを持つアセンブリを定義します。|  
  
## <a name="remarks"></a>Remarks  
 各ランタイム ディレクティブ ファイルには、`<Directives>` 要素を 1 つのみ含めることができます。  
  
 `<Directives>` 要素には、0 または 1 個の [\<Application>](application-element-net-native.md) 要素と、0 個以上の [\<Library>](library-element-net-native.md) 要素を含めることができます。  
  
## <a name="see-also"></a>関連項目

- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
