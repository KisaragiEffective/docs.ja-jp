---
title: ドメイン モデル レイヤーでの検証の設計
description: コンテナー化された .NET アプリケーション用の .NET マイクロサービス アーキテクチャ | ドメイン モデル検証の主要な概念を理解する。
ms.date: 10/08/2018
ms.openlocfilehash: 98ccc5df84c9f6f402ecbee83b077c806d6a76fc
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75899664"
---
# <a name="design-validations-in-the-domain-model-layer"></a>ドメイン モデル レイヤーでの検証を設計する

DDD では、検証ルールは不変条件として考えることができます。 集計の主な役割は、その集計内のすべてのエンティティの状態の変更にわたってインバリアントを強制することです。

ドメイン エンティティは、常に有効なエンティティである必要があります。 常に true にする必要のあるオブジェクトには、特定のインバリアント数があります。 たとえば、order item オブジェクトは、正の整数である必要がある数量と、アーティクル名および価格を常に持っている必要があります。 そのため、インバリアントの強制は、(特に集計ルートの) ドメイン エンティティの役目となり、エンティティ オブジェクトは有効になっていない限り存在できません。 インバリアント ルールは、コントラクトとして単純に表され、違反があった場合には例外または通知が発生します。

この背後にある理由は、オブジェクトがなってはならない状態にあるため、多くのバグが発生するからです。 [オンライン ディスカッション](https://jeffreypalermo.com/2009/05/the-fallacy-of-the-always-valid-entity/)での Greg Young による適切な説明を次に示します。

UserProfile を受け取る SendUserCreationEmailService があるとします。Name が null でないそのサービスでどのように合理化できるでしょうか。 再度チェックしますか。 多くの場合、わざわざチェックなどせずに、他の誰かが検証してから自分に送信してくれる "最善の結果を期待" しているのではないでしょうか。 もちろん、記述すべき最初のテストのうちの 1 つである TDD を使用すると、null 名を持つ顧客を送信した場合に、エラーが発生します。 しかし、この種のテストを繰り返し記述しているうちに、"名前が null になることを許可しなかったら、これらのテストのすべては必要ないのではないか" ということに気が付きました。

## <a name="implement-validations-in-the-domain-model-layer"></a>ドメイン モデル レイヤーでの検証を実装する

検証は通常、ドメイン エンティティ コンストラクター内、またはエンティティを更新できるメソッドに実装されます。 検証を実装するには、データを検証し、検証が失敗した場合に例外を発生させるといった、複数の方法があります。 検証に使用する仕様パターンや、例外が発生すると、検証ごとに例外を返すのではなく、エラーのコレクションを返す通知パターンのような、より高度なパターンもあります。

### <a name="validate-conditions-and-throw-exceptions"></a>条件を検証して例外をスローする

次のコード例は、例外を発生させることによってドメイン エンティティで検証するための最も簡単な方法を示しています。 このセクションの最後にある参照テーブルには、前述したパターンに基づくより高度な実装へのリンクがあります。

```csharp
public void SetAddress(Address address)
{
    _shippingAddress = address?? throw new ArgumentNullException(nameof(address));
}
```

さらによい例は、内部状態が変わっていないか、メソッドのすべての変更が発生したことを確認する必要を示すものでしょう。 たとえば、次の実装では、オブジェクトが無効な状態のままになります。

```csharp
public void SetAddress(string line1, string line2,
    string city, string state, int zip)
{
    _shippingAddress.line1 = line1 ?? throw new ...
    _shippingAddress.line2 = line2;
    _shippingAddress.city = city ?? throw new ...
    _shippingAddress.state = (IsValid(state) ? state : throw new …);
}
```

状態の値が無効な場合は、address の最初の行と city が既に変更されています。 address が無効になる可能性があります。

同様の方法は、エンティティのコンストラクターで、エンティティの作成後、例外を発生させてそのエンティティが有効であることを確認するために使用できます。

### <a name="use-validation-attributes-in-the-model-based-on-data-annotations"></a>データ注釈に基づいてモデルで検証属性を使用する

Required 属性や MaxLength 属性のようなデータ注釈を使用すると、「[テーブル マッピング](infrastructure-persistence-layer-implemenation-entity-framework-core.md#table-mapping)」セクションで詳述したように、EF Core データベースのフィールド プロパティを構成することができます。しかし、.NET Framework の EF 4.x 以降に行われているので、[それらは EF Core でのエンティティ検証では機能しなくなりました](https://github.com/dotnet/efcore/issues/3680) (<xref:System.ComponentModel.DataAnnotations.IValidatableObject.Validate%2A?displayProperty=nameWithType> メソッドも機能しません)。

コントローラーの通常どおりのアクション呼び出しの前に行われるモデル バインディングの際にモデル検証で、データ注釈と <xref:System.ComponentModel.DataAnnotations.IValidatableObject> インターフェイスを引き続き使用することができます。しかし、そのモデルは ViewModel または DTO であることを前提としており、それはドメイン モデルに関する問題ではなく、MVC または API に関する問題です。

概念の違いが明らかにされていれば、ご利用のアクションで非推奨のエンティティ クラス オブジェクトが受信される場合に、検証用のエンティティ クラス内でデータ注釈と `IValidatableObject` を引き続き使用することができます。 その場合、アクションが呼び出されるすぐ前に行われるモデル バインディングの際に検証は実行されるので、コントローラーの ModelState.IsValid プロパティを調べてその結果を確認することができます。ただし、繰り返しになりますが、それはコントローラー内で実行され、実行されるのは DbContext 内にエンティティ オブジェクトが保持される前ではありません。それは EF 4.x 以降に行われているからです。

DbContext の SaveChanges メソッドをオーバーライドすれば、データ注釈と `IValidatableObject.Validate` メソッドを使用してエンティティ クラスにカスタム検証を引き続き実装することができます。

`IValidatableObject` エンティティを検証するための実装のサンプルについては、[GitHub 上のこちらのコメント](https://github.com/dotnet/efcore/issues/3680#issuecomment-155502539)を参照してください。 そのサンプルでは属性ベースの検証は行われていません。その検証については、同じオーバーライド内でリフレクションを使用することで容易に実装できるはずです。

ただし、DDD の観点から、ドメイン モデルはエンティティの動作メソッド内の例外を使用して、または検証ルールを強制する仕様パターンと通知パターンを実装することで、リーンに保つことをお勧めします。

UI 層内でモデルの検証を許可するために、入力を受け取る ViewModel クラス内 (ドメイン エンティティではなく) のアプリケーション層でデータ注釈を使用するのは合理的です。 ただし、ドメイン モデル内での検証の実行時にはこれを行わないでください。

### <a name="validate-entities-by-implementing-the-specification-pattern-and-the-notification-pattern"></a>仕様パターンと通知パターンを実装することでエンティティを検証する

最後に、ドメイン モデルで検証を実装するより複雑な方法は、後述するその他の技術情報の一部で説明されているように、仕様パターンと通知パターンを組み合わせて実装することです。

これらのパターンの 1 つだけを使用することもできます。たとえば、コントロール ステートメントを使用して手動で検証していますが、検証エラーの一覧をスタックして返すには通知パターンを使用します。

### <a name="use-deferred-validation-in-the-domain"></a>ドメインで遅延検証を使用する

ドメインで遅延検証に対処するには、さまざまな方法があります。 Vaughn Vernon 氏は自身の著書『[Implementing Domain-Driven Design](https://www.amazon.com/Implementing-Domain-Driven-Design-Vaughn-Vernon/dp/0321834577)』で、検証に関するセクションでこれらについて説明しています。

### <a name="two-step-validation"></a>2 段階検証

2 段階検証も検討してください。 データ転送オブジェクト (DTO) コマンドにはフィールド レベルの検証を使用し、エンティティ内にはドメイン レベルの検証を使用します。 検証エラーを処理しやすくするために、例外の代わりに結果オブジェクトを返すことで、これを行うことができます。

たとえば、データ注釈を使用したフィールドの検証を使用している場合、検証定義を複製しないでください。 ただし、DTO の場合、実行はサーバー側とクライアント側の両方で可能です (たとえば、コマンドと ViewModels)。

## <a name="additional-resources"></a>その他の技術情報

- **Rachel Appel。ASP.NET Core MVC でのモデル検証の概要** \
  <https://docs.microsoft.com/aspnet/core/mvc/models/validation>

- **Rick Anderson の 検証の追加** \
  <https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/validation>

- **Martin Fowler。検証で例外のスローを通知に置き換える** \
  <https://martinfowler.com/articles/replaceThrowWithNotification.html>

- **仕様と通知のパターン** \
  <https://www.codeproject.com/Tips/790758/Specification-and-Notification-Patterns>

- **Lev Gorodinski。ドメイン駆動設計 (DDD) での検証** \
  <http://gorodinski.com/blog/2012/05/19/validation-in-domain-driven-design-ddd/>

- **Colin Jack。ドメイン モデルの検証** \
  <https://colinjack.blogspot.com/2008/03/domain-model-validation.html>

- **Jimmy Bogard。DDD 世界での検証** \
  <https://lostechies.com/jimmybogard/2009/02/15/validation-in-a-ddd-world/>

> [!div class="step-by-step"]
> [前へ](enumeration-classes-over-enum-types.md)
> [次へ](client-side-validation.md)
