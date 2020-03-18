---
title: WCF Web Service Reference を追加する
description: .NET Framework プロジェクトのサービス参照の追加と同様に、.NET Core プロジェクトと ASP.NET Core プロジェクトの機能を追加する Microsoft WCF Web Service Reference Provider Tool の概要。
author: dasetser
ms.date: 10/29/2019
ms.custom: mvc
ms.openlocfilehash: cdd6b457d289dd7b752c97c5645f0797f24b72aa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75715683"
---
# <a name="use-the-wcf-web-service-reference-provider-tool"></a>WCF Web Service Reference Provider ツールを使用する

長年にわたり、多くの Visual Studio 開発者は、.NET Framework プロジェクトが Web サービスにアクセスする必要があるときに、[**サービス参照の追加**](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)ツールが提供する生産性を利用してきました。  **WCF Web Service Reference** ツールは、.NET Core および ASP.NET Core プロジェクトのサービス参照の追加機能のようなエクスペリエンスを提供する、Visual Studio に接続されたサービス拡張機能です。 このツールは現在のソリューションの Web サービスから、ネットワーク上の場所で、あるいは WSDL ファイルからメタデータを取得し、Web サービスのアクセスに使用できる Windows Communication Foundation (WCF) クライアント プロキシ コードを含む、.NET Core 互換ソース ファイルを生成します。

> [!IMPORTANT]
> 信頼できるソースのサービスのみを参照してください。 信頼できないソースの参照を追加すると、セキュリティが損なわれる可能性があります。

## <a name="prerequisites"></a>前提条件

- [Visual Studio 2017 バージョン 15.5](https://aka.ms/vsdownload?utm_source=mscom&utm_campaign=msdocs) 以降のバージョン。

## <a name="how-to-use-the-extension"></a>拡張機能の使用方法

> [!NOTE]
> **[WCF Web Service Reference]** オプションは、次のプロジェクト テンプレートを使用して作成されたプロジェクトに適用されます。
>
> - **Visual C#**  >  **.NET Core**
> - **Visual C#**  >  **.NET Standard**
> - **Visual C#**  > **Web** > **ASP.NET Core Web Application**

この記事では、**ASP.NET Core Web Application** プロジェクト テンプレートを例として、WCF サービス参照をプロジェクトに追加する手順について説明します。

1. ソリューション エクスプローラーで、プロジェクトの **[接続済みサービス]** ノードをダブルクリックします (.NET Core または .NET Standard プロジェクトの場合、ソリューション エクスプローラーでプロジェクトの **[依存関係]** ノードを右クリックすると、このオプションを使用できます)。

    次の図に示すように、 **[接続済みサービス]** ページが表示されます。

    ![Visual Studio の .NET Core の [接続済みサービス] タブ](./media/wcf-web-service-reference-guide/wcfcs-ConnectedServicesPage.png)

2. **[接続済みサービス]** ページで、 **[Microsoft WCF Web Service Reference Provider]** をクリックします。 **[WCF Web Service Reference の構成]** ウィザードが表示されます。

    ![Visual Studio の .NET Core の [サービス エンドポイント] タブ](./media/wcf-web-service-reference-guide/wcfcs-ServiceEndpointPage.png)

3. サービスを選択します。

    3a. **[WCF Web Service Reference の構成]** ウィザードでは、いくつかのサービス検索オプションを使用できます。

     * 現在のソリューションに定義されているサービスを検索するには、 **[探索]** ボタンをクリックします。
     * 指定したアドレスでホストされているサービスを検索するには、 **[アドレス]** ボックスにサービス URL を入力し、 **[検索]** ボタンをクリックします。
     * Web サービスのメタデータ情報を含む WSDL ファイルを選択するには、 **[参照]** ボタンをクリックします。

    3b. **[サービス]** ボックスの検索結果一覧からサービスを選択します。 必要に応じて、生成されたコードの名前空間を対応する **[名前空間]** テキスト ボックスに入力します。

    3c. **[次へ]** ボタンをクリックし、 **[データ型のオプション]** と **[クライアント オプション]** ページを開きます。 または、既定のオプションを使用するには、 **[完了]** ボタンをクリックします。

4. **[データ型のオプション]** フォームを使用すると、生成されるサービス参照の構成設定を絞り込むことができます。

    ![Visual Studio の .NET Core の [データ型のオプション] タブ](./media/wcf-web-service-reference-guide/wcfcs-DataTypesPage.png)

    > [!NOTE]
    > **[参照されたアセンブリで型を再利用]** チェック ボックス オプションは、サービス参照コードの生成に必要なデータ型がプロジェクトの参照されているアセンブリのいずれかで定義されている場合に便利です。  このような既存のデータ型を再利用して、コンパイル時の型の不整合や実行時の問題を回避することが重要です。

    プロジェクトの依存関係の数やその他のシステム パフォーマンスの要因によっては、型の情報が読み込まれるときに遅延が発生することがあります。 **[参照されたアセンブリで型を再利用]** チェック ボックスがオフになっていないと、 **[完了]** ボタンは読み込み中に無効になります。

5. 完了したら、 **[完了]** をクリックします。

進行状況を表示している間、ツールでは以下の処理が実行されます。

- WCF サービスからメタデータをダウンロードします。
- *reference.cs* というファイルにサービス参照コードを生成し、プロジェクトの **[接続済みサービス]** ノードの下に追加します。
- ターゲット プラットフォームでのコンパイルと実行に必要な NuGet パッケージ参照でプロジェクト ファイル (.csproj) を更新します。

![Visual Studio の進行状況ウィンドウ](./media/wcf-web-service-reference-guide/wcfcs-ProgressWindow.png)

これらのプロセスが完了すると、生成された WCF クライアントの種類のインスタンスを作成し、サービス操作を呼び出すことができます。

## <a name="see-also"></a>参照

- [Windows Communication Foundation アプリケーション入門](../../framework/wcf/getting-started-tutorial.md)
- [Visual Studio での Windows Communication Foundation サービスと WCF データ サービス](/visualstudio/data-tools/windows-communication-foundation-services-and-wcf-data-services-in-visual-studio)
- [WCF supported features on .NET Core](https://github.com/dotnet/wcf/blob/master/release-notes/SupportedFeatures-v2.1.0.md) (.NET Core でサポートされる WCF の機能)

## <a name="feedback--questions"></a>フィードバックと質問

ご不明な点またはフィードバックについては、[Developer Community](https://developercommunity.visualstudio.com/) で[問題の報告](/visualstudio/ide/how-to-report-a-problem-with-visual-studio) ツールを使用して報告してください。

## <a name="release-notes"></a>リリース ノート

- 既知の問題を含む最新のリリース情報については、[リリース ノート](https://github.com/dotnet/wcf/blob/master/release-notes/WCF-Web-Service-Reference-notes.md)のページを参照してください。
