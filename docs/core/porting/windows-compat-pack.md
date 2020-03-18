---
title: Windows 互換機能パックを使用してコードを .NET Core に移植する
description: Windows 互換機能パックとそれを使用して既存の .NET Framework コードを .NET Core に移植する方法について紹介します。
author: terrajobst
ms.date: 12/07/2018
ms.openlocfilehash: 91a653b2345d414c18ebdb6e8b7d6d49bbdbb83e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76733611"
---
# <a name="use-the-windows-compatibility-pack-to-port-code-to-net-core"></a>Windows 互換機能パックを使用してコードを .NET Core に移植する

既存のコードを .NET Core に移植する際に発生する最も一般的な問題の一部として、.NET Framework のみに存在する API およびテクノロジへの依存があります。 *Windows 互換機能パック*には、このようなテクノロジの多くが用意されているので、.NET Core アプリケーションと .NET Standard ライブラリをはるかに簡単に構築できます。

互換性パックは論理的な [.NET Standard 2.0 の拡張機能](../whats-new/dotnet-core-2-0.md#api-changes-and-library-support)であり、API セットが大幅に増加します。 既存のコードは、ほとんど変更されずにコンパイルされます。 "あらゆる .NET 実装で提供される API のセット" でその約束を守るために、レジストリ、Windows Management Instrumentation (WMI)、リフレクション出力 API など、すべてのプラットフォームで動作しないテクノロジは .NET Standard には含まれていません。 Windows 互換機能パックは .NET Standard を基盤とし、これらの Windows 専用のテクノロジにアクセスできるようにします。 .NET Core に移行したいが、少なくとも最初の手順で Windows に留まることを計画している顧客にとって特に便利です。 このシナリオでは、Windows 専用のテクノロジを使用できるようにすることで、移行の障害を取り除きます。

## <a name="package-contents"></a>パッケージの内容

Windows 互換機能パックは、[Microsoft.Windows.Compatibility NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)経由で提供され、.NET Core または .NET Standard を対象とするプロジェクトから参照できます。

Windows 専用 API やプラットフォーム非依存 API など、約 20,000 の API を提供します。テクノロジ領域には次のものがあります。

- コード ページ
- CodeDom
- 構成
- ディレクトリ サービス
- 描画
- ODBC
- アクセス許可
- Port
- Windows アクセス制御リスト (ACL)
- Windows Communication Foundation (WCF)
- Windows 暗号化
- Windows EventLog
- Windows Management Instrumentation (WMI)
- Windows パフォーマンス カウンター
- Windows レジストリ
- Windows ランタイム キャッシュ
- Windows サービス

詳細については、[互換機能パックの仕様](https://github.com/dotnet/designs/blob/master/accepted/compat-pack/compat-pack.md)に関するページを参照してください。

## <a name="get-started"></a>はじめに

1. 移植の前に、[移植プロセス](index.md)を確認してください。

2. .NET Core または .NET Standard に既存のコードを移植するとき、[Microsoft.Windows.Compatibility NuGet パッケージ](https://www.nuget.org/packages/Microsoft.Windows.Compatibility)をインストールします。

   Windows に留まる場合、すでに用意はできています。

3. .NET Core アプリケーションまたは .NET Standard ライブラリを Linux または macOS で実行する場合、[API Analyzer](../../standard/analyzers/api-analyzer.md) を使用し、プラットフォーム非依存で機能しない API の使用を見つけます。

4. そのような API の使用を取り除くか、プラットフォーム非依存の代替で置換するか、プラットフォーム チェックで保護します (以下参照)。

    ```csharp
    private static string GetLoggingPath()
    {
        // Verify the code is running on Windows.
        if (RuntimeInformation.IsOSPlatform(OSPlatform.Windows))
        {
            using (var key = Registry.CurrentUser.OpenSubKey(@"Software\Fabrikam\AssetManagement"))
            {
                if (key?.GetValue("LoggingDirectoryPath") is string configuredPath)
                    return configuredPath;
            }
        }

        // This is either not running on Windows or no logging path was configured,
        // so just use the path for non-roaming user-specific data files.
        var appDataPath = Environment.GetFolderPath(Environment.SpecialFolder.LocalApplicationData);
        return Path.Combine(appDataPath, "Fabrikam", "AssetManagement", "Logging");
    }
    ```

デモについては、[Windows 互換機能パックの Channel 9 動画](https://channel9.msdn.com/Events/Connect/2017/T123)をご覧ください。
