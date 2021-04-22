---
title: 'チュートリアル: Windows Communication Foundation クライアントを作成する'
description: WCF アプリケーションの作成を始めるときに役立つ一連の記事の一部として、WCF サービスからメタデータを取得することによってクライアントを作成する方法について説明します。
ms.date: 03/19/2019
helpviewer_keywords:
- clients [WCF], running
- WCF clients [WCF], running
ms.assetid: a67884cc-1c4b-416b-8c96-5c954099f19f
ms.openlocfilehash: d5a2a2b5175c9e34eaf1dbaff20ac57f34117c4e
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246325"
---
# <a name="tutorial-create-a-windows-communication-foundation-client"></a>チュートリアル: Windows Communication Foundation クライアントを作成する

このチュートリアルでは、基本的な Windows Communication Foundation (WCF) アプリケーションの作成に必要な 5 つのタスクのうち 4 番目のタスクについて説明します。 このチュートリアルの概要については、「[チュートリアル: Windows Communication Foundation アプリケーション入門](getting-started-tutorial.md)」を参照してください。

WCF アプリケーションを作成するための次のタスクでは、WCF サービスからメタデータを取得してクライアントを作成します。 Visual Studio を使用してサービス参照を追加し、そこでサービスの MEX エンドポイントからメタデータを取得します。 その後、Visual Studio によって、選択した言語でクライアント プロキシ用のマネージド ソース コード ファイルが生成されます。 また、クライアント構成ファイル (*App.config*) も作成されます。 このファイルにより、クライアント アプリケーションはエンドポイントでサービスに接続できるようになります。

> [!NOTE]
> Visual Studio でクラス ライブラリ プロジェクトから WCF サービスを呼び出す場合は、**サービス参照の追加** 機能を使用して、プロキシおよび関連構成ファイルを自動的に生成します。 ただし、クラス ライブラリ プロジェクトではこの構成ファイルが使用されないため、クラス ライブラリを呼び出す実行可能ファイルの *App.config* ファイルに、生成された構成ファイルの設定を追加する必要があります。

> [!NOTE]
> 別の方法として、Visual Studio ではなく [ServiceModel メタデータ ユーティリティ ツール](#servicemodel-metadata-utility-tool)を使用して、プロキシ クラスと構成ファイルを生成することもできます。

クライアント アプリケーションは、生成されたプロキシ クラスを使用してサービスと通信します。 この手順については、[クライアントの使用のチュートリアル](how-to-use-a-wcf-client.md)に関する記事を参照してください。

このチュートリアルでは、以下の内容を学習します。
> [!div class="checklist"]
>
> - WCF クライアント用のコンソール アプリ プロジェクトを作成して構成します。
> - プロキシ クラスと構成ファイルを生成するには、WCF サービスへのサービス参照を追加します。

## <a name="create-a-windows-communication-foundation-client"></a>Windows Communication Foundation クライアントを作成する

1. Visual Studio でコンソール アプリ プロジェクトを作成します。

    1. **[ファイル]** メニューから、 **[開く]**  >  **[ソリューション/プロジェクト]** を選択し、前に作成した **GettingStarted** ソリューション (*GettingStarted.sln*) を参照します。 **[Open]** を選択します。

    2. **[表示]** メニューの **[ソリューション エクスプローラー]** を選択します。

    3. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStarted** ソリューション (最上位ノード) を選択し、ショートカット メニューの **[追加]**  >  **[新しいプロジェクト]** を選択します。

    4. **[新しいプロジェクトの追加]** ウィンドウの左側で、 **[Visual C#]** または **[Visual Basic]** の下にある **[Windows デスクトップ]** カテゴリを選択します。

    5. **[コンソール アプリ (.NET Framework)]** テンプレートを選択し、 **[名前]** に「*GettingStartedClient*」と入力します。 **[OK]** を選択します。

2. **GettingStartedClient** プロジェクトで、<xref:System.ServiceModel> アセンブリへの参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStartedClient** プロジェクトの下にある **[参照設定]** フォルダーを選択して、ショートカット メニューから **[参照の追加]** を選択します。

    2. **[参照の追加]** ウィンドウの左側にある **[アセンブリ]** で、 **[フレームワーク]** を選択します。

    3. **System.ServiceModel** を見つけて選択し、 **[OK]** を選択します。

    4. **[ファイル]**  >  **[すべて保存]** を選択して、ソリューションを保存します。

3. 電卓サービスへのサービス参照を追加します。

   1. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStartedClient** プロジェクトの下にある **[参照設定]** フォルダーを選択して、ショートカット メニューから **[サービス参照の追加]** を選択します。

   2. **[サービス参照の追加]** ウィンドウで、 **[探索]** を選択します。

      CalculatorService サービスが開始され、Visual Studio の **[サービス]** ボックスに表示されます。

   3. **CalculatorService** を選択して展開し、サービスによって実装されているサービス コントラクトを表示します。 既定の **[名前空間]** をそのまま使用して、 **[OK]** を選択します。

      Visual Studio によって、**GettingStartedClient** プロジェクトの **[接続済みサービス]** フォルダーに新しい項目が追加されます。

### <a name="servicemodel-metadata-utility-tool"></a>ServiceModel メタデータ ユーティリティ ツール

次の例では、必要に応じて、[ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) を使用してプロキシ クラス ファイルを生成する方法を示します。 このツールにより、プロキシ クラス ファイルと *App.config* ファイルが生成されます。 次の例では、それぞれ C# と Visual Basic でプロキシを生成する方法を示します。

```shell
svcutil.exe /language:cs /out:generatedProxy.cs /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

```shell
svcutil.exe /language:vb /out:generatedProxy.vb /config:app.config http://localhost:8000/GettingStarted/CalculatorService
```

### <a name="client-configuration-file"></a>クライアント構成ファイル

クライアントを作成した後、Visual Studio によって **GettingStartedClient** プロジェクトに **App.config** 構成ファイルが作成されます。それは、次の例のようになります。

```xml
    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <startup>
            <!-- specifies the version of WCF to use-->
            <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1" />
        </startup>
        <system.serviceModel>
            <bindings>
                <!-- Uses wsHttpBinding-->
                <wsHttpBinding>
                    <binding name="WSHttpBinding_ICalculator" />
                </wsHttpBinding>
            </bindings>
            <client>
                <!-- specifies the endpoint to use when calling the service -->
                <endpoint address="http://localhost:8000/GettingStarted/CalculatorService"
                    binding="wsHttpBinding" bindingConfiguration="WSHttpBinding_ICalculator"
                    contract="ServiceReference1.ICalculator" name="WSHttpBinding_ICalculator">
                    <identity>
                        <dns value="localhost" />
                    </identity>
                </endpoint>
            </client>
        </system.serviceModel>
    </configuration>
```

[\<system.serviceModel>](../configure-apps/file-schema/wcf/system-servicemodel.md) セクションの下で、[\<endpoint>](../configure-apps/file-schema/wcf/endpoint-element.md) 要素を確認します。 **&lt;endpoint&gt;** 要素によって、次のように、クライアントでサービスにアクセスするために使用するエンドポイントが定義されます。

- アドレス: `http://localhost:8000/GettingStarted/CalculatorService`。 エンドポイントのアドレス。
- サービス コントラクト: `ServiceReference1.ICalculator`。 サービス コントラクトにより、WCF クライアントとサービスの間の通信が処理されます。 このコントラクトは、**サービス参照の追加** 機能を使用したときに、Visual Studio によって生成されました。 基本的には、GettingStartedLib プロジェクトで定義したコントラクトのコピーです。
- バインディング: <xref:System.ServiceModel.WSHttpBinding>。 バインディングにより、トランスポートとして HTTP、相互運用可能なセキュリティ、その他の構成詳細が指定されます。

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、以下の内容を学習しました。
> [!div class="checklist"]
>
> - WCF クライアント用のコンソール アプリ プロジェクトを作成して構成します。
> - クライアント アプリケーション用のプロキシ クラスと構成ファイルを生成するには、WCF サービスへのサービス参照を追加します。

次のチュートリアルに進み、生成されたクライアントを使用する方法を学習してください。

> [!div class="nextstepaction"]
> [チュートリアル: WCF クライアントを使用する](how-to-use-a-wcf-client.md)
