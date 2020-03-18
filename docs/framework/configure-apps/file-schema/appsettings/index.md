---
title: アプリ設定スキーマ
ms.date: 05/01/2017
helpviewer_keywords:
- schema app settings
- app settings, schema [Windows Forms]
- Windows Forms, app settings schema
- configuration schema [.NET Framework], app settings
ms.assetid: 99347d62-3ea5-40b6-bfec-c31431011422
ms.openlocfilehash: 0a3363b35a6fc8bd27753eb034f8a1e95feb5292
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77215425"
---
# <a name="app-settings-schema"></a>アプリ設定スキーマ

ファイル パス、XML Web サービス URL、またはアプリケーションのその他のカスタム構成情報など、カスタム アプリケーションの設定が含まれています。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<appSettings>** ](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<add>** ](add-element-for-appsettings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<clear>** ](clear-element-for-appsettings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[ **\<remove>** ](remove-element-for-appsettings.md)

| 要素 | 説明 |
| ------- | ----------- |
| [ **\<appSettings>** ](appsettings-element-for-configuration.md) | アプリケーションの設定を制御する **\<add>** 、 **\<clear>** 、 **\<remove>** のタグが含まれます。 省略可能な **file** 属性があります。 |
| [ **\<add>** ](add-element-for-appsettings.md) | 設定を定義します。 **\<appSettings>** の子です。 **key** 属性と **value** 属性が必要です。 |
| [ **\<clear>** ](clear-element-for-appsettings.md) | すべての設定をクリアします。 **\<appSettings>** の子です。 属性はありません。 |
| [ **\<remove>** ](remove-element-for-appsettings.md) | 設定を削除します。 **\<appSettings>** の子です。 **key** 属性が必要です。 |

## <a name="appsettings-element"></a>\<appSettings> 要素

この要素には、アプリケーションの設定を制御する **\<add>** 、 **\<clear>** 、 **\<remove>** のタグが含まれます。 **file** の省略可能な属性を定義します。

## <a name="add-element"></a>\<add> 要素

カスタム アプリケーション設定を名前/値のペアとしてアプリケーション設定コレクションに追加します。 **key** および **value** の属性を定義します。

## <a name="clear-element"></a>\<clear> 要素

継承したカスタム アプリケーション設定へのすべての参照を削除し、 **\<clear>** 要素の後の **\<add>** 要素によって追加された参照のみを許可します。 属性は定義されません。

## <a name="remove-element"></a>\<remove> 要素

アプリケーション設定コレクションから継承したカスタム アプリケーション設定への参照を削除します。 **key** の属性を定義します。

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

## <a name="see-also"></a>参照

- [アプリケーション設定の概要](../../../winforms/advanced/application-settings-overview.md)
- [アプリケーション設定アーキテクチャ](../../../winforms/advanced/application-settings-architecture.md)
