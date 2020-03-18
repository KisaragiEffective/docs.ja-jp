---
title: <GCCpuGroup> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- GCCpuGroup element
- <GCCpuGroup> element
ms.assetid: c1fc7d6c-7220-475c-a312-5b8b201f66e0
ms.openlocfilehash: ae9c96c9d49cf3f6be94da3f77b91423cab12e0b
ms.sourcegitcommit: 9a39f2a06f110c9c7ca54ba216900d038aa14ef3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/23/2019
ms.locfileid: "74430483"
---
# <a name="gccpugroup-element"></a>\<GCCpuGroup > 要素

ガベージ コレクションが複数の CPU グループをサポートするかどうかを指定します。

[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<GCCpuGroup >**

## <a name="syntax"></a>構文

```xml
<GCCpuGroup
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>属性と要素

次のセクションでは、属性、子要素、親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br /> ガベージ コレクションが複数の CPU グループをサポートするかどうかを指定します。|

## <a name="enabled-attribute"></a>enabled 属性

|値|説明|
|-----------|-----------------|
|`false`|ガベージ コレクションは複数の CPU グループをサポートしていません。 既定値です。|
|`true`|ガベージ コレクションは、サーバーのガベージ コレクションが有効な場合に複数の CPU グループをサポートします。|

### <a name="child-elements"></a>子要素

[なし]。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|

## <a name="remarks"></a>コメント

コンピューターに複数の CPU グループがあり、サーバーのガベージコレクションが有効になっている場合 ( [\<gcServer >](gcserver-element.md)要素を参照)、この要素を有効にすると、すべての cpu グループにわたってガベージコレクションが拡張され、ヒープを作成および分散するときにすべてのコアが考慮されます。

> [!NOTE]
> この要素は、ガベージ コレクション スレッドにのみ適用されます。 ランタイムがすべての CPU グループにユーザースレッドを分散できるようにするには、 [\<Thread_UseAllCpuGroups >](thread-useallcpugroups-element.md)要素を有効にする必要もあります。

## <a name="example"></a>例

次の例は、複数の CPU グループのガベージ コレクションを有効にする方法を示しています。

```xml
<configuration>
   <runtime>
      <GCCpuGroup enabled="true"/>
      <gcServer enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>参照

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [同時実行ガベージコレクションを無効にするには](gcconcurrent-element.md#to-disable-background-garbage-collection)
- [ワークステーションとサーバーのガベージ コレクション](../../../../standard/garbage-collection/fundamentals.md#workstation-and-server-garbage-collection)
