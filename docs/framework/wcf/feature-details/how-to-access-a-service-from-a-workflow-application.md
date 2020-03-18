---
title: '方法: ワークフロー アプリケーションからサービスにアクセスする'
ms.date: 03/30/2017
ms.assetid: 925ef8ea-5550-4c9d-bb7b-209e20c280ad
ms.openlocfilehash: c524dc07106f039d9dc6c17ee6fb966321b6da24
ms.sourcegitcommit: c01c18755bb7b0f82c7232314ccf7955ea7834db
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/15/2020
ms.locfileid: "75964526"
---
# <a name="how-to-access-a-service-from-a-workflow-application"></a>方法: ワークフロー アプリケーションからサービスにアクセスする
このトピックでは、ワークフロー コンソール アプリケーションからワークフロー サービスを呼び出す方法について説明します。 これは、「[方法: メッセージングアクティビティを使用してワークフローサービスを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-workflow-service-with-messaging-activities.md)」トピックの完了に依存します。 このトピックでは、ワークフローアプリケーションからワークフローサービスを呼び出す方法について説明しますが、同じメソッドを使用して、ワークフローアプリケーションから任意の Windows Communication Foundation (WCF) サービスを呼び出すこともできます。

### <a name="create-a-workflow-console-application-project"></a>ワークフロー コンソール アプリケーション プロジェクトの作成

1. Visual Studio 2012 を起動します。

2. 「[方法: メッセージングアクティビティを使用してワークフローサービスを作成](../../../../docs/framework/wcf/feature-details/how-to-create-a-workflow-service-with-messaging-activities.md)する」のトピックで作成した myのサービスプロジェクトを読み込みます。

3. **ソリューションエクスプローラー**で**my[サービス**] ソリューションを右クリックし、 **[追加]** 、 **[新しいプロジェクト]** の順に選択します。 プロジェクトの種類の一覧から、[**インストールされたテンプレート**と**ワークフローコンソールアプリケーション**] の **[ワークフロー]** を選択します。 次の図に示すように、プロジェクトに MyWFClient という名前を付け、既定の場所を使用します。

     ![[新しいプロジェクトの追加] ダイアログ](./media/how-to-access-a-service-from-a-workflow-application/add-new-project-dialog.jpg)

     **[OK]** ボタンをクリックして、[**新しいプロジェクトの追加] ダイアログボックス**を閉じます。

4. プロジェクトが作成されると、Workflow1.xaml ファイルがデザイナーで開かれます。 まだ開いていない場合は **[ツールボックス]** タブをクリックしてツールボックスを開き、プッシュピンをクリックしてツールボックスウィンドウを開いたままにします。

5. **Ctrl**+**F5**キーを押して、サービスをビルドして起動します。 以前と同様に、ASP.NET 開発サーバーが起動され、Internet Explorer に WCF ヘルプ ページが表示されます。 このページの URI は、次の手順で使用する必要があるため、確認しておいてください。

     ![WCF ヘルプページと URI を表示している IE](./media/how-to-access-a-service-from-a-workflow-application/ie-wcf-help-page-uri.jpg)

6. **ソリューションエクスプローラー**で**myのクライアント**プロジェクトを右クリックし、[ > **サービス参照**の**追加**] を選択します。 **[検出]** ボタンをクリックして、現在のソリューションで任意のサービスを検索します。 [サービス] ボックスで、Service1.xamlx の横にある三角形をクリックします。 Service1 の横にある三角形をクリックして、Service1 サービスによって実装されるコントラクトの一覧を表示します。 **サービス**の一覧で **[Service1]** ノードを展開します。 次の図に示すように、Echo 操作は**操作**の一覧に表示されます。

     ![[サービス参照の追加] ダイアログ](./media/how-to-access-a-service-from-a-workflow-application/add-service-reference.jpg)

     既定の名前空間をそのままにし、 **[OK]** をクリックして **[サービス参照の追加]** ダイアログボックスを閉じます。 次のダイアログ ボックスが表示されます。

     ![サービス参照の追加通知ダイアログ](./media/how-to-access-a-service-from-a-workflow-application/add-service-reference-dialog.jpg)

     **[OK]** をクリックしてダイアログを閉じます。 次に、Ctrl キーと Shift キーを押しながら B キーを押して、ソリューションをビルドします。 ツールボックスに、 **[servicereference1]** という名前の新しいセクションが追加されました。 この選択肢を展開して、次の図のように、追加されている Echo アクティビティを確認します。

     ![ツールボックスの Echo アクティビティ](./media/how-to-access-a-service-from-a-workflow-application/echo-activity-toolbox.jpg)

7. <xref:System.Activities.Statements.Sequence> アクティビティをデザイナー画面にドラッグ アンド ドロップします。 これは、ツールボックスの **[制御フロー]** セクションにあります。

8. <xref:System.Activities.Statements.Sequence> アクティビティをフォーカスした状態で、 **[変数]** リンクをクリックし、`inString`という名前の文字列変数を追加します。 次の図に示すように、変数に `"Hello, world"` の既定値と `outString` という名前の文字列変数を指定します。

     ![InString 変数の追加](./media/how-to-access-a-service-from-a-workflow-application/add-instring-variable.jpg)

9. **Echo**アクティビティを <xref:System.Activities.Statements.Sequence>にドラッグアンドドロップします。 次の図に示すように、[プロパティ] ウィンドウで、`inMsg` 引数を `inString` 変数に、`outMsg` 引数を `outString` 変数にバインドします。 これにより、`inString` 変数の値を操作に渡し、戻り値を取得し、その戻り値を `outString` 変数に格納します。

     ![変数への引数のバインド](./media/how-to-access-a-service-from-a-workflow-application/bind-arguments-variables.jpg)

10. **WriteLine**アクティビティを**Echo**アクティビティの下にドラッグアンドドロップして、サービス呼び出しによって返された文字列を表示します。 **WriteLine**アクティビティは、ツールボックス の **プリミティブ** ノードにあります。 Writeline アクティビティのテキストボックスに「`outString`」と入力して、 **writeline アクティビティの** **text**引数を `outString`変数にバインドします。 この時点で、ワークフローは次の図のようになります。

     ![完全なクライアントのワークフロー](./media/how-to-access-a-service-from-a-workflow-application/complete-client-workflow.jpg)

11. Myサービス ソリューションを右クリックし、**スタートアッププロジェクトの設定**... を選択します。**マルチスタートアッププロジェクト** オプションボタンを選択し、次の図に示すように、**アクション** 列の各プロジェクトに対して **開始** を選択します。

     ![スタートアップ プロジェクトのオプション](./media/how-to-access-a-service-from-a-workflow-application/startup-project-options.jpg)

12. Ctrl キーを押しながら F5 キーを押し、サービスとクライアントの両方を起動します。 ASP.NET 開発サーバーがサービスをホストし、Internet Explorer が WCF ヘルプ ページを表示し、クライアント ワークフロー アプリケーションがコンソール ウィンドウで起動され、("Hello, world") のサービスから返される文字列が表示されます。

## <a name="see-also"></a>関連項目

- [ワークフロー サービス](../../../../docs/framework/wcf/feature-details/workflow-services.md)
- [方法 : メッセージング アクティビティを使用してワークフロー サービスを作成する](../../../../docs/framework/wcf/feature-details/how-to-create-a-workflow-service-with-messaging-activities.md)
- [Web プロジェクトでのワークフローからの WCF サービスの使用](https://docs.microsoft.com/archive/blogs/endpoint/how-to-consume-a-wcf-service-from-a-wf4-workflow)
