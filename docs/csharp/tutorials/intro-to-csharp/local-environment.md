---
title: C# の概要 - 開発ツールに対する理解を深める
description: この記事では、コンピューターで C# アプリケーションと .NET アプリケーションを開発するためのツールの基礎を提供します。
ms.date: 10/23/2018
ms.openlocfilehash: 0b1df9e733eef92b1eeb0a7f3ba3ba49602f219d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156572"
---
# <a name="become-familiar-with-the-net-development-tools"></a>.NET 開発ツールに対する理解を深める

コンピューターでチュートリアルを実行する最初の手順は、開発環境を設定することです。
Windows、Linux、または macOS 上でローカルの開発環境を設定する手順については、.NET チュートリアル [Hello World in 10 minutes](https://dotnet.microsoft.com/learn/dotnet/hello-world-tutorial/intro) (10 分で Hello World) に記載されています。

または、[.NET Core SDK](https://dotnet.microsoft.com/download) と [Visual Studio Code](https://code.visualstudio.com/) をインストールすることもできます。

## <a name="basic-application-development-flow"></a>アプリケーション開発の基本フロー

[`dotnet new`](../../../core/tools/dotnet-new.md) コマンドを使用してアプリケーションを作成します。 このコマンドによってアプリケーションに必要なファイルとアセットが生成されます。 C# の概要チュートリアルではすべて、アプリケーションの種類 `console` が使用されます。 基本を習得したら、他の種類のアプリケーションに応用できます。

その他には、実行可能ファイルをビルドする [`dotnet build`](../../../core/tools/dotnet-build.md) コマンド、実行可能ファイルを実行する [`dotnet run`](../../../core/tools/dotnet-run.md) コマンドを使用します。

## <a name="pick-your-tutorial"></a>チュートリアルを選択する

最初に次のいずれかのチュートリアルを選択します。

## <a name="numbers-in-c"></a>[C# における数値](numbers-in-csharp-local.md)

「[C# における数値](numbers-in-csharp-local.md)」チュートリアルでは、コンピューターが数値を格納する方法と、異なる数値型で計算を実行する方法について説明します。 丸めの基本と C# を使用した数学計算方法を学習します。

このチュートリアルでは、「[Hello World](hello-world.yml)」レッスンが終了していることを前提としています。

## <a name="branches-and-loops"></a>[分岐とループ](branches-and-loops-local.md)

「[分岐とループ](branches-and-loops-local.md)」のチュートリアルでは、変数に格納された値に基づいて、さまざまなパスでコードを実行するための基礎について説明します。 プログラムが決定して異なる操作を選択する上で基本となる、制御フローの基礎を学習します。

このチュートリアルでは、「[Hello World](hello-world.yml)」レッスンと「[C# における数値](numbers-in-csharp-local.md)」レッスンを終了していることを前提としています。

## <a name="list-collection"></a>[リスト コレクション](arrays-and-collections.md)

「[リスト コレクション](arrays-and-collections.md)」レッスンでは、データのシーケンスを格納するリスト コレクション型について説明します。 項目の追加方法や削除方法、項目の検索方法、リストを並べ替える方法を学習します。 さまざまな種類のリストを紹介します。

このチュートリアルでは、上に挙げたレッスンを終了していることを前提としています。

## <a name="introduction-to-classes"></a>[クラスの概要](introduction-to-classes.md)

この C# 概要の最後のチュートリアルはお使いのコンピューターでのみ実行できます。自分のローカル開発環境と .NET Core を使用する必要があります。
コンソール アプリケーションを構築し、C# 言語に含まれるオブジェクト指向の基本的な機能について学習します。
