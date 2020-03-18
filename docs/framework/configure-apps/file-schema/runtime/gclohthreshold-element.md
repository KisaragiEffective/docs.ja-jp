---
title: GCLOHThreshold 要素
ms.date: 11/20/2019
helpviewer_keywords:
- GCLOHThreshold element
- <GCLOHThreshold> element
ms.openlocfilehash: d72dc9d27984f60dfb6296217263ce8b176093c6
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74451324"
---
# <a name="gclohthreshold-element"></a>GCLOHThreshold 要素

ガベージコレクターがラージオブジェクトヒープ (LOH) にオブジェクトを配置するしきい値のサイズをバイト単位で指定します。

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<ランタイム >](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<GCLOHThreshold >

## <a name="syntax"></a>構文

```xml
<GCLOHThreshold
   enabled="nnnn"/>
```

## <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br />オブジェクトが大きなオブジェクトヒープに対して実行されるしきい値のサイズを指定します。|

### <a name="enabled-attribute"></a>enabled 属性

|値|説明|
|-----------|-----------------|
|`nnnn`|オブジェクトが大きなオブジェクトヒープに対して実行されるしきい値のサイズ (バイト単位)。|

## <a name="child-elements"></a>子要素

なし。

## <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|

## <a name="remarks"></a>コメント

この設定は .NET Framework 4.8 で導入されました。

## <a name="see-also"></a>関連項目

- [実行時設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [ガベージ コレクションの基礎](../../../../standard/garbage-collection/fundamentals.md)
- [GC の NET Core ランタイム構成オプション](../../../../core/run-time-config/garbage-collector.md)
