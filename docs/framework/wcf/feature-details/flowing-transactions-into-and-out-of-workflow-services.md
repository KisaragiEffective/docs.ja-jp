---
title: ワークフロー サービスへのトランザクションのフロー
ms.date: 03/30/2017
ms.assetid: 03ced70e-b540-4dd9-86c8-87f7bd61f609
ms.openlocfilehash: ea14bc651258684fd31940aa6b88f9731348dcd1
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73978282"
---
# <a name="flowing-transactions-into-and-out-of-workflow-services"></a>ワークフロー サービスへのトランザクションのフロー
ワークフロー サービスとワークフロー クライアントはトランザクションに参加できます。  サービス操作をアンビエント トランザクションの一部にするには、<xref:System.ServiceModel.Activities.Receive> アクティビティを <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティの中に配置します。 <xref:System.ServiceModel.Activities.Send> 内の <xref:System.ServiceModel.Activities.SendReply> または <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティによる呼び出しが行われると、アンビエント トランザクション内でも呼び出しが行われます。 ワークフロー クライアント アプリケーションでは、<xref:System.Activities.Statements.TransactionScope> アクティビティを使用してアンビエント トランザクションを作成し、そのアンビエント トランザクションを使用してサービス操作を呼び出すことができます。 ここでは、トランザクションに参加するワークフロー サービスとワークフロー クライアントを作成する手順について説明します。  
  
> [!WARNING]
> ワークフローサービスインスタンスがトランザクション内に読み込まれ、ワークフローに <xref:System.Activities.Statements.Persist> アクティビティが含まれている場合、ワークフローインスタンスは、トランザクションがタイムアウトするまでブロックします。  
  
> [!IMPORTANT]
> <xref:System.ServiceModel.Activities.TransactedReceiveScope> を使用する場合は、ワークフロー内のすべての受信を <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティに配置することをお勧めします。  
  
> [!IMPORTANT]
> <xref:System.ServiceModel.Activities.TransactedReceiveScope> を使用して、メッセージが不適切な順序で到着する場合、最初の順序を無視したメッセージを配信しようとするとワークフローは中止されます。 ワークフローがアイドル状態である場合、ワークフローは常に一致する停止ポイントにあるようにする必要があります。 これによって、ワークフローが中止された場合、前の永続性ポイントからワークフローを再開することができます。  
  
### <a name="create-a-shared-library"></a>共有ライブラリの作成  
  
1. 新しい空の Visual Studio ソリューションを作成します。  
  
2. `Common` という新しいクラス ライブラリ プロジェクトを追加します。 次のアセンブリへの参照を追加します。  
  
    - System.Activities.dll  
  
    - System.ServiceModel.dll  
  
    - System.ServiceModel.Activities.dll  
  
    - System.Transactions.dll  
  
3. `PrintTransactionInfo` という新しいクラスを `Common` プロジェクトに追加します。 このクラスは <xref:System.Activities.NativeActivity> の派生クラスで、<xref:System.Activities.NativeActivity.Execute%2A> メソッドをオーバーロードします。  
  
    ```csharp
    using System;  
    using System;  
    using System.Activities;  
    using System.Transactions;  
  
    namespace Common  
    {  
        public class PrintTransactionInfo : NativeActivity  
        {  
            protected override void Execute(NativeActivityContext context)  
            {  
                RuntimeTransactionHandle rth = context.Properties.Find(typeof(RuntimeTransactionHandle).FullName) as RuntimeTransactionHandle;  
  
                if (rth == null)  
                {  
                    Console.WriteLine("There is no ambient RuntimeTransactionHandle");  
                }  
  
                Transaction t = rth.GetCurrentTransaction(context);  
  
                if (t == null)  
                {  
                    Console.WriteLine("There is no ambient transaction");  
                }  
                else  
                {  
                    Console.WriteLine("Transaction: {0} is {1}", t.TransactionInformation.DistributedIdentifier, t.TransactionInformation.Status);  
                }  
            }  
        }  
  
    }  
    ```  
  
     これは、アンビエント トランザクションに関する情報を表示するネイティブ アクティビティで、ここで使用するサービス ワークフローとクライアント ワークフローの両方で使用されます。 ソリューションをビルドして、このアクティビティを**ツールボックス**の **[共通]** セクションで使用できるようにします。  
  
### <a name="implement-the-workflow-service"></a>ワークフロー サービスの実装  
  
1. `Common` プロジェクトに `WorkflowService` という新しい WCF ワークフローサービスを追加します。 これを行うには、`Common` プロジェクトを右クリックし、 **[追加]** 、 **[新しい項目]** の順に選択し、 **[インストールされたテンプレート]** で **[ワークフロー]** を選択して **[WCF ワークフローサービス]** を選択します。  
  
     ![ワークフロー サービスの追加](./media/flowing-transactions-into-and-out-of-workflow-services/add-workflow-service.jpg)  
  
2. 既定の `ReceiveRequest` アクティビティと `SendResponse` アクティビティを削除します。  
  
3. <xref:System.Activities.Statements.WriteLine> アクティビティを `Sequential Service` アクティビティにドラッグ アンド ドロップします。 次の例に示すように、Text プロパティを `"Workflow Service starting ..."` に設定します。  
  
     ![シーケンシャルサービスアクティビティへの WriteLine アクティビティの追加 (./media/flowing-transactions-into-and-out-of-workflow-services/add-writeline-sequential-service.jpg)  
  
4. <xref:System.ServiceModel.Activities.TransactedReceiveScope> を <xref:System.Activities.Statements.WriteLine> アクティビティの後にドラッグ アンド ドロップします。 <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティは、**ツールボックス**の **[メッセージング]** セクションにあります。 <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティは、**要求**と**本文**の2つのセクションで構成されています。 **要求**セクションには、<xref:System.ServiceModel.Activities.Receive> アクティビティが含まれています。 **Body**セクションには、メッセージを受信した後にトランザクション内で実行されるアクティビティが含まれています。  
  
     ![TransactedReceiveScope アクティビティの追加](./media/flowing-transactions-into-and-out-of-workflow-services/transactedreceivescope-activity.jpg)  
  
5. <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティを選択し、 **[変数]** ボタンをクリックします。 次の変数を追加します。  
  
     ![TransactedReceiveScope への変数の追加](./media/flowing-transactions-into-and-out-of-workflow-services/add-transactedreceivescope-variables.jpg)  
  
    > [!NOTE]
    > 既定で含まれているデータ変数は削除してかまいません。 既存のハンドル変数を使用することもできます。  
  
6. <xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティの**Request**セクション内に <xref:System.ServiceModel.Activities.Receive> アクティビティをドラッグアンドドロップします。 次のプロパティを設定します。  
  
    |property|[値]|  
    |--------------|-----------|  
    |CanCreateInstance|True (チェック ボックスをオンにする)|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     ワークフローは次のようになります。  
  
     ![Receive アクティビティの追加](./media/flowing-transactions-into-and-out-of-workflow-services/add-receive-activity.jpg)  
  
7. <xref:System.ServiceModel.Activities.Receive> アクティビティの **[定義...]** リンクをクリックし、次の設定を行います。  
  
     ![Receive アクティビティのメッセージ設定を設定する](./media/flowing-transactions-into-and-out-of-workflow-services/receive-message-settings.jpg)  
  
8. <xref:System.Activities.Statements.Sequence> アクティビティを <xref:System.ServiceModel.Activities.TransactedReceiveScope> の Body セクションにドラッグ アンド ドロップします。 <xref:System.Activities.Statements.Sequence> アクティビティに 2 つの <xref:System.Activities.Statements.WriteLine> アクティビティをドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを次の表のとおりに設定します。  
  
    |アクティビティ|[値]|  
    |--------------|-----------|  
    |1 つ目の WriteLine|"サービス: 受信が完了しました"|  
    |2 つ目の WriteLine|"Service: Received = " + requestMessage|  
  
     ワークフローは次のようになります。  
  
     ![WriteLine アクティビティを追加した後のシーケンス](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-writelines.jpg)  
  
9. `PrintTransactionInfo` アクティビティを、<xref:System.ServiceModel.Activities.TransactedReceiveScope> アクティビティの**本文**の2つ目の <xref:System.Activities.Statements.WriteLine> アクティビティの後にドラッグアンドドロップします。  
  
     ![PrintTransactionInfo を追加した後のシーケンス](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-printtransactioninfo.jpg )  
  
10. <xref:System.Activities.Statements.Assign> アクティビティを `PrintTransactionInfo` アクティビティの後にドラッグ アンド ドロップし、次の表のとおりにプロパティを設定します。  
  
    |property|[値]|  
    |--------------|-----------|  
    |終了|replyMessage|  
    |[値]|"Service: Sending reply."|  
  
11. <xref:System.Activities.Statements.WriteLine> アクティビティを <xref:System.Activities.Statements.Assign> アクティビティの後にドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Service: Begin reply" に設定します。  
  
     ワークフローは次のようになります。  
  
     ![Assign および WriteLine の追加後](./media/flowing-transactions-into-and-out-of-workflow-services/after-adding-sbr-writeline.jpg)  
  
12. <xref:System.ServiceModel.Activities.Receive> アクティビティを右クリックし、 **[SendReply の作成]** を選択して、最後の <xref:System.Activities.Statements.WriteLine> アクティビティの後に貼り付けます。 `SendReplyToReceive` アクティビティの **[定義...]** リンクをクリックし、次の設定を行います。  
  
     ![応答メッセージの設定](./media/flowing-transactions-into-and-out-of-workflow-services/reply-message-settings.jpg)  
  
13. <xref:System.Activities.Statements.WriteLine> アクティビティを `SendReplyToReceive` アクティビティの後にドラッグアンドドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Service: Reply sent" に設定します。  
  
14. <xref:System.Activities.Statements.WriteLine> アクティビティをワークフローの末尾にドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Service: Workflow ends, press ENTER to exit" に設定します。  
  
     完成したサービス ワークフローは次のようになります。  
  
     ![完全なサービス ワークフロー](./media/flowing-transactions-into-and-out-of-workflow-services/service-complete-workflow.jpg)  
  
### <a name="implement-the-workflow-client"></a>ワークフロー クライアントの実装  
  
1. `WorkflowClient` という新しい WCF ワークフロー アプリケーションを `Common` プロジェクトに追加します。 これを行うには、`Common` プロジェクトを右クリックし、 **[追加]** 、 **[新しい項目]** の順に選択し、 **[インストールさ]** れたテンプレート で **[ワークフロー]** を選択して **[アクティビティ]** を選択します。  
  
     ![アクティビティ プロジェクトの追加](./media/flowing-transactions-into-and-out-of-workflow-services/add-activity-project.jpg)  
  
2. <xref:System.Activities.Statements.Sequence> アクティビティをデザイン画面にドラッグ アンド ドロップします。  
  
3. <xref:System.Activities.Statements.Sequence> アクティビティを <xref:System.Activities.Statements.WriteLine> アクティビティにドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを `"Client: Workflow starting"` に設定します。 ワークフローは次のようになります。  
  
     ![WriteLine アクティビティを追加する](./media/flowing-transactions-into-and-out-of-workflow-services/add-writeline-activity.jpg)  
  
4. <xref:System.Activities.Statements.TransactionScope> アクティビティを <xref:System.Activities.Statements.WriteLine> アクティビティの後にドラッグ アンド ドロップします。  <xref:System.Activities.Statements.TransactionScope> アクティビティを選択し、[変数] をクリックして次の変数を追加します。  
  
     ![TransactionScope への変数の追加](./media/flowing-transactions-into-and-out-of-workflow-services/transactionscope-variables.jpg)  
  
5. <xref:System.Activities.Statements.Sequence> アクティビティを <xref:System.Activities.Statements.TransactionScope> アクティビティの Body セクションにドラッグ アンド ドロップします。  
  
6. `PrintTransactionInfo` アクティビティを <xref:System.Activities.Statements.Sequence> アクティビティにドラッグ アンド ドロップします。  
  
7. <xref:System.Activities.Statements.WriteLine> アクティビティを `PrintTransactionInfo` アクティビティの後にドラッグアンドドロップし、その <xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Client: from Send" に設定します。 ワークフローは次のようになります。  
  
     ![クライアントを追加しています: 送信アクティビティを開始しています](./media/flowing-transactions-into-and-out-of-workflow-services/client-add-cbs-writeline.jpg)  
  
8. <xref:System.ServiceModel.Activities.Send> アクティビティを <xref:System.Activities.Statements.Assign> アクティビティの後にドラッグ アンド ドロップし、次のプロパティを設定します。  
  
    |property|[値]|  
    |--------------|-----------|  
    |EndpointConfigurationName|workflowServiceEndpoint|  
    |OperationName|StartSample|  
    |ServiceContractName|ITransactionSample|  
  
     ワークフローは次のようになります。  
  
     ![Send アクティビティのプロパティの設定](./media/flowing-transactions-into-and-out-of-workflow-services/client-send-activity-settings.jpg)  
  
9. **[定義...]** リンクをクリックし、次の設定を行います。  
  
     ![Send アクティビティのメッセージの設定](./media/flowing-transactions-into-and-out-of-workflow-services/send-message-settings.jpg)  
  
10. <xref:System.ServiceModel.Activities.Send> アクティビティを右クリックし、 **[ReceiveReply の作成]** を選択します。 <xref:System.ServiceModel.Activities.ReceiveReply> アクティビティが <xref:System.ServiceModel.Activities.Send> アクティビティの後に自動的に配置されます。  
  
11. ReceiveReplyForSend アクティビティの [定義] リンクをクリックし、次の設定を行います。  
  
     ![ReceiveForSend メッセージの設定](./media/flowing-transactions-into-and-out-of-workflow-services/client-reply-message-settings.jpg)  
  
12. <xref:System.Activities.Statements.WriteLine> アクティビティを <xref:System.ServiceModel.Activities.Send> アクティビティと <xref:System.ServiceModel.Activities.ReceiveReply> アクティビティの間にドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Client: Send complete" に設定します。  
  
13. <xref:System.Activities.Statements.WriteLine> アクティビティを <xref:System.ServiceModel.Activities.ReceiveReply> アクティビティの後にドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Client side: Reply received = " + replyMessage に設定します。  
  
14. `PrintTransactionInfo` アクティビティを <xref:System.Activities.Statements.WriteLine> アクティビティの後にドラッグ アンド ドロップします。  
  
15. <xref:System.Activities.Statements.WriteLine> アクティビティをワークフローの末尾にドラッグ アンド ドロップし、<xref:System.Activities.Statements.WriteLine.Text%2A> プロパティを "Client workflow ends" に設定します。 完成したクライアント ワークフローは次の図のようになります。  
  
     ![完成したクライアントワークフロー](./media/flowing-transactions-into-and-out-of-workflow-services/client-complete-workflow.jpg)  
  
16. ソリューションをビルドします。  
  
### <a name="create-the-service-application"></a>サービス アプリケーションの作成  
  
1. `Service` という新しいコンソール アプリケーション プロジェクトをソリューションに追加します。 次のアセンブリへの参照を追加します。  
  
    1. System.Activities.dll  
  
    2. System.ServiceModel.dll  
  
    3. System.ServiceModel.Activities.dll  
  
2. 生成された Program.cs ファイルを開き、次のコードを追加します。  
  
    ```csharp
          static void Main()  
          {  
              Console.WriteLine("Building the server.");  
              using (WorkflowServiceHost host = new WorkflowServiceHost(new DeclarativeServiceWorkflow(), new Uri("net.tcp://localhost:8000/TransactedReceiveService/Declarative")))  
              {                
                  //Start the server  
                  host.Open();  
                  Console.WriteLine("Service started.");  
  
                  Console.WriteLine();  
                  Console.ReadLine();  
                  //Shutdown  
                  host.Close();  
              };         
          }  
    ```  
  
3. 次の app.config ファイルをプロジェクトに追加します。  
  
    ```xml  
    <?xml version="1.0" encoding="utf-8" ?>  
    <!-- Copyright © Microsoft Corporation.  All rights reserved. -->  
    <configuration>  
        <system.serviceModel>  
            <bindings>  
                <netTcpBinding>  
                    <binding transactionFlow="true" />  
                </netTcpBinding>  
            </bindings>  
        </system.serviceModel>  
    </configuration>  
    ```  
  
### <a name="create-the-client-application"></a>クライアント アプリケーションの作成  
  
1. `Client` という新しいコンソール アプリケーション プロジェクトをソリューションに追加します。 System.Activities.dll への参照を追加します。  
  
2. program.cs ファイルを開き、次のコードを追加します。  
  
    ```csharp
    class Program  
    {  

        private static AutoResetEvent syncEvent = new AutoResetEvent(false);  
  
        static void Main(string[] args)  
        {  
            //Build client  
            Console.WriteLine("Building the client.");  
            WorkflowApplication client = new WorkflowApplication(new DeclarativeClientWorkflow());  
            client.Completed = Program.Completed;  
            client.Aborted = Program.Aborted;  
            client.OnUnhandledException = Program.OnUnhandledException;  
            //Wait for service to start  
            Console.WriteLine("Press ENTER once service is started.");  
            Console.ReadLine();  
  
            //Start the client              
            Console.WriteLine("Starting the client.");  
            client.Run();  
            syncEvent.WaitOne();  
  
            //Sample complete  
            Console.WriteLine();  
            Console.WriteLine("Client complete. Press ENTER to exit.");  
            Console.ReadLine();  
        }  
  
        private static void Completed(WorkflowApplicationCompletedEventArgs e)  
        {  
            Program.syncEvent.Set();  
        }  
  
        private static void Aborted(WorkflowApplicationAbortedEventArgs e)  
        {  
            Console.WriteLine("Client Aborted: {0}", e.Reason);  
            Program.syncEvent.Set();  
        }  
  
        private static UnhandledExceptionAction OnUnhandledException(WorkflowApplicationUnhandledExceptionEventArgs e)  
        {  
            Console.WriteLine("Client had an unhandled exception: {0}", e.UnhandledException);  
            return UnhandledExceptionAction.Cancel;  
        }  
    }  
    ```  
  
## <a name="see-also"></a>関連項目

- [ワークフロー サービス](../../../../docs/framework/wcf/feature-details/workflow-services.md)
- [Windows Communication Foundation のトランザクションの概要](../../../../docs/framework/wcf/feature-details/transactions-overview.md)
