---
title: .NET の使用を開始する
description: Hello World .NET アプリを作成します。
author: adegeo
ms.author: adegeo
ms.date: 09/29/2020
ms.custom: vs-dotnet
ms.topic: tutorial
ms.openlocfilehash: 1c88817b0503b050faf2a7ce3bd42d892931a698
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740385"
---
# <a name="get-started-with-net"></a>.NET を使って作業を開始する

この記事では、"Hello World!" .NET アプリを作成して実行する方法について説明します。 .NET アプリ。

.NET についてよくわからない場合は、[.NET の概要](introduction.md)のページからお読みください。

## <a name="create-an-application"></a>アプリケーションの作成

まず、ご利用のコンピューターで [.NET SDK](https://dotnet.microsoft.com/download/dotnet) をダウンロードしてインストールします。

次に、**PowerShell**、**コマンド プロンプト**、**bash** などのターミナルを開きます。 次の `dotnet` コマンドを入力し、C# アプリケーションを作成して実行します。

```dotnetcli
dotnet new console --output sample1
dotnet run --project sample1
```

次の出力が表示されます。

```output
Hello World!
```

お疲れさまでした。 シンプルな .NET アプリケーションを作成しました。

## <a name="next-steps"></a>次のステップ

[ステップバイステップのチュートリアル](../standard/get-started.md)に従って、または YouTube で [.NET 101 の動画](https://www.youtube.com/playlist?list=PLdo4fOcmZ0oWoazjhXQzBKMrFuArxpW80)を視聴して、.NET アプリケーションの開発を開始します。
