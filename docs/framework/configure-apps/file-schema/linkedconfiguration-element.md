---
title: <linkedConfiguration> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/assemblyBinding/linkedConfiguration
- http://schemas.microsoft.com/.NetConfiguration/v2.0#linkedConfiguration
helpviewer_keywords:
- configuration files [.NET Framework],linked configuration files
- <linkedConfiguration> element
- including configuration files
- linked configuration files
- linkedConfiguration Element
ms.assetid: 8eb34f3b-427e-4288-a7ff-c73f489deb45
ms.openlocfilehash: 14ee2275ecf690ab16ffaabd71fbbe7e1a4897bc
ms.sourcegitcommit: 7f8eeef060ddeb2cabfa52843776faf652c5a1f5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/14/2019
ms.locfileid: "74087961"
---
# <a name="linkedconfiguration-element"></a>\<linkedConfiguration > 要素

インクルードする構成ファイルを指定します。

[ **\<configuration>** ](configuration-element.md)\
&nbsp;&nbsp;[ **\<assemblyBinding >** ](assemblybinding-element-for-configuration.md)\
&nbsp;の &nbsp;&nbsp;&nbsp; **\<linkedConfiguration >**

## <a name="syntax"></a>構文

```xml
<linkedConfiguration href="URL of linked configuration file" />
```

## <a name="attribute"></a>属性

|           | 説明 |
| --------- | ----------- |
| **href**  | 必須の属性です。<br><br>含める構成ファイルの URL。 **Href**属性でサポートされている形式は `file://`だけです。 ローカルファイルと UNC ファイルがサポートされています。 |

## <a name="parent-element"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<assemblyBinding >** Element](assemblybinding-element-for-configuration.md) | 構成レベルでのアセンブリ バインディング ポリシーを指定します。 |

## <a name="child-elements"></a>子要素

None

## <a name="remarks"></a>Remarks

**\<linkedConfiguration >** 要素は、コンポーネントアセンブリのサービスを簡略化します。 既知の場所に構成ファイルが存在するアセンブリが1つ以上のアプリケーションで使用されている場合、アセンブリを使用するアプリケーションの構成ファイルでは、構成情報を直接含めるのではなく、 **\<linkedconfiguration >** 要素を使用してアセンブリ構成ファイルを含めることができます。 コンポーネントアセンブリがサービスされている場合、共通構成ファイルを更新すると、そのアセンブリを使用するすべてのアプリケーションに対して、更新された構成情報が提供されます。

> [!NOTE]
> **\<linkedConfiguration >** 要素は、Windows サイドバイサイドマニフェストを持つアプリケーションではサポートされていません。

次の規則は、リンクされた構成ファイルの使用を制御します。

- インクルードされる構成ファイルの設定は、ローダーバインドポリシーにのみ影響し、ローダーによってのみ使用されます。 含まれる構成ファイルには、バインドポリシー以外の設定を含めることができますが、これらの設定は何の効果もありません。

- `href` 属性でサポートされている形式は `file://`だけです。 ローカルファイルと UNC ファイルがサポートされています。

- 構成ファイルごとにリンクされる構成の数に制限はありません。

- C/C++の `#include` ディレクティブの動作と同様に、すべてのリンクされた構成ファイルが1つのファイルに結合されます。

- **\<linkedConfiguration >** 要素は、アプリケーション構成ファイルでのみ使用できます。*machine.config*では無視されます。

- 循環参照が検出され、終了します。 つまり、一連の構成ファイルの **\<linkedconfiguration >** 要素がループを形成すると、ループが検出され、停止されます。

## <a name="example"></a>例

次の例は、ローカルハードディスクの構成ファイルを含める方法を示しています。

```xml
<configuration>
  <assemblyBinding xmlns="urn:schemas-microsoft-com:asm.v1">
    <linkedConfiguration href="file://c:\Program Files\Contoso\sharedConfig.xml"/>
  </assemblyBinding>
</configuration>
```

## <a name="see-also"></a>関連項目

- [ **\<assemblyBinding >** Element](assemblybinding-element-for-configuration.md)
- [.NET Framework の構成ファイルスキーマ](index.md)
