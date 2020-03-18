---
title: Visual Studio で WCF data service を作成する
ms.date: 08/24/2018
dev_langs:
- csharp
- vb
ms.assetid: 34d1d971-5e18-4c22-9bf6-d3612e27ea59
ms.openlocfilehash: 72e3b35465968674a20aa48262d3425a2190ff74
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802268"
---
# <a name="create-the-data-service"></a>データ サービスを作成する

このトピックでは、WCF Data Services を使用して、Northwind サンプルデータベースに基づく Open Data Protocol (OData) フィードを公開するサンプルデータサービスを作成します。 この作業に必要な基本手順は次のとおりです。

1. ASP.NET Web アプリケーションを作成します。

2. Entity Data Model ツールを使用して、データ モデルを定義します。

3. データ サービスを Web アプリケーションに追加します。

4. データ サービスへのアクセスを有効にします。

## <a name="create-the-aspnet-web-app"></a>ASP.NET web アプリを作成する

1. Visual Studio の **[ファイル]** メニューで､ **[新規作成]**  >  **[プロジェクト]** を選択します。

1. **新しいプロジェクト** ダイアログ ボックスで、Visual Basic または Visual C# のいずれかの選択 で、 **Web**カテゴリ、および選択**ASP.NET Web アプリケーション**します。

1. プロジェクトの名前として `NorthwindService` を入力し、[ **OK]** を選択します。

1. **[New ASP.NET Web Application]** ダイアログボックスで **[Empty]** を選択し、 **[OK]** を選択します。

1. (省略可能) Web アプリケーションに対して特定のポート番号を指定します。 注: この一連のクイックスタートトピックでは、`12345` ポート番号が使用されています。

    1. **ソリューションエクスプローラー**で、先ほど作成した ASP.NET プロジェクトを右クリックし、 **[プロパティ]** を選択します。

    2. **[Web]** タブを選択し、特定の **[ポート]** ボックスの値を `12345`に設定します。

## <a name="define-the-data-model"></a>データ モデルを定義する

1. **ソリューションエクスプローラー**で、ASP.NET プロジェクトの名前を右クリックし、[ > **新しい項目**の**追加**] をクリックします。

2. **[新しい項目の追加]** ダイアログボックスで、 **[データ]** カテゴリを選択し、 **[ADO.NET Entity Data Model]** を選択します。

3. データモデルの名前として、「`Northwind.edmx`」と入力します。

4. **Entity Data Model ウィザード**で、 **[データベースから EF Designer]** を選択し、 **[次へ]** をクリックします。

5. 次のいずれかの手順を実行して、データモデルをデータベースに接続し、 **[次へ]** をクリックします。

    - データベース接続が既に構成されていない場合は、 **[新しい接続]** をクリックして新しい接続を作成します。 詳細については、「 [How to: Create Connections to SQL Server Databases](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))」を参照してください。 この SQL Server インスタンスには、Northwind サンプル データベースがアタッチされている必要があります。

         \- または

    - Northwind データベースに接続するようにデータベース接続が既に構成されている場合は、一覧からその接続を選択します。

6. ウィザードの最終ページで、データベース内のすべてのテーブルのチェック ボックスをオンにし、ビューおよびストアド プロシージャのチェック ボックスをオフにします。

7. **[完了]** をクリックして、ウィザードを終了します。

## <a name="create-the-wcf-data-service"></a>WCF data service の作成

1. **ソリューションエクスプローラー**で、ASP.NET プロジェクトを右クリックし、[ > **新しい項目**の**追加**] を選択します。

2. **[新しい項目の追加]** ダイアログボックスで、 **Web**カテゴリから**WCF Data Service**項目テンプレートを選択します。

   ![Visual Studio 2015 の WCF Data Service 項目テンプレート](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF Data Service**テンプレートは visual studio 2015 で使用できますが、visual studio 2017 以降では使用できません。

3. サービスの名前として、「`Northwind`」と入力します。

     Visual Studio で新しいサービスの XML マークアップおよびコード ファイルが作成されます。 既定では、コード エディターのウィンドウが開きます。 **ソリューションエクスプローラー**では、サービスの名前は、 *svc.cs*または *.svc*という名前になります。

4. データ サービスのコードで、データ サービスを定義するクラスの定義にあるコメント `/* TODO: put your data source class name here */` をデータ モデルのエンティティ コンテナーである型 (この場合は `NorthwindEntities`) で置き換えます。 クラス定義は次のようになります。

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## <a name="enable-access-to-data-service-resources"></a>データサービスリソースへのアクセスを有効にする

1. データ サービスのコードで、`InitializeService` 関数のプレースホルダーのコードを次の内容で置き換えます。

     [!code-csharp[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#allreadconfig)]
     [!code-vb[Astoria Quickstart Service#AllReadConfig](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#allreadconfig)]

     これにより、承認されたクライアントは、指定したエンティティ セットのリソースに読み取りおよび書き込みアクセスできるようになります。

    > [!NOTE]
    > ASP.NET アプリケーションにアクセスできるクライアントは、データ サービスによって公開されるリソースにもアクセスできます。 運用データ サービスで、リソースへの承認されていないアクセスを防止するために、アプリケーション自身もセキュリティで保護する必要があります。 詳細については、「 [Securing WCF Data Services](securing-wcf-data-services.md)」を参照してください。

## <a name="next-steps"></a>次のステップ:

ここでは、Northwind サンプルデータベースに基づく OData フィードを公開する新しいデータサービスを作成し、ASP.NET Web アプリケーションに対するアクセス許可を持つクライアントに対してフィードへのアクセスを有効にしました。 次に、Visual Studio からデータサービスを開始し、Web ブラウザーから HTTP GET 要求を送信して OData フィードにアクセスします。

> [!div class="nextstepaction"]
> [Web ブラウザーからサービスにアクセスする](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)

## <a name="see-also"></a>参照

- [ADO.NET Entity Data Model ツール](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
