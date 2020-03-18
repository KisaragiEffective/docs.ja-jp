---
title: SingleTagSectionHandler のカスタム要素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/sectionName
helpviewer_keywords:
- custom element
ms.assetid: e62056c6-b351-40eb-afc0-cc13fc44e45e
ms.openlocfilehash: 1d0431085a04d3fb817dfe0883779acc4d693084
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214787"
---
# <a name="custom-element-for-singletagsectionhandler"></a>SingleTagSectionHandler のカスタム要素

\<セクション > 要素で定義され、<xref:System.Configuration.SingleTagSectionHandler> クラスを使用するカスタム構成セクションの設定を定義します。

[ **\<configuration>** ](configuration-element.md)   
&nbsp;&nbsp; *\<sectionName >*

## <a name="syntax"></a>構文

```xml
<sectionName key="value" key2="value2" ... />
```

## <a name="attributes"></a>属性

属性と属性値はユーザーが定義します。

## <a name="parent-element"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<configuration>** ](configuration-element.md) | 共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。 |

## <a name="child-elements"></a>子要素

なし

## <a name="remarks"></a>コメント

**\<sectionName >** 要素は、 [ **\<configsections >** ](configsections-element-for-configuration.md)要素の[ **\<セクション >** ](section-element.md)タグで定義されたカスタム要素です。 <xref:System.Configuration.Configuration.GetSection(System.String)?displayProperty=nameWithType>を呼び出すと、構成システムによって <xref:System.Collections.IDictionary> オブジェクトが返されます。

## <a name="example"></a>例

次の例では、<xref:System.Configuration.SingleTagSectionHandler> クラスによって読み取られる設定を含む **\<sampleSection >** という名前のカスタム要素を宣言します。

```xml
<configuration>
  <configSections>
    <section name="sampleSection" 
             type="System.Configuration.SingleTagSectionHandler" />
  </configSections>
  <sampleSection setting1="Value1" 
                 setting2="value two" 
                 setting3="third value" />
</configuration>
```

## <a name="configuration-file"></a>［構成ファイル］

この要素は、アプリケーション構成ファイル *、コンピューター構成*ファイル (machine.config)、およびアプリケーションディレクトリレベルでは*ない web.config ファイル*で使用できます。

## <a name="see-also"></a>参照

- [.NET Framework の構成ファイルスキーマ](index.md)
