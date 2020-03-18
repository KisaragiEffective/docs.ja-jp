---
title: <appSettings> の <configuration> 要素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings
helpviewer_keywords:
- appSettings Element
- <appSettings> Element
ms.assetid: 39694cc4-6b84-45a6-9329-385a0d8b48fe
ms.openlocfilehash: e1f285aae10a89fa49846534d5b47e15920294ea
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77452280"
---
# <a name="appsettings-element-for-configuration"></a>\<構成の appSettings > 要素を \<>

カスタムアプリケーション設定が含まれます。 これは、.NET Framework によって提供される定義済みの構成セクションです。

[ **\<configuration>** ](../configuration-element.md)   
&nbsp;&nbsp; **\<appSettings >**

## <a name="syntax"></a>構文

```xml
<appSettings>
  <!-- Elements to add, clear, or remove configuration settings -->
</appSettings>
```

## <a name="attribute"></a>属性

|           | 説明 |
| --------- | ----------- |
| **file**  | 省略可能な属性です。<br><br>カスタムアプリケーション構成設定を含む外部ファイルへの相対パスを指定します。 指定されたファイルには、> の追加 **\<、> の削除**、 **\<のクリア >** 要素の **\<** で指定されているものと同じ種類の設定が含まれており、それらの要素と同じキー/値ペアの形式を使用します。<br><br>指定されたパスは、メイン構成ファイルに対する相対パスです。 Windows フォームアプリケーションの場合、これはアプリケーション構成ファイルの場所ではなく、バイナリフォルダー ( */bin/debug*など) です。 Web フォームアプリケーションの場合、パスは、web.config ファイルが配置さ*れている*アプリケーションルートに対する相対パスです。<br><br>指定されたファイルが見つからない場合、ランタイムは属性を無視します。 |

## <a name="parent-element"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<構成 >** Element](../configuration-element.md) | 共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。 |

## <a name="child-elements"></a>子要素

|     | 説明 |
| --- | ----------- |
| [ **\<add>** ](add-element-for-appsettings.md) | カスタムアプリケーション設定を追加します。 |
| [ **\<clear>** ](clear-element-for-appsettings.md) | 以前に定義したアプリケーション設定をすべてクリアします。 |
| [ **\<remove>** ](remove-element-for-appsettings.md) | 以前に定義したアプリケーション設定を削除します。 |

## <a name="remarks"></a>コメント

**\<appSettings >** 要素には、データベース接続文字列、ファイルパス、XML Web サービス url、またはアプリケーションのその他のカスタム構成情報など、カスタムアプリケーション構成情報が格納されます。 **\<appSettings >** 要素で指定されたキーと値のペアは、<xref:System.Configuration.ConfigurationSettings> クラスを使用してコードでアクセスされます。

Web.config ファイル*とアプリケーション*構成ファイルの **\<appSettings >** 要素で**file**属性を使用できます。 この属性は、追加設定を提供するか、 **\<appSettings >** 要素で指定された設定をオーバーライドする構成ファイルを指定します。 **ファイル**属性は、アプリケーション構成ファイルで指定されたプロジェクト設定をユーザーがオーバーライドする必要がある場合など、ソース管理チームの開発シナリオで使用できます。

**File**属性で指定される構成ファイルには **\<構成 >** ではなく **\<appSettings >** のルートノードが必要です。

## <a name="example"></a>例

次の例は、カスタム アプリケーション設定を定義する外部アプリケーション設定ファイル (*custom.config*) を示しています。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<appSettings>
  <add key="MyCustomSetting" value="MyCustomSettingValue" />
</appSettings>
```

次の例は、外部設定ファイルで設定を使用して、独自のアプリケーション設定を設定するアプリケーション構成ファイルを示しています。

```xml
<configuration>
  <appSettings file="custom.config">
    <add key="ApplicationName" value="MyApplication" />
  </appSettings>
</configuration>
```

## <a name="configuration-file"></a>［構成ファイル］

この要素は、アプリケーション構成ファイル *、コンピューター構成*ファイル (machine.config)、およびアプリケーションディレクトリレベルでは*ない web.config ファイル*で使用できます。

## <a name="see-also"></a>参照

- [.NET Framework の構成ファイルスキーマ](../index.md)
