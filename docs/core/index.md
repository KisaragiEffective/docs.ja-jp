---
title: .NET Core ガイド
description: .NET Core は、Windows、Linux、macOS アプリを作成するための .NET のモジュール型の高性能な実装です。 ここでは、.NET Core の概要について説明します。
author: richlander
ms.date: 12/04/2019
ms.custom: updateeachrelease
ms.openlocfilehash: 3db98d21a7cdc80d8a98b23782a81ffa37520937
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75740751"
---
# <a name="net-core-guide"></a>.NET Core ガイド

[.NET Core](about.md) は、Microsoft および .NET コミュニティによって [GitHub](https://github.com/dotnet/runtime/blob/master/LICENSE.TXT) 上で管理されている、[オープンソース](https://github.com/dotnet/core)の汎用的な開発プラットフォームです。 クロスプラットフォーム (Windows、macOS、Linux をサポート) であり、デバイス、クラウド、および IoT アプリケーションを構築するために使用できます。

特徴、サポートされる言語とフレームワーク、キー API など、.NET Core について詳しくは、「[.NET Core について](about.md)」 をご覧ください。

「[.NET Core チュートリアル](tutorials/index.md)」では、単純な .NET Core アプリケーションを作成する方法を学習できます。 最初のアプリを、ほんの数分で起動および実行できます。 ブラウザーで .NET Core を使用してみる場合は、[C# における数値](../csharp/tutorials/intro-to-csharp/numbers-in-csharp.yml)に関するオンライン チュートリアルをご覧ください。

## <a name="download-net-core"></a>.NET Core のダウンロード

[.NET Core SDK](https://www.microsoft.com/net/download) をダウンロードして、お使いの Windows、macOS、または Linux コンピューター上で .NET Core をお試しください。 また、Docker コンテナーを使用する場合は、[.NET Core の Docker Hub](https://hub.docker.com/_/microsoft-dotnet-core/) にアクセスしてください。

別の .NET Core バージョンを探している場合も、すべての .NET Core バージョンを [.NET Core のダウンロード](https://dotnet.microsoft.com/download/dotnet-core)で入手できます。

## <a name="net-core-31"></a>.NET Core 3.1

最新バージョンは .NET Core 3.1 です。 3.1 に含まれているのは .NET Core 3.0 に対する軽微な改善ですが、.NET Core 3.1 は [長期的にサポートされるリリース](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)です。 .NET Core 3.1 リリースの詳細については、[.NET Core 3.1 の新機能](./whats-new/dotnet-core-3-1.md)を参照してください。

## <a name="create-your-first-application"></a>最初のアプリケーションを作成する

.NET Core SDK をインストールしたらコマンド プロンプトを開きます。 次の `dotnet` コマンドを入力し、C# アプリケーションを作成して実行します。

```dotnetcli
dotnet new console
dotnet run
```

次の出力が表示されます。

```output
Hello World!
```

## <a name="support"></a>サポート

.NET Core は、[Microsoft によって](https://dotnet.microsoft.com/platform/support/policy)、Windows、macOS、Linux 上でサポートされています。 年に数回、通常は毎月、セキュリティと品質向上のために更新されます。

.NET Core のバイナリ形式の配布は、Azure 内のマイクロソフトが管理するサーバーで構築されてテストされ、他のすべてのマイクロソフト製品と同様にサポートされます。

Red Hat Enterprise Linux (RHEL) では [.NET Core は Red Hat によってサポート](http://redhatloves.net/)されます。 Red Hat がソースから .NET Core をビルドして、[Red Hat ソフトウェア コレクション](https://developers.redhat.com/products/softwarecollections/overview/)で使用できるようにします。 Red Hat とマイクロソフトが共同して、.NET Core が RHEL 上で適切に動作するようにします。
