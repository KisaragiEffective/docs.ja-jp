---
title: 意思決定テーブル。 Docker に使用する .NET Frameworks
description: '.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ | 意思決定テーブル、Docker に使用する .NET Frameworks'
ms.date: 09/11/2018
ms.openlocfilehash: 8ffe2b7bc0bee976d3a63b274994dbcc8aef0c61
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77628320"
---
# <a name="decision-table-net-frameworks-to-use-for-docker"></a>意思決定テーブル: Docker に使用する .NET Frameworks

次の意思決定テーブルは、.NET Framework と .NET Core のどちらを使用するかをまとめたものです。 Linux コンテナーには、Linux ベースの Docker ホスト (VM またはサーバー) が必要であり、Windows コンテナーには、Windows Server ベースの Docker ホスト (VM またはサーバー) が必要であることを覚えておいてください。

> [!IMPORTANT]
> 開発用のコンピューターは、Linux または Windows の Docker ホストを 1 つ実行します。 1 つのソリューションで同時に実行してテストしたい関連するマイクロサービスは、同じコンテナー プラットフォームで実行する必要があります。

| アーキテクチャ/アプリの種類 | Linux コンテナー | Windows コンテナー |
|-------------------------|------------------|--------------------|
| コンテナー上のマイクロサービス | .NET Core | .NET Core |
| モノリシック アプリ | .NET Core | .NET Framework <br/> .NET Core |
| クラス最高のパフォーマンスとスケーラビリティ | .NET Core | .NET Core |
| Windows Server レガシー アプリ ("ブラウンフィールド") のコンテナーへの移行 | -- | .NET Framework |
| コンテナー ベースの新しい開発 ("グリーンフィールド") | .NET Core | .NET Core |
| ASP.NET Core | .NET Core | .NET Core (推奨) <br/> .NET Framework |
| ASP.NET 4 (MVC 5、Web API 2、Web Forms) | -- | .NET Framework |
| SignalR サービス | .NET Core 2.1 以降のバージョン | .NET Framework <br/> .NET Core 2.1 以降のバージョン |
| WCF、WF などのレガシ フレームワーク | .NET Core の WCF (クライアント ライブラリのみ) | .NET Framework <br/> .NET Core の WCF (クライアント ライブラリのみ) |
| Azure サービスの使用 | .NET Core <br/> (いずれ、ほとんどの Azure サービスで .NET Core 用のクライアント SDK が提供される予定です) | .NET Framework <br/> .NET Core <br/> (いずれ、ほとんどの Azure サービスで .NET Core 用のクライアント SDK が提供される予定です) |

>[!div class="step-by-step"]
>[前へ](net-framework-container-scenarios.md)
>[次へ](net-container-os-targets.md)
