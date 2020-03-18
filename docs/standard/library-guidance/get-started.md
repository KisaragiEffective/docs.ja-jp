---
title: 高品質な .NET ライブラリの作成の概要
description: .NET ライブラリの構築を始めましょう。
ms.date: 10/02/2018
ms.openlocfilehash: 10576219d8470a171ad0f1f347196999b2a2ee03
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75706492"
---
# <a name="get-started"></a>はじめに

## <a name="cross-platform-targeting"></a>[クロス プラットフォーム ターゲット](./cross-platform-targeting.md)

.NET Standard およびマルチ ターゲットを使用してクロスプラットフォーム ライブラリを作成する方法について。 .NET はさまざまな場所で実行されるため、優れた .NET ライブラリは、できるだけ多くのプラットフォームと開発者をサポートするために存続する必要があります。

## <a name="strong-naming"></a>[厳密な名前付け](./strong-naming.md)

厳密な名前付けとその長所と短所について説明します。 .NET ライブラリの厳密な名前付けにより多くの開発者が使用できるようになるため、これは推奨されるベスト プラクティスです。

## <a name="nuget-and-open-source-libraries"></a>[NuGet およびオープン ソース ライブラリ](./nuget.md)

NuGet.org で一般公開されているすべてのパッケージに推奨されるメタデータを含むオープン ソースの .NET ライブラリで NuGet パッケージを作成する最善の方法です。

### <a name="dependencies"></a>[の依存関係](./dependencies.md)

NuGet により、.NET ライブラリの構築時に既存のパッケージが使いやすくなります。 NuGet の依存関係の一般的な競合原因と、その回避方法について説明します。

### <a name="source-link"></a>[ソース リンク](./sourcelink.md)

ソース リンクは、.NET ライブラリのユーザーがデバッグ中にそのソース コードにステップ インできるようにする優れたツールです。 この記事では、ソース リンクとは何かと、それを使用する理由の概要を示します。

### <a name="publishing"></a>[発行](./publish-nuget-package.md)

NuGet.org は最も広く知られ、使用されているリポジトリで、NuGet パッケージを発行する場所は数多くあります。 使用可能な NuGet パッケージ リポジトリのそれぞれの違いと、.NET ライブラリを発行するためのセキュリティのベスト プラクティスについて説明します。

## <a name="versioning"></a>[バージョン管理](./versioning.md)

優れた .NET ライブラリは時間の経過と共に進化し、機能が追加され、バグが修正され、後のリリースになるほどパフォーマンスが向上します。 各バージョン番号と、破壊的変更を開発者に伝える方法について説明します。

### <a name="breaking-changes"></a>[重大な変更](./breaking-changes.md)

.NET ライブラリでは既存のユーザーに向けた安定性と将来に向けたイノベーションとの間でバランスを取ることが重要です。 さまざまな種類の破壊的変更と、下位互換性を維持しながら新しい機能を追加するための戦略について説明します。

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](cross-platform-targeting.md)
