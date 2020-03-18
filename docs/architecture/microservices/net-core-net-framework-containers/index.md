---
title: Docker コンテナー用 .NET Core と .NET Framework の選択
description: '.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ | Docker コンテナー用 .NET Core と .NET Framework の選択'
ms.date: 09/11/2018
ms.openlocfilehash: b01aaf25f4071e8e4a07905a12ec9dd0d89a738d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "70849277"
---
# <a name="choosing-between-net-core-and-net-framework-for-docker-containers"></a>Docker コンテナー用 .NET Core と .NET Framework の選択

.NET を使用してサーバー側のコンテナー化された Docker アプリケーションをビルドする場合に選択できるサポート対象のフレームワークには、[.NET Framework と .NET Core](https://dotnet.microsoft.com/download) の 2 つがあります。 それらは多数の .NET プラットフォームのコンポーネントを共有しているため、両者でコードを共有できます。 ただし、それらには基本的な違いがあり、どちらのフレームワークを使用するかは実行内容によって決まります。 このセクションでは、それぞれのフレームワークを選択する場合のガイダンスを提供します。

>[!div class="step-by-step"]
>[前へ](../container-docker-introduction/docker-containers-images-registries.md)
>[次へ](general-guidance.md)
