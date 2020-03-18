---
title: 長時間のワークフロー サービスの作成
ms.date: 03/30/2017
ms.assetid: 4c39bd04-5b8a-4562-a343-2c63c2821345
ms.openlocfilehash: 3e422c138b49fa19aa29e4fa1488d61a2c9bc2f8
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75348100"
---
# <a name="create-a-long-running-workflow-service"></a>実行時間の長いワークフローサービスを作成する

ここでは、実行時間の長いワークフロー サービスを作成する方法について説明します。 実行時間の長いワークフロー サービスは、長期間にわたって実行できます。 ワークフローでは、いくつかの追加情報を待つ間アイドル状態になることがあります。 アイドル状態になると、ワークフローは SQL データベースに永続化され、メモリから削除されます。 追加情報が使用可能になると、ワークフロー インスタンスがメモリに読み込み直されて、実行を継続します。  このシナリオでは、非常に簡略化された注文システムを実装します。  クライアントは、最初のメッセージをワークフロー サービスに送信して注文を開始します。 ワークフロー サービスは、注文 ID をクライアントに返します。 この時点で、ワークフロー サービスは、クライアントからの別のメッセージを待機しており、アイドル状態に入って、SQL Server データベースに永続化されます。  クライアントが次のメッセージを送信して項目を注文すると、ワークフロー サービスはメモリに読み込み直されて、注文の処理を終了します。 次のコード例では、項目が注文に追加されたことを示す文字列を返します。 このコード例は、テクノロジの実際の適用を意図するものではなく、実行時間の長いワークフロー サービスを示す簡単な例です。 このトピックでは、Visual Studio 2012 のプロジェクトとソリューションの作成方法を理解していることを前提としています。

## <a name="prerequisites"></a>Prerequisites

このチュートリアルを使用するには、次のソフトウェアがインストールされている必要があります。

1. Microsoft SQL Server 2008

2. Visual Studio 2012

3. Microsoft [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)]

4. WCF および Visual Studio 2012 に精通しており、プロジェクト/ソリューションの作成方法を理解していること。

## <a name="set-up-the-sql-database"></a>SQL Database を設定する

1. ワークフロー サービス インスタンスが永続化されるようにするには、Microsoft SQL Server をインストールして、永続化ワークフロー インスタンスを保存するようにデータベースを設定する必要があります。 **[スタート]** ボタンをクリックし、すべての **[プログラム]** 、 **[Microsoft SQL Server 2008]** 、 **[microsoft sql Management Studio]** の順に選択して、microsoft sql Management Studio を実行します。

2. **[接続]** ボタンをクリックして、SQL Server インスタンスにログオンします。

3. ツリービューで **[データベース]** を右クリックし、 **[新しいデータベース]** をクリックします。 `SQLPersistenceStore`という名前の新しいデータベースを作成します。

4. SQLPersistenceStore データベースの C:\Windows\Microsoft.NET\Framework\v4.0\SQL\en ディレクトリにある SqlWorkflowInstanceStoreSchema.sql スクリプト ファイルを実行して、必要なデータベース スキーマを設定します。

5. SQLPersistenceStore データベースの C:\Windows\Microsoft.NET\Framework\v4.0\SQL\en ディレクトリにある SqlWorkflowInstanceStoreLogic.sql スクリプト ファイルを実行して、必要なデータベース ロジックを設定します。

## <a name="create-the-web-hosted-workflow-service"></a>Web ホストワークフローサービスを作成する

1. 空の Visual Studio 2012 ソリューションを作成し、`OrderProcessing`という名前を指定します。

2. `OrderService` という新しい WCF ワークフロー サービス アプリケーション プロジェクトをソリューションに追加します。

3. プロジェクトのプロパティ ダイアログボックスで、 **Web** タブを選択します。

    1. **[開始アクション]** で**特定のページ**を選択し、`Service1.xamlx`を指定します。

        ![ワークフローサービスプロジェクトの Web プロパティ](./media/creating-a-long-running-workflow-service/start-action-specific-page-option.png "Web ホステッドワークフローサービス固有のページオプションを作成する")

    2. **[サーバー]** で、 **[ローカル IIS Web サーバーを使用する]** を選択します。

        ![ローカル Web サーバーの設定](./media/creating-a-long-running-workflow-service/use-local-web-server.png "Web ホストワークフローサービスの作成-[ローカル IIS Web サーバーを使用する] オプション")

        > [!WARNING]
        > この設定を行うには、Visual Studio 2012 を管理者モードで実行する必要があります。

        次の 2 つの手順で、ワークフロー サービス プロジェクトが IIS でホストされるように設定します。

4. まだ開いていない場合は `Service1.xamlx` を開き、既存の**receiverequest アクティビティ**アクティビティと**sendresponse**アクティビティを削除します。

5. **[シーケンシャルサービス]** アクティビティを選択し、 **[変数]** リンクをクリックして、次の図に示す変数を追加します。 こうするといくつかの変数が追加され、後でワークフロー サービスで使用されます。

    > [!NOTE]
    > 変数の種類 ボックスの一覧に CorrelationHandle が表示されない場合は、ドロップダウンから **型の参照** を選択します。 **[型名]** ボックスに「CorrelationHandle」と入力し、Listbox から CorrelationHandle を選択して、 **[OK]** をクリックします。

    ![変数の追加](./media/creating-a-long-running-workflow-service/add-variables-sequential-service-activity.gif "シーケンシャルサービスアクティビティに変数を追加します。")

6. **Receiveandsendreply**アクティビティテンプレートを **[シーケンシャルサービス]** アクティビティにドラッグアンドドロップします。 このアクティビティのセットは、クライアントからメッセージを受信して、返信を送信します。

    1. Receive アクティビティを選択し、次の図で強調表示**さ**れているプロパティを設定します。

        ![Receive アクティビティのプロパティの設定](./media/creating-a-long-running-workflow-service/set-receive-activity-properties.png "Receive アクティビティのプロパティを設定します。")

        DisplayName プロパティは、デザイナーに表示される Receive アクティビティの名前を設定します。 ServiceContractName プロパティと OperationName プロパティは、Receive アクティビティで実装されるサービス コントラクトおよび操作の名前を指定します。 ワークフローサービスでのコントラクトの使用方法の詳細については、「[ワークフローでのコントラクトの使用](../../../../docs/framework/wcf/feature-details/using-contracts-in-workflow.md)」を参照してください。

    2. **Receivestartorder**アクティビティの **[定義...]** リンクをクリックし、次の図に示されているプロパティを設定します。  **[パラメーター]** オプションボタンが選択されていて、`p_customerName` という名前のパラメーターが `customerName` 変数にバインドされていることに注意してください。 これにより、Receive アクティビティがデータを受信し、そのデータをローカル変数にバインドするように構成**さ**れます。

        ![Receive アクティビティによって受信されるデータの設定](./media/creating-a-long-running-workflow-service/set-properties-for-receive-content.png "Receive アクティビティによって受信されるデータのプロパティを設定します。")

    3. **SendReplyToReceive**アクティビティを選択し、次の図に示すように、強調表示されているプロパティを設定します。

        ![SendReply アクティビティのプロパティの設定](./media/creating-a-long-running-workflow-service/set-properties-for-reply-activities.png "SetReplyProperties")

    4. **SendReplyToStartOrder**アクティビティの **[定義...]** リンクをクリックし、次の図に示すプロパティを設定します。 **[パラメーター]** オプションボタンが選択されていることに注意してください。`p_orderId` という名前のパラメーターが `orderId` 変数にバインドされています。 この設定により、SendReplyToStartOrder アクティビティが型文字列の値を呼び出し元に返すように指定されます。

        ![SendReply アクティビティのコンテンツデータの構成](./media/creating-a-long-running-workflow-service/setreplycontent-for-sendreplytostartorder-activity.png "SetReplyToStartOrder アクティビティの設定を構成します。")

    5. **Receive**アクティビティと**SendReply**アクティビティの間に Assign アクティビティをドラッグアンドドロップし、次の図に示すようにプロパティを設定します。

        ![Assign アクティビティの追加](./media/creating-a-long-running-workflow-service/add-an-assign-activity.png "Assign アクティビティを追加します。")

        これにより、新しい注文 ID が作成され、orderId 変数に値が配置されます。

    6. **ReplyToStartOrder**アクティビティを選択します。 プロパティ ウィンドウで、 **Correlationinitializers** の省略記号ボタンをクリックします。 **初期化子の追加** リンクを選択し、初期化子 テキストボックスに「`orderIdHandle`」と入力して、関連付けの種類 で クエリ関連付け初期化子 を選択し、XPATH クエリ ボックスの p_orderId を選択します。 これらの設定を次の図に示します。 **[OK]** をクリックします。  これにより、クライアントとワークフロー サービスのこのインスタンス間の相関関係が初期化されます。 この注文 ID を含むメッセージが受信されると、ワークフロー サービスのこのインスタンスにルーティングされます。

        ![関連付け初期化子の追加](./media/creating-a-long-running-workflow-service/add-correlationinitializers.png "関連付け初期化子を追加します。")

7. 別の**Receiveandsendreply**アクティビティをワークフローの最後 (最初の**Receive**アクティビティと**SendReply**アクティビティを含む**シーケンス**の外側) にドラッグアンドドロップします。 これで、クライアントで送信された 2 つ目のメッセージを受信し、それに応答します。

    1. 新しく追加された**Receive**アクティビティと**SendReply**アクティビティを含む**シーケンス**を選択し、 **[変数]** ボタンをクリックします。 次の図で強調表示されている変数を追加します。

        ![新しい変数の追加](./media/creating-a-long-running-workflow-service/add-the-itemid-variable.png "ItemId 変数を追加します。")

        また、`Sequence` スコープに**文字列**として `orderResult` を追加します。

    2. **Receive**アクティビティを選択し、次の図に示すプロパティを設定します。

        ![Receive アクティビティのプロパティを設定する](./media/creating-a-long-running-workflow-service/set-receive-activities-properties.png "Receive アクティビティのプロパティを設定します。")

        > [!NOTE]
        > 必ず、`../IAddItem`で**ServiceContractName**フィールドを変更してください。

    3. **Receiveadditem**アクティビティの **[定義...]** リンクをクリックし、次の図に示すパラメーターを追加します。これにより、receive アクティビティで2つのパラメーター、注文 ID、および順序付けされている項目の id が受け入れられるように構成されます。

        ![2番目の受信のパラメーターの指定](./media/creating-a-long-running-workflow-service/add-receive-two-parameters.png "2つのパラメーターを受け取るように receive アクティビティを構成します。")

    4. **CorrelateOn**の省略記号ボタンをクリックし、「`orderIdHandle`」と入力します。 **[XPath クエリ]** の下にあるドロップダウン矢印をクリックし、[`p_orderId`] を選択します。 これにより、2 つ目の Receive アクティビティに相関関係が設定されます。 相関関係の詳細については、「[関連付け](../../../../docs/framework/wcf/feature-details/correlation.md)」を参照してください。

        ![Correlateson プロパティの設定](./media/creating-a-long-running-workflow-service/correlateson-setting.png "Correlateson プロパティを設定します。")

    5. **If**アクティビティを、 **receiveadditem**アクティビティの直後にドラッグアンドドロップします。 このアクティビティは、if ステートメントと同様に動作します。

        1. **Condition**プロパティを `itemId=="Zune HD" (itemId="Zune HD" for Visual Basic)` に設定します。

        2. 次の図に示すように、 **assign アクティビティを** **[Then** ] セクションと **[Else]** セクションにドラッグアンドドロップして、 **assign**アクティビティのプロパティを設定します。

            ![サービス呼び出しの結果の割り当て](./media/creating-a-long-running-workflow-service/assign-result-of-service-call.png "サービス呼び出しの結果を割り当てます。")

            条件が `true` 場合は、 **Then**セクションが実行されます。 条件がの場合 `false` **Else**セクションが実行されます。

        3. **SendReplyToReceive**アクティビティを選択し、次の図に示す**DisplayName**プロパティを設定します。

            ![SendReply アクティビティのプロパティの設定](./media/creating-a-long-running-workflow-service/send-reply-activity-property.png "SendReply アクティビティのプロパティを設定します。")

        4. **SetReplyToAddItem**アクティビティの **[定義...]** リンクをクリックし、次の図に示すように構成します。 これにより、`orderResult` 変数の値を返すように**SendReplyToAddItem**アクティビティが構成されます。

            ![SendReply アクティビティのデータバインディングの設定](./media/creating-a-long-running-workflow-service/set-property-for-sendreplytoadditem.gif "SendReplyToAddItem アクティビティのプロパティを設定します。")

8. Web.config ファイルを開き、[\<behavior >] セクションに次の要素を追加して、ワークフローの永続化を有効にします。

    ```xml
    <sqlWorkflowInstanceStore connectionString="Data Source=your-machine\SQLExpress;Initial Catalog=SQLPersistenceStore;Integrated Security=True;Asynchronous Processing=True" instanceEncodingOption="None" instanceCompletionAction="DeleteAll" instanceLockedExceptionAction="BasicRetry" hostLockRenewalPeriod="00:00:30" runnableInstancesDetectionPeriod="00:00:02" />
              <workflowIdle timeToUnload="0"/>
    ```

    > [!WARNING]
    > 前のコード スニペットでホストと SQL Server インスタンス名を置換するようにします。

9. ソリューションをビルドします。

## <a name="create-a-client-application-to-call-the-workflow-service"></a>ワークフローサービスを呼び出すクライアントアプリケーションを作成する

1. `OrderClient` という新しいコンソール アプリケーション プロジェクトをソリューションに追加します。

2. 次のアセンブリへの参照を `OrderClient` プロジェクトに追加します。

    1. System.ServiceModel.dll

    2. System.ServiceModel.Activities.dll

3. サービス参照をワークフロー サービスに追加し、`OrderService` を名前空間として指定します。

4. クライアント プロジェクトの `Main()` メソッド内に次のコードを追加します。

    ```csharp
    static void Main(string[] args)
    {
       // Send initial message to start the workflow service
       Console.WriteLine("Sending start message");
       StartOrderClient startProxy = new StartOrderClient();
       string orderId = startProxy.StartOrder("Kim Abercrombie");

       // The workflow service is now waiting for the second message to be sent
       Console.WriteLine("Workflow service is idle...");
       Console.WriteLine("Press [ENTER] to send an add item message to reactivate the workflow service...");
       Console.ReadLine();

       // Send the second message
       Console.WriteLine("Sending add item message");
       AddItemClient addProxy = new AddItemClient();
       AddItem item = new AddItem();
       item.p_itemId = "Zune HD";
       item.p_orderId = orderId;

       string orderResult = addProxy.AddItem(item);
       Console.WriteLine("Service returned: " + orderResult);
    }
    ```

5. ソリューションをビルドし、`OrderClient` アプリケーションを実行します。 クライアントに次のテキストが表示されます。

    ```output
    Sending start messageWorkflow service is idle...Press [ENTER] to send an add item message to reactivate the workflow service...
    ```

6. ワークフローサービスが永続化されていることを確認するには、**スタート** メニューに移動し、**すべてのプログラム**、 **Microsoft SQL Server 2008**、 **SQL Server Management Studio** の順に選択して SQL Server Management Studio を開始します。

    1. 左側のウィンドウで、 **[データベース]** 、 **[SQLPersistenceStore]** 、 **[ビュー]** の順に展開し、 **system.activities.durableinstancing.instances**を右クリックして、[**上位1000行の選択]** を選択します。 **結果**ウィンドウで、少なくとも1つのインスタンスが表示されていることを確認します。 実行中に例外が発生した場合は、前の実行のその他のインスタンスがある可能性があります。 既存の行を削除するには、 **system.activities.durableinstancing.instances**を右クリックして **[上位200行の編集]** を選択し、 **[実行]** ボタンを押して、結果ペインのすべての行を選択し、 **[削除]** を選択します。  データベースに表示されているインスタンスが、アプリケーションで作成されたインスタンスであることを確認するには、クライアントを実行する前に、Instances ビューが空であることを確認します。 クライアントが実行されると、クエリ ([上位 1000 行を選択]) を再実行し、新しいインスタンスが追加されていることを確認します。

7. Enter キーを押して、項目の追加メッセージをワークフロー サービスに送信します。 クライアントに次のテキストが表示されます。

    ```output
    Sending add item messageService returned: Item added to orderPress any key to continue . . .
    ```

## <a name="see-also"></a>関連項目

- [ワークフロー サービス](../../../../docs/framework/wcf/feature-details/workflow-services.md)
