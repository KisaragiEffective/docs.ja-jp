---
title: サーバーレス アプリのサンプルのビジネス シナリオとユースケース
description: 画像処理からモバイル バックエンドおよび ETL パイプラインに至る、サンプルへのアクセスによるハンズオン アプローチを使用してサーバーレスついて学習します。
author: JEREMYLIKNESS
ms.author: jeliknes
ms.date: 06/26/2018
ms.openlocfilehash: 5f0d7a4c5cd736d1168ec76c1c0ea19627505f15
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "76787891"
---
# <a name="serverless-business-scenarios-and-use-cases"></a>サーバーレスのビジネス シナリオとユース ケース

サーバーレス アプリケーションには、多くのユースケースとシナリオがあります。 この章には、さまざまなシナリオを示すサンプルが含まれています。 シナリオには、関連するドキュメントおよびパブリック ソース コード リポジトリへのリンクが含まれています。 この章のサンプルを使用して、サーバーレス ソリューションの独自の構築および実装を開始することができます。

## <a name="analyze-and-archive-images"></a>画像を分析してアーカイブする

このサンプルでは、サーバーレス イベント (Event Grid)、ワークフロー (Logic App)、およびコード (Azure Functions) を示します。 また、別のリソース (この場合、画像分析用の Cognitive Services) と統合する方法を示します。

コンソール アプリケーションでは、Web 上の URL にリンクを渡すことができます。 アプリでは、URL をイベント グリッド メッセージとして公開します。 並列して、サーバーレス関数アプリとロジック アプリでメッセージがサブスクライブされます。 サーバーレス関数アプリでは、画像を Blob ストレージにシリアル化します。 また、Azure Table Storage に情報を格納します。 メタデータには、元の画像の URL と BLOB 画像の名前が格納されます。 ロジック アプリでは、Custom Vision API とやり取りして画像を分析し、コンピューターによって生成されるキャプションを作成します。 キャプションはメタデータ テーブルに格納されます。

![画像の分析とアーカイブのアーキテクチャ](./media/image-processing-example.png)

個別のシングル ページ アプリケーション (SPA) ではサーバーレス関数を呼び出して、画像とメタデータのリストを取得します。 画像ごとに、アーカイブから画像データを配信する別の関数を呼び出します。 最終的な結果は、キャプションが自動のギャラリーとなります。

![自動画像ギャラリー](./media/automated-image-gallery.png)

完全なリポジトリとロジック アプリを構築するための手順については、こちらを参照してください:[Event Grid Glue](https://github.com/JeremyLikness/Event-Grid-Glue)。

## <a name="cross-platform-mobile-client-using-xamarinforms-and-functions"></a>Xamarin.Forms と関数を使用するクロスプラットフォーム モバイル クライアント

Azure Web Portal または Visual Studio で、シンプルなサーバーレス Azure 関数を実装する方法を確認します。 Android、iOS、Windows で実行される Xamarin.Forms を使用してクライアントを構築します。 その後、アプリケーションは、サーバーレス バックエンドを使用するモバイル クライアントとサーバーの間の通信メディアとして、JavaScript Object Notation (JSON) を使用するように調整されます。

詳細については、「[Xamarin.Forms クライアントを使用するシンプルな Azure 関数の実装](https://docs.microsoft.com/samples/azure-samples/functions-xamarin-getting-started/implementing-a-simple-azure-function-with-a-xamarinforms-client/)」を参照してください。

## <a name="generate-a-photo-mosaic-with-serverless-image-recognition"></a>サーバーレス画像認識で写真のモザイクを生成する

サンプルでは、Azure Functions と Microsoft Cognitive Services Custom Vision Service を使用して、入力画像から写真のモザイクを生成します。 モデルは画像を認識するようにトレーニングされています。 画像がアップロードされると、その画像が認識され、Bing で検索されます。 元の画像は、検索結果を使用して再構成されます。

![オーランド アイの写真とモザイク](./media/orlando-eye-both.png)

たとえば、オーランド アイなど、オーランドのランドマークを使用してモデルをトレーニングできます。 Custom Vision ではオーランド アイの画像を認識し、関数で "オーランド アイ" の Bing 画像検索結果で構成された写真のモザイクが作成されます。

詳細については、[Azure Functions の写真のモザイク ジェネレーター](https://github.com/Azure-Samples/functions-dotnet-photo-mosaic)に関するページを参照してください。

## <a name="migrate-an-existing-application-to-the-cloud"></a>既存のアプリケーションをクラウドに移行する

前の章で説明したように、アプリケーションをオンプレミスでホストする場合は N 層アーキテクチャを採用するのが一般的です。 仮想マシンを使用してリソースを "そのまま" 移行することは、クラウドへの最も危険なパスですが、多くの企業ではアプリケーションをリファクタリングする機会を利用することを選びます。 さいわい、リファクタリングは、"オールオアナッシング" の作業である必要はありません。 実際には、アプリを移行してから、コンポーネントをクラウド ネイティブの対応するものに段階的に置き換えることができます。

アプリケーションでは、Azure Functions のプロキシ機能を使用して、従来のオンプレミス コードからサーバーレス エンドポイントへのエンドポイントのリファクタリングを有効にします。

![移行のアーキテクチャ](./media/migration-architecture.png)

プロキシでは、サーバーレス関数に移動されるたびに個々の要求を再ルーティングするために更新される、1 つの API エンドポイントが提供されます。

移行全体の手順を示す、こちらのビデオをご覧いただけます: [サーバーレスの Azure 関数を使用するリフト アンド シフト](https://channel9.msdn.com/Events/Connect/2017/E102)。 サンプル コードにアクセスする場合は、こちらをご覧ください: [独自のアプリを持ち込む](https://github.com/JeremyLikness/bring-own-app-connect-17)。

## <a name="parse-a-csv-file-and-insert-into-a-database"></a>CSV ファイルを解析してデータベースに挿入する

抽出、変換、読み込み (ETL) は、さまざまなシステムを統合する一般的なビジネス機能です。 従来のアプローチでは、多くの場合、専用の FTP サーバーを設定してから、スケジュールされたジョブをデプロイしてファイルを解析し、ビジネスで使用できるように変換します。 サーバーレス アーキテクチャを使用すると、ファイルがアップロードされたときにトリガーが起動するため、ジョブがより簡単になります。 Azure Functions では、特定の問題に焦点を当てる小さなコードの理想的な構成を通じて、ETL などのタスクに取り組みます。

![csv 解析プロセスを示すスクリーンショット。](./media/serverless-business-scenarios/csv-parse-database-import.png)

ソース コードとハンズオン ラボについては、[CSV インポート ラボ](https://github.com/JeremyLikness/azure-fn-file-process-hol)に関するページを参照してください。

## <a name="shorten-links-and-track-metrics"></a>リンクを短くしてメトリックを追跡する

リンクの短縮ツールは、当初は、短い Twitter 投稿の URL をエンコードし、140 文字の制限に対応するのに役立ちました。 これらは複数の用途をカバーするために強化され、最も一般的な用途は分析のためにクリックスルーを追跡することです。 リンクの短縮ツールのシナリオは、リンクとレポートのメトリックを管理する完全なサーバーレス アプリケーションです。

Azure Functions は、長い URL を貼り付けて短い URL を生成できる、シングル ページ アプリケーション (SPA) を提供するために使用されます。 URL には、キャンペーン (トピック) やメディア (リンクが投稿されるソーシャル ネットワークなど) を追跡するためにタグが付けられます。 短いコードはキーとして Azure Table Storage に格納され、長い URL は値として格納されます。 短いリンクをクリックすると、別の関数によって長い URL が検索され、リダイレクトが送信され、イベントに関する情報がキューに配置されます。 キューは別の Azure 関数で処理され、情報は Azure Cosmos DB 内に配置されます。

![リンクの短縮ツールのアーキテクチャ](./media/link-shortener-architecture.png)

その後、Power BI ダッシュボードを作成して、収集されたデータに関する分析情報を収集できます。 バックエンドでは、Application Insights によって重要なメトリックが提供されます。 テレメトリには、平均的なユーザーのリダイレクトにかかる時間と、Azure Table Storage にアクセスするためにかかる時間が含まれます。

![Power BI の例](./media/power-bi-example.png)

手順を含む完全なリンクの短縮ツール リポジトリについては、こちらを参照してください: [サーバーレスの URL 短縮ツール](https://github.com/jeremylikness/serverless-url-shortener)。 簡略化されたバージョンについては、こちらを参照してください: [数分のサーバーレス .NET アプリの Azure Storage](https://devblogs.microsoft.com/aspnet/azure-storage-for-serverless-net-apps-in-minutes/)。

## <a name="verify-device-connectivity-using-a-ping"></a>ping を使用してデバイスの接続を確認する

サンプルは、Azure IoT Hub と Azure 関数で構成されています。 IoT Hub の新しいメッセージで、Azure 関数がトリガーされます。 サーバーレス コードでは、送信元のデバイスに同じメッセージの内容が送り返されます。 プロジェクトには、ソリューションに必要なすべてのコードとデプロイの構成が含まれています。

詳細については、[Azure IoT Hub ping](https://github.com/Azure-Samples/iot-hub-node-ping) に関するページを参照してください。

## <a name="recommended-resources"></a>推奨リソース

- [Azure Functions の写真のモザイク ジェネレーター](https://github.com/Azure-Samples/functions-dotnet-photo-mosaic)
- [Azure IoT Hub ping](https://github.com/Azure-Samples/iot-hub-node-ping)
- [数分のサーバーレス .NET アプリの Azure Storage](https://devblogs.microsoft.com/aspnet/azure-storage-for-serverless-net-apps-in-minutes/)
- [独自のアプリを持ち込む](https://github.com/JeremyLikness/bring-own-app-connect-17)
- [CSV インポート ラボ](https://github.com/JeremyLikness/azure-fn-file-process-hol)
- [Event Grid Glue](https://github.com/JeremyLikness/Event-Grid-Glue)
- [Xamarin.Forms クライアントを使用するシンプルな Azure 関数の実装](https://docs.microsoft.com/samples/azure-samples/functions-xamarin-getting-started/implementing-a-simple-azure-function-with-a-xamarinforms-client/)
- [サーバーレスの Azure 関数を使用するリフトアンドシフト](https://channel9.msdn.com/Events/Connect/2017/E102)
- [サーバーレスの URL 短縮ツール](https://github.com/jeremylikness/serverless-url-shortener)

>[!div class="step-by-step"]
>[前へ](orchestration-patterns.md)
>[次へ](serverless-conclusion.md)
