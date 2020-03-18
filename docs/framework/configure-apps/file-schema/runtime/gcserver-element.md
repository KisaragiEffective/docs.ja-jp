---
title: gcServer 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/runtime/gcServer
- http://schemas.microsoft.com/.NetConfiguration/v2.0#gcServer
helpviewer_keywords:
- gcServer element
- <gcServer> element
ms.assetid: 8d25b80e-2581-4803-bd87-a59528e3cb03
ms.openlocfilehash: 8eab5e36bab90510aff4f1a3e15328197ac59ed7
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968944"
---
# <a name="gcserver-element"></a>\<gcServer > 要素

共通言語ランタイムがサーバーのガベージ コレクションを実行するかどうかを指定します。

[\<configuration>](../configuration-element.md)\
&nbsp;&nbsp;[\<ランタイム >](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp;\<gcServer >

## <a name="syntax"></a>構文

```xml
<gcServer
   enabled="true|false"/>
```

## <a name="attributes-and-elements"></a>属性と要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|`enabled`|必須の属性です。<br /><br />ランタイムがサーバーのガベージ コレクションを実行するかどうかを指定します。|

#### <a name="enabled-attribute"></a>enabled 属性

|[値]|説明|
|-----------|-----------------|
|`false`|サーバーのガベージ コレクションを実行しません。 既定値です。|
|`true`|サーバーのガベージ コレクションを実行します。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|

## <a name="remarks"></a>Remarks

共通言語ランタイム (CLR) は、2 種類のガベージ コレクションをサポートしています。1 つはワークステーション ガベージ コレクションで、すべてのシステムで使用できるものです。もう 1 つはサーバー ガベージ コレクションで、マルチプロセッサ システムで使用できるものです。 CLR が実行するガベージコレクションの種類を制御するには、 **gcServer**要素を使用します。 <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType> プロパティを使用して、サーバー ガベージ コレクションが有効かどうかを決定します。

シングル プロセッサ コンピューターの場合、既定のワークステーション ガベージ コレクションが催促のオプションです。 2 つのプロセッサを搭載するコンピューターで、ワークステーションかサーバーのいずれかを使用できます。 3 つ以上のプロセッサでは、サーバー ガベージ コレクションが最速のオプションです。 ほとんどの場合、マルチプロセッササーバーシステムはサーバー GC を無効にし、サーバーアプリの多くのインスタンスが同じコンピューターで実行されるときに、ワークステーション GC を使用します。

この要素は、アプリケーション構成ファイルでのみ使用できます。要素がマシン構成ファイルにある場合には無視されます。

> [!NOTE]
> .NET Framework 4 以前のバージョンでは、サーバー ガベージ コレクションを有効にすると同時実行ガベージ コレクションが使用できません。 .NET Framework 4.5 以降では、サーバーのガベージコレクションは同時に実行されます。 非同時サーバーガベージコレクションを使用するには、 **gcServer**要素を `true` に設定し、 [gcConcurrent 要素](gcconcurrent-element.md)を `false`に設定します。

.NET Framework 4.6.2 以降では、次の要素を使用してサーバー GC を構成することもできます。

- [GCNoAffinitize](gcnoaffinitize-element.md)。サーバー GC ヒープとプロセッサの間にアフィニティがあるかどうかを指定します。 既定では、プロセッサごとに1つのサーバー GC ヒープがあります。

- "プロセスで使用されるヒープの[数を制限](gcheapcount-element.md)する"。

- [GCHeapAffinitizeMask](gcheapaffinitizemask-element.md)は、使用可能なサーバー GC ヒープと個々のプロセッサ間の関係を定義します。

## <a name="example"></a>例

次の例では、サーバーのガベージコレクションを有効にします。

```xml
<configuration>
   <runtime>
      <gcServer enabled="true"/>
   </runtime>
</configuration>
```

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.GCSettings.IsServerGC%2A?displayProperty=nameWithType>
- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
- [同時実行ガベージコレクションを無効にするには](gcconcurrent-element.md#to-disable-background-garbage-collection)
