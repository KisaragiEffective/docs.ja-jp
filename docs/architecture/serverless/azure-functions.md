---
title: Azure Functions - サーバーレス アプリ
description: Azure Functions には、イベント ドリブンのインスタント スケール コードを提供する、複数の言語 (C#、JavaScript、Java) とプラットフォーム向けのサーバーレス機能が用意されています。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 8764e6a33f3fdd53e60fa767d0fb584a9c07de7e
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79401485"
---
# <a name="azure-functions"></a>Azure Functions

Azure Functions によってサーバーレスのコンピューティング エクスペリエンスが提供されます。 *トリガー* (HTTP エンドポイントへのアクセスやタイマーなど) によって呼び出される関数によって、コードまたはビジネス ロジックのブロックが実行されます。 ストレージやキューなどのリソースに接続する特別な "*バインド*" も Functions でサポートされています。

![Azure Functions のロゴ](./media/azure-functions-logo.png)

Azure Functions フレームワークには 2 つのバージョンがあります。 レガシ バージョンでは完全な .NET Framework がサポートされており、新しいランタイムではクロスプラットフォームの .NET Core アプリケーションがサポートされています。 C# 以外に、JavaScript、F#、Java などのその他の言語がサポートされています。 ポータルで作成する関数には、豊富なスクリプト構文が用意されています。 スタンドアロン プロジェクトとして作成した関数を、完全なプラットフォームのサポートと機能を使用して展開できます。

詳細については、「[Azure Functions のドキュメント](https://docs.microsoft.com/azure/azure-functions)」を参照してください。

## <a name="functions-v1-vs-v2"></a>Functions v1 と v2

Azure Functions ランタイムには 2 つのバージョンがあります: 1.x と 2.x。 バージョン 1.x は一般提供 (GA) されています。 ポータルまたは Windows コンピューターからの .NET 開発がサポートされており、.NET Framework が使用されています。 1.x では C#、JavaScript、F# がサポートされており、Python、PHP、TypeScript、Batch、Bash、PowerShell が実験的にサポートされています。

[現在はバージョン 2.x も一般提供されています](https://azure.microsoft.com/blog/introducing-azure-functions-2-0/)。 .NET Core を使用し、Windows、macOS、Linux コンピューターでのクロスプラットフォーム開発がサポートされています。 2.x には Java の最高レベルのサポートが追加されていますが、実験的な言語はまだ直接サポートされていません。 バージョン 2.x では、新しいバインド拡張モデルを使用して、プラットフォームに対するサードパーティ製の拡張機能、バインドの独立したバージョン管理、より効率化された実行環境を実現しています。

> **1. x には[バインド リダイレクトのサポート](https://github.com/Azure/azure-functions-host/issues/992)に関する既知の問題があります。** その問題は .NET 開発に固有です。 ランタイムに含まれているライブラリとは異なるバージョンのライブラリに依存しているプロジェクトに影響します。 Functions チームは、問題の具体的な進展に努めています。 チームは、2. x が一般提供される前にバインド リダイレクトに対応する予定です。 修正案と回避策が記載された公式のチーム ステートメントについては、こちらを参照してください: [Azure Functions でのアセンブリ解決](https://github.com/Azure/azure-functions-host/wiki/Assembly-Resolution-in-Azure-Functions)。

詳細については、[1.x と 2.x の比較](https://docs.microsoft.com/azure/azure-functions/functions-versions)のページを参照してください。

## <a name="programming-language-support"></a>プログラミング言語のサポート

次の言語は、一般提供 (GA)、プレビュー、試験段階のいずれかでサポートされています。

|言語      |1.x         |2.x      |
|--------------|------------|---------|
|**C#**        |GA          |[プレビュー]  |
|**JavaScript**|GA          |[プレビュー]  |
|**F#**        |GA          |         |
|**Java**      |            |[プレビュー]  |
|**Python**    |実験用|         |
|**PHP**       |実験用|         |
|**TypeScript**|実験用|         |
|**Batch**     |実験用|         |
|**Bash**      |実験用|         |
|**PowerShell**|実験用|         |

詳細については、[サポートされている言語](https://docs.microsoft.com/azure/azure-functions/supported-languages)に関するページを参照してください。

## <a name="app-service-plans"></a>App Service プラン

Functions は "*App Service プラン*" によってサポートされています。 このプランでは、関数アプリによって使用されるリソースを定義します。 プランをリージョンに割り当て、使用される仮想マシンのサイズと数を決定し、価格レベルを選択することができます。 真のサーバーレス アプローチとして、関数アプリで**従量課金**プランを使用することができます。 従量課金プランでは、負荷に基づいてバックエンドが自動的にスケーリングされます。

詳細については、[App Service プラン](https://docs.microsoft.com/azure/app-service/azure-web-sites-web-hosting-plans-in-depth-overview)に関するページを参照してください。

## <a name="create-your-first-function"></a>最初の関数を作成する

関数アプリを作成するには、3 つの一般的な方法があります。

- ポータルで関数をスクリプト化します。
- Azure CLI を使用して必要なリソースを作成します。
- お気に入りの IDE を使用してローカルで関数をビルドし、Azure に発行します。

スクリプト化された関数をポータル上で作成する方法の詳細については、「[Azure portal で初めての関数を作成する](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function)」を参照してください。

Azure CLI からビルドするには、[Azure CLI を使用して初めての関数を作成する](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function-azure-cli)方法に関するページを参照してください。

Visual Studio で関数を作成するには、「[Visual Studio を使用して初めての関数を作成する](https://docs.microsoft.com/azure/azure-functions/functions-create-your-first-function-visual-studio)」を参照してください。

## <a name="understand-triggers-and-bindings"></a>トリガーとバインドについて

関数は "*トリガー*" によって呼び出されます。関数に設定できるトリガーは 1 つだけです。 関数を呼び出すだけでなく、特定のトリガーにはバインドとしての機能もあります。 トリガーに加えて、複数のバインドを定義することもできます。 "*バインド*" は、宣言によってデータをコードに接続する方法です。 渡される場合 (入力) と、データを受け取る場合 (出力) があります。 トリガーとバインドを使用すると、関数を使用した作業が簡単になります。 バインドによって、データベースまたはファイル システムの接続を手動で作成するオーバーヘッドが解消されます。 バインドに必要なすべての情報は、*functions.json* というスクリプト用の特別なファイル内に格納されます。または、コード内で属性を使用して宣言します。

次のような一般的なトリガーがあります。

- Blob Storage: ストレージ内でファイルまたはフォルダーがアップロードまたは変更されたときに、関数を呼び出します。
- HTTP: REST API のように関数を呼び出します。
- キュー: キューに項目が存在する場合に関数を呼び出します。
- タイマー: 関数を定期的に呼び出します。

バインドの例を次に示します。

- CosmosDB: データベースに簡単に接続して、ファイルを読み込んだり保存したりできます。
- Table Storage: 関数アプリからキー/値のストレージを操作します。
- Queue Storage: キューからの項目の取得、または新しい項目のキューへの配置を簡単に行うことができます。

次の *functions.json* ファイルの例では、トリガーとバインドを定義しています。

```json
{
  "bindings": [
    {
      "name": "myBlob",
      "type": "blobTrigger",
      "direction": "in",
      "path": "images/{name}",
      "connection": "AzureWebJobsStorage"
    },
    {
      "name": "$return",
      "type": "queue",
      "direction": "out",
      "queueName": "images",
      "connection": "AzureWebJobsStorage"
    }
  ],
  "disabled": false
}
```

この例では、`images` コンテナー内での BLOB ストレージに対する変更によって関数がトリガーされます。 ファイルの情報が渡され、トリガーはバインドとしても機能します。 `images` という名前のキューに情報を格納するための別のバインドが存在します。

関数の C# スクリプトを次に示します。

```csharp
public static string Run(Stream myBlob, string name, TraceWriter log)
{
    log.Info($"C# Blob trigger function Processed blob\n Name:{name} \n Size: {myBlob.Length} Bytes");
    return name;
}
```

この例は、BLOB ストレージ内の変更またはアップロードされたファイルの名前を取得し、後で処理するためにキューに配置する単純な関数です。

トリガーとバインドの完全な一覧については、「[Azure Functions でのトリガーとバインドの概念](https://docs.microsoft.com/azure/azure-functions/functions-triggers-bindings)」を参照してください。

## <a name="proxies"></a>プロキシ

プロキシによってアプリケーションにリダイレクト機能が提供されます。 プロキシはエンドポイントを公開し、そのエンドポイントを別のリソースにマップするものです。 プロキシを使用すると、次のことができます。

- 受信要求を別のエンドポイントに再ルーティングする。
- 受信要求を、渡す前に変更する。
- 応答を変更または提供する。

プロキシは次のようなシナリオに使用されます。

- URL を簡略化、短縮、または変更する。
- 複数のバックエンド サービスに一貫した API プレフィックスを提供する。
- 開発中のエンドポイントに対する応答のモックを作成する。
- 既知のエンドポイントに対する静的な応答を提供する。
- バックエンドの移動または移行中に、API エンドポイントの整合性を維持する。

プロキシは JSON 定義として格納されます。 次に例を示します。

```json
{
  "$schema": "http://json.schemastore.org/proxies",
  "proxies": {
    "Domain Redirect": {
      "matchCondition": {
        "route": "/{shortUrl}"
      },
      "backendUri": "http://%WEBSITE_HOSTNAME%/api/UrlRedirect/{shortUrl}"
    },
    "Root": {
      "matchCondition": {
        "route": "/"
      },
      "responseOverrides": {
        "response.statusCode": "301",
        "response.statusReason": "Moved Permanently",
        "response.headers.location": "https://docs.microsoft.com/"
      }
    }
  }
}
```

`Domain Redirect` プロキシによって短いルートを取得し、それをルートの長い関数リソースにマップします。 この変換は次のようになります。

`https://--shorturl--/123` -> `https://--longurl--.azurewebsites.net/api/UrlRedirect/123`

`Root` プロキシは、ルート URL (`https://--shorturl--/`) に送信されたものすべてを取得し、それをドキュメント サイトにリダイレクトします。

プロキシの使用例については、ビデオ「[Azure: サーバーレスの Azure Functions を使用してアプリをクラウドに持ち込む](https://channel9.msdn.com/events/Connect/2017/E102)」をご覧ください。 ローカルの SQL Server 上で実行されている ASP.NET Core アプリケーションが、リアルタイムで Azure Cloud に移行されます。 プロキシは、従来の Web API プロジェクトを、関数を使用するようにリファクターするために使用されます。

プロキシの詳細については、「[Azure Functions プロキシの操作](https://docs.microsoft.com/azure/azure-functions/functions-proxies)」を参照してください。

>[!div class="step-by-step"]
>[前へ](azure-serverless-platform.md)
>[次へ](application-insights.md)
