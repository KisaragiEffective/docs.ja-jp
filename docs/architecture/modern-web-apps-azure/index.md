---
title: ASP.NET Core および Azure での最新の Web アプリケーションの設計
description: ASP.NET Core と Azure を使用したモノリシックな Web アプリケーションの構築に関するエンドツーエンドのガイダンスを提供するガイドです。
author: ardalis
ms.author: wiwagn
ms.date: 12/4/2019
ms.openlocfilehash: c19e5e90cfb96463f744cfb064abe72ee5db2e9f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77449330"
---
# <a name="architect-modern-web-applications-with-aspnet-core-and-azure"></a>ASP.NET Core および Azure での最新の Web アプリケーションの設計

![最新の Web アプリケーションの設計ガイドのブック カバー画像。](./media/index/web-application-guide-cover-image.png)

**エディション v3.1** - ASP.NET Core 3.1 に更新

発行者

Microsoft 開発部門、.NET および Visual Studio 製品チーム

A division of Microsoft Corporation

One Microsoft Way

Redmond, Washington 98052-6399

Copyright © 2020 by Microsoft Corporation

All rights reserved. 本書のいかなる部分も、書面による発行者の許可なしに、いかなる形式または方法によっても、複製または伝送することを禁じます。

本書は "現状有姿" で提供され、著者の見解と意見を表しています。 URL および他の参照されているインターネットの Web サイトをはじめ、本書に記載されている見解、意見、および情報は、通知なく変更されることがあります。

ここに記載したいくつかの例は、説明のためだけに提供された架空のものです。 実在のものとの関連性または関係性は一切ありません。

[https://www.microsoft.com](https://www.microsoft.com ) の "商標" Web ページに記載されている Microsoft および商標は、Microsoft グループの商標です。

Mac および macOS は Apple Inc. の商標です。

Docker のクジラのロゴは Docker, Inc. の登録商標です。許可を得て使用しています。

その他のすべてのマークおよびロゴは、該当する各社が所有しています。

作成者:

> **Steve "ardalis" Smith** - ソフトウェア アーキテクトおよびトレーナー - [Ardalis.com](https://ardalis.com)

編集者:

> **Maira Wenzel**

## <a name="introduction"></a>はじめに

.NET Core と ASP.NET Core は、従来の .NET 開発よりも優れたいくつかの利点を提供します。 次の一部またはすべてがアプリケーションの成功に重要な場合は、サーバー アプリケーションに .NET Core を使用してください。

- クロス プラットフォーム サポート。

- マイクロサービスの使用。

- Docker コンテナーの使用。

- 高パフォーマンスとスケーラビリティの要件。

- 同じサーバー上のアプリケーション別の .NET バージョンのサイド バイ サイドのバージョン管理。

従来の .NET アプリケーションはこれらの要件の多くをサポートできますが、ASP.NET Core と .NET Core は、上記のシナリオに対する改善されたサポートを提供するように最適化されています。

ますます多くの組織が、Microsoft Azure などのサービスを使用してクラウドで Web アプリケーションをホストすることを選択しています。 アプリケーションまたは組織にとって次のことが重要な場合は、クラウドでのアプリケーションのホストを検討してください。

- データ センターのコストに対する投資の削減 (ハードウェア、ソフトウェア、スペース、ユーティリティ、サーバー管理など)。

- 柔軟な料金 (アイドル状態の容量ではなく、使用量に基づく支払い)。

- 極限の信頼性。

- 強化されたアプリのモビリティ。アプリが展開されている場所と方法を簡単に変更できること。

- 柔軟な容量。実際のニーズに基づいたスケール アップまたはスケール ダウン。

Azure でホストされる ASP.NET Core での Web アプリケーションを構築すると、従来型の代替手段よりも優れた多くの競争上の優位性が得られます。 ASP.NET Core は、最新の Web アプリケーションの開発作業とクラウド ホスティングのシナリオ用に最適化されています。 このガイドでは、これらの機能を最適に活用するために ASP.NET Core アプリケーションを設計する方法を学習します。

## <a name="purpose"></a>目的

このガイドでは、ASP.NET Core と Azure を使った "*モノリシック*" Web アプリケーションの構築に関するエンド ツー エンドのガイダンスを提供します。 このコンテキストにおける "モノリシック" とは、相互作用するサービスとアプリケーションのコレクションとしてではなく、単一のユニットとしてこれらのアプリケーションをデプロイするという意味です。

このガイドは、エンタープライズ アプリケーションをホストするために Docker、マイクロサービス、およびコンテナーのデプロイに注目した、[" _.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ_"](../microservices/index.md) を補完するものです。

### <a name="net-microservices-architecture-for-containerized-net-applications"></a>.NET マイクロサービス。 コンテナー化された .NET アプリケーションのアーキテクチャ

- **電子ブック**  
  <https://aka.ms/MicroservicesEbook>
- **サンプル アプリケーション**  
  <https://aka.ms/microservicesarchitecture>

## <a name="who-should-use-this-guide"></a>対象読者

このガイドの対象ユーザーは、主にクラウドでの Microsoft テクノロジとサービスを使用した最新の Web アプリケーションの構築に関心がある開発者、開発担当者、およびアーキテクトです。

2 番目の対象ユーザーは、ASP.NET や Azure を既に使い慣れていて、新規または既存のプロジェクトに対して ASP.NET Core にアップグレードすることに意味があるかどうかに関する情報を探している技術的な意思決定者です。

## <a name="how-you-can-use-this-guide"></a>このガイドを使用する方法

このガイドは、最新の .NET テクノロジと Windows Azure を使用した Web アプリケーションの構築に焦点を当てた比較的小さなドキュメントに凝縮されています。 そのため、このようなアプリケーションと、技術的な考慮事項を理解するための基盤を提供するものとして、全体を読むことができます。 ガイドは、サンプル アプリケーションと共に、最初に参照するものとして使用できます。 関連するサンプル アプリケーションをテンプレートとして使用するか、アプリケーションのコンポーネント部分を整理する方法を参照してください。 独自のアプリケーションでこれらの選択肢を比較検討する場合、ガイドの基本原則とアーキテクチャのカバレッジ、テクノロジのオプション、および決定時の考慮事項を参照してください。

これらの考慮事項と営業案件の共通理解を確保するために、チームにこのガイドを自由に転送してください。 用語の共通セットと基本原則を理解して全員が作業すると、アーキテクチャのパターンと実践方法を一貫して適用できるようになります。

## <a name="references"></a>関連項目

- **サーバー アプリ用 .NET Core と .NET Framework の選択**  
  [https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server](../../standard/choosing-core-framework-server.md)

>[!div class="step-by-step"]
>[次へ](modern-web-applications-characteristics.md)
