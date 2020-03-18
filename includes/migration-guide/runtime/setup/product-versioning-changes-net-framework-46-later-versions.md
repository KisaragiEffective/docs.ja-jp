---
ms.openlocfilehash: a02887327390afb6859c0d5f78a8875bff9539e6
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70997656"
---
### <a name="product-versioning-changes-in-the-net-framework-46-and-later-versions"></a>.NET Framework 4.6 以降のバージョンでの製品のバージョン管理に関する変更点

|   |   |
|---|---|
|説明|製品のバージョン管理が、.NET Framework の以前のリリース (特に .NET Framework 4、4.5、4.5.1、4.5.2) から変更されました。 変更の詳細は次のとおりです。<ul><li><code>Version</code> キーの <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code> エントリの値が、.NET Framework 4.6 とそのポイント リリースの場合は <code>4.6.xxxxx</code> に、.NET Framework 4.7 と 4.7.1 の場合は <code>4.7.xxxxx</code> に、それぞれ変更されました。 .NET Framework 4.5、4.5.1、および 4.5.2 では、<code>4.5.xxxxx</code> という形式でした。</li><li>.NET Framework ファイルにおけるファイルおよび製品のバージョン管理は、以前のバージョン管理方式である 4.0.30319.x から、.NET Framework 4.6 とそのポイント リリースの場合は 4.6.X.0 に、.NET Framework 4.7 と 4.7.1 の場合は 4.7.X.0 に、それぞれ変更されました。 ファイルを右クリックしてファイルの [プロパティ] を表示すると、これらの新しい値を確認できます。</li><li>マネージド アセンブリの <xref:System.Reflection.AssemblyFileVersionAttribute> 属性と <xref:System.Reflection.AssemblyInformationalVersionAttribute> 属性の Version 値は、.NET Framework 4.6 とそのポイント リリースの場合は 4.6.X.0 という形式に、.NET Framework 4.7 と 4.7.1 の場合は 4.7.X.0 という形式になります。</li><li>.NET Framework 4.6、4.6.1、4.6.2、4.7、4.7.1 では、<xref:System.Environment.Version?displayProperty=nameWithType> プロパティは固定のバージョン文字列 <code>4.0.30319.42000</code> を返します。 .NET Framework 4、4.5、4.5.1、および 4.5.2 では、<code>4.0.30319.xxxxx</code> という形式のバージョン文字列が返されます (例: &quot;4.0.30319.18010&quot;)。 アプリケーションのコードで Environment.Version プロパティに新しい依存関係を設定することは推奨されていないことに注意してください。</li></ul>詳しくは、「[方法: インストールされている .NET Framework バージョンを確認する](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md)」をご覧ください。|
|提案される解決策|一般に、.NET Framework のランタイムのバージョンやインストール ディレクトリを検出する際、アプリケーションは推奨される技法に従う必要があります。<ul><li>.NET Framework のランタイムのバージョンを検出するには、「[方法 : インストールされている .NET Framework バージョンを確認する](~/docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md)」を参照してください。</li><li>.NET Framework のインストール パスを確認するには、<code>InstallPath</code> キーの <code>HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full</code> エントリの値を使用します。</li></ul> <blockquote> [!IMPORTANT] サブキー名は、<code>NET Framework Setup</code> ではなく <code>.NET Framework Setup</code> です。</blockquote> <ul><li>.NET Framework の共通言語ランタイムへのディレクトリ パスを確認するには、<xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory?displayProperty=nameWithType> メソッドを呼び出します。</li><li>CLR のバージョンを取得するには、<xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion?displayProperty=nameWithType> メソッドを呼び出します。 .NET Framework 4 とそのポイント リリース (.NET Framework 4.5、4.5.1、4.5.2、および .NET Framework 4.6、4.6.1、4.6.2、4.7、4.7.1) の場合、このメソッドは文字列 v4.0.30319 を返します。</li></ul>|
|スコープ|Minor|
|バージョン|4.6|
|[種類]|ランタイム|
