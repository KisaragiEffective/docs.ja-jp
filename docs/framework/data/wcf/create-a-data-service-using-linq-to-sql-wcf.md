---
title: '方法: LINQ to SQL データ ソースを使用してデータ サービスを作成する (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, LINQ to SQL
- WCF Data Services, providers
ms.assetid: 3b01c2fd-8c6e-4bf5-b38f-9e61bdc3c328
ms.openlocfilehash: cde5b9903a1fd164ce106a6a408ac4bb79976642
ms.sourcegitcommit: 32a575bf4adccc901f00e264f92b759ced633379
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/04/2019
ms.locfileid: "74802286"
---
# <a name="how-to-create-a-data-service-using-a-linq-to-sql-data-source-wcf-data-services"></a>方法: LINQ to SQL データ ソースを使用してデータ サービスを作成する (WCF Data Services)

WCF Data Services は、エンティティデータをデータサービスとして公開します。 リフレクションプロバイダーを使用すると、<xref:System.Linq.IQueryable%601> の実装を返すメンバーを公開するクラスに基づくデータモデルを定義できます。 データ ソース内のデータに更新を加えるには、これらのクラスも <xref:System.Data.Services.IUpdatable> インターフェイスを実装する必要があります。 詳細については、「 [Data Services プロバイダー](data-services-providers-wcf-data-services.md)」を参照してください。 このトピックでは、リフレクション プロバイダーを使用して Northwind サンプル データベースにアクセスする LINQ to SQL クラスを作成する方法と、これらのデータ クラスに基づくデータ サービスを作成する方法について説明します。

## <a name="to-add-linq-to-sql-classes-to-a-project"></a>LINQ to SQL クラスをプロジェクトに追加するには

1. C# Visual Basic またはアプリケーション内から、 **[プロジェクト]** メニューの [ > **新しい項目**の**追加**] をクリックします。

2. **[LINQ to SQL クラス]** テンプレートをクリックします。

3. 名前を「 **Northwind. .dbml**」に変更します。

4. **[追加]** をクリックします。

     Northwind.dbml ファイルがプロジェクトに追加され、オブジェクト リレーショナル デザイナー (O/R デザイナー) が開きます。

5. [**サーバー**/**データベースエクスプローラー**で、Northwind] の下の **[テーブル]** を展開し、`Customers` テーブルをオブジェクトリレーショナルデザイナー (O/R デザイナー) にドラッグします。

     `Customer` エンティティ クラスが作成され、デザイン サーフェイスに表示されます。

6. `Orders` テーブル、`Order_Details` テーブル、および `Products` テーブルに対して手順 6 を繰り返します。

7. LINQ to SQL クラスを表す新しい .dbml ファイルを右クリックし、[コードの**表示**] をクリックします。

     <xref:System.Data.Linq.DataContext> クラス (この場合は `NorthwindDataContext`) から継承するクラスの部分クラス定義を含む Northwind.cs という新しい分離コード ページが作成されます。

8. Northwind.cs コード ファイルの内容を次のコードに置き換えます。 このコードでは、<xref:System.Data.Linq.DataContext> と、および LINQ to SQL によって生成されたデータ クラスを拡張して、リフレクション プロバイダーを実装します。

     [!code-csharp[Astoria Linq Provider#Linq2SqlProvider](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_linq_provider/cs/northwind.cs#linq2sqlprovider)]
     [!code-vb[Astoria Linq Provider#Linq2SqlProvider](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_linq_provider/vb/northwind.vb#linq2sqlprovider)]

### <a name="to-create-a-data-service-by-using-a-linq-to-sql-based-data-model"></a>LINQ to SQL ベースのデータ モデルを使用してデータ サービスを作成するには

1. **ソリューションエクスプローラー**で、ASP.NET プロジェクトの名前を右クリックし、[ > **新しい項目**の**追加**] をクリックします。

2. **[新しい項目の追加]** ダイアログボックスで、 **[Web]** カテゴリの **[WCF Data Service]** テンプレートを選択します。

   ![Visual Studio 2015 の WCF Data Service 項目テンプレート](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF Data Service**テンプレートは visual studio 2015 で使用できますが、visual studio 2017 以降では使用できません。

3. サービスの名前を指定し、[ **OK]** をクリックします。

     Visual Studio で新しいサービスの XML マークアップおよびコード ファイルが作成されます。 既定では、コード エディターのウィンドウが開きます。

4. データ サービスのコードで、データ サービスを定義するクラスの定義にあるコメント `/* TODO: put your data source class name here */` をデータ モデルのエンティティ コンテナーである型 (この場合は `NorthwindDataContext`) で置き換えます。

5. データ サービスのコードで、`InitializeService` 関数のプレースホルダーのコードを次の内容で置き換えます。

     [!code-csharp[Astoria Linq Provider#EnableAccess](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_linq_provider/cs/northwind.svc.cs#enableaccess)]
     [!code-vb[Astoria Linq Provider#EnableAccess](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_linq_provider/vb/northwind.svc.vb#enableaccess)]

     指定した 3 つのエンティティ セットのリソースへのクライアント アクセスが承認されます。

6. Web ブラウザーを使用して Northwind .svc データサービスをテストするには、「 [Web ブラウザーからサービスにアクセス](accessing-the-service-from-a-web-browser-wcf-data-services-quickstart.md)する」の手順に従います。

## <a name="see-also"></a>参照

- [方法: ADO.NET Entity Framework データ ソースを使用してデータ サービスを作成する](create-a-data-service-using-an-adonet-ef-data-wcf.md)
- [方法: リフレクション プロバイダーを使用してデータ サービスを作成する](create-a-data-service-using-rp-wcf-data-services.md)
- [Data Services プロバイダー](data-services-providers-wcf-data-services.md)
