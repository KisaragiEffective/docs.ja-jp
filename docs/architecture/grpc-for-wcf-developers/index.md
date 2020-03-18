---
title: WCF 開発者向け ASP.NET Core gRPC - WCF 開発者向け gRPC
description: WCF 開発者向け ASP.NET Core 3.0 で gRPC サービスを構築する方法の概要
ms.date: 09/02/2019
ms.openlocfilehash: 40307124c521659a00339884bacf48881fa3e048
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "77543236"
---
# <a name="aspnet-core-grpc-for-wcf-developers"></a>WCF 開発者向け ASP.NET Core gRPC

![カバーの画像](./media/cover.png)

発行者

Microsoft 開発部門、.NET および Visual Studio 製品チーム

A division of Microsoft Corporation

One Microsoft Way

Redmond, Washington 98052-6399

Copyright © 2019 by Microsoft Corporation

All rights reserved. 本書のいかなる部分も、書面による発行者の許可なしに、いかなる形式または方法によっても、複製または伝送することを禁じます。

本書は "現状有姿" で提供され、著者の見解と意見を表しています。 URL および他の参照されているインターネットの Web サイトをはじめ、本書に記載されている見解、意見、および情報は、通知なく変更されることがあります。

ここに記載したいくつかの例は、説明のためだけに提供された架空のものです。 実在のものとの関連性または関係性は一切ありません。

[https://www.microsoft.com](https://www.microsoft.com ) の "商標" Web ページに記載されている Microsoft および商標は、Microsoft グループの商標です。

Docker のクジラのロゴは Docker, Inc. の登録商標です。許可を得て使用しています。

その他のすべてのマークおよびロゴは、該当する各社が所有しています。

作成者:

> **Mark Rendle** - 技術部門最高責任者 - [Visual ReCode](https://visualrecode.com)
>
> **Miranda Steiner** - 技術文書作成者

エディター:

> **Maira Wenzel** - シニア コンテンツ開発者 - Microsoft

## <a name="introduction"></a>はじめに

gRPC とは、ネットワーク接続されたサービスと分散型アプリケーションを構築するための最新のフレームワークです。 SOAP のクロスプラットフォーム相互運用性と組み合わせた、Windows Communication Foundation (WCF) NetTCP バインドのパフォーマンスを想像してください。 gRPC は、アプリケーションとサービス間のハイ パフォーマンスで低帯域幅の通信を実現できるように、HTTP/2 および Protobuf メッセージエンコード プロトコルに基づいて構築されています。 .NET、Java、Python、Node.js、Go、C++ など、最も一般的なプログラミング言語およびプラットフォームにわたって、サーバー コードおよびクライアント コードの生成がサポートされています。 ASP.NET Core 3.0 での gRPC 用の最上級のサポートと共に、.NET 4.x 用の既存の gRPC ツールとライブラリを利用できることから、.NET Core を組織で採用することを検討している開発チームにとって、gRPC は WCF に替わる優れた方法です。

## <a name="who-should-use-this-guide"></a>対象読者

このガイドは、.NET Framework または .NET Core で作業している開発者の中でも、以前に WCF を使用したことがある方々、使用しているアプリケーションを .NET Core 3.0 以降のバージョン用の最新の RPC 環境に移行しようとしている方々向けに作成されています。 より一般的に、このガイドは、NET Core 3.0 へのアップグレードを実施または検討していて、組み込みの gRPC ツールを使用する場合にも役立ちます。

## <a name="how-you-can-use-this-guide"></a>このガイドを使用する方法

ここでは、ASP.NET Core 3.0 で gRPC サービスを構築する方法について簡単に説明します。特に、類似のプラットフォームとして WCF について説明します。 gRPC の原則について説明しており、各概念をそれに相当する WCF の機能に関連付けています。また、既存の WCF アプリケーションを gRPC に移行する方法を紹介しています。 WCF の経験があり、新しいサービスを構築する目的で gRPC の知識を求めている開発者にとっても役立ちます。 独自のプロジェクトのためのテンプレートまたは参照としてサンプル アプリケーションを利用できます。本書やそのサンプルのコードは自由にコピーしたり、再利用したりしてかまいません。

これらの考慮事項と営業案件の共通理解を確保するために、チームにこのガイドを自由に転送してください。 用語の共通セットと基本原則を理解して全員が作業すると、アーキテクチャのパターンと実践方法を一貫して適用できるようになります。

## <a name="references"></a>関連項目

- **gRPC の Web サイト**
  <https://grpc.io>
- **サーバー アプリ用 .NET Core と .NET Framework の選択**
  <https://docs.microsoft.com/dotnet/standard/choosing-core-framework-server>

>[!div class="step-by-step"]
>[次へ](introduction.md)
