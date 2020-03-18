---
title: <section> 要素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/section
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/configSections/sectionGroup/section
helpviewer_keywords:
- section Element
- <section> Element
ms.assetid: ec7d4110-2403-47ac-8218-499bfe9d5ddb
ms.openlocfilehash: 8785523d664294e3ca3792fb0f84d739d1f1a376
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215726"
---
# <a name="section-element"></a>\<section > 要素

構成セクションの宣言が含まれています。

[ **\<configuration>** ](configuration-element.md)\
&nbsp;&nbsp;[ **\<configSections >** ](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<セクション >**

[ **\<configuration>** ](configuration-element.md)\
&nbsp;&nbsp;[ **\<configSections >** ](configsections-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<sectionGroup >** ](sectiongroup-element-for-configsections.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;\<**セクション >**

## <a name="syntax"></a>構文

```xml
<section name="section name"
         type="configuration section handler class, assembly"
         allowDefinition="Everywhere|MachineOnly|MachineToApplication" 
         allowLocation="true|false" />
```

## <a name="required-attributes"></a>必須属性

|           | 説明 |
| --------- | ----------- |
| **name**  | 構成セクションの名前を指定します。 |
| **type**  | 構成ファイルからセクションを読み取る構成セクションハンドラークラスの名前を指定します。 型の値には、"完全修飾名"、"完全修飾名"、". アセンブリ名" の構文があります。 単純なアセンブリ名は、 *.dll*ファイル拡張子のないルートファイル名です。 |

## <a name="optional-attributes"></a>省略可能な属性

次の属性は、ASP.NET アプリケーションにのみ適用されます。 構成システムは、他のアプリケーションの種類に対してこれらの属性を無視します。

|                     | 説明 |
| ------------------- | ----------- |
| **allowDefinition** | セクションを使用できる構成ファイルを指定します。 次のいずれかの値を使用します。<br><br>**全体**<br>すべての構成ファイルでセクションを使用できるようにします。 これは既定値です。<br>**MachineOnly**<br>セクション*をコンピューターの構成ファイル (machine.config*) でのみ使用できるようにします。<br>**MachineToApplication**<br>コンピューターの構成ファイルまたはアプリケーション構成ファイルでセクションを使用できるようにします。 |
| **allowLocation**   | セクションを **\<location >** 要素内で使用できるかどうかを判断します。 次のいずれかの値を使用します。<br><br>**true**<br>セクションを **\<location >** 要素内で使用できるようにします。 これは既定値です。<br>**false**<br>では、 **\<location >** 要素内でセクションを使用することはできません。 |

## <a name="parent-elements"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<configSections >** Element](configsections-element-for-configuration.md) | 構成セクションと名前空間の宣言が含まれています。 |
| [ **\<sectionGroup >** Element](sectiongroup-element-for-configsections.md) | 構成セクションの名前空間を定義します。 |

> [!NOTE]
> **\<section >** 要素は **\<configsections >** または **\<sectionGroup >** のいずれかの子要素であり、両方ではありません。

## <a name="child-elements"></a>子要素

なし

## <a name="remarks"></a>コメント

構成セクションを宣言すると、基本的に構成ファイルの新しい要素が定義されます。 新しい要素には、構成セクションハンドラー (つまり、<xref:System.Configuration.IConfigurationSectionHandler> インターフェイスを実装するクラス) が読み取る設定が含まれています。 定義するセクションの属性と子要素は、設定の読み取りに使用するセクションハンドラーによって異なります。

*Machine.config*ファイルで構成セクションハンドラーを宣言すると、 **allowdefinition**属性で特に指定されていない限り、そのコンピューター上の任意のアプリケーション構成ファイルの構成セクションを使用できます。

## <a name="example"></a>例

次の例は、構成セクションを定義し、そのセクションの設定を定義する方法を示しています。

```xml
<configuration>
  <configSections>
    <section name="sampleSection"
             type="System.Configuration.SingleTagSectionHandler" 
             allowLocation="false" />
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
