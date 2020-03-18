---
title: CQRS マイクロサービスに読み取り/クエリを実装する
description: コンテナー化された .NET アプリケーション用の .NET マイクロサービス アーキテクチャ | Dapper を使用した eShopOnContainers でのオーダリング マイクロサービスの CQRS のクエリ側の実装を理解する。
ms.date: 10/08/2018
ms.openlocfilehash: 235b0e471a17e2a37a883a111cf499b7837f3ea1
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73972084"
---
# <a name="implement-readsqueries-in-a-cqrs-microservice"></a>CQRS マイクロサービスに読み取り/クエリを実装する

読み取り/クエリの場合、eShopOnContainers 参照アプリケーションのオーダリング マイクロサービスでは、DDD モデルおよびトランザクション領域とは別にクエリを実装します。 この主な理由は、クエリとトランザクションの要求が大幅に異なるためです。 書き込みでは、ドメイン ロジックに準拠する必要があるトランザクションが実行されます。 一方、クエリはべき等であり、ドメイン ルールから分離することができます。

図 7-3 に示すように、方法は単純です。 API インターフェイスは、Dapper のようなマイクロ オブジェクト リレーショナル マッパー (ORM) などの任意のインフラストラクチャを使用して、UI アプリケーションのニーズに応じて動的な ViewModel を返すことで Web API コントローラーによって実装されます。

![簡略化された CQRS における上位レベルのクエリ側を示す図。](./media/cqrs-microservice-reads/simple-approach-cqrs-queries.png)

**図 7-3**。 CQRS マイクロサービスでのクエリの最も単純な方法

単純化された CQRS アプローチのクエリ側の最も単純なアプローチは、動的な ViewModels を返す Micro-ORM のような Dapper を使用して、データベースをクエリすることによって実装できます。 クエリ定義ではデータベースをクエリし、各クエリに対してオンザフライでビルドされた動的な ViewModel を返します。 クエリはべき等であるため、クエリの実行回数に関係なく、データが変更されることはありません。 したがって、トランザクション側で使用される DDD パターン (集計などのパターン) によって制限される必要はありません。これが、クエリがトランザクション領域から分離される理由です。 UI に必要なデータについてデータベースをクエリし、SQL ステートメント自体を除く、任意の場所 (ViewModel のクラスがない) で静的に定義する必要のない動的な ViewModel を返すだけです。

これは単純な方法であるため、クエリ側で必要なコード ([Dapper](https://github.com/StackExchange/Dapper) のようなマイクロ ORM を使用するコードなど) を[同じ Web API プロジェクト内で](https://github.com/dotnet-architecture/eShopOnContainers/blob/master/src/Services/Ordering/Ordering.API/Application/Queries/OrderQueries.cs)実装することができます。 これは図 7-4 に示されています。 クエリは eShopOnContainers 内の **Ordering.API** マイクロサービス プロジェクトで定義されています。

![Ordering.API プロジェクトの Queries フォルダーのスクリーンショット。](./media/cqrs-microservice-reads/ordering-api-queries-folder.png)

**図 7-4**。 eShopOnContainers でのオーダリング マイクロサービスのクエリ

## <a name="use-viewmodels-specifically-made-for-client-apps-independent-from-domain-model-constraints"></a>ドメイン モデル制約とは別の、クライアント アプリ専用の ViewModel の使用

クエリはクライアント アプリケーションで必要なデータを取得するために実行されるため、戻り値の型は、クエリによって返されるデータに基づいて、クライアント専用に作成することができます。 これらのモデル、またはデータ転送オブジェクト (DTO)、は ViewModel と呼ばれます。

返されるデータ (ViewModel) は、データベースの複数のエンティティまたはテーブルからの、あるいはトランザクション領域のドメイン モデルで定義されている複数の集計全体のデータの結合結果である場合があります。 ここでは、ドメイン モデルには依存しないクエリを作成するため、集計の境界と制約は完全に無視され、必要になる可能性のあるテーブルと列をクエリするのは自由です。 この方法では、開発者がクエリの作成または更新を行う場合に優れた柔軟性と生産性が提供されます。

ViewModel として、クラスで定義されている静的な型を指定することができます。 または、実行 (オーダリング マイクロサービスで実装) されたクエリに基づいて動的に作成することもできます。これは、開発者にとって非常にアジャイルな方法です。

## <a name="use-dapper-as-a-micro-orm-to-perform-queries"></a>クエリを実行するためのマイクロ ORM としての Dapper の使用

クエリでは、任意のマイクロ ORM、Entity Framework Core、またはプレーン ADO.NET を使用することができます。 サンプル アプリケーションでは、一般的なマイクロ ORM の良い例として、eShopOnContainers でのオーダリング マイクロサービスに対して Dapper が選択されました。 これは非常に軽量なフレームワークであるため、パフォーマンスの優れたプレーン SQL クエリを実行できます。 Dapper を使用することで、複数のテーブルのアクセスと結合が可能な SQL クエリを記述できます。

Dapper はオープンソースのプロジェクト (Sam Saffron によって最初に作成された) であり、[スタック オーバーフロー](https://stackoverflow.com/)で使用される構成要素の一部です。 Dapper を使用するために必要になるのは、次の図に示すように、[Dapper NuGet パッケージ](https://www.nuget.org/packages/Dapper)を使用してインストールすることだけです。

![[NuGet パッケージ] ビューの Dapper パッケージのスクリーンショット。](./media/cqrs-microservice-reads/drapper-package-nuget.png)

コードで Dapper 拡張メソッドにアクセスできるようにするために、ステートメントを追加する必要もあります。

コードで Dapper を使用する場合は、<xref:System.Data.SqlClient> 名前空間で使用可能な <xref:System.Data.SqlClient.SqlConnection> クラスを直接使用します。 QueryAsync メソッドと、<xref:System.Data.SqlClient.SqlConnection> クラスを拡張するその他の拡張メソッドを使用して、簡単でパフォーマンスの高い方法でクエリを実行するだけで済みます。

## <a name="dynamic-versus-static-viewmodels"></a>動的な ViewModel と静的な ViewModel

ViewModel をサーバー側からクライアント アプリに返す場合、エンティティ モデルの内部ドメイン エンティティとは異なる可能性のある DTO (Data Transfer Object) として ViewModel を考えることができます。これは、ViewModel がクライアント アプリで必要な方法でデータを保持するためです。 したがって、多くの場合、複数のドメイン エンティティからのデータを集計し、クライアント アプリでそのデータがいかに必要であるかに従って ViewModel を正確に構成できます。

これらの ViewModel または DTO は、後述のコード スニペットで示す `OrderSummary` クラスと同様に (データ ホルダー クラスとして) 明示的に定義できます。あるいは、動的な型として、クエリによって返される属性に基づいて、単に動的な ViewModel または動的な DTO を返すこともできます。

### <a name="viewmodel-as-dynamic-type"></a>動的な型としての ViewModel

次のコードに示すように、`ViewModel` は、内部的にクエリによって返される属性に基づく*動的な*型を単に返すことで、クエリで直接返すことができます。 これは、返される属性のサブセットがクエリ自体に基づいていることを意味します。 そのため、クエリまたは結合に新しい列を追加する場合、そのデータは返される `ViewModel` に動的に追加されます。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
    public async Task<IEnumerable<dynamic>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<dynamic>(
                @"SELECT o.[Id] as ordernumber,
                o.[OrderDate] as [date],os.[Name] as [status],
                SUM(oi.units*oi.unitprice) as total
                FROM [ordering].[Orders] o
                LEFT JOIN[ordering].[orderitems] oi ON o.Id = oi.orderid
                LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                GROUP BY o.[Id], o.[OrderDate], os.[Name]");
        }
    }
}
```

重要な点は、動的な型を使用することで、返されるデータ コレクションが ViewModel として動的にアセンブルされることです。

**長所 :** この方法では、クエリの SQL 文を更新するたびに静的な ViewModel クラスを変更する必要性が低くなります。したがって、この設計方法は非常にアジャイルなコーディング方法であり、簡単で、将来の変更に合わせてすばやく進化します。

**短所 :** 長期的に見ると、動的な型はクライアント アプリのサービスの明確性と互換性に悪影響を与える場合があります。 さらに、Swashbuckle のようなミドルウェア ソフトウェアでは、動的な型を使用する場合に戻り値の型で同じレベルのドキュメントを提供できません。

### <a name="viewmodel-as-predefined-dto-classes"></a>定義済み DTO クラスとしての ViewModel

**長所** :明示的な DTO クラスに基づく "コントラクト" のように、静的な定義済み ViewModel クラスを使用することは、パブリック API だけでなく、長期的なマイクロサービスにも確実に適しています。同じアプリケーションでのみ使用される場合でも同様です。

Swagger の応答型を指定する場合は、戻り値の型として明示的な DTO クラスを使用する必要があります。 したがって、定義済み DTO クラスを使用すれば、Swagger からより豊富な情報を提供することができます。 これにより、API 利用時の API のドキュメントと互換性が改善されます。

**短所** :前述のとおり、コードを更新する場合、DTO クラスを更新するためにさらにいくつかの手順を実行します。

*経験に基づくヒント:* 開発段階の初期には非常に簡単でアジャイルであるため、eShopOnContainers のオーダリング マイクロサービスに実装したクエリには、動的な ViewModel を使用して開発を開始しました。 しかし、開発が安定した後で、API をリファクタリングし、ViewModel に静的な、または定義済みの DTO を使用することにしました。これは、マイクロサービスのコンシューマーが、"コントラクト" として使用される、明示的な DTO 型を認識しやすいためです。

次の例では、明示的な ViewModel DTO クラスである OrderSummary クラスを使用して、クエリでどのようにデータが返されるかを確認できます。

```csharp
using Dapper;
using Microsoft.Extensions.Configuration;
using System.Data.SqlClient;
using System.Threading.Tasks;
using System.Dynamic;
using System.Collections.Generic;

public class OrderQueries : IOrderQueries
{
  public async Task<IEnumerable<OrderSummary>> GetOrdersAsync()
    {
        using (var connection = new SqlConnection(_connectionString))
        {
            connection.Open();
            return await connection.QueryAsync<OrderSummary>(
                  @"SELECT o.[Id] as ordernumber,
                  o.[OrderDate] as [date],os.[Name] as [status],
                  SUM(oi.units*oi.unitprice) as total
                  FROM [ordering].[Orders] o
                  LEFT JOIN[ordering].[orderitems] oi ON  o.Id = oi.orderid
                  LEFT JOIN[ordering].[orderstatus] os on o.OrderStatusId = os.Id
                  GROUP BY o.[Id], o.[OrderDate], os.[Name]
                  ORDER BY o.[Id]");
        }
    }
}
```

#### <a name="describe-response-types-of-web-apis"></a>Web API の応答型の説明

Web API とマイクロサービスを利用する開発者は、何が返されるか (具体的には、応答型とエラー コード (標準以外の場合)) を最も考慮します。 これらは、XML のコメントおよびデータ注釈で処理されます。

Swagger UI に適切なドキュメントがないと、コンシューマーは返される型や返される可能性のある HTTP コードを認識できません。 この問題は <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute?displayProperty=nameWithType> を追加することで解決できるため、次のコードに示すように、Swashbuckle は API の戻りモデルと値に関する情報をより多く生成できます。

```csharp
namespace Microsoft.eShopOnContainers.Services.Ordering.API.Controllers
{
    [Route("api/v1/[controller]")]
    [Authorize]
    public class OrdersController : Controller
    {
        //Additional code...
        [Route("")]
        [HttpGet]
        [ProducesResponseType(typeof(IEnumerable<OrderSummary>),
            (int)HttpStatusCode.OK)]
        public async Task<IActionResult> GetOrders()
        {
            var userid = _identityService.GetUserIdentity();
            var orders = await _orderQueries
                .GetOrdersFromUserAsync(Guid.Parse(userid));
            return Ok(orders);
        }
    }
}
```

ただし、`ProducesResponseType` 属性では型として動的な型を使用できず、次の例に示すように、`OrderSummary` ViewModel DTO などの明示的な型を使用する必要があります。

```csharp
public class OrderSummary
{
    public int ordernumber { get; set; }
    public DateTime date { get; set; }
    public string status { get; set; }
    public double total { get; set; }
}
```

これが、長期的に見ると、明示的な戻り値の型が動的な型より適しているもう 1 つの理由です。 `ProducesResponseType` 属性を使用する場合、200、400 などの考えられる HTTP エラー/コードで予期できる結果を指定することもできます。

次のイメージで、Swagger UI にどのように ResponseType 情報が表示されるかを確認できます。

![Ordering API 用の Swagger UI ページのスクリーンショット。](./media/cqrs-microservice-reads/swagger-ordering-http-api.png)

**図 7-5**。 Web API からの応答型と考えられる HTTP ステータス コードを示す Swagger UI

上のイメージでは、ViewModel 型に基づくいくつかの値の例と、返される可能性のある HTTP ステータス コードを確認できます。

## <a name="additional-resources"></a>その他の技術情報

- **Dapper**  
 <https://github.com/StackExchange/dapper-dot-net>

- **Julie Lerman。データ ポイント - Dapper、Entity Framework、およびハイブリッド アプリ (MSDN マガジンの記事)**  
  <https://docs.microsoft.com/archive/msdn-magazine/2016/may/data-points-dapper-entity-framework-and-hybrid-apps>

- **Swagger を使用する ASP.NET Core Web API のヘルプ ページ**  
  <https://docs.microsoft.com/aspnet/core/tutorials/web-api-help-pages-using-swagger?tabs=visual-studio>

>[!div class="step-by-step"]
>[前へ](eshoponcontainers-cqrs-ddd-microservice.md)
>[次へ](ddd-oriented-microservice.md)
