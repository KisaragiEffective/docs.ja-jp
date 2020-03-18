---
title: '方法: IIS 上で実行する WCF Data Service を開発する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, developing
- WCF Data Services, deploying
- WCF Data Services, hosting
ms.assetid: f6f768c5-4989-49e3-a36f-896ab4ded86e
ms.openlocfilehash: 5c75425783d3468ac42ef7cb32cd9c93e812192a
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75338348"
---
# <a name="how-to-develop-a-wcf-data-service-running-on-iis"></a>方法: IIS で実行されている WCF データサービスを開発する

このトピックでは、WCF Data Services を使用して、インターネットインフォメーションサービス (IIS) 上で実行されている ASP.NET Web アプリケーションでホストされている Northwind サンプルデータベースに基づくデータサービスを作成する方法について説明します。 ASP.NET 開発サーバーで実行される ASP.NET Web アプリケーションと同じ Northwind データサービスを作成する方法の例については、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を参照してください。

> [!NOTE]
> Northwind データ サービスを作成するには、ローカル コンピューターに Northwind サンプル データベースをインストールしておく必要があります。 このサンプル データベースをダウンロードするには、ダウンロード ページ「 [SQL Server 用サンプル データベース](https://go.microsoft.com/fwlink/?linkid=24758)」を参照してください。

このトピックでは、Entity Framework プロバイダーを使用してデータ サービスを作成する方法を示します。 その他のデータ サービス プロバイダーを利用することもできます。 詳細については、「 [Data Services プロバイダー](data-services-providers-wcf-data-services.md)」を参照してください。

サービスを作成した後に、データ サービス リソースへのアクセスを明示的に提供する必要があります。 詳細については、「[方法: データサービスへのアクセスを有効](how-to-enable-access-to-the-data-service-wcf-data-services.md)にする」を参照してください。

## <a name="create-the-aspnet-web-application-that-runs-on-iis"></a>IIS で実行される ASP.NET web アプリケーションを作成する

1. Visual Studio の **[ファイル]** メニューで､ **[新規作成]**  >  **[プロジェクト]** を選択します。

2. **[新しいプロジェクト]** ダイアログボックスで、**インストールされている**> [ **C#ビジュアル**または**Visual Basic**] > **Web**カテゴリを選択します。

3. **[ASP.NET Web アプリケーション]** テンプレートを選択します。

4. プロジェクトの名前として `NorthwindService` を入力します。

5. **[OK]** をクリックします。

6. **[プロジェクト]** メニューの **[NorthwindService のプロパティ]** をクリックします。

7. **[Web]** タブを選択し、 **[ローカル IIS Web サーバーを使用する]** を選択します。

8. **[仮想ディレクトリの作成]** をクリックし、 **[OK]** をクリックします。

9. 管理特権を持つコマンド プロンプトから、次のコマンドを実行します (オペレーティング システムによって異なります)。

    - 32 ビット システム:

        ```console
        "%windir%\Microsoft.NET\Framework\v3.0\Windows Communication Foundation\ServiceModelReg.exe" -i
        ```

    - 64 ビット システム:

        ```console
        "%windir%\Microsoft.NET\Framework64\v3.0\Windows Communication Foundation\ServiceModelReg.exe" -i
        ```

     これにより、コンピューターに Windows Communication Foundation (WCF) が登録されます。

10. 管理特権を持つコマンド プロンプトから、次のコマンドを実行します (オペレーティング システムによって異なります)。

    - 32 ビット システム:

        ```console
        "%windir%\Microsoft.NET\Framework\v4.0.30319\aspnet_regiis.exe" -i -enable
        ```

    - 64 ビット システム:

        ```console
        "%windir%\Microsoft.NET\Framework64\v4.0.30319\aspnet_regiis.exe" -i -enable
        ```

     これにより、WCF がコンピューターにインストールされた後、IIS は正常に実行されます。 IIS の再起動が必要になる場合があります。

11. ASP.NET アプリケーションが IIS7 で実行されると、次の手順も実行する必要があります。

    1. IIS マネージャーを開き、 **[既定の Web サイト]** の下にある photoservice アプリケーションに移動します。

    2. **[機能ビュー]** で **[認証]** をダブルクリックします。

    3. **[認証]** ページで、 **[匿名認証]** を選択します。

    4. **[操作]** ウィンドウで **[編集]** をクリックして、匿名ユーザーがサイトに接続するときに使用するセキュリティプリンシパルを設定します。

    5. **[匿名認証資格情報の編集]** ダイアログボックスで、 **[アプリケーションプール id]** を選択します。

    > [!IMPORTANT]
    > ネットワーク サービス アカウントを使用する際、そのアカウントに関連するすべての内部ネットワーク アクセス権を匿名ユーザーに付与します。

12. SQL Server Management Studio、sqlcmd.exe ユーティリティ、または Visual Studio の Transact-SQL エディターを使用して、Northwind データベースがアタッチされた SQL Server のインスタンスに対して次の Transact-SQL コマンドを実行します。

    ```sql
    CREATE LOGIN [NT AUTHORITY\NETWORK SERVICE] FROM WINDOWS;
    GO
    ```

    これにより、IIS の実行に使用される Windows アカウントに対して、SQL Server インスタンスのログインが作成されます。 IIS は、これを使用して SQL Server インスタンスに接続できるようになります。

13. Northwind データベースをアタッチして、次の Transact-SQL コマンドを実行します。

    ```sql
    USE Northwind
    GO
    CREATE USER [NT AUTHORITY\NETWORK SERVICE]
    FOR LOGIN [NT AUTHORITY\NETWORK SERVICE] WITH DEFAULT_SCHEMA=[dbo];
    GO
    ALTER LOGIN [NT AUTHORITY\NETWORK SERVICE]
    WITH DEFAULT_DATABASE=[Northwind];
    GO
    EXEC sp_addrolemember 'db_datareader', 'NT AUTHORITY\NETWORK SERVICE'
    GO
    EXEC sp_addrolemember 'db_datawriter', 'NT AUTHORITY\NETWORK SERVICE'
    GO
    ```

    これにより、新しいログインに権限が付与され、IIS は Northwind データベースに対してデータの読み取りおよび書き込みを行うことができるようになります。

## <a name="define-the-data-model"></a>データ モデルを定義する

1. **ソリューションエクスプローラー**で、ASP.NET プロジェクトの名前を右クリックし、[ > **新しい項目**の**追加**] をクリックします。

2. **[新しい項目の追加]** ダイアログボックスで、 **[ADO.NET Entity Data Model]** を選択します。

3. データモデルの名前として「`Northwind.edmx`」と入力します。

4. Entity Data Model ウィザードで、 **[データベースから生成]** を選択し、 **[次へ]** をクリックします。

5. 次のいずれかの手順を実行して、データモデルをデータベースに接続し、 **[次へ]** をクリックします。

    - データベース接続が既に構成されていない場合は、 **[新しい接続]** をクリックして新しい接続を作成します。 詳細については、「 [How to: Create Connections to SQL Server Databases](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2008/s4yys16a(v=vs.90))」を参照してください。 この SQL Server インスタンスには、Northwind サンプル データベースがアタッチされている必要があります。

         \- または

    - Northwind データベースに接続するようにデータベース接続が既に構成されている場合は、一覧からその接続を選択します。

6. ウィザードの最終ページで、データベース内のすべてのテーブルのチェック ボックスをオンにし、ビューおよびストアド プロシージャのチェック ボックスをオフにします。

7. **[完了]** をクリックして、ウィザードを終了します。

## <a name="create-the-data-service"></a>データ サービスを作成する

1. **ソリューションエクスプローラー**で、ASP.NET プロジェクトの名前を右クリックし、[ > **新しい項目**の**追加**] をクリックします。

2. **[新しい項目の追加]** ダイアログボックスで、 **[WCF Data Service]** を選択します。

   ![Visual Studio 2015 の WCF Data Service 項目テンプレート](./media/wcf-data-service-item-template.png)

   > [!NOTE]
   > **WCF Data Service**テンプレートは visual studio 2015 で使用できますが、visual studio 2017 以降では使用できません。

3. サービスの名前として、「`Northwind`」と入力します。

     Visual Studio で新しいサービスの XML マークアップおよびコード ファイルが作成されます。 既定では、コード エディターのウィンドウが開きます。 **ソリューションエクスプローラー**では、サービスの名前は Northwind で、拡張子は svc.cs または .svc です。

4. データ サービスのコードで、データ サービスを定義するクラスの定義にあるコメント `/* TODO: put your data source class name here */` をデータ モデルのエンティティ コンテナーである型 (この場合は `NorthwindEntities`) で置き換えます。 クラス定義は次のようになります。

     [!code-csharp[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_quickstart_service/cs/northwind.svc.cs#servicedefinition)]
     [!code-vb[Astoria Quickstart Service#ServiceDefinition](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_quickstart_service/vb/northwind.svc.vb#servicedefinition)]

## <a name="see-also"></a>関連項目

- [サービスとしてのデータの公開](exposing-your-data-as-a-service-wcf-data-services.md)
