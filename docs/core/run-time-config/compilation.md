---
title: コンパイルの構成設定
description: .NET Core アプリの JIT コンパイラの動作方法を構成するランタイム設定について説明します。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: adf1f01dba7387b89ee56784e33653d6a132c0e3
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77092890"
---
# <a name="run-time-configuration-options-for-compilation"></a>コンパイルのランタイム構成オプション

## <a name="tiered-compilation"></a>階層型コンパイル

- Just-In-Time (JIT) コンパイラで[階層型コンパイル](../whats-new/dotnet-core-3-0.md#tiered-compilation)を使用するかどうかを構成します。 階層型コンパイルでは、2 つの階層を使用して手法を切り替えます。
  - 最初の階層では、コードの生成を高速化 ([クイック JIT](#quick-jit)) するか、プリコンパイル済みコード ([ReadyToRun](#readytorun)) を読み込みます。
  - 2 番目の階層では、最適化されたコードをバックグラウンドで生成します ("JIT の最適化")。
- .NET Core 3.0 以降、階層型コンパイルは既定で有効に設定されます。
- .NET Core 2.1 および 2.2 では、階層型コンパイルは既定で無効に設定されます。
- 詳細については、「[階層型コンパイル ガイド](https://github.com/dotnet/runtime/blob/master/docs/design/features/tiered-compilation-guide.md)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation` | `true` - 有効<br/>`false` - 無効 |
| **MSBuild のプロパティ** | `TieredCompilation` | `true` - 有効<br/>`false` - 無効 |
| **環境変数** | `COMPlus_TieredCompilation` | `1` - 有効<br/>`0` - 無効 |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation": false
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilation>false</TieredCompilation>
  </PropertyGroup>

</Project>
```

## <a name="quick-jit"></a>クイック JIT

- JIT コンパイラで "*クイック JIT*" を使用するかどうかを構成します。 ループを含まず、事前コンパイル済みのコードを使用できないメソッドの場合、クイック JIT では最適化を行わずに高速にコンパイルされます。
- クイック JIT を有効にすると、起動時間が短縮されますが、パフォーマンス特性が低下したコードが生成される可能性があります。 たとえば、コードによって、より多くのスタック領域が使用され、より多くのメモリが割り当てられて、コードの実行速度が低下することがあります。
- クイック JIT を無効にして "[階層化コンパイル](#tiered-compilation)" を有効にした場合は、プリコンパイル済みのコードのみが階層型コンパイルの対象になります。 メソッドが [ReadyToRun](#readytorun) を使用してプリコンパイルされていない場合、JIT の動作は[階層型コンパイル](#tiered-compilation)が無効になっている場合と同じです。
- .NET Core 3.0 以降、クリック JIT は既定で有効になっています。
- .NET Core 2.1 および 2.2 では、クイック JIT は既定で無効になっています。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation.QuickJit` | `true` - 有効<br/>`false` - 無効 |
| **MSBuild のプロパティ** | `TieredCompilationQuickJit` | `true` - 有効<br/>`false` - 無効 |
| **環境変数** | `COMPlus_TC_QuickJit` | `1` - 有効<br/>`0` - 無効 |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation.QuickJit": false
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilationQuickJit>false</TieredCompilationQuickJit>
  </PropertyGroup>

</Project>
```

## <a name="quick-jit-for-loops"></a>ループに対するクイック JIT

- ループを含むメソッドに対して JIT コンパイラでクリック JIT を使用するかどうかを構成します。
- ループに対するクイック JIT を有効にすると、起動時のパフォーマンスが向上する可能性があります。 ただし、長時間にわたって実行されるループでは、長期間にわたってあまり最適化されていないコードでスタックする可能性があります。
- [クイック JIT](#quick-jit) が無効になっている場合、この設定は効果がありません。
- 既定:無効 (`false`)。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Runtime.TieredCompilation.QuickJitForLoops` | `false` - 無効<br/>`true` - 有効 |
| **MSBuild のプロパティ** | `TieredCompilationQuickJitForLoops` | `false` - 無効<br/>`true` - 有効 |
| **環境変数** | `COMPlus_TC_QuickJitForLoops` | `0` - 無効<br/>`1` - 有効 |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Runtime.TieredCompilation.QuickJitForLoops": false
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TieredCompilationQuickJitForLoops>false</TieredCompilationQuickJitForLoops>
  </PropertyGroup>

</Project>
```

## <a name="readytorun"></a>ReadyToRun

- 使用可能な ReadyToRun データを含むイメージに対して、.NET Core ランタイムでプリコンパイル済みコードを使用するかどうかを構成します。 このオプションを無効にすると、ランタイムでフレームワーク コードが JIT コンパイルされます。
- 詳細については、「[ReadyToRun](../whats-new/dotnet-core-3-0.md#readytorun-images)」を参照してください。
- 既定:有効 (`1`)。

| | 設定の名前 | 値 |
| - | - | - |
| **環境変数** | `COMPlus_ReadyToRun` | `1` - 有効<br/>`0` - 無効 |
