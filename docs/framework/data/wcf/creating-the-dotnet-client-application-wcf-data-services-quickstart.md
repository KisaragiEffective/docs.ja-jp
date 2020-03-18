---
title: .NET Framework クライアント アプリケーションの作成 (WCF Data Services クイック スタート)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 41ade767-eeab-437d-9121-9797e8fb8045
ms.openlocfilehash: 4beaba24e42b15ebc45ece6e5319a2b14df54ab6
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73975381"
---
# <a name="creating-the-net-framework-client-application-wcf-data-services-quickstart"></a>.NET Framework クライアント アプリケーションの作成 (WCF Data Services クイック スタート)

これは、WCF Data Services のクイックスタートの最後のタスクです。 このタスクでは、コンソールアプリケーションをソリューションに追加し、この新しいクライアントアプリケーションに Open Data Protocol (OData) フィードへの参照を追加します。生成されたクライアントデータサービスクラスとクライアントを使用して、クライアントアプリケーションから OData フィードにアクセスします。ライブラリ.

> [!NOTE]
> データ フィードへのアクセスには .NET Framework ベースのクライアント アプリケーションは必要ありません。 データサービスには、OData フィードを使用する任意のアプリケーションコンポーネントからアクセスできます。 詳細については、「[クライアントアプリケーションでのデータサービスの使用](using-a-data-service-in-a-client-application-wcf-data-services.md)」を参照してください。

## <a name="to-create-the-client-application-by-using-visual-studio"></a>Visual Studio を使用してクライアント アプリケーションを作成するには

1. **ソリューションエクスプローラー**で、ソリューションを右クリックし、 **[追加]** をクリックして、 **[新しいプロジェクト]** をクリックします。

2. 左側のウィンドウで、**インストールされ**た > [ **C#ビジュアル**または**Visual Basic**] > **[Windows デスクトップ]** の順に選択し、 **[WPF アプリケーション]** テンプレートを選択します。

3. プロジェクト名として `NorthwindClient` を入力し、[ **OK]** をクリックします。

4. ファイル MainWindow.xaml を開き、XAML コードを次のコードに置き換えます。

     [!code-xaml[Astoria Quickstart Client#Window1Xaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml#window1xaml)]

## <a name="to-add-a-data-service-reference-to-the-project"></a>データ サービス参照をプロジェクトに追加するには

1. **ソリューションエクスプローラー**で、NorthwindClient プロジェクトを右クリックし、[ > **サービス参照**の**追加**] をクリックして、 **[探索]** をクリックします。

     最初のタスクで作成した Northwind データ サービスが表示されます。

2. **[名前空間]** テキストボックスに「`Northwind`」と入力し、[ **OK]** をクリックします。

     プロジェクトに新しいコード ファイルが追加されます。このコード ファイルには、データ サービス リソースにアクセスし、オブジェクトとしてデータ サービス リソースと対話するデータ クラスが含まれています。 データ クラスは、名前空間 `NorthwindClient.Northwind` で作成されます。

## <a name="to-access-data-service-data-in-the-wpf-application"></a>WPF アプリケーションのデータ サービスにアクセスするには

1. **[NorthwindClient]** の下の**ソリューションエクスプローラー**で、プロジェクトを右クリックし、 **[参照の追加]** をクリックします。

2. **参照の追加** ダイアログボックスで、 **.net** タブをクリックし、system.servicemodel アセンブリを選択して、 **OK** をクリックします。

3. **NorthwindClient**の下の**ソリューションエクスプローラー**で、mainwindow.xaml ファイルのコードページを開き、次の `using` ステートメント (Visual Basic で`Imports`) を追加します。

    [!code-csharp[Astoria Quickstart Client#Using](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#using)]
    [!code-vb[Astoria Quickstart Client#Using](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#using)]

4. 次のコードを挿入します。このコードは、データ サービスをクエリし、<xref:System.Data.Services.Client.DataServiceCollection%601> に対する結果を `MainWindow` クラスにバインドします。

    > [!NOTE]
    > ホスト名 `localhost:12345` を Northwind データ サービスのインスタンスをホストするサーバーとポートで置き換えます。

     [!code-csharp[Astoria Quickstart Client#QueryCode](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#querycode)]
     [!code-vb[Astoria Quickstart Client#QueryCode](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#querycode)]

5. 次のコードを挿入します。このコードは、変更内容を `MainWindow` クラスに保存します。

     [!code-csharp[Astoria Quickstart Client#SaveChanges](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_client/cs/window1.xaml.cs#savechanges)]
     [!code-vb[Astoria Quickstart Client#SaveChanges](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_client/vb/window1.xaml.vb#savechanges)]

## <a name="to-build-and-run-the-northwindclient-application"></a>NorthwindClient アプリケーションをビルドして実行するには

1. **ソリューションエクスプローラー**で、NorthwindClient プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択します。

2. **F5**キーを押してアプリケーションを起動します。

     ソリューションがビルドされ、クライアント アプリケーションが起動します。 データがサービスから要求され、コンソールに表示されます。

3. データグリッドの**Quantity**列の値を編集し、 **[保存]** をクリックします。

     変更内容はデータ サービスに保存されます。

    > [!NOTE]
    > このバージョンの NorthwindClient アプリケーションでは、エンティティの追加と削除はサポートされません。

## <a name="next-steps"></a>次のステップ

これで、サンプルの Northwind OData フィードにアクセスするクライアントアプリケーションが正常に作成されました。 WCF Data Services クイックスタートも完了しました。

.NET Framework アプリケーションから OData フィードにアクセスする方法の詳細については、「 [WCF Data Services クライアントライブラリ](wcf-data-services-client-library.md)」を参照してください。

## <a name="see-also"></a>関連項目

- [はじめに](getting-started-with-wcf-data-services.md)
- [リソース](wcf-data-services-resources.md)
