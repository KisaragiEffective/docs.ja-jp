---
title: 'チュートリアル: 基本的な Windows Communication Foundation サービスをホストおよび実行する'
description: WCF アプリケーションの作成を始めるときに役立つ一連の記事の一部として、コンソール アプリケーションで WCF サービスをホストする方法について説明します。
ms.date: 03/19/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF services [WCF]
- WCF services [WCF], running
ms.assetid: 31774d36-923b-4e2d-812e-aa190127266f
ms.openlocfilehash: 5318991087e71430523681d601d3b38c4513027b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246131"
---
# <a name="tutorial-host-and-run-a-basic-windows-communication-foundation-service"></a>チュートリアル: 基本的な Windows Communication Foundation サービスをホストおよび実行する

このチュートリアルでは、基本的な Windows Communication Foundation (WCF) アプリケーションの作成に必要な 5 つのタスクのうち 3 番目のタスクについて説明します。 このチュートリアルの概要については、「[チュートリアル: Windows Communication Foundation アプリケーション入門](getting-started-tutorial.md)」を参照してください。

WCF アプリケーションを作成するための次のタスクは、コンソール アプリケーションで WCF サービスをホストすることです。 WCF サービスにより 1 つ以上の "*エンドポイント*" が公開され、そのそれぞれで 1 つ以上のサービス操作が公開されます。 サービス エンドポイントにより次の情報が指定されます。

- サービスを検索できるアドレス。
- クライアントがサービスと通信するために必要な方法を説明する情報が格納されているバインド。
- サービスからクライアントに提供される機能が定義されているコントラクト。

このチュートリアルでは、以下の内容を学習します。
> [!div class="checklist"]
>
> - WCF サービスをホストするためのコンソール アプリ プロジェクトを作成して構成します。
> - WCF サービスをホストするためのコードを追加します。
> - 構成ファイルを更新します。
> - WCF サービスを開始し、動作していることを確認します。

## <a name="create-and-configure-a-console-app-project-for-hosting-the-service"></a>サービスをホストするためのコンソール アプリ プロジェクトを作成して構成する

1. Visual Studio でコンソール アプリ プロジェクトを作成します。

    1. **[ファイル]** メニューから、 **[開く]**  >  **[ソリューション/プロジェクト]** を選択し、前に作成した **GettingStarted** ソリューション (*GettingStarted.sln*) を参照します。 **[Open]** を選択します。

    2. **[表示]** メニューの **[ソリューション エクスプローラー]** を選択します。

    3. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStarted** ソリューション (最上位ノード) を選択し、ショートカット メニューの **[追加]**  >  **[新しいプロジェクト]** を選択します。

    4. **[新しいプロジェクトの追加]** ウィンドウの左側で、 **[Visual C#]** または **[Visual Basic]** の下にある **[Windows デスクトップ]** カテゴリを選択します。

    5. **[コンソール アプリ (.NET Framework)]** テンプレートを選択し、 **[名前]** に「*GettingStartedHost*」と入力します。 **[OK]** を選択します。

2. **GettingStartedHost** プロジェクトに **GettingStartedLib** プロジェクトへの参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStartedHost** プロジェクトの下にある **[参照設定]** フォルダーを選択して、ショートカット メニューから **[参照の追加]** を選択します。

    2. **[参照の追加]** ダイアログ ボックスで、ウィンドウの左側にある **[プロジェクト]** の下の **[ソリューション]** を選択します。

    3. ウィンドウの中央のセクションで **GettingStartedLib** を選択し、 **[OK]** を選択します。

       これにより、**GettingStartedLib** プロジェクトで定義されている型を、**GettingStartedHost** プロジェクトで使用できるようになります。

3. **GettingStartedHost** プロジェクトで、<xref:System.ServiceModel> アセンブリへの参照を追加します。

    1. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStartedHost** プロジェクトの下にある **[参照設定]** フォルダーを選択して、ショートカット メニューから **[参照の追加]** を選択します。

    2. **[参照の追加]** ウィンドウの左側にある **[アセンブリ]** で、 **[フレームワーク]** を選択します。

    3. **System.ServiceModel** を選択して、 **[OK]** を選択します。

    4. **[ファイル]**  >  **[すべて保存]** を選択して、ソリューションを保存します。

## <a name="add-code-to-host-the-service"></a>サービスをホストするためのコードを追加する

サービスをホストするには、次の手順を実行するコードを追加します。

1. ベース アドレスの URI を作成します。
2. サービスをホストするためのクラス インスタンスを作成します。
3. サービス エンドポイントを作成します。
4. メタデータ交換を有効にします。
5. 受信メッセージをリッスンするためのサービス ホストを開きます。
  
コードに次の変更を加えます。

1. **GettingStartedHost** プロジェクトの **Program.cs** または **Module1.vb** ファイルを開き、そのコードを次のコードで置き換えます。

    ```csharp
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Description;
    using GettingStartedLib;

    namespace GettingStartedHost
    {
        class Program
        {
            static void Main(string[] args)
            {
                // Step 1: Create a URI to serve as the base address.
                Uri baseAddress = new Uri("http://localhost:8000/GettingStarted/");

                // Step 2: Create a ServiceHost instance.
                ServiceHost selfHost = new ServiceHost(typeof(CalculatorService), baseAddress);

                try
                {
                    // Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), "CalculatorService");

                    // Step 4: Enable metadata exchange.
                    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
                    smb.HttpGetEnabled = true;
                    selfHost.Description.Behaviors.Add(smb);

                    // Step 5: Start the service.
                    selfHost.Open();
                    Console.WriteLine("The service is ready.");

                    // Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.");
                    Console.WriteLine();
                    Console.ReadLine();
                    selfHost.Close();
                }
                catch (CommunicationException ce)
                {
                    Console.WriteLine("An exception occurred: {0}", ce.Message);
                    selfHost.Abort();
                }
            }
        }
    }
    ```

    ```vb
    Imports System.ServiceModel
    Imports System.ServiceModel.Description
    Imports GettingStartedLib.GettingStartedLib

    Module Service

        Class Program
            Shared Sub Main()
                ' Step 1: Create a URI to serve as the base address.
                Dim baseAddress As New Uri("http://localhost:8000/GettingStarted/")

                ' Step 2: Create a ServiceHost instance.
                Dim selfHost As New ServiceHost(GetType(CalculatorService), baseAddress)
               Try

                    ' Step 3: Add a service endpoint.
                    selfHost.AddServiceEndpoint( _
                        GetType(ICalculator), _
                        New WSHttpBinding(), _
                        "CalculatorService")

                    ' Step 4: Enable metadata exchange.
                    Dim smb As New ServiceMetadataBehavior()
                    smb.HttpGetEnabled = True
                    selfHost.Description.Behaviors.Add(smb)

                    ' Step 5: Start the service.
                    selfHost.Open()
                    Console.WriteLine("The service is ready.")

                    ' Close the ServiceHost to stop the service.
                    Console.WriteLine("Press <Enter> to terminate the service.")
                    Console.WriteLine()
                    Console.ReadLine()
                    selfHost.Close()

                Catch ce As CommunicationException
                    Console.WriteLine("An exception occurred: {0}", ce.Message)
                    selfHost.Abort()
                End Try
            End Sub
        End Class

    End Module
    ```

    このコードの動作については、「[サービス ホスティング プログラムの手順](#service-hosting-program-steps)」を参照してください。

2. プロジェクトのプロパティを更新します。

   1. **[ソリューション エクスプローラー]** ウィンドウで、**GettingStartedHost** フォルダーを選択した後、ショートカット メニューの **[プロパティ]** を選択します。

   2. **GettingStartedHost** のプロパティ ページで、 **[アプリケーション]** タブを選択します。

      - C# プロジェクトの場合は、 **[スタートアップ オブジェクト]** ボックスの一覧から **GettingStartedHost.Program** を選択します。

      - Visual Basic プロジェクトの場合は、 **[スタートアップ オブジェクト]** ボックスの一覧から **Service.Program** を選択します。

   3. **[ファイル]** メニューから **[すべて保存]** を選択します。

## <a name="verify-the-service-is-working"></a>サービスが機能していることを確認する

1. ソリューションをビルドし、Visual Studio 内から **GettingStartedHost** コンソール アプリケーションを実行します。

    サービスは、管理者権限で実行する必要があります。 Visual Studio を管理者特権で開いたので、Visual Studio で **GettingStartedHost** を実行すると、アプリケーションも管理者特権で実行されます。 または、管理者として新しいコマンド プロンプトを開き (ショートカット メニューから **[その他]**  >  **[管理者として実行]** を選択)、その中で **GettingStartedHost.exe** を実行することもできます。

2. Web ブラウザーを開き、サービスのページ `http://localhost:8000/GettingStarted/CalculatorService` を参照します。

   > [!NOTE]
   > このようなサービスには、リッスンを行うコンピューター上で HTTP アドレスを登録するための適切なアクセス許可が必要です。 管理者アカウントにはこのアクセス許可がありますが、管理者以外のアカウントの場合は、HTTP 名前空間へのアクセス許可を付与する必要があります。 名前空間の予約を構成する方法については、「[Configuring HTTP and HTTPS](feature-details/configuring-http-and-https.md)」 (HTTP と HTTPS を構成する) を参照してください。

## <a name="service-hosting-program-steps"></a>サービス ホスティング プログラムの手順

サービスをホストするために追加したコードの手順は、次のように記述されています。

- **ステップ 1**: サービスのベース アドレスを保持する `Uri` クラスのインスタンスを作成します。 ベース アドレスを含む URL には、サービスを識別するオプションの URI があります。 ベース アドレスの形式は次のとおりです: `<transport>://<machine-name or domain><:optional port #>/<optional URI segment>`。 電卓サービスのベース アドレスには、HTTP トランスポート、localhost、ポート 8000、および URI セグメント "GettingStarted" が使用されます。

- **ステップ 2**: サービスをホストするために使用する <xref:System.ServiceModel.ServiceHost> クラスのインスタンスを作成します。 コンストラクターは、サービス コントラクトを実装するクラスの型と、サービスのベース アドレスの、2 つのパラメーターを受け取ります。

- **ステップ 3**: <xref:System.ServiceModel.Description.ServiceEndpoint> のインスタンスを作成します。 サービス エンドポイントは、アドレス、バインディング、およびサービス コントラクトから構成されます。 <xref:System.ServiceModel.Description.ServiceEndpoint> コンストラクターは、サービス コントラクト インターフェイスの型、バインディング、およびアドレスで構成されます。 サービス コントラクトは、サービス型に定義および実装した `ICalculator` です。 このサンプルのバインディングは <xref:System.ServiceModel.WSHttpBinding> で、これは組み込みのバインディングであり、WS-* 仕様に準拠するエンドポイントに接続します。 WCF バインディングの詳細については、[WCF バインディングの概要](bindings-overview.md)に関するページを参照してください。 エンドポイントを示すため、ベース アドレスにアドレスを追加します。 コードで、アドレスとして CalculatorService を指定し、エンドポイントの完全修飾アドレスとして `http://localhost:8000/GettingStarted/CalculatorService` を指定します。

    > [!IMPORTANT]
    > .NET Framework バージョン 4 以降では、サービス エンドポイントの追加は省略可能です。 これらのバージョンの場合、コードまたは構成を追加しないと、WCF により、サービスで実装されているベース アドレスとコントラクトの組み合わせごとに、1 つの既定のエンドポイントが追加されます。 既定のエンドポイントの詳細については、「[エンドポイント アドレスの指定](specifying-an-endpoint-address.md)」を参照してください。 既定のエンドポイントについては、「[簡略化された構成](simplified-configuration.md)」と「[WCF サービスの簡略化された構成](samples/simplified-configuration-for-wcf-services.md)」を参照してください。

- **ステップ 4**: メタデータ交換を有効にします。 サービス操作を呼び出すためのプロキシを生成するため、クライアントによりメタデータ交換が使用されます。 メタデータ交換を有効化するには、<xref:System.ServiceModel.Description.ServiceMetadataBehavior> のインスタンスを作成し、その <xref:System.ServiceModel.Description.ServiceMetadataBehavior.HttpGetEnabled> プロパティを `true` に設定して、`ServiceMetadataBehavior` オブジェクトを <xref:System.ServiceModel.ServiceHost> インスタンスの <xref:System.ServiceModel.Description.ServiceDescription.Behaviors%2A> コレクションに追加します。

- **ステップ 5**: 受信メッセージをリッスンするために <xref:System.ServiceModel.ServiceHost> を開きます。 アプリケーションは、**Enter** キーが押されるのを待機しています。 アプリケーションによって <xref:System.ServiceModel.ServiceHost> がインスタンス化された後、try/catch ブロックが実行されます。 <xref:System.ServiceModel.ServiceHost> によってスローされる例外を安全にキャッチする方法の詳細については、「[close と abort を使用して WCF クライアントのリソースを解放する](samples/use-close-abort-release-wcf-client-resources.md)」を参照してください。

> [!IMPORTANT]
> WCF サービス ライブラリを追加すると、サービス ホストを開始することによりそれをデバッグしている場合は、Visual Studio によって自動的にホストされます。 競合を回避するため、Visual Studio によって WCF サービス ライブラリがホストされないようにすることができます。
>
> 1. **ソリューション エクスプローラー** で **GettingStartedLib** プロジェクトを選択し、ショートカット メニューから **[プロパティ]** を選択します。
> 2. **[WCF オプション]** を選択し、 **[同じソリューション内での別のプロジェクトのデバッグ時に WCF サービス ホストを開始する]** をオフにします。

## <a name="next-steps"></a>次のステップ

このチュートリアルでは、以下の内容を学習しました。
> [!div class="checklist"]
>
> - WCF サービスをホストするためのコンソール アプリ プロジェクトを作成して構成します。
> - WCF サービスをホストするためのコードを追加します。
> - 構成ファイルを更新します。
> - WCF サービスを開始し、動作していることを確認します。

次のチュートリアルに進み、WCF クライアントを作成する方法を学習してください。

> [!div class="nextstepaction"]
> [チュートリアル: WCF クライアントを作成する](how-to-create-a-wcf-client.md)
