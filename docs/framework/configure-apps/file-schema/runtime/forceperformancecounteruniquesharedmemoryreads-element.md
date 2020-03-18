---
title: <forcePerformanceCounterUniqueSharedMemoryReads> 要素
ms.date: 03/30/2017
helpviewer_keywords:
- forcePerformanceCounterUniqueSharedMemoryReads element
- <forcePerformanceCounterUniqueSharedMemoryReads> element
ms.assetid: 91149858-4810-4f65-9b48-468488172c9b
ms.openlocfilehash: efa6dce1035f7d2cf63b74c6a03d911b5dede722
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73116954"
---
# <a name="forceperformancecounteruniquesharedmemoryreads-element"></a>\<forcePerformanceCounterUniqueSharedMemoryReads > 要素
PerfCounter.dll が、.NET Framework バージョン 1.1 のアプリケーションの CategoryOptions レジストリ設定を使用してするかどうかを指定して、カテゴリ別の共有メモリとグローバル メモリのどちらからパフォーマンス カウンター データを読み込むかを決定します。  
  
[ **\<configuration>** ](../configuration-element.md)\
&nbsp;&nbsp;[ **\<runtime>** ](runtime-element.md)\
&nbsp;&nbsp;&nbsp;&nbsp; **\<forcePerformanceCounterUniqueSharedMemoryReads >**  
  
## <a name="syntax"></a>構文  
  
```xml  
<forcePerformanceCounterUniqueSharedMemoryReads   
enabled="true|false"/>  
```  
  
## <a name="attributes-and-elements"></a>属性および要素  
 以降のセクションでは、属性、子要素、および親要素について説明します。  
  
### <a name="attributes"></a>属性  
  
|属性|説明|  
|---------------|-----------------|  
|`enabled`|必須の属性です。<br /><br /> PerfCounter .dll が category Options レジストリ設定を使用して、カテゴリ固有の共有メモリまたはグローバルメモリからパフォーマンスカウンターデータを読み込むかどうかを決定するかどうかを示します。|  
  
## <a name="enabled-attribute"></a>enabled 属性  
  
|[値]|説明|  
|-----------|-----------------|  
|`false`|PerfCounter .dll は、カテゴリオプションのレジストリ設定を使用しません。これが既定値です。|  
|`true`|PerfCounter .dll は、カテゴリオプションのレジストリ設定を使用します。|  
  
### <a name="child-elements"></a>子要素  
 なし。  
  
### <a name="parent-elements"></a>親要素  
  
|要素|説明|  
|-------------|-----------------|  
|`configuration`|共通言語ランタイムおよび .NET Framework アプリケーションで使用されるすべての構成ファイルのルート要素です。|  
|`runtime`|アセンブリのバインディングとガベージ コレクションに関する情報が含まれています。|  
  
## <a name="remarks"></a>Remarks  
 .NET Framework 4 より前の .NET Framework のバージョンでは、これに読み込まれたバージョンの PerfCounter が、プロセスに読み込まれたランタイムに読み込まれていました。 コンピューターに .NET Framework バージョン1.1 と .NET Framework 2.0 の両方がインストールされている場合、.NET Framework 1.1 アプリケーションは .NET Framework 1.1 バージョンの PerfCounter を読み込みます。 .NET Framework 4 以降では、インストールされている最新バージョンの PerfCounter が読み込まれます。 これは、.NET Framework 4 がコンピューターにインストールされている場合、.NET Framework 1.1 アプリケーションが .NET Framework 4 バージョンの PerfCounter を読み込むことを意味します。  
  
 .NET Framework 4 以降では、パフォーマンスカウンターを使用するときに、PerfCounter によって各プロバイダーのカテゴリオプションのレジストリエントリがチェックされ、カテゴリ固有の共有メモリとグローバル共有メモリのどちらから読み取る必要があるかが判断されます。 .NET Framework 1.1 PerfCounter は、カテゴリ固有の共有メモリを認識しないため、そのレジストリエントリを読み取りません。常に、グローバルな共有メモリから読み取ります。  
  
 旧バージョンとの互換性のために、.NET Framework 4 PerfCounter .dll は、.NET Framework 1.1 アプリケーションで実行されているときに、カテゴリオプションのレジストリエントリをチェックしません。 .NET Framework 1.1 と同様に、グローバルな共有メモリを使用します。 ただし、`<forcePerformanceCounterUniqueSharedMemoryReads>` 要素を有効にすることで、レジストリ設定をチェックするように .NET Framework 4 PerfCounter .dll に指示できます。  
  
> [!NOTE]
> `<forcePerformanceCounterUniqueSharedMemoryReads>` 要素を有効にしても、カテゴリ固有の共有メモリが使用されることは保証されません。 を `true` に設定すると、PerfCounter .dll がカテゴリオプションのレジストリ設定を参照することになります。 カテゴリのオプションの既定の設定では、カテゴリ固有の共有メモリを使用します。ただし、カテゴリのオプションを変更して、グローバルな共有メモリを使用する必要があることを示すことができます。  
  
 カテゴリオプションの設定を含むレジストリキーは、HKEY_LOCAL_MACHINE\System\CurrentControlSet\Services\\< の区分名\>-パフォーマンスです。 既定では、カテゴリのオプションは3に設定されています。これにより、PerfCounter は、カテゴリ固有の共有メモリを使用するように指示します。 [カテゴリ] オプションが0に設定されている場合、PerfCounter .dll はグローバル共有メモリを使用します。 インスタンスデータが再利用されるのは、作成されるインスタンスの名前が再利用されるインスタンスと同一である場合だけです。 すべてのバージョンがカテゴリに書き込むことができます。 Category オプションが1に設定されている場合、グローバル共有メモリが使用されますが、カテゴリ名が再利用されるカテゴリと同じ長さの場合は、インスタンスデータを再利用できます。  
  
 設定0および1を使用すると、メモリリークが発生し、パフォーマンスカウンターのメモリがいっぱいになる可能性があります。  
  
## <a name="example"></a>例  
 次の例では、PerfCounter .dll が category Options レジストリエントリを参照して、カテゴリ固有の共有メモリを使用するかどうかを判断するように指定する方法を示します。  
  
```xml  
<configuration>  
  <runtime>  
    <forcePerformanceCounterUniqueSharedMemoryReads enabled="true"/>  
  </runtime>  
</configuration>  
```  
  
## <a name="see-also"></a>関連項目

- [ランタイム設定スキーマ](index.md)
- [構成ファイル スキーマ](../index.md)
