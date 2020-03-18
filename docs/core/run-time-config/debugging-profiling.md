---
title: デバッグとプロファイルの構成設定
description: .NET Core アプリのデバッグとプロファイルを構成するランタイム設定について説明します。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: c57cfa7233f48def890ded3c9d589b7f268147df
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "74998859"
---
# <a name="run-time-configuration-options-for-debugging-and-profiling"></a>デバッグとプロファイルのランタイム構成オプション

## <a name="enable-diagnostics"></a>診断を有効化する

- デバッガー、プロファイラー、EventPipe 診断を有効にするか、または無効にするかを構成します。
- 既定:有効 (`1`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `COMPlus_EnableDiagnostics` | `1` - 有効<br/>`0` - 無効 |

## <a name="enable-profiling"></a>プロファイルを有効にする

- 現在実行中のプロセスに対してプロファイルを有効にするかどうかを構成します。
- 既定:無効 (`0`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `CORECLR_ENABLE_PROFILING` | `0` - 無効<br/>`1` - 有効 |

## <a name="profiler-guid"></a>プロファイラー GUID

- 現在実行中のプロセスに読み込むプロファイラーの GUID を指定します。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `CORECLR_PROFILER` | *string-guid* |

## <a name="profiler-location"></a>プロファイラーの場所

- 現在実行中のプロセス (32 ビット プロセスまたは 64 ビット プロセス) に読み込むプロファイラー DLL へのパスを指定します。
- 複数の変数が設定されている場合、ビット固有の変数が優先されます。 これらは、ロードするプロファイラーのビットを指定します。
- 詳細については、「[プロファイラー ライブラリを検索する](https://github.com/dotnet/runtime/blob/master/docs/design/coreclr/profiling/Profiler%20Loading.md)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **環境変数** | `CORECLR_PROFILER_PATH` | *string-path* |
| **環境変数** | `CORECLR_PROFILER_PATH_32` | *string-path* |
| **環境変数** | `CORECLR_PROFILER_PATH_64` | *string-path* |

## <a name="write-perf-map"></a>パフォーマンス マップの作成

- Linux システムでの */tmp/perf-$pid.map* の作成を有効または無効にします。
- 既定:無効 (`0`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `COMPlus_PerfMapEnabled` | `0` - 無効<br/>`1` - 有効 |

## <a name="perf-log-markers"></a>パフォーマンス ログのマーカー

- `COMPlus_PerfMapEnabled` が `1` に設定されている場合、指定されたシグナルを有効または無効にして、パフォーマンス ログのマーカーとして受け入れて無視します。
- 既定:無効 (`0`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | N/A | N/A |
| **環境変数** | `COMPlus_PerfMapIgnoreSignal` | `0` - 無効<br/>`1` - 有効 |
