---
title: バージョン管理および .NET ライブラリ
description: .NET ライブラリのバージョン管理に関するベスト プラクティスの推奨事項。
ms.date: 12/10/2018
ms.openlocfilehash: a274410714791e2790da0e3deb2a595390ee9389
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398497"
---
# <a name="versioning"></a>バージョン管理

ソフトウェア ライブラリがバージョン 1.0 で完成することは、ほとんどありません。 優れたライブラリは時間と共に進化し、機能が追加され、バグが修正され、パフォーマンスが向上します。 既存のユーザーの互換性を損なわずに、各バージョンに追加の値を付与して、.NET ライブラリの新しいバージョンをリリースできることが重要です。

## <a name="breaking-changes"></a>互換性に影響する変更

バージョン間の互換性に影響する変更の処理方法については、「[重大な変更](./breaking-changes.md)」をご覧ください。

## <a name="version-numbers"></a>バージョン番号

.NET ライブラリには、バージョンを指定する多数の方法があります。 最も重要なバージョンを以下に示します。

### <a name="nuget-package-version"></a>NuGet パッケージ バージョン

[NuGet パッケージ バージョン](/nuget/reference/package-versioning)は NuGet.org、Visual Studio NuGet パッケージ マネージャーに表示され、パッケージの使用時にソース コードに追加されます。 NuGet パッケージ バージョンは、一般的にユーザーに表示されるバージョンであり、使用しているライブラリのバージョンについてユーザーが話すときは、このバージョンを参照します。 NuGet パッケージ バージョンは NuGet によって使用され、実行時の動作に影響を及ぼしません。

```xml
<PackageVersion>1.0.0-alpha1</PackageVersion>
```

NuGet パッケージ バージョンと組み合わせる NuGet パッケージの識別子は、NuGet でパッケージを識別するために使用されます。 たとえば、`Newtonsoft.Json` + `11.0.2` です。 サフィックスを持つパッケージはプレリリース パッケージであり、テストに最適な特別な動作を備えています。 詳細については、「[プレリリース パッケージ](./nuget.md#pre-release-packages)」をご覧ください。

NuGet パッケージ バージョンは、最も開発者の目に留まるバージョンなので、[セマンティック バージョニング (SemVer)](https://semver.org/) を使用してこれを更新することをお勧めします。 SemVer はリリース間の変更の重大度を示し、使用するバージョンを選択するときに、開発者は十分な情報を基に判断できます。 たとえば、`1.0` から `2.0` への移行は、潜在的に互換性に影響する変更があることを示します。

ご利用の NuGet パッケージのバージョンに [SemVer 2.0.0](https://semver.org/) を使用することを ✔️ 検討してください。

✔️ 実行 NuGet パッケージ バージョンは一般的にユーザーに表示されるバージョン番号なので、パブリック ドキュメントにはこれを使用する。

✔️ 実行 非安定版パッケージをリリースする場合は、プレリリース サフィックスを含める。

> ユーザーは、プレリリース パッケージを取得することを選ぶ必要があります。パッケージが完成版ではないことがわかります。

### <a name="assembly-version"></a>アセンブリ バージョン

アセンブリ バージョンは、どのバージョンのアセンブリを読み込むかを選択するために CLR が実行時に使用するバージョンです。 バージョン管理を使用したアセンブリの選択は、厳密な名前を持つアセンブリにしか適用されません。

```xml
<AssemblyVersion>1.0.0.0</AssemblyVersion>
```

Windows .NET Framework の CLR では、厳密な名前のアセンブリを読み込むには、完全一致が求められます。 たとえば、`Libary1, Version=1.0.0.0` が `Newtonsoft.Json, Version=11.0.0.0` への参照を使ってコンパイルされたとします。 .NET Framework は、該当の正確なバージョンである `11.0.0.0` のみを読み込みます。 実行時に別のバージョンを読み込むには、バインド リダイレクトが .NET アプリケーションの構成ファイルに追加される必要があります。

アセンブリ バージョンと組み合わせた厳密な名前によって、[厳密なアセンブリ バージョンの読み込み](../assembly/versioning.md)が可能になります。 ライブラリに厳密な名前を付与すると多くのメリットがありますが、しばしば、アセンブリが見つからないという実行時例外に陥り、修正のために`app.config`/`web.config` の[バインド リダイレクトを要求する](../../framework/configure-apps/redirect-assembly-versions.md)必要があります。 .NET Core アセンブリの読み込みが厳密ではなくなり、.NET Core CLR は、実行時により新しいバージョンでアセンブリを自動的に読み込みます。

✔️ 検討 AssemblyVersion にメジャー バージョンのみを含める。

> 例: Library 1.0 と Library 1.0.1 は両方とも `1.0.0.0` の AssemblyVersion を備えているのに対して、Library 2.0 は `2.0.0.0` のAssemblyVersion を備えています。 アセンブリ バージョンの変更の頻度が低い場合は、バインド リダイレクトが減少します。

✔️ 検討 AssemblyVersion のメジャー バージョン番号と NuGet パッケージ バージョンの同期を保持する。

> 例外メッセージのアセンブリ名とアセンブリ修飾型名など、AssemblyVersion はユーザーに表示される一部の情報メッセージに含まれています。 バージョン間の関係を維持することで、使用しているバージョンに関する詳細情報を開発者に提供できます。

❌ 禁止 固定の AssemblyVersion を使用する。

> 固定の AssemblyVersion ではバインド リダイレクトの必要性を回避できるものの、単一のアセンブリ バージョンしかグローバル アセンブリ キャッシュ (GAC) にインストールできないことを意味します。 また、別のアプリケーションが、互換性に影響する変更によって GAC アセンブリを更新した場合、GAC のアセンブリを参照するアプリケーションは中断されます。

### <a name="assembly-file-version"></a>アセンブリ ファイル バージョン

アセンブリ ファイル バージョンでは、Windows でファイル バージョンを表示するために使用され、実行時の動作に影響を及ぼしません。 このバージョンの設定は省略可能です。 これは、Windows エクスプローラーの [ファイルのプロパティ] ダイアログに表示できます。

```xml
<FileVersion>11.0.2.21924</FileVersion>
```

![Windows エクスプローラー](./media/versioning/win-properties.png "Windows エクスプローラー")

✔️ 検討 AssemblyFileVersion リビジョンとして、継続的インテグレーションのビルド番号を含める。

> たとえば、プロジェクトのバージョン 1.0.0 を構築しており、継続的インテグレーションのビルド番号が 99 だとすると、お使いの AssemblyFileVersion は 1.0.0.99 になります。

✔️ 実行 ファイル バージョンに形式 `Major.Minor.Build.Revision` を使用する。

> ファイル バージョンは .NET では使用されませんが、[Windows ではファイル バージョン](/windows/desktop/menurc/versioninfo-resource)が `Major.Minor.Build.Revision` 形式である必要があります。 バージョンがこの形式に従っていない場合は、警告メッセージが表示されます。

### <a name="assembly-informational-version"></a>アセンブリの情報バージョン

アセンブリの情報バージョンは、追加のバージョン情報を記録するために使用され、実行時の動作に影響を及ぼしません。 このバージョンの設定は省略可能です。 ソース リンクを使用している場合、NuGet パッケージ バージョンとソース管理バージョンを使って、このバージョンがビルド上に設定されます。 たとえば、`1.0.0-beta1+204ff0a` には、アセンブリの構成元となったソース コードのコミット ハッシュが含まれます。 詳細については、「[ソース リンク](./sourcelink.md)」を参照してください。

```xml
<AssemblyInformationalVersion>The quick brown fox jumped over the lazy dog.</AssemblyInformationalVersion>
```

> [!NOTE]
> このバージョンが `Major.Minor.Build.Revision` の形式に従っていない場合、古いバージョンの Visual Studio によってビルドの警告メッセージが表示されます。 警告は、無視してかまいません。

❌ 回避 アセンブリの情報バージョンを自分で設定する。

> NuGet とソース管理メタデータを含むバージョンを SourceLink が自動生成することを許可します。

>[!div class="step-by-step"]
>[前へ](publish-nuget-package.md)
>[次へ](breaking-changes.md)
