---
title: .NET Core での自己完結型アプリのデプロイにおけるランタイムのロール フォワード
description: 自己完結型展開における dotnet publish の変更について説明します。
author: KathleenDollard
ms.date: 05/31/2018
ms.openlocfilehash: 22385c7b5d2bf87755fd51cd6268d21fe3431c74
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75740782"
---
# <a name="self-contained-deployment-runtime-roll-forward"></a>自己完結型展開ランタイムのロール フォワード

.NET Core [自己完結型のアプリケーション展開](index.md)には、.NET Core ライブラリと .NET Core ランタイムの両方が含まれます。 .NET Core 2.1 SDK (バージョン 2.1.300) 以降、自己完結型のアプリケーションのデプロイ[では、お使いのコンピューター上にある最新の修正プログラムのランタイムを発行するようになりました](https://github.com/dotnet/designs/pull/36)。 既定では、自己完結型展開のための [`dotnet publish`](../tools/dotnet-publish.md) では、発行マシン上に SDK の一部としてインストールされている最新バージョンが選択されます。 これにより、展開したアプリケーションを `publish` 時に利用可能なセキュリティ修正プログラム (およびその他の修正) と共に実行できます。 新しい修正プログラムを取得するには、アプリケーションを再発行する必要があります。 `-r <RID>` コマンドで `dotnet publish` を指定するか、プロジェクト ファイル (csproj / vbproj) 内またはコマンド ライン上で [ランタイム識別子 (RID)](../rid-catalog.md) を指定すると、自己完結型のアプリケーションが作成されます。

## <a name="patch-version-roll-forward-overview"></a>修正プログラムのバージョンのロール フォワードの概要

[`restore`](../tools/dotnet-restore.md)、[`build`](../tools/dotnet-build.md) および [`publish`](../tools/dotnet-publish.md) は、個別に実行できる `dotnet` コマンドです。 ランタイムの選択は、`restore` や `publish` ではなく、`build` 操作の一部です。 `publish` を呼び出す場合、修正プログラムの最新バージョンが選択されます。 `publish` 引数で `--no-restore` を呼び出した場合、以前の `restore` はポリシーを発行する新しい自己完結型のアプリケーションで実行されていない可能性があるため、必要な修正プログラムのバージョンを取得しない可能性があります。 この場合、次のようなテキストでビルド エラーが生成されます。

  "プロジェクトは Microsoft.NETCore.App バージョン 2.0.0 を使用して復元されましたが、代わりに最新の設定、バージョン 2.0.6 が使用されます。 この問題を解決するには、復元およびこれ以降の操作 (ビルドや発行など) で同じ設定を使用していることをご確認ください。 通常、この問題は、RuntimeIdentifier プロパティが復元時ではなく、ビルドや発行時に設定された場合に発生します。"

> [!NOTE]
> `restore` と `build` は、`publish` などの別のコマンドの一部として暗黙的に実行できます。 別のコマンドの一部として暗黙的に実行すると、追加のコンテキストと共に提供されるため、適切な成果物が生成されます。 ランタイム (例: `publish`) で `dotnet publish -r linux-x64` を行う場合、暗黙的な `restore` では linux-x64 ランタイムのパッケージを復元します。 明示的に `restore` を呼び出す場合、そのコンテキストが含まれないため、既定でランタイム パッケージは復元されません。

## <a name="how-to-avoid-restore-during-publish"></a>発行時に復元を回避する方法

`restore` 操作の一部として `publish` を実行することは、お客様のシナリオにとって望ましくない場合があります。 自己完結型のアプリケーションを作成しているときの `restore` 時に `publish` を回避するには、次の操作を行います。

- `RuntimeIdentifiers` プロパティに、発行されるすべての [RID](../rid-catalog.md) をセミコロンで区切って設定します。
- `TargetLatestRuntimePatch` プロパティを `true` に設定します。

## <a name="no-restore-argument-with-dotnet-publish-options"></a>dotnet publish オプションでの no-restore 引数

同じプロジェクト ファイルで自己完結型のアプリケーションと[フレームワークに依存するアプリケーション](index.md)の両方を作成する場合、`--no-restore` で `dotnet publish` 引数を使用する場合は、次のいずれかを選択します。

1. フレームワークに依存する動作を優先します。 アプリケーションがフレームワークに依存する場合、これが既定の動作になります。 アプリケーションが自己完結型で、修正プログラムが適用されていない 2.1.0 のローカル ランタイムを使用できる場合、プロジェクト ファイルで `TargetLatestRuntimePatch` を `false` に設定します。

2. 自己完結型の動作を優先します。 アプリケーションが自己完結型の場合、これが既定の動作になります。 アプリケーションがフレームワークに依存し、最新の修正プログラムがインストールされている必要がある場合、プロジェクト ファイルで `TargetLatestRuntimePatch` を `true` に設定します。

3. プロジェクト ファイルで `RuntimeFrameworkVersion` に特定の修正プログラムのバージョンを設定して、ランタイム フレームワークのバージョンの明示的なコントロールを取得します。
