---
title: .NET Core でマイクロサービス ドメイン モデルを実装する
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | DDD 指向ドメイン モデルの実装の詳細について
ms.date: 10/08/2018
ms.openlocfilehash: bff9cbda08e519038056268151a1721427f0ac01
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "73972040"
---
# <a name="implement-a-microservice-domain-model-with-net-core"></a>.NET Core でマイクロサービス ドメイン モデルを実装する

前のセクションでは、ドメイン モデルの基本的な設計原則と設計パターンを説明しました。 ここでは、.NET Core (プレーンな C\# コード) と EF Core を使用してドメイン モデルを実装するために可能な手段を調べます。 ドメイン モデルを構成するのは自分が書くコードだけであることにご注目ください。 EF Core モデルの要件があるだけで、EF に対する実際の依存関係は存在しません。 ドメイン モデルには EF Core または他の ORM への緊密な依存関係や参照を含めないでください。

## <a name="domain-model-structure-in-a-custom-net-standard-library"></a>カスタム .NET Standard ライブラリのドメイン モデル構造

eShopOnContainers 参照アプリケーションに使用されているフォルダー編成は、このアプリケーションの DDD モデルを示しています。 アプリケーションによっては、別のフォルダー編成の方が、選択する設計をより明確に表現できる場合もあります。 図 7-10 に示されているように、注文ドメイン モデルには、注文集約と購入者集約という 2 つの集約があります。 それぞれの集約はドメイン エンティティと値オブジェクトから成るグループですが、単一のドメイン エンティティ (集約ルートまたはルート エンティティ) で集約を構成することもできます。

:::image type="complex" source="./media/net-core-microservice-domain-model/ordering-microservice-container.png" alt-text="ソリューション エクスプローラーの Ordering.Domain プロジェクトのスクリーンショット。":::
BuyerAggregate および OrderAggregate フォルダーが含まれる AggregatesModel フォルダーが示された Ordering.Domain プロジェクトのソリューション エクスプローラーの表示。それぞれのフォルダーにそのエンティティ クラス、値オブジェクト ファイルなどが含まれています。
:::image-end:::

**図 7-10**。 eShopOnContainers の注文マイクロサービスのドメイン モデル構造

また、このドメイン モデル レイヤーには、ドメイン モデルのインフラストラクチャ要件ともなっているリポジトリ コントラクト (インターフェイス) も含まれています。 つまり、これらのインターフェイスによって、インフラストラクチャ レイヤーで実装しなければならないリポジトリとメソッドが示されます。 リポジトリの実装はドメイン モデル レイヤーの外側、インフラストラクチャ レイヤー ライブラリ内に配置することが重要です。そのようにすることによって、ドメイン モデル レイヤーは、Entity Framework などのインフラストラクチャ テクノロジの API やクラスに "汚染" されずに済みます。

また、[SeedWork](https://martinfowler.com/bliki/Seedwork.html) フォルダーも参照できます。このフォルダーには、ドメイン エンティティと値オブジェクトの基礎として使用できるカスタム基底クラスが含まれるため、各ドメインのオブジェクト クラスで冗長コードをなくすことができます。

## <a name="structure-aggregates-in-a-custom-net-standard-library"></a>カスタム .NET Standard ライブラリでの構造の集約

集約とは、トランザクションの整合性を保つためにグループ化される、ドメイン オブジェクトのクラスターのことです。 こうしたオブジェクトは、(集約ルートやルート エンティティなどの) エンティティ インスタンスに、追加の値オブジェクトを付加したものとなる場合があります。

トランザクションの整合性とは、ビジネス アクションの最後の時点で集約が一貫した状態になり、なおかつ最新状態になることを保証することです。 たとえば、eShopOnContainers 注文マイクロサービス ドメイン モデルの注文集約は、図 7-11 のように構成されています。

:::image type="complex" source="./media/net-core-microservice-domain-model/vs-solution-explorer-order-aggregate.png" alt-text="OrderAggregate フォルダーとそのクラスのスクリーンショット。":::
OrderAggregate フォルダーの詳細な表示:Address.cs は値オブジェクト、IOrderRepository はリポジトリ インターフェイス、Order.cs は集約ルート、OrderItem.cs は子エンティティ、OrderStatus.cs は列挙クラス。
:::image-end:::

**図 7-11**。 Visual Studio ソリューションにおける注文集約

集約フォルダーのいずれかのファイルを開くと、[SeedWork](https://github.com/dotnet-architecture/eShopOnContainers/tree/master/src/Services/Ordering/Ordering.Domain/SeedWork) フォルダーで実装されるエンティティまたは値オブジェクトの場合と同じように、それがカスタム基底クラスとインターフェイスのいずれかとしてマークされている様子を確認できます。

## <a name="implement-domain-entities-as-poco-classes"></a>POCO クラスとしてドメイン エンティティを実装する

ドメイン エンティティを実装する POCO クラスを作成して、.NET でドメイン モデルを実装します。 次の例では、Order クラスがエンティティとして、さらには集約ルートとしても定義されています。 Order クラスは Entity 基底クラスから派生しているため、エンティティに関連する共通コードを再利用できます。 これらの基底クラスとインターフェイスはドメイン モデル プロジェクト内で自分で定義するため、EF などの ORM のインフラストラクチャ コードではなくユーザー コードです。

```csharp
// COMPATIBLE WITH ENTITY FRAMEWORK CORE 2.0
// Entity is a custom base class with the ID
public class Order : Entity, IAggregateRoot
{
    private DateTime _orderDate;
    public Address Address { get; private set; }
    private int? _buyerId;

    public OrderStatus OrderStatus { get; private set; }
    private int _orderStatusId;

    private string _description;
    private int? _paymentMethodId;

    private readonly List<OrderItem> _orderItems;
    public IReadOnlyCollection<OrderItem> OrderItems => _orderItems;

    public Order(string userId, Address address, int cardTypeId, string cardNumber, string cardSecurityNumber,
            string cardHolderName, DateTime cardExpiration, int? buyerId = null, int? paymentMethodId = null)
    {
        _orderItems = new List<OrderItem>();
        _buyerId = buyerId;
        _paymentMethodId = paymentMethodId;
        _orderStatusId = OrderStatus.Submitted.Id;
        _orderDate = DateTime.UtcNow;
        Address = address;

        // ...Additional code ...
    }

    public void AddOrderItem(int productId, string productName,
                            decimal unitPrice, decimal discount,
                            string pictureUrl, int units = 1)
    {
        //...
        // Domain rules/logic for adding the OrderItem to the order
        // ...

        var orderItem = new OrderItem(productId, productName, unitPrice, discount, pictureUrl, units);

        _orderItems.Add(orderItem);

    }
    // ...
    // Additional methods with domain rules/logic related to the Order aggregate
    // ...
}
```

重要なこととして、これが POCO クラスとして実装されるドメイン エンティティである点にご注意ください。 Entity Framework Core または他のインフラストラクチャ フレームワークへの直接の依存関係はありません。 この実装は、DDD の場合と同様に、ドメイン モデルを実装する C\# コードに過ぎません。

また、このクラスは IAggregateRoot というインターフェイスで修飾されます。 このインターフェイスは、このエンティティ クラスが集約ルートでもあることを示すためだけに使用される空のインターフェイスであり、*マーカー インターフェイス*と呼ばれることもあります。

マーカー インターフェイスはアンチ パターンと見なされることもありますが、このインターフェイスが進化していく可能性がある場合には特に、クラスをマークするためのわかりやすい方法ともなります。 属性はマーカーのもう 1 つの選択肢ですが、クラスの上に集約属性マーカーを配置するよりも、IAggregate インターフェイスの隣に基底クラス (Entity) を配置する方がより迅速に確認できます。 いずれにせよ、これは好みの問題です。

集約ルートを設けるということは、その集約のエンティティの整合性やビジネス ルールに関連するコードのほとんどを注文集約ルート クラスでメソッドとして実装する必要があることを意味します (たとえば、OrderItem オブジェクトを集約に追加する場合の AddOrderItem など)。 独立的にであれ、直接的にであれ、OrderItems オブジェクトを作成も更新もしないでください。AggregateRoot クラスが、その子エンティティに対するすべての更新操作を制御し、整合性を保持する必要があります。

## <a name="encapsulate-data-in-the-domain-entities"></a>ドメイン エンティティのデータをカプセル化する

エンティティ モデルでよくある問題として、コレクションのナビゲーション プロパティが一般にアクセス可能なリスト型として公開されることがあります。 そうすると、コラボレーターの開発者が誰でもこれらのコレクション型のコンテンツを操作できるようになり、結果として、コレクションに関連する重要なビジネス ルールがバイパスされ、オブジェクトが無効な状態のままになる可能性があります。 これに対する解決策は、関連するコレクションへの読み取り専用アクセスを公開することと、クライアントがこれらのコレクションを操作する方法を定義したメソッドを明示的に提供することです。

前のコードでは、多くの属性が読み取り専用かプライベートであり、クラス メソッドによってでなければ更新できないことにご注意ください。そのため、更新ではどれもビジネス ドメインの不変条件と、クラス メソッドに指定されたロジックが考慮されます。

たとえば、DDD パターンに従い、コマンド ハンドラー メソッドやアプリケーション レイヤー クラスから**以下を実行すべきでは *ありません*** (実際には、そのようにできないようになっています)。

```csharp
// WRONG ACCORDING TO DDD PATTERNS – CODE AT THE APPLICATION LAYER OR
// COMMAND HANDLERS
// Code in command handler methods or Web API controllers
//... (WRONG) Some code with business logic out of the domain classes ...
OrderItem myNewOrderItem = new OrderItem(orderId, productId, productName,
    pictureUrl, unitPrice, discount, units);

//... (WRONG) Accessing the OrderItems collection directly from the application layer // or command handlers
myOrder.OrderItems.Add(myNewOrderItem);
//...
```

この場合、Add メソッドは、単に OrderItems コレクションに直接アクセスしてデータを追加する操作に過ぎません。 そのため、子エンティティに対するその操作に関連するドメイン ロジック、ルール、検証の大部分は、アプリケーション レイヤー (コマンド ハンドラーと Web API コントローラー) が担当します。

集約ルートで処理する場合、集約ルートでは不変条件、有効性、整合性を保証できません。 結果として、解読困難なコードやトランザクション スクリプト コードとなります。

DDD パターンに従うには、エンティティのどのエンティティ プロパティにもパブリック セッターがあってはなりません。 エンティティ内の変更は、エンティティ内で実行する変更に関する明示的なユビキタス言語を使った、明示的なメソッドにより駆動する必要があります。

また、(注文項目などの) エンティティ内のコレクションは読み取り専用プロパティでなければなりません (AsReadOnly メソッドについて後ほど説明します)。 集約ルート クラス メソッドか子エンティティ メソッドの中からでなければ、それを更新できないようになっている必要があります。

Order 集約ルートのコードに示されているように、すべてのセッターはプライベートにするか、少なくとも外部的に読み取り専用とする必要があります。こうして、エンティティのデータまたはその子エンティティに対するあらゆる操作がエンティティ クラスのメソッドをとおして実行されなければならなくなります。 その結果、整合性は制御された、オブジェクト指向の方法で確保され、トランザクション スクリプト コードを実装する必要はなくなります。

次のコード スニペットは、OrderItem オブジェクトを Order 集約に追加するタスクをコーディングするための適切な方法を示しています。

```csharp
// RIGHT ACCORDING TO DDD--CODE AT THE APPLICATION LAYER OR COMMAND HANDLERS
// The code in command handlers or WebAPI controllers, related only to application stuff
// There is NO code here related to OrderItem object’s business logic
myOrder.AddOrderItem(productId, productName, pictureUrl, unitPrice, discount, units);

// The code related to OrderItem params validations or domain rules should
// be WITHIN the AddOrderItem method.

//...
```

このスニペットでは、OrderItem オブジェクトの作成に関連するほとんどの検証またはロジックが Order 集約ルートの AddOrderItem メソッドの制御下に置かれます。集約の他の要素に関連する検証とロジックは特にそう言えます。 たとえば、AddOrderItem に対して複数の呼び出しを実行した結果として、同じ製品項目を取得できます。 そのメソッドでは、製品項目を検証し、同じ製品項目を、いくつかの単位で構成される単一の OrderItem オブジェクトに統合できます。 さらに、複数の割引額があるものの製品 ID が同じである場合、大きい方の割り引きを適用したいと思うかもしれません。 この原則を、OrderItem オブジェクトの他のドメイン ロジックに適用します。

さらに、新しい OrderItem(params) 操作も、Order 集約ルートの AddOrderItem メソッドにより制御され、実行されます。 そのため、この操作に関連するほとんどのロジックまたは検証 (特に、他の子エンティティ間の整合性に影響を与えるあらゆるもの) は、集約ルート内の 1 か所に配置されます。 これが、集約ルート パターンの最終目的です。

Entity Framework Core 1.1 以降を使用する場合、DDD エンティティをより優れた方法で表すことができます。プロパティに加えて、[フィールドへのマッピング](https://docs.microsoft.com/ef/core/modeling/backing-field)が行えるためです。 これは、子エンティティや値オブジェクトのコレクションを保護する場合に役立ちます。 この機能拡張によって、プロパティではなく簡単なプライベート フィールドを使用できます。また、パブリック メソッドでフィールド コレクションへの更新を実装し、AsReadOnly メソッドを使用して読み取り専用アクセスを提供できます。

DDD では、エンティティのメソッド (またはコンストラクター) だけをとおしてエンティティを更新して、不変条件とデータの整合性を制御し、こうしてプロパティが get アクセサーによってのみ定義されることが望まれます。 プロパティは、プライベート フィールドによってサポートされます。 プライベート メンバーには、クラス内からのみアクセスできます。 ただし、1 つ例外があり、EF Core ではこれらのフィールドも設定する必要があります (適切な値でオブジェクトを返せるようにするため)。

### <a name="map-properties-with-only-get-accessors-to-the-fields-in-the-database-table"></a>get アクセサーのみでプロパティをデータベース テーブル内のフィールドにマッピングする

プロパティをデータベース テーブル列にマッピングすることはドメインの責任ではなく、インフラストラクチャと永続レイヤーの役割です。 ここでこれに言及するのは、エンティティをモデル化する方法に関係する EF Core 1.1 以降の新機能をお知らせするために過ぎません。 このトピックに関するその他の詳細情報については、インフラストラクチャと永続化に関するセクションで説明します。

EF Core 1.0 以降を使用する場合、DbContext で、ゲッターによってのみ定義されるプロパティを、データベース テーブルの実際のフィールドにマップする必要があります。 これは、PropertyBuilder クラスの HasField メソッドを使用して行います。

### <a name="map-fields-without-properties"></a>プロパティを使用しないでフィールドをマッピングする

EF Core 1.1 以降の機能で列をフィールドにマップすれば、プロパティを使用しないで済ませることもできます。 代わりに、テーブルからフィールドに列をマップするだけです。 この方法の一般的なユース ケースとしては、エンティティの外部からアクセスする必要のない、内部状態用のプライベート フィールドがあります。

たとえば、前述の OrderAggregate コード例の場合、`_paymentMethodId` フィールドなどのいくつかのプライベート フィールドがあり、これにはセッターやゲッターに関連するプロパティがあません。 このフィールドは注文のビジネス ロジック内で計算でき、注文のメソッドから使用できますが、データベース内に保存することも必要です。 そのため EF Core (v1.1 以降) では、関連プロパティを使用しないでデータベース内の列にフィールドをマップする方法が備わっています。 これは、このガイドの[インフラストラクチャ レイヤー](ddd-oriented-microservice.md#the-infrastructure-layer)に関するセクションでも説明されています。

### <a name="additional-resources"></a>その他の技術情報

- **Vaughn Vernon。DDD および Entity Framework による集約のモデル化。** これは、Entity Framework Core では*ない*ことにご注意ください。 \
  <https://kalele.io/blog-posts/modeling-aggregates-with-ddd-and-entity-framework/>

- **Julie Lerman。データ ポイント - ドメイン駆動設計のコーディング:データを重視する開発者のためのヒント** \
  <https://docs.microsoft.com/archive/msdn-magazine/2013/august/data-points-coding-for-domain-driven-design-tips-for-data-focused-devs>

- **Udi Dahan。完全にカプセル化されたドメイン モデルを作成する方法** \
  <http://udidahan.com/2008/02/29/how-to-create-fully-encapsulated-domain-models/>

> [!div class="step-by-step"]
> [前へ](microservice-domain-model.md)
> [次へ](seedwork-domain-model-base-classes-interfaces.md)
