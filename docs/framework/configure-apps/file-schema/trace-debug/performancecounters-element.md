---
title: <performanceCounters> 要素
ms.date: 03/30/2017
f1_keywords:
- http://schemas.microsoft.com/.NetConfiguration/v2.0#configuration/system.diagnostics/performanceCounters
- http://schemas.microsoft.com/.NetConfiguration/v2.0#performanceCounters
helpviewer_keywords:
- performanceCounters element
- <performanceCounters> element
ms.assetid: a71f605b-c7d9-4501-a5c3-abcbb964a43f
ms.openlocfilehash: f52fdb2d5b0b7911de63f96663e70735d2f2496c
ms.sourcegitcommit: 3094dcd17141b32a570a82ae3f62a331616e2c9c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/01/2019
ms.locfileid: "71697152"
---
# <a name="performancecounters-element"></a>\<performanceCounters > 要素

パフォーマンス カウンターが共有するグローバル メモリのサイズを指定します。

[ **\<configuration>** ](../configuration-element.md)  
&nbsp;&nbsp;[ **\<system.diagnostics>** ](system-diagnostics-element.md)  
&nbsp;&nbsp;&nbsp;&nbsp; **\<performanceCounters>**  

## <a name="syntax"></a>構文

```xml
<performanceCounters filemappingsize="524288" />
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|説明|
|---------------|-----------------|
|filemappingsize|必須の属性です。<br /><br /> パフォーマンスカウンターによって共有されるグローバルメモリのサイズ (バイト単位) を指定します。 既定値は 524288 です。|

### <a name="child-elements"></a>子要素

なし。

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|`Configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|
|`system.diagnostics`|ASP.NET 構成セクションのルート要素を指定します。|

## <a name="remarks"></a>コメント

パフォーマンスカウンターは、メモリマップトファイルまたは共有メモリを使用して、パフォーマンスデータをパブリッシュします。  共有メモリのサイズによって、一度に使用できるインスタンスの数が決まります。  共有メモリには、グローバル共有メモリと個別の共有メモリの2種類があります。  グローバル共有メモリは、.NET Framework バージョン1.0 または1.1 と共にインストールされたすべてのパフォーマンスカウンターカテゴリによって使用されます。  .NET Framework バージョン2.0 と共にインストールされるパフォーマンスカウンターカテゴリには、個別の共有メモリが使用されます。各パフォーマンスカウンターカテゴリには独自のメモリがあります。

グローバル共有メモリのサイズは、構成ファイルでのみ設定できます。  既定のサイズは 524288 byes、最大サイズは33554432バイト、最小サイズは32768バイトです。  グローバル共有メモリはすべてのプロセスとカテゴリによって共有されるため、最初の作成者はサイズを指定します。  アプリケーション構成ファイルでサイズを定義する場合、そのサイズは、アプリケーションがパフォーマンスカウンターを実行する最初のアプリケーションである場合にのみ使用されます。  したがって、`filemappingsize` の値を指定する正しい場所は machine.config ファイルです。  グローバル共有メモリ内のメモリは、個々のパフォーマンスカウンターによって解放されることはないため、異なる名前を持つ多数のパフォーマンスカウンターインスタンスが作成されると、最終的にグローバル共有メモリが使い果たされます。

個別の共有メモリのサイズについては、レジストリキー HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services @ no__t-0 *\< category name >* \Performance が最初に参照され、その後に値が続きます。構成ファイル内のグローバル共有メモリに対して指定されます。 [FileMappingSize] の値が存在しない場合は、構成ファイルのグローバル設定の1つ目の共有メモリサイズが4番目 (1/4) に設定されます。

## <a name="see-also"></a>関連項目

- <xref:System.Diagnostics.PerformanceCounter>
- <xref:System.Diagnostics.PerformanceCounterCategory>
- <xref:System.Diagnostics.PerformanceCounter.InstanceLifetime%2A>
- <xref:System.Diagnostics.PerformanceCounterInstanceLifetime>
