---
title: ガベージ コレクター構成の設定
description: ガベージ コレクターでの .NET Core アプリ用のメモリの管理方法を構成するための、実行時設定について学習します。
ms.date: 01/09/2020
ms.topic: reference
ms.openlocfilehash: 044083d69601f5092724a46d358b2ee5673d428d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76733525"
---
# <a name="run-time-configuration-options-for-garbage-collection"></a>ガベージ コレクションの実行時構成オプション

このページには、実行時に変更できるガベージ コレクター (GC) の設定に関する情報が含まれています。 実行中のアプリのピーク時のパフォーマンスを実現しようとしている場合は、これらの設定の使用を検討してください。 しかし、一般的な状況では、既定値でほとんどのアプリケーションに最適なパフォーマンスが得られます。

このページでは、設定はグループにまとめられます。 各グループ内の設定は一般的に、特定の結果を得るために相互に組み合わせて使用されます。

> [!NOTE]
>
> - これらの設定は、実行中にアプリで動的に変更することもできます。したがって、設定した実行時の設定が上書きされる可能性があります。
> - [待機時間レベル](../../standard/garbage-collection/latency.md)などの一部の設定は、通常、デザイン時に API を介してのみ設定されます。 このページでは、このような設定は省略されています。
> - 数値の場合、*runtimeconfig.json* ファイルの設定には 10 進表記、環境変数の設定には 16 進表記を使用します。 16 進値の場合は、プレフィックス "0x" を付けても付けなくても指定できます。

## <a name="flavors-of-garbage-collection"></a>ガベージ コレクションのフレーバー

ガベージ コレクションの主な 2 つのフレーバーは、ワークステーション GC とサーバー GC です。 2 つの間の相違点について詳しくは、「[ガベージ コレクションの基礎](../../standard/garbage-collection/fundamentals.md#workstation-and-server-garbage-collection)」を参照してください。

ガベージ コレクションのサブフレーバーは、バックグラウンドと非同時実行です。

ガベージ コレクションのフレーバーを選択するには、以下の設定を使用します。

### <a name="systemgcservercomplus_gcserver"></a>System.GC.Server/COMPlus_gcServer

- アプリケーションで、ワークステーション ガベージ コレクションとサーバー ガベージ コレクションのどちらを使用するかを構成します。
- 既定:ワークステーション ガベージ コレクション (`false`)。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Server` | `false` - ワークステーション<br/>`true` - サーバー | .NET Core 1.0 |
| **MSBuild のプロパティ** | `ServerGarbageCollection` | `false` - ワークステーション<br/>`true` - サーバー | .NET Core 1.0 |
| **環境変数** | `COMPlus_gcServer` | `0` - ワークステーション<br/>`1` - サーバー | .NET Core 1.0 |
| **.NET Framework 用の app.config** | [GCServer](../../framework/configure-apps/file-schema/runtime/gcserver-element.md) | `false` - ワークステーション<br/>`true` - サーバー |  |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Server": true
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ServerGarbageCollection>true</ServerGarbageCollection>
  </PropertyGroup>

</Project>
```

### <a name="systemgcconcurrentcomplus_gcconcurrent"></a>System.GC.Concurrent/COMPlus_gcConcurrent

- バックグラウンド (同時実行) ガベージ コレクションを有効にするかどうかを構成します。
- 既定:有効 (`true`)。
- 詳細については、[バックグラウンド ガベージ コレクション](../../standard/garbage-collection/fundamentals.md#background-workstation-garbage-collection)に関する記事と、「[バックグラウンド サーバー ガベージ コレクション](../../standard/garbage-collection/fundamentals.md#background-server-garbage-collection)」を参照してください。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.Concurrent` | `true` - バックグラウンド GC<br/>`false` -非同時実行 GC | .NET Core 1.0 |
| **MSBuild のプロパティ** | `ConcurrentGarbageCollection` | `true` - バックグラウンド GC<br/>`false` -非同時実行 GC | .NET Core 1.0 |
| **環境変数** | `COMPlus_gcConcurrent` | `true` - バックグラウンド GC<br/>`false` -非同時実行 GC | .NET Core 1.0 |
| **.NET Framework 用の app.config** | [gcConcurrent](../../framework/configure-apps/file-schema/runtime/gcconcurrent-element.md) | `true` - バックグラウンド GC<br/>`false` -非同時実行 GC |  |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.Concurrent": false
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ConcurrentGarbageCollection>false</ConcurrentGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="manage-resource-usage"></a>リソース使用量を管理する

このセクションで説明する設定を使用して、ガベージ コレクターのメモリとプロセッサの使用量を管理します。

これらの設定のいくつかの詳細については、[ワークステーションおよびサーバー GC 間の妥協点](https://devblogs.microsoft.com/dotnet/middle-ground-between-server-and-workstation-gc/)に関するブログ エントリを参照してください。

### <a name="systemgcheapcountcomplus_gcheapcount"></a>System.GC.HeapCount/COMPlus_GCHeapCount

- ガベージ コレクターによって作成されるヒープの数を制限します。
- サーバー ガベージ コレクションにのみ適用されます。
- GC プロセッサの関係が有効になっている場合 (既定値)、ヒープ数の設定では、最初の `n` プロセッサに `n` GC ヒープやスレッドを関連付けます (関係付けマスクまたは関係付け範囲の設定を使用して、関係付けるプロセッサを正確に指定します)。
- GC プロセッサの関係が無効になっている場合、この設定によって GC ヒープの数が制限されます。
- 詳細については、[GCHeapCount の解説](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md#remarks)に関する記述を参照してください。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapCount` | "*10 進値*" | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCHeapCount` | "*16 進値*" | .NET Core 3.0 |
| **.NET Framework 用の app.config** | [GCHeapCount](../../framework/configure-apps/file-schema/runtime/gcheapcount-element.md) | "*10 進値*" | .NET Framework 4.6.2 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapCount": 16
      }
   }
}
```

> [!TIP]
> *runtimeconfig.json* でオプションを設定する場合は、10 進値を指定します。 環境変数としてオプションを設定する場合は、16 進値を指定します。 たとえば、ヒープの数を 16 に制限する場合、値は JSON ファイルでは 16、環境変数では 0x10 または 10 になります。

### <a name="systemgcheapaffinitizemaskcomplus_gcheapaffinitizemask"></a>System.GC.HeapAffinitizeMask/COMPlus_GCHeapAffinitizeMask

- ガベージ コレクター スレッドで使用する必要がある正確なプロセッサを指定します。
- `System.GC.NoAffinitize` を `true` に設定してプロセッサの関係を無効にした場合、この設定は無視されます。
- サーバー ガベージ コレクションにのみ適用されます。
- この値は、プロセスで使用できるプロセッサを定義するビット マスクです。 たとえば、10 進値の 1023 (環境変数を使用する場合は、16 進値の 0x3FF または 3FF) は、バイナリ表記では 0011 1111 1111 となります。 これにより、最初の 10 プロセッサが使用されることが指定されます。 次の 10 プロセッサ (つまり、10 から 19 プロセッサ) を指定するには、10 進値の 1047552 (または 16 進値の 0xFFC00 または FFC00) を指定します。これは、バイナリ値の 1111 1111 1100 0000 0000 に相当します。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapAffinitizeMask` | "*10 進値*" | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCHeapAffinitizeMask` | "*16 進値*" | .NET Core 3.0 |
| **.NET Framework 用の app.config** | [GCHeapAffinitizeMask](../../framework/configure-apps/file-schema/runtime/gcheapaffinitizemask-element.md) | "*10 進値*" | .NET Framework 4.6.2 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapAffinitizeMask": 1023
      }
   }
}
```

### <a name="systemgcgcheapaffinitizerangescomplus_gcheapaffinitizeranges"></a>System.GC.GCHeapAffinitizeRanges/COMPlus_GCHeapAffinitizeRanges

- ガベージ コレクター スレッドに使用するプロセッサのリストを指定します。
- この設定は `System.GC.HeapAffinitizeMask` に似ていますが、64 を超えるプロセッサを指定できる点が異なります。
- Windows オペレーティング システムの場合、プロセッサの番号または範囲の前に、対応する [CPU グループ](/windows/win32/procthread/processor-groups)を付けます。たとえば、"0:1-10,0:12,1:50-52,1:70" となります。
- `System.GC.NoAffinitize` を `true` に設定してプロセッサの関係を無効にした場合、この設定は無視されます。
- サーバー ガベージ コレクションにのみ適用されます。
- 詳細については、Maoni Stephens のブログの「[CPU が 64 を超えるコンピューター上での GC のための CPU 構成の向上](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)」を参照してください。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.GCHeapAffinitizeRanges` | プロセッサ番号またはプロセッサ番号の範囲のコンマ区切りリスト。<br/>Unix の例:"1-10,12,50-52,70"<br/>Windows の例:"0:1-10,0:12,1:50-52,1:70" | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCHeapAffinitizeRanges` | プロセッサ番号またはプロセッサ番号の範囲のコンマ区切りリスト。<br/>Unix の例:"1-10,12,50-52,70"<br/>Windows の例:"0:1-10,0:12,1:50-52,1:70" | .NET Core 3.0 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.GCHeapAffinitizeRanges": "0:1-10,0:12,1:50-52,1:70"
      }
   }
}
```

### <a name="complus_gccpugroup"></a>COMPlus_GCCpuGroup

- ガベージ コレクターで [CPU グループ](/windows/win32/procthread/processor-groups)を使用するかどうかを構成します。

  64 ビットの Windows コンピューターに複数の CPU グループがある (つまり、64 を超えるプロセッサがある) 場合、この要素を有効にすると、すべての CPU グループ全体にガベージ コレクションが拡張されます。 ガベージ コレクターではすべてのコアを使用し、ヒープを作成して分散させます。

- 64 ビット Windows オペレーティング システムのみのサーバー ガベージ コレクションに適用されます。
- 既定:無効 (`0`)。
- 詳細については、Maoni Stephens のブログの「[CPU が 64 を超えるコンピューター上での GC のための CPU 構成の向上](https://devblogs.microsoft.com/dotnet/making-cpu-configuration-better-for-gc-on-machines-with-64-cpus/)」を参照してください。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | N/A | N/A | N/A |
| **環境変数** | `COMPlus_GCCpuGroup` | `0` - 無効<br/>`1` - 有効 | .NET Core 1.0 |
| **.NET Framework 用の app.config** | [GCCpuGroup](../../framework/configure-apps/file-schema/runtime/gccpugroup-element.md) | `false` - 無効<br/>`true` - 有効 |  |

> [!NOTE]
> すべての CPU グループ全体にスレッド プールからのスレッドも分散するように共通言語ランタイム (CLR) を構成するには、[Thread_UseAllCpuGroups 要素](../../framework/configure-apps/file-schema/runtime/thread-useallcpugroups-element.md)オプションを有効にします。 .NET Core アプリの場合は、`COMPlus_Thread_UseAllCpuGroups` 環境変数の値を `1` に設定することによって、このオプションを有効にすることができます。

### <a name="systemgcnoaffinitizecomplus_gcnoaffinitize"></a>System.GC.NoAffinitize/COMPlus_GCNoAffinitize

- ガベージ コレクション スレッドをプロセッサに "*関係付ける*" かどうかを指定します。 GC スレッドを関係付けることは、その特定の CPU でのみ実行できることを意味します。 各 GC スレッドに対してヒープが作成されます。
- サーバー ガベージ コレクションにのみ適用されます。
- 既定:ガベージ コレクション スレッドをプロセッサに関係付けます (`false`)。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.NoAffinitize` | `false` -関係付ける<br/>`true` -関係付けない | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCNoAffinitize` | `0` -関係付ける<br/>`1` -関係付けない | .NET Core 3.0 |
| **.NET Framework 用の app.config** | [GCNoAffinitize](../../framework/configure-apps/file-schema/runtime/gcnoaffinitize-element.md) | `false` -関係付ける<br/>`true` -関係付けない | .NET Framework 4.6.2 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.NoAffinitize": true
      }
   }
}
```

### <a name="systemgcheaphardlimitcomplus_gcheaphardlimit"></a>System.GC.HeapHardLimit/COMPlus_GCHeapHardLimit

- GC ヒープおよび GC ブックキーピングに対して、最大コミット サイズをバイト単位で指定します。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimit` | "*10 進値*" | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCHeapHardLimit` | "*16 進値*" | .NET Core 3.0 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimit": 209715200
      }
   }
}
```

> [!TIP]
> *runtimeconfig.json* でオプションを設定する場合は、10 進値を指定します。 環境変数としてオプションを設定する場合は、16 進値を指定します。 たとえば、ヒープのハード制限を 200 メビバイト (MiB) に指定する場合、値は JSON ファイルでは 209715200、環境変数では 0xC800000 または C800000 になります。

### <a name="systemgcheaphardlimitpercentcomplus_gcheaphardlimitpercent"></a>System.GC.HeapHardLimitPercent/COMPlus_GCHeapHardLimitPercent

- GC ヒープ使用量を、合計メモリに対する割合として指定します。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.HeapHardLimitPercent` | "*10 進値*" | .NET Core 3.0 |
| **環境変数** | `COMPlus_GCHeapHardLimitPercent` | "*16 進値*" | .NET Core 3.0 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.HeapHardLimitPercent": 30
      }
   }
}
```

> [!TIP]
> *runtimeconfig.json* でオプションを設定する場合は、10 進値を指定します。 環境変数としてオプションを設定する場合は、16 進値を指定します。 たとえば、ヒープの使用量を 30% に制限する場合、値は JSON ファイルでは 30、環境変数では 0x1E または 1E になります。

### <a name="systemgcretainvmcomplus_gcretainvm"></a>System.GC.RetainVM/COMPlus_GCRetainVM

- 削除する必要があるセグメントを、後で使用するためにスタンバイ リストに配置するか、解放してオペレーティング システム (OS) に戻すかを構成します。
- 既定:セグメントを解放してオペレーティング システムに戻します (`false`)。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.RetainVM` | `false` -OS に解放する<br/>`true` - スタンバイに配置する | .NET Core 1.0 |
| **MSBuild のプロパティ** | `RetainVMGarbageCollection` | `false` -OS に解放する<br/>`true` - スタンバイに配置する | .NET Core 1.0 |
| **環境変数** | `COMPlus_GCRetainVM` | `0` -OS に解放する<br/>`1` - スタンバイに配置する | .NET Core 1.0 |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.RetainVM": true
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <RetainVMGarbageCollection>true</RetainVMGarbageCollection>
  </PropertyGroup>

</Project>
```

## <a name="large-pages"></a>大きいページ

### <a name="complus_gclargepages"></a>COMPlus_GCLargePages

- ヒープのハード制限が設定されている場合に、大きいページを使用する必要があるかどうかを指定します。
- 既定:無効 (`0`)。
- これは試験段階の設定です。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | N/A | N/A | N/A |
| **環境変数** | `COMPlus_GCLargePages` | `0` - 無効<br/>`1` - 有効 | .NET Core 3.0 |

## <a name="large-objects"></a>大きなオブジェクト

### <a name="complus_gcallowverylargeobjects"></a>COMPlus_gcAllowVeryLargeObjects

- 合計サイズが 2 ギガバイト (GB) を超える配列に対して、64 ビット プラットフォームでのガベージ コレクター サポートを構成します。
- 既定:有効 (`1`)。
- このオプションは、将来のバージョンの .NET で廃止される可能性があります。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | N/A | N/A | N/A |
| **環境変数** | `COMPlus_gcAllowVeryLargeObjects` | `1` - 有効<br/> `0` - 無効 | .NET Core 1.0 |
| **.NET Framework 用の app.config** | [gcAllowVeryLargeObjects](../../framework/configure-apps/file-schema/runtime/gcallowverylargeobjects-element.md) | `1` - 有効<br/> `0` - 無効 | .NET Framework 4.5 |

## <a name="large-object-heap-threshold"></a>大きなオブジェクト ヒープのしきい値

### <a name="systemgclohthresholdcomplus_gclohthreshold"></a>System.GC.LOHThreshold/COMPlus_GCLOHThreshold

- オブジェクトが大きなオブジェクト ヒープ (LOH) に移る原因となる、しきい値のサイズ (バイト単位) を指定します。
- 既定のしきい値は 85,000 バイトです。
- 指定する値は、既定のしきい値より大きくする必要があります。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | `System.GC.LOHThreshold` | "*10 進値*" | .NET Core 1.0 |
| **環境変数** | `COMPlus_GCLOHThreshold` | "*16 進値*" | .NET Core 1.0 |
| **.NET Framework 用の app.config** | [GCLOHThreshold](../../framework/configure-apps/file-schema/runtime/gclohthreshold-element.md) | "*10 進値*" | .NET Framework 4.8 |

例:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.GC.LOHThreshold": 120000
      }
   }
}
```

> [!TIP]
> *runtimeconfig.json* でオプションを設定する場合は、10 進値を指定します。 環境変数としてオプションを設定する場合は、16 進値を指定します。 たとえば、しきい値のサイズを 120,000 バイトに設定する場合、値は JSON ファイルでは 120000、環境変数では 0x1D4C0 または 1D4C0 になります。

## <a name="standalone-gc"></a>スタンドアロン GC

### <a name="complus_gcname"></a>COMPlus_GCName

- ランタイムで読み込む対象となる、ガベージ コレクターを含むライブラリへのパスを指定します。
- 詳細については、「[スタンドアロン GC ローダーの設計](https://github.com/dotnet/runtime/blob/master/docs/design/features/standalone-gc-loading.md)」を参照してください。

| | 設定の名前 | 値 | 導入されたバージョン |
| - | - | - | - |
| **runtimeconfig.json** | N/A | N/A | N/A |
| **環境変数** | `COMPlus_GCName` | *string_path* | .NET Core 2.0 |
