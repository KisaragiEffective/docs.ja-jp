---
title: <module> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#module
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/module
helpviewer_keywords:
- module element
- <module> element
ms.assetid: 10318725-9666-4d65-ab61-b94c64e59f13
ms.openlocfilehash: 78f6418160b80096214c6e37268a5a90498d6d4d
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74089247"
---
# <a name="module-element-network-settings"></a>\<module> 要素 (ネットワーク設定)
新しいプロキシ モジュールをアプリケーションに追加します。  

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<system.net>** ](system-net-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<[**defaultProxy>** ](defaultproxy-element-network-settings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**paste>**

## <a name="syntax"></a>構文  
  
```xml  
<module   
  type="type_fullname, assembly_fullname"   
/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|**属性**|**説明**|  
|-------------------|---------------------|  
|`type`|プロキシを実装する完全修飾型名 (<xref:System.Type.FullName%2A> プロパティによって示されます) とアセンブリ名 (<xref:System.Reflection.Assembly.FullName%2A> プロパティによって示されます) をコンマで区切って指定します。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|**要素**|**説明**|  
|-----------------|---------------------|  
|[defaultProxy](defaultproxy-element-network-settings.md)|ハイパーテキスト転送プロトコル (HTTP: Hypertext Transfer Protocol) プロキシ サーバーを構成します。|  
  
## <a name="remarks"></a>Remarks  
 `module` 要素は、<xref:System.Net.IWebProxy> インターフェイスを実装するプロキシクラスを登録します。 プロキシクラスを登録すると、`module` を使用して、サポートされているプロキシ経由で情報を要求できます。  
  
 `type` 属性の値は、モジュールのクラス名と、対応するダイナミックリンクライブラリ (DLL) の名前にする必要があります。  
  
## <a name="configuration-files"></a>構成ファイル  
 この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。  
  
## <a name="example"></a>例  
 次の例では、カスタムプロキシクラスを登録します。  
  
```xml  
<configuration>  
  <system.net>  
    <defaultProxy>  
      <module  
        type="Test.CustomWebProxy, TestProxy, Version=2.0.3600.0, Culture=neutral, PublicKeyToken=b23a5c561934e385"  
      />  
    </defaultProxy>  
  </system.net>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- <xref:System.Net.IWebProxy?displayProperty=nameWithType>
- [ネットワーク設定スキーマ](index.md)
