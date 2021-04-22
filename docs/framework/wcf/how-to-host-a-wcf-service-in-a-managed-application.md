---
title: '方法: マネージド アプリケーションで WCF サービスをホストする'
description: 自己ホスト型サービスを作成してテストすることにより、マネージド アプリケーション内で WCF サービスをホストする方法について説明します。
ms.date: 09/17/2018
dev_langs:
- csharp
- vb
ms.assetid: 5eb29db0-b6dc-4e77-8c68-0a62f79d743b
ms.openlocfilehash: 7d1d61b683f60a6c643d2a2f03d367a6ae6c6c15
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246169"
---
# <a name="how-to-host-a-wcf-service-in-a-managed-app"></a>方法: マネージド アプリで WCF サービスをホストする

マネージド アプリケーションでサービスをホストするには、マネージド アプリケーション コード内にサービスのコードを埋め込み、サービスのエンドポイントをコードで強制的に定義するか、構成を使用して宣言により定義してから、または既定のエンドポイントを使用して、<xref:System.ServiceModel.ServiceHost> のインスタンスを作成します。

メッセージの受信を開始するには、<xref:System.ServiceModel.ICommunicationObject.Open%2A> で <xref:System.ServiceModel.ServiceHost> を呼び出します。 これにより、サービスのリスナーが作成されて開きます。 この方法によるサービスのホストは、マネージド アプリケーション自体がホスト作業を行うため、"自己ホスト" と呼ばれることがあります。 サービスを閉じるには、<xref:System.ServiceModel.Channels.CommunicationObject.Close%2A?displayProperty=nameWithType> で <xref:System.ServiceModel.ServiceHost> を呼び出します。

サービスは、マネージド Windows サービス、インターネット インフォメーション サービス (IIS)、または Windows プロセス アクティブ化サービス (WAS) でホストすることもできます。 サービスのホスティング オプションの詳細については、「[ホスティング サービス](hosting-services.md)」を参照してください。

マネージド アプリケーションでのサービスのホスティングは、展開するインフラストラクチャが最小限で済むため、最も柔軟性があります。 マネージド アプリケーションでのホスティング サービスの詳細については、「[マネージド アプリケーションのホスト](./feature-details/hosting-in-a-managed-application.md)」を参照してください。

次の手順では、自己ホスト型サービスをコンソール アプリケーションに実装する方法を示します。

## <a name="create-a-self-hosted-service"></a>自己ホスト型サービスを作成する

1. 新しいコンソール アプリケーションを作成します。

   1. Visual Studio を開き、 **[ファイル]** メニューから **[新規]**  >  **[プロジェクト]** を選択します。

   2. **[インストール済みのテンプレート]** の一覧で **[Visual C#]** または **[Visual Basic]** を選択し、 **[Windows デスクトップ]** を選択します。

   3. **[コンソール アプリ]** テンプレートを選択します。 **[名前]** ボックスに「`SelfHost`」と入力して、 **[OK]** を選択します。

2. **ソリューション エクスプローラー** で **SelfHost** を右クリックして、 **[参照の追加]** を選択します。 **[.NET]** タブで **System.ServiceModel** を選択して、 **[OK]** を選択します。

    > [!TIP]
    > **[ソリューション エクスプローラー]** ウィンドウが表示されない場合は、 **[表示]** メニューの **[ソリューション エクスプローラー]** を選択します。

3. まだ開いていない場合は、**ソリューション エクスプローラー** で **Program.cs** または **Module1.vb** をダブルクリックして、コード ウィンドウで開きます。 ファイルの先頭に、次のステートメントを追加します。

     [!code-csharp[CFX_SelfHost4#1](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#1)]
     [!code-vb[CFX_SelfHost4#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#1)]

4. サービス コントラクトを定義して実装します。 この例では、サービスへの入力に基づいてメッセージを返す `HelloWorldService` を定義します。

     [!code-csharp[CFX_SelfHost4#2](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#2)]
     [!code-vb[CFX_SelfHost4#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#2)]

    > [!NOTE]
    > サービス インターフェイスを定義して実装する方法の詳細については、[サービス コントラクトの定義方法](how-to-define-a-wcf-service-contract.md)および[サービス コントラクトの実装方法](how-to-implement-a-wcf-contract.md)に関する記事を参照してください。

5. `Main` メソッドの上部で、サービスのベース アドレスで <xref:System.Uri> クラスのインスタンスを作成します。

     [!code-csharp[CFX_SelfHost4#3](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#3)]
     [!code-vb[CFX_SelfHost4#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#3)]

6. <xref:System.ServiceModel.ServiceHost> クラスのインスタンスを作成して、サービス型を表す <xref:System.Type> とベース アドレス URI (Uniform Resource Identifier) を <xref:System.ServiceModel.ServiceHost.%23ctor%28System.Type%2CSystem.Uri%5B%5D%29> に渡します。 メタデータ公開を有効にして、<xref:System.ServiceModel.ICommunicationObject.Open%2A> で <xref:System.ServiceModel.ServiceHost> メソッドを呼び出し、サービスを初期化してメッセージを受信する準備をします。

     [!code-csharp[CFX_SelfHost4#4](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#4)]
     [!code-vb[CFX_SelfHost4#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#4)]

    > [!NOTE]
    > この例では、既定のエンドポイントを使用するので、このサービスには構成ファイルは必要ありません。 エンドポイントが構成されていない場合、ランタイムは、サービスによって実装されたサービス コントラクトごとに 1 つのエンドポイントを各ベース アドレスに作成します。 既定のエンドポイントの詳細については、「[簡易構成](simplified-configuration.md)」と「[WCF サービスの簡易構成](./samples/simplified-configuration-for-wcf-services.md)」を参照してください。

7. **Ctrl** + **Shift** + **B** キーを押して、ソリューションをビルドします。

## <a name="test-the-service"></a>サービスをテストする

1. **Ctrl** + **F5** キーを押してサービスを実行します。

2. **[WCF テスト クライアント]** を開きます。

    > [!TIP]
    > **[WCF テスト クライアント]** を開くには、Visual Studio の開発者コマンド プロンプトを開き、**WcfTestClient.exe** を実行します。

3. **[ファイル]** メニューの **[サービスの追加]** を選択します。

4. アドレス ボックスに「`http://localhost:8080/hello`」と入力して、 **[OK]** をクリックします。

    > [!TIP]
    > サービスが実行していることを確認してください。サービスが実行していない場合、この手順は失敗します。 コードでベース アドレスを変更した場合は、この手順で、変更したアドレスを使用します。

5. **[マイ サービス プロジェクト]** ノードの **SayHello** をダブルクリックします。 自分の名前を **[要求]** ボックスの **[値]** 列に入力して、 **[起動]** をクリックします。

   応答メッセージが **[応答]** の一覧に表示されます。

## <a name="example"></a>例

<xref:System.ServiceModel.ServiceHost> 型のサービスをホストする `HelloWorldService` オブジェクトを作成し、<xref:System.ServiceModel.ICommunicationObject.Open%2A> で <xref:System.ServiceModel.ServiceHost> メソッドを呼び出す例を、次に示します。 ベース アドレスがコードで指定され、メタデータ公開が有効化されていて、既定のエンドポイントが使用されています。

[!code-csharp[CFX_SelfHost4#5](../../../samples/snippets/csharp/VS_Snippets_CFX/cfx_selfhost4/cs/program.cs#5)]
[!code-vb[CFX_SelfHost4#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/cfx_selfhost4/vb/module1.vb#5)]

## <a name="see-also"></a>関連項目

- <xref:System.Uri>
- <xref:System.Configuration.ConfigurationManager.AppSettings%2A>
- <xref:System.Configuration.ConfigurationManager>
- [方法: IIS で WCF サービスをホストする](./feature-details/how-to-host-a-wcf-service-in-iis.md)
- [自己ホスト](./samples/self-host.md)
- [ホスティング サービス](hosting-services.md)
- [方法: サービス コントラクトを定義する](how-to-define-a-wcf-service-contract.md)
- [方法: サービス コントラクトを実装する](how-to-implement-a-wcf-contract.md)
- [ServiceModel メタデータ ユーティリティ ツール (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md)
- [サービスとクライアントを構成するためのバインディングの使用](using-bindings-to-configure-services-and-clients.md)
- [システム標準のバインディング](system-provided-bindings.md)
