---
ms.openlocfilehash: 99153bb9cf3287951a1e52e716c799ee64eb82ad
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78950988"
---
# <a name="labels-and-projects-roadmap"></a>ラベルとプロジェクトのロードマップ

.NET docs チームは、[GitHub ラベル](https://github.com/dotnet/docs/labels) を広範に使用して、作業を整理します。 ラベルの組み合わせに基づいてフィルター処理することで、[.NET docs Web サイト](https://docs.microsoft.com/dotnet)上の目的のセクションにすばやく焦点を合わせることができます。

また、[GitHub プロジェクト](https://github.com/dotnet/docs/projects)を使用して、スプリントやその他の目標志向のエピックを整理します。

このロードマップでは、これらの組織ツールの使用方法について説明するほか、目的の領域を見つけるために使用できる便利なフィルターへのリンクも用意されています。

## <a name="labels"></a>ラベル

これが [dotnet/docs](https://github.com/dotnet/docs) に投稿する初めての経験である場合は、[up-for-grabs](https://github.com/dotnet/docs/labels/up-for-grabs) イシューから開始してください。 これらは、より範囲が絞られたイシューです。 これは、最初の投稿を作成するのに最適な方法です。 up-for-grabs ビューから、領域や優先度に基づいてイシューをさらにフィルター処理できます。 最初の投稿を小さくする必要がある場合は、[good-first-issue](https://github.com/dotnet/docs/labels/good-first-issue) を持つ初心者向けの適切なイシューが特定されています。

ラベルを使用して、さまざまな方法でイシューを分類します。

- [.NET ガイド](#find-a-single-net-guide) と [ガイドのセクション](#search-one-section-of-a-guide)。
- [ターゲット リリース](#releases)
- [優先順位](#priority)

各セット (ガイド、リリース、優先順位) からのラベルを組み合わせて、範囲を絞って作業対象のイシューを見つけることができます。

### <a name="find-a-single-net-guide"></a>単一の .NET ガイドを検索する

アーキテクチャ電子書籍ごとに、および .NET ガイドごとにラベルを使用します。

![:book: guide、明るい緑色の背景](./images/guide.png "アーキテクチャ ガイド ラベルのプレフィックス")

アーキテクチャの電子書籍には、`:book: guide` プレフィックスが付いていて、背景は明るい緑色になっています。 これらは、推奨アーキテクチャをカバーする長い形式の領域です。 .NET アーキテクチャ ガイドごとにフィルター処理された現在のイシューを以下に示します。

- [ASP.NET Core Web アプリ](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20ASP.NET%20Core%20web%20apps)
- [Blazor](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Blazor)
- [クラウド ネイティブ](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Cloud%20Native)
- [Docker ライフサイクル](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Docker%20lifecycle)
- [gRPC](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20gRPC)
- [w/ Windows コンテナーの最新化](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Modernizing%20w%2F%20Windows%20containers)
- [.NET マイクロサービス](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20.NET%20Microservices)
- [サーバーレス アプリ](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Serverless%20apps)

このラベル スタイルは、[フレームワーク デザインのガイドライン](https://github.com/dotnet/docs/labels/%3Abook%3A%20guide%20-%20Framework%20Design%20Guidelines)にも適用されます。 この領域には同じラベル デザインがありますが、コミュニティ PR はお勧めしません。 これは許可を得て転載された資料です。編集しないでください。

![:books:Area、濃い青色の背景](./images/area.png ".NET ガイド領域ラベルのプレフィックス")

各 .NET ガイドには `:books: Area` プレフィックスが付いていて、背景は濃い青色になっています。 .NET ガイドごとにフィルター処理された現在のイシューを以下に示します。

- [API リファレンス](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20API%20Reference)
- [Azure .NET SDK](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20Azure%20.NET%20SDk)
- [C# のガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20C%23%20Guide)
- [デスクトップ ガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20Desktop%20Guide)
- [F# のガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20F%23%20Guide)
- [ML.NET ガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20ML.NET%20Guide)
- [.NET アーキテクチャ ガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20.NET%20Architecture%20Guide) - 削除可能です
- [.NET Core のガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20.NET%20Core%20Guide)
- [.NET for Apache Spark ガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20.NET%20for%20Apache%20Spark%20Guide)
- [.NET Framework ガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20.NET%20Framework%20Guide)
- [.NET のガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20.NET%20Guide)
- [Roslyn API リファレンス](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20Roslyn%20API%20Reference) - 削除可能です。
- [Visual Basic のガイド](https://github.com/dotnet/docs/labels/%3Abooks%3A%20Area%20-%20Visual%20Basic%20Guide)

#### <a name="search-one-section-of-a-guide"></a>ガイドの 1 つのセクションを検索する

![:card_file_box:Area、濃さが中程度の青色の背景](./images/technology.png ".NET ガイド サブ区分ラベルのプレフィックス")

.NET ガイドは大きいので、これらのラベルでは、ガイドのセクションによってスコープがさらに制限されます。 各 .NET ガイドのサブ領域には `:card_file_box: Technology` プレフィックスが付いていて、背景は中程度の濃さの青色になっています。 これらのラベルの多くは複数のガイドに適用されますが、他は 1 つのガイドにのみ含まれます。 領域をフィルター処理したら、以下のラベルのいずれかを追加して、イシューの範囲をさらに制限します。

- AppCompat
- 非同期タスク
- C# の詳細な概念
- C# コンパイラ
- C# の基礎
- C# はじめに
- C# 言語リファレンス
- C# Null 安全性
- C# 新機能
- CLI
- データ アクセス
- Docker
- インストーラー
- LINQ
- NCL
- .NET Standard
- Roslyn API
- セキュリティ
- WCF
- WF
- WIF
- WinForms
- WPF

### <a name="releases"></a>リリース

![:checkered_flag:Release:、濃い黄色](./images/release.png "リリース ラベルのプレフィックス")

特定のリリースにタグが付けられたイシューには、`:checkered_flag: Release:` プレフィックスが付いていて、背景は濃い黄色になっています。

- .NET Core 2.2
- .NET Core 3.0

### <a name="priority"></a>優先度

優先順位ラベルはすべて、`P` の後に 1 桁の数字が続きます。 数値が小さいほど優先度が高くなります。

- P0
- P1
- P2

### <a name="what-about-the-other-labels"></a>他のラベルについてはどうでしょうか?

イシューのさまざまな分類を管理するためにコンテンツ チームが使用するラベルが他にも多数あります。 コンテンツ チームに所属していない場合、そのような他のラベルは無視してかまいません。

## <a name="projects"></a>プロジェクト

共同作成者は、[.NET コミュニティ コラボレーター用のプロジェクト](https://github.com/dotnet/docs/projects/35)を確認する必要があります。 メンテナンス、ドキュメントの更新、および新しいコンテンツ タスク用のさまざまな列が作成されています。 これにより、目的のタスクを検索することができます (プロジェクト ビューは、前述のラベルを使用してフィルター処理できます)。

また、プロジェクトは、他に次の 2 つの方法でも使用されます。

- Month YYYY プロジェクト タイプ: これらは、各月の作業計画のスクラム ボードです。
- 長期エピック: これらは、作業が数か月にわたって行われる場合、目標に向けてタスクを整理するために使用されます。
