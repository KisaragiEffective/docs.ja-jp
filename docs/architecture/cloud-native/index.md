---
title: Azure 向けクラウド ネイティブ .NET アプリケーションの設計
description: Azure のコンテナー、マイクロサービス、サーバーレス機能を活用するクラウドネイティブなアプリケーションを構築するためのガイド。
author: ardalis
ms.date: 03/07/2019
ms.openlocfilehash: 7f14a690d0153edc43f0ce7f4e91c9e9cd2c6858
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "71696772"
---
# <a name="architecting-cloud-native-net-applications-for-azure"></a>Azure 向けクラウド ネイティブ .NET アプリケーションの設計

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

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

Mac および macOS は Apple Inc. の商標です。

Docker のクジラのロゴは Docker, Inc. の登録商標です。許可を得て使用しています。

その他のすべてのマークおよびロゴは、該当する各社が所有しています。

作成者:

> **Steve "ardalis" Smith** - ソフトウェア アーキテクトおよびトレーナー - [Ardalis.com](https://ardalis.com)
>
> **Rob Vettor** - Microsoft - プリンシパル クラウド システム アーキテクト/IP アーキテクト - [RobVettor.com](https://robvettor.com)

参加者とレビュー担当者:

> **Cesar De la Torre**、Microsoft、.NET チーム、プリンシパル プログラム マネージャー
>
> **Nish Anil**、Microsoft、.NET チーム、シニア プログラム マネージャー

編集者:

> **Maira Wenzel**、Microsoft、.NET チーム、コンテンツ開発者

## <a name="who-should-use-this-guide"></a>対象読者

本ガイドの対象読者は主に、クラウド向けに設計されたアプリケーションを構築する方法に関心がある開発者、開発リーダー、アーキテクトです。

2 番目の対象読者は、クラウドネイティブな手法でアプリケーションを構築するかどうかを決定する予定の技術部門の意思決定者です。

## <a name="how-you-can-use-this-guide"></a>このガイドを使用する方法

本書ではまず、クラウド ネイティブを定義し、クラウドネイティブなプリンシパルとテクノロジで構築された参照アプリケーションを紹介します。 その最初の 2 章以降、本書の残りの部分は、ほとんどのクラウドネイティブ アプリケーションに共通するトピックに特化した章に分割されています。 任意の章までジャンプし、次に関するクラウドネイティブ手法について知ることができます。

- データとデータ アクセス
- 通信パターン
- スケーリングとスケーラビリティ
- アプリケーションの回復性
- 監視と正常性
- ID とセキュリティ
- DevOps

このガイドは PDF 形式とオンラインの両方で利用できます。 本書やそのオンライン版のリンクをチームに転送し、トピックの共通理解に役立ててください。 トピックのほとんどは、基礎的な原則、基礎的なパターン、トピックに関連する意思決定に関係する折り合いが同じように理解されることで効果を発揮します。 本書の目標は、アプリケーションのアーキテクチャ、開発、ホスティングについて、十分に情報が与えられた上で意思決定するために必要な情報をチームとそのリーダーに与えることです。

>[!div class="step-by-step"]
>[次へ](introduction.md)
