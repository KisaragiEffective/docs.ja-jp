---
title: GCHeapAffinitizeMask 要素
ms.date: 11/08/2019
helpviewer_keywords:
- gcHeapCount element
- <gcHeapCount> element
ms.openlocfilehash: 09d6523fb10692dd3617a3827d5bccf112bc632b
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73978421"
---
# <a name="gcheapaffinitizemask-element"></a>\<GCHeapAffinitizeMask > 要素

GC ヒープと個々のプロセッサ間の関係を定義します。

\<構成 > \
&nbsp;&nbsp;\<ランタイム > \
&nbsp;&nbsp;&nbsp;&nbsp;\<GCHeapAffinitizeMask >

## <a name="syntax"></a>構文

```xml
<GCHeapAffinitizeMask
   enabled="nnnn"/>
```

## <a name="attributes-and-elements"></a>属性と要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br />GC ヒープと個々のプロセッサ間の関係を指定します。 |

#### <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`nnnn`|サーバー GC ヒープと個々のプロセッサ間の関係を定義するビットマスクを形成する10進値。 |

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|

## <a name="remarks"></a>Remarks

既定では、サーバー GC スレッドはそれぞれの CPU とハード関係があるため、1つの GC ヒープ、1つのサーバー GC スレッド、および各プロセッサにつき1つのバックグラウンドサーバー GC スレッドが存在するようになります。 4\.6.2 .NET Framework 以降では、ヒープの数が**g Apcount**要素によって制限されている場合に、 **GCHeapAffinitizeMask**要素を使用して、サーバー GC ヒープとプロセッサ間の関係を制御できます。

**GCHeapAffinitizeMask**は通常、次の2つのフラグと共に使用されます。

- [GCNoAffinitize](gcnoaffinitize-element.md)。サーバー GC スレッド/ヒープが cpu と関連付けられているかどうかを制御します。 [GCNoAffinitize](gcnoaffinitize-element.md)要素の `enabled` 属性は、 **GCHeapAffinitizeMask**設定が使用されるように `false` (既定値) である必要があります。

- "サーバー GC のプロセスで使用されるヒープの[数を制限](gcheapcount-element.md)する"。 既定では、プロセッサごとに1つのヒープがあります。

**nnnn**は、10進値として表現されるビットマスクです。 バイト0のビット0はプロセッサ0を表し、バイト0のビット1はプロセッサ1を表します。 (例:

```xml
<GCHeapAffinitizeMask enabled="1023"/>
```

値1023は、0x3FF または 0011 1111 1111b です。 このプロセスでは、プロセッサ0からプロセッサ9までの10個のプロセッサが使用されます。

## <a name="example"></a>例

次の例は、アプリケーションが10個のヒープ/スレッドを持つサーバー GC を使用することを示しています。 これらのヒープは、システムで実行されている他のアプリケーションのヒープと重複しないようにするため、 **GCHeapAffinitizeMask**を使用して、プロセスで cpu 0 ~ 9 を使用するように指定します。

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
      <GCHeapCount enabled="10"/>
      <GCHeapAffinitizeMask enabled="1023"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [GCNoAffinitize 要素](gcnoaffinitize-element.md)
- [G不正 Apcount 要素](gcheapcount-element.md)
- [ガベージ コレクションの基礎](../../../../standard/garbage-collection/fundamentals.md)
- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
