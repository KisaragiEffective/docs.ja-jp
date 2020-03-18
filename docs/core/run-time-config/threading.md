---
title: スレッドの構成設定
description: .NET Core アプリのスレッドを構成するランタイム設定について説明します。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: 68b8e93ca6ec3f708a7a627307655ada1955500a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76789847"
---
# <a name="run-time-configuration-options-for-threading"></a>スレッドのランタイム構成オプション

## <a name="cpu-groups"></a>CPU グループ

- CPU グループ全体にスレッドを自動的に配布するかどうかを構成します。
- 既定:無効 (`0`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `COMPlus_Thread_UseAllCpuGroups` | `0` - 無効<br/>`1` - 有効 |

## <a name="minimum-threads"></a>最小スレッド数

- ワーカー スレッド プールの最小スレッド数を指定します。
- <xref:System.Threading.ThreadPool.SetMinThreads%2A?displayProperty=nameWithType> メソッドに対応します。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Threading.ThreadPool.MinThreads` | 最小スレッド数を表す整数 |
| **MSBuild のプロパティ** | `ThreadPoolMinThreads` | 最小スレッド数を表す整数 |
| **環境変数** | N/A | N/A |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Threading.ThreadPool.MinThreads": 4
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ThreadPoolMinThreads>4</ThreadPoolMinThreads>
  </PropertyGroup>

</Project>
```

## <a name="maximum-threads"></a>最大スレッド数

- ワーカー スレッド プールの最大スレッド数を指定します。
- <xref:System.Threading.ThreadPool.SetMaxThreads%2A?displayProperty=nameWithType> メソッドに対応します。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Threading.ThreadPool.MaxThreads` | 最大スレッド数を表す整数 |
| **MSBuild のプロパティ** | `ThreadPoolMaxThreads` | 最大スレッド数を表す整数 |
| **環境変数** | N/A | N/A |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Threading.ThreadPool.MaxThreads": 20
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <ThreadPoolMaxThreads>20</ThreadPoolMaxThreads>
  </PropertyGroup>

</Project>
```
