---
title: グローバリゼーションの構成設定
description: .NET Core アプリのグローバリゼーションの側面 (たとえば、日本語の日付の解析方法など) を構成するランタイム設定について説明します。
ms.date: 11/27/2019
ms.topic: reference
ms.openlocfilehash: 3764d0eb714c094b44ae843a1e626073ff8d82e4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76733457"
---
# <a name="run-time-configuration-options-for-globalization"></a>グローバリゼーションのランタイム構成オプション

## <a name="invariant-mode"></a>インバリアント モード

- .NET Core を、カルチャ固有のデータや動作にアクセスせずにグローバリゼーション インバリアント モードで実行するか、またはカルチャ データにアクセスできるかどうかを決定します。
- 既定:カルチャ データにアクセスしてアプリを実行します (`false`)。
- 詳細については、「[.NET Core のグローバリゼーション インバリアント モード](https://github.com/dotnet/runtime/blob/master/docs/design/features/globalization-invariant-mode.md)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `System.Globalization.Invariant` | `false` - カルチャ データにアクセスする<br/>`true` - インバリアント モードで実行する |
| **MSBuild のプロパティ** | `InvariantGlobalization` | `false` - カルチャ データにアクセスする<br/>`true` - インバリアント モードで実行する |
| **環境変数** | `DOTNET_SYSTEM_GLOBALIZATION_INVARIANT` | `0` - カルチャ データにアクセスする<br/>`1` - インバリアント モードで実行する |

### <a name="examples"></a>使用例

*runtimeconfig.json* ファイル:

```json
{
   "runtimeOptions": {
      "configProperties": {
         "System.Globalization.Invariant": true
      }
   }
}
```

プロジェクト ファイル:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <InvariantGlobalization>true</InvariantGlobalization>
  </PropertyGroup>

</Project>
```

## <a name="era-year-ranges"></a>年号の範囲

- 複数の年号をサポートするカレンダーの範囲チェックを緩和するか、または日付が年号の日付範囲をオーバーフローした場合に <xref:System.ArgumentOutOfRangeException> をスローするかを決定します。
- 既定:範囲チェックを緩和します (`false`)。
- 詳細については、「[カレンダー、時代 (年号)、および日付範囲: 緩やかな範囲のチェック](../../standard/datetime/working-with-calendars.md#calendars-eras-and-date-ranges-relaxed-range-checks)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.EnforceJapaneseEraYearRanges` | `false` - 緩和された範囲チェック<br/>`true` - オーバーフローによって例外が発生する |
| **環境変数** | N/A | N/A |

## <a name="japanese-date-parsing"></a>日本の日付の解析

- 年として "1" または "元年" のいずれかを含む文字列を正しく解析するか、または "1" のみをサポートするかを決定します。
- 既定:"1" または "元年" のいずれかを含む文字列を年として解析します (`false`)。
- 詳細については、「[複数の時代 (年号) のカレンダーで日付を表す](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.EnforceLegacyJapaneseDateParsing` | `false` - "元年" または "1" をサポートする<br/>`true` - "1" のみをサポートする |
| **環境変数** | N/A | N/A |

## <a name="japanese-year-format"></a>日本の年の形式

- 和暦の元年を "元年" または "1" のどちらで書式設定するかを決定します。
- 既定:元年を "元年" として書式設定します (`false`)。
- 詳細については、「[複数の時代 (年号) のカレンダーで日付を表す](../../standard/datetime/working-with-calendars.md#represent-dates-in-calendars-with-multiple-eras)」を参照してください。

| | 設定の名前 | 値 |
| - | - | - |
| **runtimeconfig.json** | `Switch.System.Globalization.FormatJapaneseFirstYearAsANumber` | `false` - "元年" として書式設定する<br/>`true` - 数字として書式設定する |
| **環境変数** | N/A | N/A |
