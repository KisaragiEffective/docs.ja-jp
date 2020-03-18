---
title: ワークフローからの OData フィードの使用-WF
ms.date: 03/30/2017
ms.assetid: 1b26617c-53e9-476a-81af-675c36d95919
ms.openlocfilehash: ceac2c2d07351fcb79e2345068f07fa22f356411
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743794"
---
# <a name="consuming-odata-feeds-from-a-workflow"></a>ワークフローからの OData フィードの使用

WCF Data Services は、Representational State Transfer (REST) のセマンティクスを使用して、Web またはイントラネット上のデータを公開および使用するための Open Data Protocol (OData) を使用するサービスを作成できるようにする、.NET Framework のコンポーネントです。 OData は、URI でアドレス指定できるリソースとしてデータを公開します。 HTTP 要求を送信し、データ サービスが返す OData フィードを処理できるのであれば、どのようなアプリケーションでも OData ベースのデータ サービスと対話できます。 さらに、WCF Data Services には、.NET Framework アプリケーションから OData フィードを使用するときに、より高度なプログラミングエクスペリエンスを提供するクライアントライブラリが含まれています。 このトピックでは、クライアント ライブラリを使用した場合と使用しない場合のワークフローでの OData フィードの使用の概要について説明します。

## <a name="using-the-sample-northwind-odata-service"></a>サンプルの Northwind OData サービスの使用

このトピックの例では、<https://services.odata.org/Northwind/Northwind.svc/>にあるサンプルの Northwind データサービスを使用します。 このサービスは [OData SDK](https://www.odata.org/ecosystem/#sdk) に含まれており、Northwind データベース サンプルへの読み取り専用アクセスを提供します。 書き込みアクセスが必要な場合、またはローカルの WCF Data Service が必要な場合は、「 [クイック スタート (WCF Data Services)](../data/wcf/quickstart-wcf-data-services.md) 」の手順に従って、Northwind データベースへのアクセスを提供するローカルの OData サービスを作成できます。 クイックスタートの手順に従う場合は、このトピックのコード例に指定されている URI をローカルの URI に置き換えてください。

## <a name="consuming-an-odata-feed-using-the-client-libraries"></a>クライアントライブラリを使用した OData フィードの使用

WCF Data Services には、.NET Framework とクライアントアプリケーションから OData フィードを簡単に使用できるようにするクライアントライブラリが含まれています。 これらのライブラリは、HTTP メッセージの送受信を簡略化します。 また、メッセージ ペイロードをエンティティ データを表す CLR オブジェクトに変換します。 クライアント ライブラリには、 <xref:System.Data.Services.Client.DataServiceContext> および <xref:System.Data.Services.Client.DataServiceQuery%601>という 2 つのコア クラスがあります。 これらのクラスを使用すると、データ サービスをクエリして、返されるエンティティ データを CLR オブジェクトとして処理できます。 ここではクライアント ライブラリを使用するアクティビティを作成するための 2 つの方法を説明します。

### <a name="adding-a-service-reference-to-the-wcf-data-service"></a>WCF Data service へのサービス参照の追加

Northwind クライアントライブラリを生成するには、Visual Studio 2012 の **[サービス参照の追加]** ダイアログボックスを使用して、northwind OData サービスへの参照を追加します。

![[サービス参照の追加] ダイアログボックスが表示されたスクリーンショット。](./media/consuming-odata-feeds-from-a-workflow/add-service-reference-dialog.gif)

サービスによって公開されるサービス操作はなく、 **[サービス]** ボックスの一覧には Northwind データ サービスによって公開されるエンティティを表す項目が含まれていることに注意してください。 サービス参照を追加すると、これらのエンティティに対するクラスが生成され、クライアント コードで使用できるようになります。 このトピックの例ではこれらのクラスと `NorthwindEntities` クラスを使用してクエリを実行します。

> [!NOTE]
> 詳細については、「[データサービスクライアントライブラリの生成 (WCF Data Services)](../data/wcf/generating-the-data-service-client-library-wcf-data-services.md)」を参照してください。

### <a name="using-asynchronous-methods"></a>非同期メソッドの使用

Web のリソースにアクセスするときに発生することのある、待機時間に伴う問題に対処するために、WCF Data Services には非同期でアクセスすることをお勧めします。 WCF Data Services クライアントライブラリには、クエリを呼び出すための非同期メソッドが含まれています。 Windows Workflow Foundation (WF) には、非同期アクティビティを作成するための <xref:System.Activities.AsyncCodeActivity> クラスが用意されています。 <xref:System.Activities.AsyncCodeActivity> 派生アクティビティは、非同期メソッドを持つ .NET Framework クラスを利用するために記述できます。また、非同期に実行されるコードをメソッドに格納し、デリゲートを使用して呼び出すこともできます。 ここでは、 <xref:System.Activities.AsyncCodeActivity> 派生アクティビティの例を 2 つ紹介します。1 つは WCF Data Services クライアント ライブラリの非同期メソッドを使用し、もう 1 つはデリゲートを使用しています。

> [!NOTE]
> 詳細については、「[非同期操作 (WCF Data Services)](../data/wcf/asynchronous-operations-wcf-data-services.md) 」および「[非同期アクティビティの作成](creating-asynchronous-activities-in-wf.md)」を参照してください。

### <a name="using-client-library-asynchronous-methods"></a>クライアントライブラリの非同期メソッドの使用

<xref:System.Data.Services.Client.DataServiceQuery%601> クラスには、OData サービスを非同期で照会するための <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> メソッドと <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> メソッドが用意されています。 これらのメソッドは、 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> 派生クラスの <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> オーバーライドと <xref:System.Activities.AsyncCodeActivity> オーバーライドから呼び出すことができます。 <xref:System.Activities.AsyncCodeActivity> <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> のオーバーライドが返されると、ワークフローはアイドル状態になることがあります (永続化はできません)。また、非同期処理が完了すると、ランタイムによって <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> が呼び出されます。

次の例では、2 つの入力引数がある `OrdersByCustomer` アクティビティが定義されています。 引数 `CustomerId` は、返す注文を識別する顧客を表し、引数 `ServiceUri` は、照会する OData サービスの URI を表します。 アクティビティは `AsyncCodeActivity<IEnumerable<Order>>` から派生するので、クエリ結果を返すために使用される <xref:System.Activities.Activity%601.Result%2A> 出力引数も存在します。 <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> オーバーライドは、指定された顧客の注文をすべて選択する LINQ クエリを作成します。 このクエリは、渡された <xref:System.Activities.AsyncCodeActivityContext.UserState%2A> の <xref:System.Activities.AsyncCodeActivityContext>として指定され、その後クエリの <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> メソッドが呼び出されます。 クエリの <xref:System.Data.Services.Client.DataServiceQuery%601.BeginExecute%2A> に渡されるコールバックと状態は、アクティビティの <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> メソッドに渡されるものであることに注意してください。 クエリの実行が完了すると、アクティビティの <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> メソッドが呼び出されます。 クエリは <xref:System.Activities.AsyncCodeActivityContext.UserState%2A>から取得され、その後クエリの <xref:System.Data.Services.Client.DataServiceQuery%601.EndExecute%2A> メソッドが呼び出されます。 このメソッドは、指定されたエンティティ型の <xref:System.Collections.Generic.IEnumerable%601> を返します。この場合は `Order`になります。 `IEnumerable<Order>` は <xref:System.Activities.AsyncCodeActivity%601>のジェネリック型であるため、この <xref:System.Collections.IEnumerable> はアクティビティの <xref:System.Activities.Activity%601.Result%2A> <xref:System.Activities.OutArgument%601> として設定されます。

[!code-csharp[CFX_WCFDataServicesActivityExample#100](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#100)]

次の例では、指定された顧客の注文一覧を `OrdersByCustomer` アクティビティが取得した後、返された注文を <xref:System.Activities.Statements.ForEach%601> アクティビティが列挙し、各注文の日付をコンソールに書き込みます。

[!code-csharp[CFX_WCFDataServicesActivityExample#10](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#10)]

このワークフローを呼び出すと、次のデータがコンソールに書き込まれます。

```console
Calling WCF Data Service...
8/25/1997
10/3/1997
10/13/1997
1/15/1998
3/16/1998
4/9/1998
```

> [!NOTE]
> OData サーバーへの接続を確立できない場合は、次のような例外が表示されます。
>
> ハンドルされていない例外: System.InvalidOperationException: この要求の処理中にエラーが発生しました。 ---> System.Net.WebException: リモート サーバーに接続できません。---> System.Net.Sockets.SocketException: 接続済みの呼び出し先が一定の時間を過ぎても正しく応答しなかったため、接続できませんでした。または接続済みのホストが応答しなかったため、確立された接続は失敗しました。

クエリによって返されたデータをさらに処理する必要がある場合は、アクティビティの <xref:System.Activities.AsyncCodeActivity%601.EndExecute%2A> オーバーライドで実行できます。 <xref:System.Activities.AsyncCodeActivity%601.BeginExecute%2A> と <xref:System.Activities.AsyncCodeActivity%601.EndExecute%2A> はどちらもワークフロー スレッドを使用して呼び出されます。これらのオーバーライド内のコードは非同期では実行されません。 追加の処理が大規模なものであるか、時間がかかるか、クエリ結果がページングされる場合は、次のセクションで説明する方法を検討してください。この方法では、デリゲートを使用してクエリを実行し、追加の処理を非同期で行います。

### <a name="using-a-delegate"></a>デリゲートの使用

.NET Framework クラスの非同期メソッドを呼び出すだけでなく、<xref:System.Activities.AsyncCodeActivity>ベースのアクティビティでは、そのメソッドのいずれかで非同期ロジックを定義することもできます。 このメソッドは、アクティビティの <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> オーバーライド内でデリゲートを使用することで指定します。 このメソッドが戻ると、ランタイムによってアクティビティの <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> オーバーライドが呼び出されます。 OData サービスをワークフローから呼び出すときは、このメソッドを使用してサービスを照会し、追加の処理を実行できます。

次の例では、 `ListCustomers` アクティビティを定義します。 このアクティビティは、Northwind データ サービス サンプルを照会し、Northwind データベース内の顧客をすべて含む `List<Customer>` を返します。 非同期操作は `GetCustomers` メソッドによって実行されます。 このメソッドは、サービスに対してすべての顧客を照会し、これらの顧客を `List<Customer>`にコピーします。 次に、結果がページングされているかどうかを確認します。 ページングされている場合は、サービスに対して結果の次のページを照会し、それを一覧に追加します。処理は顧客データをすべて取得するまで続行されます。

> [!NOTE]
> WCF Data Services でのページングの詳細については、「[方法: ページングされた結果を読み込む (WCF Data Services)](../data/wcf/how-to-load-paged-results-wcf-data-services.md)」を参照してください。

顧客がすべて追加されると、一覧が返されます。 `GetCustomers` メソッドはアクティビティの <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A> オーバーライドで指定されます。 メソッドには戻り値があるので、メソッドを指定するために `Func<string, List<Customer>>` が作成されます。

> [!NOTE]
> 非同期操作を実行するメソッドに戻り値がない場合は、<xref:System.Action> の代わりに <xref:System.Func%601> が使用されます。 両方の方法を使用して非同期の例を作成する例については、「[非同期アクティビティの作成](creating-asynchronous-activities-in-wf.md)」を参照してください。

この <xref:System.Func%601> は <xref:System.Activities.AsyncCodeActivityContext.UserState%2A> に割り当てられ、`BeginInvoke` が呼び出されます。 呼び出すメソッドはアクティビティの引数の環境にアクセスできないので、引数 `ServiceUri` 値は <xref:System.Activities.AsyncCodeActivity.BeginExecute%2A>に渡されたコールバックおよび状態と共に、最初のパラメーターとして渡されます。 `GetCustomers` が戻ると、ランタイムによって <xref:System.Activities.AsyncCodeActivity.EndExecute%2A>が呼び出されます。 <xref:System.Activities.AsyncCodeActivity.EndExecute%2A> 内のコードはデリゲートを <xref:System.Activities.AsyncCodeActivityContext.UserState%2A>から取得し、 `EndInvoke`を呼び出し、結果を返します。この結果は、 `GetCustomers` メソッドから返された顧客の一覧です。

[!code-csharp[CFX_WCFDataServicesActivityExample#200](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#200)]

次の例では、顧客の一覧を `ListCustomers` アクティビティが取得した後、その一覧を <xref:System.Activities.Statements.ForEach%601> アクティビティが列挙し、各顧客の会社名と連絡先名をコンソールに書き込みます。

[!code-csharp[CFX_WCFDataServicesActivityExample#20](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#20)]

このワークフローを呼び出すと、次のデータがコンソールに書き込まれます。 このクエリは多くの顧客を返すので、ここでは出力の一部のみが表示されています。

```console
Calling WCF Data Service...
Alfreds Futterkiste, Contact: Maria Anders
Ana Trujillo Emparedados y helados, Contact: Ana Trujillo
Antonio Moreno Taquería, Contact: Antonio Moreno
Around the Horn, Contact: Thomas Hardy
Berglunds snabbköp, Contact: Christina Berglund
...
```

## <a name="consuming-an-odata-feed-without-using-the-client-libraries"></a>クライアントライブラリを使用せずに OData フィードを使用する

OData は、URI でアドレス指定できるリソースとしてデータを公開します。 クライアント ライブラリを使用するとこれらの URI が自動的に作成されますが、必ずしもクライアント ライブラリを使用する必要はありません。 OData サービスはクライアント ライブラリを使用せずに直接アクセスすることもできます。 クライアント ライブラリを使用しない場合、サービスの場所と必要なデータは URI によって指定され、結果が HTTP 要求への応答の中で返されます。 この生データはその後自由に処理または操作できます。 OData クエリの結果を取得する方法の 1 つとして、 <xref:System.Net.WebClient> クラスを使用する方法があります。 この例では、キー ALFKI が表す顧客の連絡先名が取得されます。

[!code-csharp[CFX_WCFDataServicesActivityExample#2](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#2)]

このコードが実行されると、次の出力がコンソールに表示されます。

```console
Raw data returned:
<?xml version="1.0" encoding="utf-8" standalone="yes"?>
<ContactName xmlns="http://schemas.microsoft.com/ado/2007/08/dataservices">Maria Anders</ContactName>
```

ワークフローでは、この例のコードを <xref:System.Activities.CodeActivity.Execute%2A> ベースのカスタム アクティビティの <xref:System.Activities.CodeActivity> オーバーライドに組み込むことができますが、<xref:System.Activities.Expressions.InvokeMethod%601> アクティビティを使用して同じ機能を実行することもできます。 <xref:System.Activities.Expressions.InvokeMethod%601> アクティビティは、ワークフローの作成者がクラスの静的メソッドやインスタンス メソッドを呼び出すことができるようにします。また、指定したメソッドを非同期で呼び出すオプションもあります。 次の例では、 <xref:System.Activities.Expressions.InvokeMethod%601> アクティビティは <xref:System.Net.WebClient.DownloadString%2A> クラスの <xref:System.Net.WebClient> メソッドを呼び出して顧客一覧を返すように構成されています。

[!code-csharp[CFX_WCFDataServicesActivityExample#3](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#3)]

<xref:System.Activities.Expressions.InvokeMethod%601> は、クラスの静的メソッドとインスタンス メソッドの両方を呼び出すことができます。 <xref:System.Net.WebClient.DownloadString%2A> は <xref:System.Net.WebClient> クラスのインスタンス メソッドであるため、 <xref:System.Net.WebClient> のために <xref:System.Activities.Expressions.InvokeMethod%601.TargetObject%2A>クラスの新規インスタンスが指定されます。 `DownloadString` は <xref:System.Activities.Expressions.InvokeMethod%601.MethodName%2A>として指定され、クエリを含む URI は <xref:System.Activities.Expressions.InvokeMethod%601.Parameters%2A> のコレクションで指定され、戻り値が <xref:System.Activities.Activity%601.Result%2A> の値に指定されます。 <xref:System.Activities.Expressions.InvokeMethod%601.RunAsynchronously%2A> 値は `true`に設定されます。これは、ワークフローに関するメソッドの呼び出しが非同期で実行されることを意味します。 次の例では、 <xref:System.Activities.Expressions.InvokeMethod%601> アクティビティを使用するワークフローが作成され、Northwind データ サービス サンプルに対して特定の顧客の注文一覧が照会されます。返されたデータはコンソールに書き込まれます。

[!code-csharp[CFX_WCFDataServicesActivityExample#1](~/samples/snippets/csharp/VS_Snippets_CFX/CFX_WCFDataServicesActivityExample/cs/Program.cs#1)]

このワークフローが呼び出されると、次の出力がコンソールに表示されます。 このクエリは複数の注文を返すので、ここでは出力の一部のみが表示されています。

```console
Calling WCF Data Service...
Raw data returned:

<?xml version="1.0" encoding="utf-8" standalone="yes"?>*
<feed
xml:base="http://services.odata.org/Northwind/Northwind.svc/"
xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices"
xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata"
xmlns="http://www.w3.org/2005/Atom">
<title type="text">Orders\</title>
<id>http://services.odata.org/Northwind/Northwind.svc/Customers('ALFKI')/Orders\</id>
<updated>2010-05-19T19:37:07Z\</updated>
<link rel="self" title="Orders" href="Orders" />
<entry>
<id>http://services.odata.org/Northwind/Northwind.svc/Orders(10643)\</id>
<title type="text">\</title>
<updated>2010-05-19T19:37:07Z\</updated>
<author>
<name />
</author>
<link rel="edit" title="Order" href="Orders(10643)" />
<link rel="http://schemas.microsoft.com/ado/2007/08/dataservices/related/Customer" type="application/atom+xml;type=entry" title="Customer" href="Orders(10643)/Customer" />
...
```

この例は、ワークフロー アプリケーションの作成者が OData サービスから返された生データを使用できる方法の 1 つを示しています。 Uri を使用した WCF Data Services へのアクセスの詳細については、「[データサービスリソースへのアクセス (WCF Data Services)](../data/wcf/accessing-data-service-resources-wcf-data-services.md) 」および「 [OData: URI 規則](https://www.odata.org/documentation/odata-version-2-0/uri-conventions/)」を参照してください。
