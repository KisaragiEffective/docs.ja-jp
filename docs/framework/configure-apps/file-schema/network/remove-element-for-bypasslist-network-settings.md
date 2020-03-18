---
title: bypasslist の <remove> 要素 (ネットワーク設定)
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.net/defaultProxy/bypasslist/remove
- http://schemas.microsoft.com/.NetConfiguration/v2.0#remove
helpviewer_keywords:
- <bypasslist>, remove element
- remove element, bypasslist
- bypasslist, remove element
- remove element, bypasslist
ms.assetid: 61dcfb4a-e3d9-4abf-a2cd-7d685fe2f64b
ms.openlocfilehash: 97b49a8a520d6a4f72945366874991d2deb18710
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697893"
---
# <a name="remove-element-for-bypasslist-network-settings"></a>bypasslist (ネットワーク設定) の \<remove> 要素

プロキシバイパスリストから IP アドレスまたは DNS 名を削除します。

[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<system.net>** ](system-net-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<defaultProxy>** ](defaultproxy-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[ **\<bypasslist>** ](bypasslist-element-network-settings.md)  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; **\<remove>**  

## <a name="syntax"></a>構文

```xml
<remove
  address="regular expression"
/>
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|**属性**|**[説明]**|
|-------------------|---------------------|
|`address`|IP アドレスまたは DNS 名を記述する正規表現。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|**要素**|**[説明]**|
|-----------------|---------------------|
|[bypasslist](bypasslist-element-network-settings.md)|プロキシを使用しないアドレスを記述する一連の正規表現を提供します。|

## <a name="remarks"></a>コメント

@No__t-0 要素は、プロキシサーバーをバイパスするアドレスの一覧から、IP アドレスまたは DNS サーバー名を記述する正規表現を削除します。 これらのアドレスは、構成ファイルで既に定義されているか、構成階層の上位レベルに定義されています。

@No__t-0 属性の値は、一連の IP アドレスまたはホスト名を表す正規表現である必要があります。

正規表現の詳細については、「」を参照してください。[正規表現を .NET Framework](../../../../standard/base-types/regular-expressions.md)します。

## <a name="configuration-files"></a>構成ファイル

この要素は、アプリケーション構成ファイルまたはマシン構成ファイル (Machine.config) で使用できます。

## <a name="example"></a>例

次の例では、adventure-works.com ドメインの以前の定義を削除し、contoso.com ドメインをバイパスリストに追加します。

```xml
<configuration>
  <system.net>
    <defaultProxy>
      <bypasslist>
        <remove address="[a-z]+\.adventure-works\.com$" />
        <add address="[a-z]+\.contoso\.com$" />
      </bypasslist>
    </defaultProxy>
  </system.net>
</configuration>
```

## <a name="see-also"></a>関連項目

- <xref:System.Net.WebProxy?displayProperty=nameWithType>
- [ネットワーク設定スキーマ](index.md)
