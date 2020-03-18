---
title: <clear> の <appSettings> 要素
ms.date: 05/01/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/appSettings/clear
helpviewer_keywords:
- clear Element
- <clear> Element
ms.assetid: 6d18c7be-27db-438b-8fb5-765d396b0b7b
ms.openlocfilehash: 266d32ccb8b322f0472e0f552f9c0fc877c9a78e
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77214805"
---
# <a name="clear-element-for-appsettings"></a>\<appSettings > の \<クリア > 要素

カスタムアプリケーション設定をクリアします。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<appSettings>** ](appsettings-element-for-configuration.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<クリア >**

## <a name="syntax"></a>構文

```xml
<appSettings>
  <clear />
</appSettings>
```

## <a name="attributes"></a>属性

なし

## <a name="parent-element"></a>親要素

|     | 説明 |
| --- | ----------- |
| [ **\<appSettings>** ](appsettings-element-for-configuration.md) | ファイルパス、XML Web サービス Url、その他のカスタムアプリケーション構成情報などのカスタムアプリケーション設定が含まれます。 |

## <a name="child-elements"></a>子要素

なし

## <a name="example"></a>例

次の例は、カスタム構成設定をクリアする方法を示しています。

```xml
<appSettings>
  <clear />
</appSettings>
```

## <a name="see-also"></a>参照

- [.NET Framework の構成ファイルスキーマ](../index.md)
