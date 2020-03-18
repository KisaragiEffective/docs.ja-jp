---
title: 既定のプローブ - .NET Core
description: 依存関係を特定するための .NET Core の System.Runtime.Loader.AssemblyLoadContext.Default プローブの概要。
ms.date: 08/09/2019
author: sdmaclea
ms.author: stmaclea
ms.openlocfilehash: 500ee6ee863b1f311970a9e718936f57f7d4efd6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79398005"
---
# <a name="default-probing"></a>既定のプローブ

<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> インスタンスは、アセンブリの依存関係を検索する役目を持っています。 この記事では、<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> インスタンスのプローブ ロジックについて説明します。

## <a name="host-configured-probing-properties"></a>ホストで構成されたプローブ プロパティ

ランタイムが開始されると、ランタイム ホストは <xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> プローブ パスを構成する一連の名前付きプローブ プロパティを提供します。

各プローブ プロパティはオプションです。 存在する場合、各プロパティは区切り記号で区切られた絶対パスのリストを含む文字列値です。 区切り記号は、Windows では ';'、他のすべてのプラットフォームでは ':' になります。

|プロパティ名                 |[説明]  |
|------------------------------|---------|
|`TRUSTED_PLATFORM_ASSEMBLIES`   | プラットフォームとアプリケーション アセンブリ ファイル パスの一覧。 |
|`PLATFORM_RESOURCE_ROOTS`       | サテライト リソース アセンブリを検索するディレクトリ パスの一覧。 |
|`NATIVE_DLL_SEARCH_DIRECTORIES` | アンマネージド (ネイティブ) ライブラリを検索するディレクトリ パスの一覧。        |
|`APP_PATHS`                     | マネージド アセンブリを検索するディレクトリ パスの一覧。 |
|`APP_NI_PATHS`                  | マネージド アセンブリのネイティブ イメージを検索するディレクトリ パスの一覧。 |

### <a name="how-are-the-properties-populated"></a>プロパティの設定方法

*\<myapp>.deps.json* ファイルが存在するかどうかに応じて、プロパティを設定するための 2 つの主なシナリオがあります。

- *\*.deps.json* ファイルが存在する場合は、ファイルが解析されてプローブ プロパティが設定されます。
- *\*.deps.json* ファイルが存在しない場合、アプリケーションのディレクトリに、すべての依存関係が含まれていると見なされます。 ディレクトリの内容が、プローブ プロパティを設定するために使用されます。

また、参照されているフレームワークの *\*.deps.json* ファイルも同様に解析されます。

最後に、環境変数 `ADDITIONAL_DEPS` を使用して、依存関係を追加できます。

### <a name="how-do-i-see-the-probing-properties-from-managed-code"></a>マネージド コードからプローブ プロパティを参照する方法

各プロパティは、上記の表のプロパティ名を使用して <xref:System.AppContext.GetData(System.String)?displayProperty=nameWithType> 関数を呼び出すことによって使用できます。

### <a name="how-do-i-debug-the-probing-properties-construction"></a>プローブ プロパティの構築をデバッグする方法

以下に示す特定の環境変数が有効になっている場合、.NET Core ランタイム ホストは便利なトレース メッセージを出力します。

|環境変数        |[説明]  |
|----------------------------|---------|
|`COREHOST_TRACE=1`          |トレースを有効にします。|
|`COREHOST_TRACEFILE=<path>` |既定の `stderr` ではなくファイル パスにトレースします。|
|`COREHOST_TRACE_VERBOSITY`  |詳細度を 1 (最低) から 4 (最高) に設定します。|

## <a name="managed-assembly-default-probing"></a>マネージド アセンブリの既定のプローブ

マネージド アセンブリを探すためにプローブするとき、<xref:System.Runtime.Loader.AssemblyLoadContext.Default%2A?displayProperty=nameWithType> は次の順序で検索します。

- <xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 内の `TRUSTED_PLATFORM_ASSEMBLIES` に一致するファイル (ファイル拡張子を削除した後)。
- 共通のファイル拡張子を持つ `APP_NI_PATHS` のネイティブ イメージ アセンブリ ファイル。
- 共通のファイル拡張子を持つ `APP_PATHS` のアセンブリ ファイル。

## <a name="satellite-resource-assembly-probing"></a>サテライト (リソース) アセンブリのプローブ

サテライト アセンブリを検索して特定のカルチャを見つけるために、一連のファイル パスを構築します。

`PLATFORM_RESOURCE_ROOTS` および `APP_PATHS` の各パスに対して、<xref:System.Globalization.CultureInfo.Name?displayProperty=nameWithType> 文字列、ディレクトリ区切り記号、<xref:System.Reflection.AssemblyName.Name?displayProperty=nameWithType> 文字列、および拡張子 '.dll' を追加します。

一致するファイルが存在する場合は、それを読み込んで返すことを試行します。

## <a name="unmanaged-native-library-probing"></a>アンマネージド (ネイティブ) ライブラリのプローブ

アンマネージド ライブラリを探すためにプローブする場合、一致するライブラリを見つけるために `NATIVE_DLL_SEARCH_DIRECTORIES` が検索されます。
