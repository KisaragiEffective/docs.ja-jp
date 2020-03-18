---
title: .NET Core ランタイムと SDK をバージョン管理する方法
description: この記事では、.NET Core SDK と ランタイムをバージョン管理する方法について説明します (セマンティック バージョニングと似ています)。
ms.date: 07/26/2018
ms.openlocfilehash: c85a2112b439768068663688947960ac814de824
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75777314"
---
# <a name="overview-of-how-net-core-is-versioned"></a>.NET Core をバージョン管理する方法の概要

.NET Core とは .NET Core ランタイムと .NET Core SDK を示しており、これにはアプリケーションを開発するのに必要なツールが含まれています。 .NET Core SDK は、前のバージョンの .NET Core ランタイムをどれでも使用できるように設計されています。 この記事では、ランタイムおよび SDK のバージョン戦略について説明します。 .NET Standard のバージョン番号については、[.NET Standard](../../standard/net-standard.md#net-implementation-support) に関する記事を参照してください。

.NET Core ランタイムと .NET Core SDK ではそれぞれ異なるレートで新しい機能が追加されます。一般に、運用環境で使用しているランタイムが .NET Core ランタイムによって変更されるよりも速く、.NET Core SDK でのツールの更新が行われます。

## <a name="versioning-details"></a>バージョン管理の詳細

".NET Core 2.1" は、.NET Core ランタイムのバージョン番号を示しています。 .NET Core ランタイムでは、バージョン管理に対してメジャー/マイナー/パッチ アプローチが使用されており、これは[セマンティック バージョニング](#semantic-versioning)に従っています。

.NET Core SDK の場合は、セマンティック バージョニングに従っていません。 .NET Core SDK の方が速くリリースされます。そのバージョンでは、揃えられたランタイムと SDK 独自のマイナー/パッチ リリースの両方が示される必要があります。 .NET Core SDK バージョンの 1 番目と 2 番目の位置は、一緒にリリースされる .NET Core ランタイムと揃えられます。 SDK の各バージョンでは、このランタイムまたはより低いバージョンに対するアプリケーションを作成できます。

SDK バージョン番号の 3 番目の位置には、マイナー番号とパッチ番号の両方が示されます。 マイナー バージョンには 100 が乗算されます。 マイナー バージョン 1、パッチ バージョン 2 の場合は、102 と表現されます。 最後の 2 桁はパッチの番号を示しています。 たとえば、.NET Core 2.2 のリリースの場合、次の表のようなリリースが作成される可能性があります。

| Change                | .NET Core ランタイム | .NET Core SDK (\*) |
|-----------------------|-------------------|-------------------|
| 最初のリリース       | 2.2.0             | 2.2.100           |
| SDK パッチ             | 2.2.0             | 2.2.101           |
| ランタイムおよび SDK パッチ | 2.2.1             | 2.2.102           |
| SDK 機能変更    | 2.2.1             | 2.2.200           |

(\*) 上記の表では、2.2 .NET Core Runtime が例として使用されています。 .NET Core 2.1 に対する最初の SDK が 2.1.300 であることが、履歴アーティファクトで示されていたからです。 詳細については、「[.NET Core のバージョンの選択](selection.md)」を参照してください。

メモ:

- SDK において、ランタイムの機能更新プログラムの前に 10 の機能更新プログラムがある場合、バージョン番号は 1000 シリーズに展開され、番号は 2.2.1000 のようになります。これは 2.2.900 に続く機能リリースを示しています。 このような状況が発生することは想定されていません。
- 機能リリースなしで 99 のパッチ リリースは、発生しません。 リリースがこの数に近づくと、機能リリースが強制的に適用されます。

詳細については、[dotnet/designs](https://github.com/dotnet/designs/pull/29) リポジトリにある初期の提案を参照してください。

## <a name="semantic-versioning"></a>セマンティック バージョニング

.NET Core *ランタイム*は、[セマンティック バージョニング (SemVer)](https://semver.org/) にほぼ準拠しています。`MAJOR.MINOR.PATCH` バージョン管理の使用が採用され、バージョン番号のさまざまな部分を使用して変更の程度と種類が記述されています。

```
MAJOR.MINOR.PATCH[-PRERELEASE-BUILDNUMBER]
```

省略可能な `PRERELEASE` と `BUILDNUMBER` の部分はサポートされるリリースに含まれることはなく、ソース ターゲットからローカルでビルドされた夜間のビルドおよびサポートされていないプレビュー リリースにのみ存在します。

### <a name="understand-runtime-version-number-changes"></a>ランタイム バージョン番号の変更を理解する

`MAJOR` は次のときに増分されます。

- 製品に重大な変更が加えられた。または製品の方向性が新しくなった。
- 互換性に影響する変更が行われた。 互換性に影響する変更の受け入れには大きな制約があります。
- 古いバージョンがサポート対象から除外された。
- 既存の依存関係の新しい `MAJOR` バージョンが採用された。

`MINOR` は次のときに増分されます。

- パブリック API アクセス領域が追加された。
- 新しい動作が追加された。
- 既存の依存関係の新しい `MINOR` バージョンが採用された。
- 新しい依存関係が導入された。

`PATCH` は次のときに増分されます。

- バグの修正が行われた。
- より新しいプラットフォームのサポートが追加された。
- 既存の依存関係の新しい `PATCH` バージョンが採用された。
- 前のケースのいずれかに一致しない他の変更。

複数の変更が存在する場合、個々の変更によって影響を受ける最高位置の要素が増分され、それに続く要素はゼロにリセットされます。 たとえば、`MAJOR` が増分され、`MINOR` と `PATCH` はゼロにリセットされます。 `MINOR` が増分されるときには、`PATCH` はゼロにリセットされますが、`MAJOR` は変更されません。

## <a name="version-numbers-in-file-names"></a>ファイル名に含まれるバージョン番号

.NET Core でダウンロードされるファイルには、バージョンが伴います (たとえば、`dotnet-sdk-2.1.300-win10-x64.exe` のように)。

### <a name="preview-versions"></a>プレビュー バージョン

プレビュー バージョンには、`-preview[number]-([build]|"final")` がバージョンに追加されます。 たとえば、「 `2.0.0-preview1-final` 」のように入力します。

### <a name="servicing-versions"></a>サービスのバージョン

リリースされると、通常はリリース ブランチが毎日のビルドの生成を停止し、代わりにサービスのビルドの生成を開始します。 サービスのバージョンには、`-servicing-[number]` がバージョンに追加されます。 たとえば、「 `2.0.1-servicing-006924` 」のように入力します。

## <a name="relationship-to-net-standard-versions"></a>.NET Standard との関係

.NET Standard は、.NET 参照アセンブリで構成されています。 各プラットフォームに固有の複数の実装があります。 参照アセンブリには、指定された .NET Standard バージョンの一部である .NET API の定義が含まれています。 各実装では、特定のプラットフォームに対する .NET Standard コントラクトが満たされます。 .NET Standard の詳細については、.NET ガイド内の [.NET Standard](../../standard/net-standard.md) に関する記事を参照してください。

.NET Standard 参照アセンブリでは、`MAJOR.MINOR` バージョン管理スキームが使用されます。 .NET Standard の場合、`PATCH` レベルは有用ではありません。 .NET Standard では、API 仕様しか公開されず (実装は対象外)、定義上、API に対する変更はいずれも機能セット内の変更を示すものであり、`MINOR` バージョンとなるためです。

各プラットフォーム上の実装の場合は、通常、プラットフォーム リリースの一部として更新されるので、そのプラットフォーム上で .NET Standard を使用しているプログラマには明らかにされません。

.NET Core の各バージョンでは、.NET Standard のバージョンが実装されます。 .NET Standard のバージョンを実装すると、以前のバージョンの .NET Standard のサポートが含まれます。 .NET Standard と .NET Core のバージョンは独立しています。 .NET Core 2.0 によって .NET Standard 2.0 が実装されるのは偶然です。 .NET core 2.1 では .NET Standard 2.0 も実装されます。 .NET Core では、.NET Standard の将来のバージョンについても、リリースされた場合はサポートされます。

| .NET Core | .NET Standard |
|-----------|---------------|
| 1.0       | 最大 1.6     |
| 2.0       | 最大 2.0     |
| 2.1       | 最大 2.0     |
| 2.2       | 最大 2.0     |
| 3.0       | 最大 2.1     |

## <a name="see-also"></a>参照

- [ターゲット フレームワーク](../../standard/frameworks.md)
- [.NET Core の配布パッケージ](../build/distribution-packaging.md)
- [.NET Core サポート ライフサイクルのファクト シート](https://dotnet.microsoft.com/platform/support/policy)
- [.NET core 2 + バージョン バインディング](https://github.com/dotnet/designs/issues/3)
- [.NET Core の Docker イメージ](https://hub.docker.com/_/microsoft-dotnet-core/)
