---
title: ASP.NET Web Forms 開発者向け Blazor
description: Blazor と .NET Core を使用し、使い慣れた簡単な方法で、.NET を含むフルスタック Web アプリを構築する方法について説明します。
author: danroth27
ms.author: daroth
ms.date: 09/11/2019
ms.openlocfilehash: 394d11038b59f4cbe9e9955df43b6198eb5daaf8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73088126"
---
# <a name="blazor-for-aspnet-web-forms-developers"></a>ASP.NET Web Forms 開発者向け Blazor

[!INCLUDE [book-preview](../../../includes/book-preview.md)]

![サーバーレス アプリの e-book のカバーを示すスクリーン ショット。](./media/index/blazor-for-web-forms-developers-cover.png)

> 次の場所でダウンロードできます: <https://aka.ms/blazor-ebook>

発行者

Microsoft 開発部門、.NET および Visual Studio 製品チーム

A division of Microsoft Corporation

One Microsoft Way

Redmond, Washington 98052-6399

Copyright © 2019 by Microsoft Corporation

All rights reserved. 本書のいかなる部分も、書面による発行者の許可なしに、いかなる形式または方法によっても、複製または伝送することを禁じます。

本書は "現状有姿" で提供され、著者の見解と意見を表しています。 URL および他の参照されているインターネットの Web サイトをはじめ、本書に記載されている見解、意見、および情報は、通知なく変更されることがあります。

使用している例は、例示のみを目的に提供された、架空のものです。 実際の関連やつながりはなく、推測によるものです。

<https://www.microsoft.com> の "商標" Web ページに記載されている Microsoft および商標は、Microsoft グループの商標です。

Mac および macOS は Apple Inc. の商標です。

その他のすべてのマークおよびロゴは、該当する各社が所有しています。

作成者:

> **[Daniel Roth](https://github.com/danroth27)** 、Microsoft Corporation のプリンシパル プログラム マネージャー

> **[Jeff Fritz](https://github.com/csharpfritz)** 、Microsoft Corporation のシニア プログラム マネージャー

> **[Taylor Southwick](https://github.com/twsouthwick)** 、Microsoft Corporation のシニア ソフトウェア エンジニア

> **[Scott Addie](https://github.com/scottaddie)** 、Microsoft Corporation のシニア コンテンツ開発者

## <a name="introduction"></a>はじめに

.NET では、あらゆる種類の Web アプリを構築するためのフレームワークとツールの包括的セットである ASP.NET を利用した Web アプリの開発を長期にわたりサポートしてきました。 ASP.NET には独自の系列の Web フレームワークとテクノロジがあり、その原点は代表的な Active Server Pages (ASP) までさかのぼります。 ASP.NET Web Forms、ASP.NET MVC、ASP.NET Web Pages、最近の ASP.NET Core などのフレームワークでは、*サーバーでレンダリングする* Web アプリを生産的かつ強力な方法で構築できます。サーバーでレンダリングする Web アプリでは、HTTP 要求に応答し、UI コンテンツがサーバー上で動的に生成されます。 各 ASP.NET framework は異なる対象ユーザーとアプリ構築方針に対応しています。 ASP.NET Web Forms は .NET Framework の初回リリースに付属しました。再利用可能な UI コントロールと単純なイベント処理など、デスクトップ開発者にとっておなじみのパターンを多く利用する Web 開発を可能にしました。 しかしながら、どの ASP.NET サービスでも、ユーザーのブラウザーで実行されるコードを実行する方法がありません。 それを行うには、JavaScript を記述し、jQuery、Knockout、Angular、React など、ここ数年、人気が上がったり下がったりしているさまざまな JavaScript フレームワーク/ツールのいずれかを使用する必要があります。

[Blazor](https://blazor.net) は、.NET で Web アプリを構築したときにできることを変える、新しい Web フレームワークです。 Blazor は、JavaScript ではなく、C# を基盤とするクライアント側の Web UI フレームワークです。 Blazor を利用すると、クライアント側のロジックと UI コンポーネントを C# で記述し、通常の .NET アセンブリにコンパイルしたら、WebAssembly という名前の新しいオープン Web 標準を利用し、ブラウザーで直接実行できます。 あるいは、サーバー上で .NET UI コンポーネントを実行し、ブラウザーとのリアルタイム接続ですべての UI インタラクションを流動的に処理することが Blazor で可能です。 サーバー上で実行されている .NET とペアリングした場合、Blazor では .NET によるフルスタックの Web 開発が可能になります。 再利用可能なコンポーネント モデルやユーザー イベントを処理する簡単な方法など、Blazor には ASP.NET Web Forms との間にさまざまな共通点があり、さらに、高性能な最新式の Web 開発体験を与える目的で、Blazor は .NET Core を基礎に構築されています。

本書をお読みいただくと、ASP.NET Web Forms の開発者は慣れた便利な方法で Blazor を導入できます。 本書では Blazor の概念を ASP.NET Web Forms における類似の概念と一緒に紹介し、さらに、あまり知られていないかもしれない新しい概念について説明します。 コンポーネントの作成、ルーティング、レイアウト、構成、セキュリティなど、さまざまなトピックと懸念事項を取り上げます。 また、本書の内容は新しい開発を可能にすることを第一としていますが、既存のアプリを最新式にするときのために、既存の ASP.NET Web Forms を Blazor に移行するためのガイドラインと方策についても取り上げます。

## <a name="who-should-use-the-book"></a>対象読者

本書は、既存の知識や技能に関連付けて Blazor を説明してくれる文章を探している ASP.NET Web Forms 開発者向けとなっています。 本書は Blazor を基盤とする新しいプロジェクトを短期間で始める際や、既存の ASP.NET Web Forms アプリケーションを最新式にするための計画を立てる際に役立ちます。

## <a name="how-to-use-the-book"></a>本書の使用方法

本書の最初の部分では、Blazor の概要を取り上げ、ASP.NET Web Forms による Web アプリ開発と比較します。 次に、章ごとに Blazor に関するさまざまなトピックを取り上げ、Blazor の概念を ASP.NET Web Forms においてそれに対応する概念に関連付けます。あるいは、まったく新しい概念であれば、それについて十分に説明します。 また、本書では、Blazor の機能の実演や ASP.NET Web Forms から Blazor に移行する際の見本として、ASP.NET Web Forms と Blazor の両方で実装されている完全なサンプル アプリがたびたび取り上げられます。 [GitHub](https://github.com/dotnet-architecture/eshoponblazor) には、サンプル アプリの両方の実装 (ASP.NET Web Forms 版と Blazor 版) があります。

## <a name="what-this-book-doesnt-cover"></a>本書で取り上げないもの

本書は Blazor の紹介文であり、包括的な移行ガイドではありません。 ASP.NET Web Forms から Blazor にプロジェクトを移行する方法に関するガイドが含まれていますが、微妙な違いや詳細をあらゆる点で説明する試みはありません。 ASP.NET から ASP.NET Core に移行する一般的な手順については、ASP.NET Core ドキュメントの[移行ガイダンス](https://docs.microsoft.com/aspnet/core/migration/proper-to-2x/)を参照してください。

### <a name="additional-resources"></a>その他のリソース

Blazor の公式ホーム ページとドキュメントは <https://blazor.net> にあります。

## <a name="send-your-feedback"></a>フィードバックの送信

本書と関連サンプルは継続的に更新されるので、お客様のフィードバックを歓迎しています。 本書を改善する方法についてコメントがある場合、[GitHub の問題](https://github.com/dotnet/docs/issues)に関して作成されたあらゆるページの下部にあるフィードバック セクションをご利用ください。

>[!div class="step-by-step"]
>[次へ](introduction.md)
