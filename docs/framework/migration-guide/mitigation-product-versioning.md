---
title: '軽減策: 製品のバージョン管理'
ms.date: 03/30/2017
ms.assetid: 1c4de9d7-9aba-427a-8f38-0ab9bfb8f85e
ms.openlocfilehash: 64a68d2b79a0a3ccdd806949ffd6cb3760974390
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73457812"
---
# <a name="mitigation-product-versioning"></a>軽減策: 製品のバージョン管理

.NET Framework 4.6 以降のバージョンでは、製品のバージョン管理が .NET Framework の以前のリリース (.NET Framework 4、4.5、4.5.1、4.5.2) から変更されました。

## <a name="product-versioning-changes"></a>製品のバージョン管理の変更点

変更の詳細は次のとおりです。

- `Version` キーの `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` 値エントリの形式は、.NET Framework 4.6 とそのポイント リリースの場合は `4.6.`*xxxxx* に、.NET Framework 4.7 の場合は `4.7.`*xxxxx* にそれぞれ変更されました。 .NET Framework 4.5、4.5.1、および 4.5.2 では `4.5.`*xxxxx* という形式でした。

- .NET Framework ファイルにおけるファイルおよび製品のバージョン管理は、.NET Framework 4.6 とそのポイント リリースの場合は以前のスキーマのバージョン管理 `4.0.30319.x` から `4.6.X.0` に、.NET Framework 4.7 とそのポイント リリースの場合は `4.7.X.0` にそれぞれ変更されました。 ファイルを右クリックしてファイルの **[プロパティ]** を表示すると、これらの新しい値が表示されます。

- マネージド アセンブリの <xref:System.Reflection.AssemblyFileVersionAttribute> 属性と <xref:System.Reflection.AssemblyInformationalVersionAttribute> 属性の <xref:System.Version> 値は、.NET Framework 4.6 とそのポイント リリースの場合は `4.6.X.0` という形式、.NET Framework 4.7 の場合は `4.7.X.0` という形式になります。

- .NET Framework 4.6 以降では、<xref:System.Environment.Version%2A?displayProperty=nameWithType> プロパティによって固定のバージョン文字列 `4.0.30319.42000` が返されます。 .NET Framework 4、4.5、4.5.1、4.5.2 では、`4.0.30319.xxxxx` という形式のバージョン文字列が返されます。ここでの `xxxxx` は 42000 より小さいです (例: "4.0.30319.18010")。 アプリケーションのコードで <xref:System.Environment.Version%2A?displayProperty=nameWithType> プロパティに新しい依存関係を設定することは推奨していないことに注意してください。

### <a name="handling-the-product-versioning-changes"></a>製品のバージョン管理の変更に対する処置

一般に、.NET Framework のランタイムのバージョンやインストール ディレクトリを検出する際、アプリケーションは推奨される技法に従う必要があります。

- .NET Framework のランタイムのバージョンを検出するには、「[方法 : インストールされている .NET Framework バージョンを確認する](how-to-determine-which-versions-are-installed.md)」を参照してください。

- .NET Framework のインストール パスを確認するには、`InstallPath` キーの `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` エントリの値を使用します。

  > [!IMPORTANT]
  > サブキー名は、`NET Framework Setup` ではなく `.NET Framework Setup` です。

- .NET Framework の共通言語ランタイムへのディレクトリ パスを確認するには、<xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory%2A?displayProperty=nameWithType> メソッドを呼び出します。

- CLR のバージョンを取得するには、<xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion%2A?displayProperty=nameWithType> メソッドを呼び出します。   .NET Framework 4 とそのポイント リリース (.NET Framework 4.5、4.5.1、4.5.2、および .NET Framework 4.6、4.6.1、4.6.2、4.7) の場合、それによって文字列 `v4.0.30319` が返されます。

## <a name="see-also"></a>参照

- [アプリケーションの互換性](application-compatibility.md)
