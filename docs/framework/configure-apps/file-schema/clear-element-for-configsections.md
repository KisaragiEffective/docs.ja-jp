---
title: <clear> の <configSections> 要素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/clear
helpviewer_keywords:
- clear Element
- <clear> Element
ms.assetid: 77f1d761-ff45-4001-8f36-3a3e5c41fa63
ms.openlocfilehash: e8c9b0479bba839a74dff300f0766838b5d99c8d
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214835"
---
# <a name="clear-element-for-configsections"></a>\<configSections の > 要素をクリア \<>

以前に定義したセクションとセクショングループをすべて消去します。

[ **\<configuration>** ](configuration-element.md)   
&nbsp;&nbsp;[ **\<configSections >** ](configsections-element-for-configuration.md)   
&nbsp;&nbsp;&nbsp;&nbsp; **\<クリア >**

## <a name="syntax"></a>構文

```xml
<clear/>
```

## <a name="attribute"></a>属性

|           | 説明 |
| --------- | ----------- |
| **name**  | 必須の属性です。<br><br>削除するセクションまたはセクショングループの名前を指定します。 |

## <a name="parent-element"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<configSections >** Element](configsections-element-for-configuration.md) | 構成セクションと名前空間の宣言が含まれています。 |

## <a name="child-elements"></a>子要素

なし

## <a name="remarks"></a>コメント

**\<clear >** 要素は、現在の構成ファイルまたは構成ファイル階層の上位レベルで定義されたすべてのセクションとセクショングループをアプリケーションから削除します。

## <a name="example"></a>例

この例では、マシン構成ファイルとアプリケーション構成ファイルを定義し、アプリケーション構成ファイルで **\<clear >** 要素を使用して、マシン構成ファイルで以前に定義したセクションをクリアする方法を示します。

次のマシン構成ファイルコードでは、アプリケーション構成ファイルの前に読み取られた2つのセクション **\<sampleSection >** と **\<anotherSampleSection >** が宣言されています。

```xml
<!-- Machine.config file -->
<configuration>
  <configSections>
    <section name="sampleSection"
             type="System.Configuration.SingleTagSectionHandler" />
    <section name="anotherSampleSection"
             type="System.Configuration.NameValueSectionHandler" />
  </configSections>
  <sampleSection setting1="Value1" 
                 setting2="value two" 
                 setting3="third value" />
</configuration>
```

次のアプリケーション構成ファイルのコードは、以前に宣言されたすべてのセクションを消去します。 アプリケーションでは、コンピューターの構成ファイルで宣言されているセクションのいずれかで設定を使用または取得することはできません。 ただし、 **\<clear >** 要素の後にあるため、\<別の**セクション >** の設定を使用できます。

```xml
<!-- Application configuration file -->
<configuration>
  <configSections>
    <clear/>
    <section name="anotherSection"
             type="System.Configuration.NameValueSectionHandler" />
  </configSections>
  <anotherSection setting1="Value1" 
                 setting2="value two" 
                 setting3="third value" />
</configuration>
```

## <a name="configuration-file"></a>［構成ファイル］

この要素は、アプリケーション構成ファイル *、コンピューター構成*ファイル (machine.config)、およびアプリケーションディレクトリレベルでは*ない web.config ファイル*で使用できます。

## <a name="see-also"></a>参照

- [.NET Framework の構成ファイルスキーマ](index.md)
