---
title: 一般的なガイダンス
description: '.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ | 一般的なガイダンス'
ms.date: 09/11/2018
ms.openlocfilehash: 2fa66d7593b764a8df4d9acc20f93d3f8fb26174
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73089650"
---
# <a name="general-guidance"></a>一般的なガイダンス

このセクションでは、どのような場合に .NET Core または .NET Framework を選択すべきかの概要を示します。 後続のセクションでこれらの選択の詳細について説明します。

次の場合には、コンテナー化された Docker サーバー アプリケーションで、.NET Core と Linux または Windows のコンテナーを併用する必要があります。

- クロスプラット フォームが必要である。 たとえば、Linux と Windows のコンテナーの両方を使用する場合。

- アプリケーション アーキテクチャがマイクロサービスに基づく場合。

- コンテナーを高速で起動する必要があり、かつコンテナーあたりのフットプリントを小さくして密度を高めたり、ハードウェア ユニットあたりのコンテナーを増やしてコストを削減したりしたい場合。

つまり、コンテナー化された .NET アプリケーションを新たに作成する場合には、.NET Core を既定の選択肢として考慮する必要があります。 これには多くの利点があり、コンテナーの理念や作業スタイルとぴったりと適合します。

.NET Core を使用するその他の利点は、同じマシンで複数の .NET バージョンのアプリケーションを同時に実行できることです。 この利点は、コンテナーを使用しないサーバーまたは VM にとってはさらに重要になります。コンテナーは、アプリが必要とする .NET バージョンを特定するためです。 (基礎となる OS と互換性がある場合。)

次の場合には、コンテナー化された Docker サーバー アプリケーションで、.NET Framework を使用する必要があります。

- 現在、アプリケーションで .NET Framework を使用し、Windows で強い依存関係がある場合。

- .NET Core がサポートしていない Windows API を使用する必要がある場合。

- .NET Core で使用できないサードパーティ製の .NET ライブラリまたは NuGet パッケージを使用する必要がある場合。

Docker で .NET Framework を使用すると、展開に関する問題を最小限に抑えて、展開のエクスペリエンスを改善できます。 この ["リスト アンド シフト" シナリオ](https://aka.ms/liftandshiftwithcontainersebook)は、ASP.NET WebForms、MVC Web アプリ、WCF (Windows Communication Foundation) サービスなどの従来の .NET Framework を使用して開発されたレガシー アプリケーションをコンテナー化する場合に重要です。

### <a name="additional-resources"></a>その他の技術情報

- **電子書籍: Modernize existing .NET Framework applications with Azure and Windows Containers (Azure および Windows コンテナーで既存の .NET Framework アプリケーションを最新化する)**  
    https://aka.ms/liftandshiftwithcontainersebook

- **サンプル アプリ:Windows コンテナーを使用した従来の ASP.NET Web アプリの最新化**  
    https://aka.ms/eshopmodernizing

>[!div class="step-by-step"]
>[前へ](index.md)
>[次へ](net-core-container-scenarios.md)
