---
title: 値オブジェクトの実装
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | 新しい Entity Framework 機能を使用し、値オブジェクトを実装する方法の詳細とオプション。
ms.date: 01/30/2020
ms.openlocfilehash: 4ace5c141b1cbd2dcfefb7ea7165a4006b130479
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77502508"
---
# <a name="implement-value-objects"></a>値オブジェクトを実装する

これまでのエンティティと集計に関するセクションで説明したように、ID はエンティティの基礎です。 一方、システムには、ID と ID の追跡を必要としないオブジェクトとデータ項目が多数あります。たとえば、値オブジェクトなどです。

値オブジェクトは他のエンティティを参照できます。 たとえば、あるポイントから別のポイントに到達する方法を示すルートを生成するアプリケーションの場合、そのルートが値オブジェクトです。 これは特定のルート上にあるポイントのスナップショットですが、内部的には City、Road などのエンティティを参照していても、この提案されるルートに ID はありません。

図 7-13 は、Order 集計内の Address 値オブジェクトを示しています。

![Order 集計内の Address 値オブジェクトを示す図。](./media/implement-value-objects/value-object-within-aggregate.png)

**図 7-13**。 Order 集計内の Address 値オブジェクト

図 7-13 に示すように、通常、エンティティは複数の属性で構成されます。 たとえば、`Order` エンティティは、ID があるエンティティとしてモデル化し、内部的に OrderId、OrderDate、OrderItems などの一連の属性で構成することができます。ただし、住所は、単に国/地域、市区町村、番地などで構成された複合値であり、このドメイン内での ID は含まれないため、値をモデル化し、値オブジェクトとして扱う必要があります。

## <a name="important-characteristics-of-value-objects"></a>値オブジェクトの重要な特性

値オブジェクトには主に 2 つの特性があります。

- ID がない。

- 不変である。

1 つ目の特性については既に説明しました。 不変性は重要な要件です。 値オブジェクトが作成された後は、その値を不変にする必要があります。 そのため、オブジェクトの構築時に必要な値を指定する必要がありますが、オブジェクトの有効期間中は変更を許可しない必要があります。

値オブジェクトを使用すると、不変の性質がパフォーマンスのために役立つことがあります。 特に、何千もの値オブジェクト インスタンスが存在する可能性があり、インスタンスの多くが同じ値を持つシステムで役に立ちます。 不変の性質なので、再利用することができます。値が同じで、ID を持たないので、相互に交換可能なオブジェクトにすることができます。 このような最適化で、低速で実行されるソフトウェアとパフォーマンスが良好なソフトウェアの間で違いが生じる場合があります。 当然ながら、このようないずれの場合でも、アプリケーション環境と展開コンテキストによって変わります。

## <a name="value-object-implementation-in-c"></a>C\# での値オブジェクトの実装

実装の観点からは、(値オブジェクトは ID に基づいてはならないので) すべての属性と他の基本的な特性の比較に基づいて、等値などの基本的なユーティリティ メソッドを持つ値オブジェクトの基底クラスを持つことができます。 次の例は、eShopOnContainers の注文マイクロサービスで使用される値オブジェクトの基底クラスを示しています。

```csharp
public abstract class ValueObject
{
    protected static bool EqualOperator(ValueObject left, ValueObject right)
    {
        if (ReferenceEquals(left, null) ^ ReferenceEquals(right, null))
        {
            return false;
        }
        return ReferenceEquals(left, null) || left.Equals(right);
    }

    protected static bool NotEqualOperator(ValueObject left, ValueObject right)
    {
        return !(EqualOperator(left, right));
    }

    protected abstract IEnumerable<object> GetAtomicValues();

    public override bool Equals(object obj)
    {
        if (obj == null || obj.GetType() != GetType())
        {
            return false;
        }

        ValueObject other = (ValueObject)obj;
        IEnumerator<object> thisValues = GetAtomicValues().GetEnumerator();
        IEnumerator<object> otherValues = other.GetAtomicValues().GetEnumerator();
        while (thisValues.MoveNext() && otherValues.MoveNext())
        {
            if (ReferenceEquals(thisValues.Current, null) ^
                ReferenceEquals(otherValues.Current, null))
            {
                return false;
            }

            if (thisValues.Current != null &&
                !thisValues.Current.Equals(otherValues.Current))
            {
                return false;
            }
        }
        return !thisValues.MoveNext() && !otherValues.MoveNext();
    }

    public override int GetHashCode()
    {
        return GetAtomicValues()
         .Select(x => x != null ? x.GetHashCode() : 0)
         .Aggregate((x, y) => x ^ y);
    }
    // Other utility methods
}
```

次の例に示す Address 値オブジェクトと同様に、実際の値オブジェクトを実装するときにこのクラスを使用できます。

```csharp
public class Address : ValueObject
{
    public String Street { get; private set; }
    public String City { get; private set; }
    public String State { get; private set; }
    public String Country { get; private set; }
    public String ZipCode { get; private set; }

    private Address() { }

    public Address(string street, string city, string state, string country, string zipcode)
    {
        Street = street;
        City = city;
        State = state;
        Country = country;
        ZipCode = zipcode;
    }

    protected override IEnumerable<object> GetAtomicValues()
    {
        // Using a yield return statement to return each element one at a time
        yield return Street;
        yield return City;
        yield return State;
        yield return Country;
        yield return ZipCode;
    }
}
```

ご覧のように、Address のこの値オブジェクト実装では ID が与えられません。そのため、Address クラスでも、ValueObject クラスでも ID フィールドがありません。

Entity Framework (EF) で使用するクラスに ID フィールドを置かないことは、EF Core 2.0 までは不可能でした。これにより、ID のない値オブジェクトの実装が大幅に改善されます。 これについては次のセクションで説明します。

不変である値オブジェクトは読み取り専用 (つまり get-only プロパティ) にすべきであるという意見が出るかもしれませんが、そのとおりです。 しかしながら、値オブジェクトは通常、シリアル化/逆シリアル化されてメッセージ キューを通過します。読み取り専用であれば、デシリアライザーによる値の割り当てが停止します。そのため、十分に実用的な範囲で読み取り専用になるプライベート セットとして残します。

## <a name="how-to-persist-value-objects-in-the-database-with-ef-core-20-and-later"></a>EF Core 2.0 以降でデータベース内の値オブジェクトを永続化する方法

ここまでは、ドメイン モデルで値オブジェクトを定義する方法について説明しました。 それでは、通常は ID のあるエンティティをターゲットとする Entity Framework Core を使用して、データベースに永続化するにはどうすればよいでしょうか。

### <a name="background-and-older-approaches-using-ef-core-11"></a>EF Core 1.1 を使用する背景と以前のアプローチ

背景として、EF Core 1.0 と 1.1 を使用する場合、従来の .NET Framework で EF 6.x で定義されているような[複合型](xref:System.ComponentModel.DataAnnotations.Schema.ComplexTypeAttribute)を使用できないという制限がありました。 そのため、EF Core 1.0 または 1.1 を使用する場合、値オブジェクトを ID フィールドを持つ EF エンティティとして格納する必要がありました。 そこで、ID を持たない値オブジェクトのように見えるように、ID を非表示にすることがありました。これで、値オブジェクトの ID がドメイン モデルで重要ではないことがはっきりします。 この ID を非表示にするには、[シャドウ プロパティ](https://docs.microsoft.com/ef/core/modeling/shadow-properties )として ID を使用します。 モデル内の ID を非表示にする構成は EF インフラストラクチャ レベルで設定されるため、ドメイン モデルでも透過的になります。

eShopOnContainers の初期バージョン (.NET Core 1.1) では、EF Core インフラストラクチャに必要な非表示の ID は、次のように、インフラストラクチャ プロジェクトで Fluent API を使用して DbContext レベルで実装されていました。 そのため、ID はドメイン モデルの観点からは非表示でしたが、インフラストラクチャには存在していました。

```csharp
// Old approach with EF Core 1.1
// Fluent API within the OrderingContext:DbContext in the Infrastructure project
void ConfigureAddress(EntityTypeBuilder<Address> addressConfiguration)
{
    addressConfiguration.ToTable("address", DEFAULT_SCHEMA);

    addressConfiguration.Property<int>("Id")  // Id is a shadow property
        .IsRequired();
    addressConfiguration.HasKey("Id");   // Id is a shadow property
}
```

ただし、その値オブジェクトのデータベースへの永続化は、別のテーブルの通常のエンティティと同様に実行されていました。

EF Core 2.0 以降には、値オブジェクトを永続化するための新しく優れた方法があります。

## <a name="persist-value-objects-as-owned-entity-types-in-ef-core-20-and-later"></a>EF Core 2.0 以降で所有エンティティ型として値オブジェクトを永続化する

DDD の標準の値オブジェクト パターンと EF Core の所有エンティティ型の間にいくつかのギャップがあるとしても、現在は EF Core 2.0 以降を使用して値オブジェクトを永続化する方法が最善です。 制限事項については、このセクションの末尾を参照してください。

所有エンティティ型の機能は、EF Core バージョン 2.0 以降に追加されました。

所有エンティティ型を使用すると、ドメイン モデルで明示的に定義された独自の ID を持たない型をマップし、任意のエンティティ内で値オブジェクトなどのプロパティとして使用することができます。 所有エンティティ型は、同じ CLR 型を別のエンティティ型 (つまり、通常のクラス) と共有します。 定義となるナビゲーションを含むエンティティは、所有者エンティティです。 所有者のクエリを実行すると、所有型が既定で含まれます。

ドメイン モデルのみを見ると、所有型には ID がないように見えます。 実際には所有型に ID はありますが、所有者ナビゲーション プロパティはこの ID の一部です。

所有型のインスタンスの ID は、完全に独自のものではありません。 この ID は 3 つのコンポーネントで構成されています。

- 所有者の ID

- これらを指すナビゲーション プロパティ

- 所有型のコレクションの場合は、独立したコンポーネント (EF Core 2.2 以降でサポートされています)。

たとえば、eShopOnContainers の Ordering ドメイン モデルでは、Order エンティティの一部である Address 値オブジェクトは、所有者エンティティ (Order エンティティ) 内の所有エンティティ型として実装されます。 Address は、ドメイン モデルに定義されている ID プロパティのない型です。 特定の注文の配送先住所を指定するために、Order 型のプロパティとして使用されます。

規約によって、所有されている型に対してシャドウ主キーが作成され、テーブル分割を利用し、同じテーブルに所有者としてマップされます。 そのため、従来の .NET Framework の EF6 で複合型を使用する方法と同様に所有型を使用できます。

EF Core の規約で所有型が検出されることはないので、明示的に宣言する必要がある点に注意してください。

eShopOnContainers では、OnModelCreating() メソッド内の OrderingContext.cs に複数のインフラストラクチャ構成が適用されています。 そのうちの 1 つが Order エンティティに関連しています。

```csharp
// Part of the OrderingContext.cs class at the Ordering.Infrastructure project
//
protected override void OnModelCreating(ModelBuilder modelBuilder)
{
    modelBuilder.ApplyConfiguration(new ClientRequestEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new PaymentMethodEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new OrderEntityTypeConfiguration());
    modelBuilder.ApplyConfiguration(new OrderItemEntityTypeConfiguration());
    //...Additional type configurations
}
```

次のコードでは、Order エンティティについて永続化インフラストラクチャが定義されています。

```csharp
// Part of the OrderEntityTypeConfiguration.cs class
//
public void Configure(EntityTypeBuilder<Order> orderConfiguration)
{
    orderConfiguration.ToTable("orders", OrderingContext.DEFAULT_SCHEMA);
    orderConfiguration.HasKey(o => o.Id);
    orderConfiguration.Ignore(b => b.DomainEvents);
    orderConfiguration.Property(o => o.Id)
        .ForSqlServerUseSequenceHiLo("orderseq", OrderingContext.DEFAULT_SCHEMA);

    //Address value object persisted as owned entity in EF Core 2.0
    orderConfiguration.OwnsOne(o => o.Address);

    orderConfiguration.Property<DateTime>("OrderDate").IsRequired();

    //...Additional validations, constraints and code...
    //...
}
```

前のコードでは、`orderConfiguration.OwnsOne(o => o.Address)` メソッドは、`Address` プロパティが `Order` 型の所有エンティティであることを指定しています。

既定の EF Core 規約では、所有エンティティ型のプロパティのデータベース列に `EntityProperty_OwnedEntityProperty` と名前が付けられます。 そのため、`Address` の内部プロパティは、`Orders` テーブルで `Address_Street`、`Address_City` (`State`、`Country`、`ZipCode` など) という名前で表示されます。

`Property().HasColumnName()` fluent メソッドを付加して、これらの列の名前を変更することができます。 `Address` がパブリック プロパティの場合、マッピングは次のようになります。

```csharp
orderConfiguration.OwnsOne(p => p.Address)
                            .Property(p=>p.Street).HasColumnName("ShippingStreet");

orderConfiguration.OwnsOne(p => p.Address)
                            .Property(p=>p.City).HasColumnName("ShippingCity");
```

fluent マッピングでは、`OwnsOne` メソッドを連鎖させることができます。 次の仮定例では、`OrderDetails` が `BillingAddress` と `ShippingAddress` を所有しています (いずれも `Address` 型です)。 また、`OrderDetails` は `Order` 型に所有されています。

```csharp
orderConfiguration.OwnsOne(p => p.OrderDetails, cb =>
    {
        cb.OwnsOne(c => c.BillingAddress);
        cb.OwnsOne(c => c.ShippingAddress);
    });
//...
//...
public class Order
{
    public int Id { get; set; }
    public OrderDetails OrderDetails { get; set; }
}

public class OrderDetails
{
    public Address BillingAddress { get; set; }
    public Address ShippingAddress { get; set; }
}

public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
}
```

### <a name="additional-details-on-owned-entity-types"></a>所有エンティティ型に関するその他の詳細情報

- 所有型は、OwnsOne fluent API を使用してナビゲーション プロパティを特定の型に構成するときに定義されます。

- メタデータ モデルの所有型の定義は、所有者型、ナビゲーション プロパティ、所有型の CLR 型のコンポジットです。

- スタック内の所有型インスタンスの ID (キー) は、所有者型の ID と所有型の定義のコンポジットです。

#### <a name="owned-entities-capabilities"></a>所有エンティティの機能

- 所有型は、他の所有 (入れ子にされた所有型) エンティティまたは非所有 (他のエンティティに対する通常の参照ナビゲーション プロパティ) エンティティを参照できます。

- 同じ所有者エンティティの同じ CLR 型を、個別のナビゲーション プロパティを使用して異なる所有型としてマップすることができます。

- テーブル分割は規約で設定されますが、ToTable を使用して所有型を別のテーブルにマップすることでオプト アウトすることができます。

- Eager の読み込みは、所有型に対して自動的に実行されます。つまり、クエリで `.Include()` を呼び出す必要はありません。

- EF Core 2.1 以降で、`[Owned]` 属性 を使用して構成できます。

- 所有型のコレクションを処理できます (バージョン2.2 以降を使用)。

#### <a name="owned-entities-limitations"></a>所有エンティティの制限事項

- 所有型の `DbSet<T>` を作成することはできません (仕様)。

- 所有型で `ModelBuilder.Entity<T>()` を呼び出すことはできません (現在は仕様です)。

- 同じテーブル内の所有者とマップされている (つまり、テーブル分割を使用している) 省略可能な (つまり、null を許容する) 所有型はサポートされていません。 これはプロパティごとにマッピングが行われるためです。全体としての null 複合値を個別に見張ることはしません。

- 所有型の継承マッピングはサポートされていませんが、異なる所有型と同じ継承階層の 2 つのリーフ型をマップすることはできます。 EF Core は、同じ階層に属することの理由にはなりません。

#### <a name="main-differences-with-ef6s-complex-types"></a>EF6 の複合型との主な違い

- テーブル分割は省略可能です。つまり、所有型のまま、必要に応じて別のテーブルにマップすることができます。

- 他のエンティティを参照することができます (つまり、他の非所有型との関係で依存側として機能することができます)。

## <a name="additional-resources"></a>その他の技術情報

- **Martin Fowler。ValueObject パターン** \
  <https://martinfowler.com/bliki/ValueObject.html>

- **Eric Evans。Domain-Driven Design:Tackling Complexity in the Heart of Software (ドメイン駆動設計: ソフトウェア中心部の複雑さへの取り組み)。** (書籍、値オブジェクトについての記載あり) \
  <https://www.amazon.com/Domain-Driven-Design-Tackling-Complexity-Software/dp/0321125215/>

- **Vaughn Vernon。ドメイン駆動型設計の実装** (書籍、値オブジェクトについての記載あり) \
  <https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577/>

- **所有エンティティ型** \
  <https://docs.microsoft.com/ef/core/modeling/owned-entities>

- **シャドウ プロパティ** \
  <https://docs.microsoft.com/ef/core/modeling/shadow-properties>

- **複合型と値オブジェクト**。 EF Core GitHub リポジトリのディスカッション ([問題] タブ) \
  <https://github.com/dotnet/efcore/issues/246>

- **ValueObject.cs.** eShopOnContainers の基底値オブジェクト クラス。 \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/SeedWork/ValueObject.cs>

- **Address クラス。** eShopOnContainers の値オブジェクト クラスのサンプル。 \
  <https://github.com/dotnet-architecture/eShopOnContainers/blob/dev/src/Services/Ordering/Ordering.Domain/AggregatesModel/OrderAggregate/Address.cs>

> [!div class="step-by-step"]
> [前へ](seedwork-domain-model-base-classes-interfaces.md)
> [次へ](enumeration-classes-over-enum-types.md)
