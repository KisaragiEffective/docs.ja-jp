---
title: 単体テストを記述するためのベスト プラクティス
description: .NET Core プロジェクトと .NET Standard プロジェクトのコードの品質と回復性を向上させる単体テストを記述するためのベスト プラクティスについて説明します。
author: jpreese
ms.author: wiwagn
ms.date: 07/28/2018
ms.openlocfilehash: 586373381bcb18384cbf29bb2ca2bd220a2b2d3d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "78240962"
---
# <a name="unit-testing-best-practices-with-net-core-and-net-standard"></a>.NET Core と .NET Standard での単体テストのベスト プラクティス

単体テストの記述には多大な利点があります。回帰の防止に役立ち、ドキュメントを提供して、優れた設計を容易に行うことができます。 ただし、読みにくくて不安定な単体テストは、コード ベースに打撃を与える可能性があります。 この記事では、.NET Core プロジェクトと .NET Standard プロジェクトの単体テストの設計に関するベスト プラクティスについて説明します。

このガイドでは、テストの回復性とわかりやすさを維持するよう単体テストを記述する際のいくつかのベスト プラクティスについて説明します。

著者: [John Reese](https://reese.dev)、協力者: [Roy Osherove](https://osherove.com/)

## <a name="why-unit-test"></a>単体テストを記述する理由

### <a name="less-time-performing-functional-tests"></a>機能テストの実行時間を短縮
機能テストはコストがかかります。 通常、これらのテストでは、アプリケーションを開き、想定される動作を検証するためにお客様 (または他のだれか) が従う必要のある一連の手順を実行します。 テスト担当者は、これらの手順を必ずしもが把握しているとは限りません。つまり、テストを実行するために、その領域に精通している他者に支援を求める必要があります。 テスト自体は、小さな変更の場合は数秒かかり、大きな変更の場合は数分かかる可能性があります。 最後に、システムで変更を行うたびにこのプロセスを繰り返す必要があります。

一方、単体テストは数ミリ秒しかかからず、ボタンを押すだけで実行でき、概してシステムの知識は必ずしも必要ありません。 テストに合格するか失敗するかは、個人ではなくテスト ランナーの責任となります。

### <a name="protection-against-regression"></a>回帰の防止
回帰問題は、アプリケーションに変更が行われると発生する欠陥です。 一般的にテスト担当者は、新しい機能をテストするだけでなく、以前に実装されていた機能が今も想定どおりに機能していることを確認するために、事前に存在していた機能もテストします。

単体テストでは、ビルドを行うたびに、さらにはコード行を変更するたびに、テストのスイート全体を再実行できます。 これにより、新しいコードが既存の機能を損なわないことを確信できます。

### <a name="executable-documentation"></a>実行可能なドキュメント
特定の入力によって、特定のメソッドが何を実行するか、またはそのメソッドがどのように動作するかは、必ずしも明らかではありません。 空の文字列や Null を渡した場合にこのメソッドがどのように動作するか、自分自身わからないことが あります。

適切な名前の単体テストのスイートがある場合、各テストは、特定の入力に対して想定される出力を明確に説明できる必要があります。 さらに、実際に動作することを確認できる必要があります。

### <a name="less-coupled-code"></a>疎結合コード
コードが密結合されている場合、単体テストは難しくなる可能性があります。 記述しているコードに対して単体テストを作成しないと、結合を明確に認識できません。

したがって、コードに対するテストを記述する際には、必然的にコードを分離します。そうしないと、テストが困難になるからです。

## <a name="characteristics-of-a-good-unit-test"></a>適切な単体テストの特性

- **高速**。 完成度の高いプロジェクトに数千もの単体テストが含まれることは、珍しくありません。 単体テストの実行にかかる時間はごくわずかで、 数ミリ秒です。
- **独立性**。 単体テストはスタンドアロンであり、独立して実行でき、ファイル システムやデータベースなどの外部要因に対する依存関係がありません。
- **反復可能**。 単体テストの実行では、その結果に一貫性がある必要があります。つまり、実行と実行の間で何も変更しない場合、常に同じ結果を返す必要があります。
- **自己チェック**。 テストは人の介入なしに、合格したか失敗したかを自動的に検出できる必要があります。
- **タイムリー**。 単体テストは、テスト対象のコードの記述と比較して時間がかかりすぎないようにする必要があります。 コードの記述と比較してコードのテストに時間がかかりすぎる場合は、よりテストしやすい設計を検討してください。

## <a name="lets-speak-the-same-language"></a>用語を統一する
テストに関して、*モック*という用語はよく誤用されます。 単体テストの記述時における最も一般的な種類の*フェイク*の定義を次に示します。

*フェイク* - フェイクは、スタブまたはモック オブジェクトのいずれかを示すのに使用できる汎用的な用語です。 スタブとモックのどちらであるかは、使用されているコンテキストによって決まります。 つまり、フェイクはスタブとモックのどちらにもなりえます。

*モック* - モック オブジェクトは、単体テストに合格したか失敗したかを判断する、システム内のフェイク オブジェクトです。 モックは、アサートされるまでフェイクとして開始します。

*スタブ* - スタブは、システム内の既存の依存関係 (コラボレーター) の制御可能な置換です。 スタブを使用すると、依存関係を直接処理することなく、コードをテストできます。 既定では、フェイクはスタブとして開始します。

次のようなコード スニペットがあるとします。

```csharp
var mockOrder = new MockOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

これは、モックとして参照されるスタブの例です。 ここでは、実際にはスタブです。 `Purchase` (テスト対象のシステム) をインスタンス化する手段として、Order を渡しているだけです。 Order はモックでないので、名前 `MockOrder` も非常に紛らわしいものです。

より適切なアプローチを以下に示します。

```csharp
var stubOrder = new FakeOrder();
var purchase = new Purchase(stubOrder);

purchase.ValidateOrders();

Assert.True(purchase.CanBeShipped);
```

クラスの名前を `FakeOrder` に変更して、クラスをより汎用的な名前にし、クラスをモックまたはスタブとして使用できるようにしました。 テスト ケースに適切な方をどちらでも使用できます。 上記の例では、`FakeOrder` はスタブとして使用されています。 アサート時には、どのような形状または形式でも `FakeOrder` を使用していません。 `FakeOrder` は、コンストラクターの要件を満たすために `Purchase` クラスに渡されただけです。

これをモックとして使用するには、たとえば次のようにします。

```csharp
var mockOrder = new FakeOrder();
var purchase = new Purchase(mockOrder);

purchase.ValidateOrders();

Assert.True(mockOrder.Validated);
```

この場合は、フェイクのプロパティをチェック (フェイクに対してアサート) しているので、上記のコード スニペット内で `mockOrder` はモックです。

> [!IMPORTANT]
> この用語を正確に理解することが重要です。 スタブを "モック" と名付けた場合、他の開発者はあなたの意図を誤って解釈してしまいます。

モックとスタブに関する重要な留意点として、モックとスタブはよく似ていますが、モック オブジェクトに対してはアサートを行うのに対し、スタブに対してはアサートを行いません。

## <a name="best-practices"></a>ベスト プラクティス

### <a name="naming-your-tests"></a>テストの名前付け
テストの名前は、次の 3 つの部分で構成される必要があります。

- テスト対象のメソッドの名前。
- それがテストされるシナリオ。
- シナリオが呼び出されたときに想定される動作。

#### <a name="why"></a>なぜでしょうか。

- 名前付け規則は、テストの目的を明示的に表すので重要です。

テストは、コードが機能することを確認するだけでなく、ドキュメントも提供します。 単体テストのスイートを見るだけで、コード自体を見なくても、コードの動作を推測できます。 さらに、テストが失敗した際には、どのシナリオが想定を満たしていないかを正確に判断できます。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeNaming](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeNaming)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="arranging-your-tests"></a>テストの配置
**Arrange、Act、Assert** は、単体テスト時に共通するパターンです。 名前が示すように、これは次の 3 つの主なアクションで構成されます。

- オブジェクトを*配置 (Arrange)* し、必要に応じて作成および設定します。
- オブジェクトを*操作 (Act)* します。
- 何かが想定どおりであることを*アサート (Assert)* します。

#### <a name="why"></a>なぜでしょうか。

- *配置*と*アサート*の手順で、テストする対象を明確に区別します。
- "Act" コードにより、アサーションが混合される可能性が低くなります。

読みやすさは、テストの作成時において最も重要な側面の 1 つです。 テスト内でこれらの各アクションを分離することで、コードの呼び出しに必要な依存関係、コードを呼び出す方法、および何をアサートしようとしているかが明確に区別されます。 いくつかの手順を結合し、テストのサイズを小さくすることは可能ですが、主な目的はテストをできるだけ読みやすくすることにあります。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeArranging)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterArranging](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterArranging)]

### <a name="write-minimally-passing-tests"></a>最小限の情報で合格するテストを記述する
単体テストで使用する入力は、現在テストしている動作を検証するために、できる限り単純である必要があります。

#### <a name="why"></a>なぜでしょうか。

- コードベースでの今後の変更に対して、テストの回復力が高くなります。
- 実装よりもテストの動作に的が絞られます。

テストに合格するために必要以上の情報を含むテストは、テストでエラーが発生する可能性が高く、テストの目的が不明確になる可能性があります。 テストの記述時には、動作に的を絞る必要があります。 モデルに追加のプロパティを設定したり、不要な場合に 0 以外の値を使用すると、証明しようとしている内容が不明瞭になるだけです。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMinimallyPassing)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterNamingAndMinimallyPassing](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterNamingAndMinimallyPassing)]

### <a name="avoid-magic-strings"></a>マジック文字列を回避する
単体テストにおける変数の名前付けは、より重要というわけではなくとも、運用コードにおける変数の名前付けと同等に重要です。 単体テストにマジック文字列を含めることはできません。

#### <a name="why"></a>なぜでしょうか。

- 特殊な値となっている原因を特定するために、テストを読む人が運用コードを調べる必要がなくなります。
- *実現*しようとしている内容ではなく*証明*しようとしている内容が明示的に示されます。

マジック文字列は、テストを読む人に混乱をきたす可能性があります。 文字列が通常からかけ離れて見えれば、パラメーターや戻り値としてその特定の値がなぜ選択されたかが気になります。 そのような場合、テストに集中するのではなく、実装の詳細を調べようとします。

> [!TIP]
> テストの記述時には、その意図をできる限り表現することを目指す必要があります。 マジック文字列の場合の適切なアプローチとしては、これらの値を定数に割り当てます。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMagicString)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterMagicString](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMagicString)]

### <a name="avoid-logic-in-tests"></a>テストで論理を回避する
単体テストの記述時には、手動による文字列の連結、および `if`、`while`、`for`、`switch` などの論理条件を回避します。

#### <a name="why"></a>なぜでしょうか。

- テスト内にバグが発生する可能性が低くなります。
- 実装の詳細ではなく、最終的な結果に的が絞られます。

テスト スイートに論理を導入すると、バグが発生する可能性が著しく高まります。 テスト スイート内でバグが発見されることは避けなければなりません。 テストが機能するという高レベルの確信が持てる必要があります。そうでなければ、テストを信頼することはできません。 信頼できないテストは、何の値も提供しません。 テストが失敗した場合は、コードに実際に問題があり、無視できないことを実感できる必要があります。

> [!TIP]
> テスト内の論理を避けられない場合は、テストを 2 つ以上の別々のテストに分割することを検討してください。

#### <a name="bad"></a>不適切な例:
[!code-csharp[LogicInTests](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#LogicInTests)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterTestLogic](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterTestLogic)]

### <a name="prefer-helper-methods-to-setup-and-teardown"></a>設定と破棄よりもヘルパー メソッドを優先する
テストに同様のオブジェクトまたは状態が必要な場合は、設定属性と破棄属性を利用する (存在する場合) よりもヘルパー メソッドを優先します。

#### <a name="why"></a>なぜでしょうか。

- 各テスト内からコード全体を見ることができるので、テストを読むときに混乱が少なくなります。
- 特定のテストに対する設定が多すぎたり少なすぎたりする可能性が低くなります。
- テスト間で状態を共有して、テスト間に不要な依存関係が生じる可能性が低くなります。

単体テスト フレームワークでは、テスト スイート内のすべての各単体テスト前に `Setup` が呼び出されます。 これは便利なツールとして見なされる場合もありますが、通常はテストが膨張して読みづらくなってしまいます。 各テストには、テストが稼働するためにさまざまな要件があります。 しかし、`Setup` は各テストに対してまったく同じ要件を使用することを強制します。

> [!NOTE]
> xUnit では、バージョン 2.x の時点で SetUp と TearDown の両方が削除されています。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeSetup)]

```csharp
// more tests...
```

[!code-csharp[BeforeHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeHelperMethod)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterHelperMethod](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterHelperMethod)]

```csharp
// more tests...
```

[!code-csharp[AfterSetup](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterSetup)]

### <a name="avoid-multiple-asserts"></a>複数のアサートを回避する
テストを記述する際には、テストごとにアサートを 1 つだけ含めるようにしてください。 アサートを 1 つだけ使用する一般的なアプローチには、次のものがあります。

- アサートごとに別々のテストを作成します。
- パラメーター化されたテストを使用します。

#### <a name="why"></a>なぜでしょうか。

- 1 つのアサートに失敗した場合、後続のアサートは評価されません。
- テストで複数のケースをアサートしないようにします。
- テストが失敗した理由に関する全体像をとらえることができます。

テスト ケースに複数のアサートを導入した場合、すべてのアサートが実行されることは保証されません。 ほとんどの単体テスト フレームワークでは、単体テストでアサーションが失敗すると、続行中のテストは自動的に失敗と見なされます。 この際、実際には動作している機能が失敗として示されるので、混乱を招きます。

> [!NOTE]
> この規則の一般的な例外は、オブジェクトに対してアサートを行う場合です。 この場合、オブジェクトが想定どおりの状態にあること保証するために、通常は各プロパティに対して複数のアサートを使用することが許容されます。

#### <a name="bad"></a>不適切な例:
[!code-csharp[BeforeMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/before/StringCalculatorTests.cs#BeforeMultipleAsserts)]

#### <a name="better"></a>より適切な例:
[!code-csharp[AfterMultipleAsserts](../../../samples/snippets/core/testing/unit-testing-best-practices/csharp/after/StringCalculatorTests.cs#AfterMultipleAsserts)]

### <a name="validate-private-methods-by-unit-testing-public-methods"></a>パブリック メソッドの単体テストを行うことでプライベート メソッドを検証する
ほとんどの場合、プライベート メソッドをテストする必要はありません。 プライベート メソッドは実装の詳細です。 プライベート メソッドは独立して存在することはない、と考えることができます。 いずれかの時点で、実装の一部としてプライベート メソッドを呼び出す、パブリックに公開されたメソッドが存在することになります。 鍵となるのは、プライベート メソッドを呼び出すパブリック メソッドの最終結果です。

次のケースを考えてみます。

```csharp
public string ParseLogLine(string input)
{
    var sanitizedInput = TrimInput(input);
    return sanitizedInput;
}

private string TrimInput(string input)
{
    return input.Trim();
}
```

メソッドが想定どおりに動作していることを確認する必要があるので、まずは、`TrimInput` に対してテストを記述しようと考えるかもしれません。 しかし、想定外の方法で `ParseLogLine` が `sanitizedInput` を操作し、`TrimInput` に対するテストが無効になる可能性があります。

実際のテストは、パブリックに公開された `ParseLogLine` メソッドに対して行う必要があります。最終的にはこのメソッドが鍵となるためです。

```csharp
public void ParseLogLine_ByDefault_ReturnsTrimmedResult()
{
    var parser = new Parser();

    var result = parser.ParseLogLine(" a ");

    Assert.Equals("a", result);
}
```

この観点で、プライベート メソッドがある場合は、パブリック メソッドを見つけて、そのメソッドに対してテストを記述します。 プライベート メソッドが想定どおりの結果を返すからといって、最終的にプライベート メソッドを呼び出すシステムが、結果を正しく使用するとは限りません。

### <a name="stub-static-references"></a>スタブの静的参照
単体テストの原則の 1 つは、テスト対象システムに対してフル コントロールを持つ必要があることです。 このことは、運用コードに静的参照への呼び出しが含まれる場合に問題となります (例: `DateTime.Now`)。 次のコードを考えてみます。

```csharp
public int GetDiscountedPrice(int price)
{
    if(DateTime.Now.DayOfWeek == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

このコードに対して単体テストを行うには、どのようにすればよいでしょうでしょうか。 次のようなアプローチを試してみることができます。

```csharp
public void GetDiscountedPrice_ByDefault_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(2, actual)
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();

    var actual = priceCalculator.GetDiscountedPrice(2);

    Assert.Equals(1, actual);
}
```

ただし、このテストにはいくつかの問題があることがすぐにわかります。

- テスト スイートが火曜日に実行される場合、2 番目のテストには合格しますが、最初のテストには失敗します。
- テスト スイートが他のいずれかの曜日に実行される場合、最初のテストには合格しますが、2 番目のテストには失敗します。

こうした問題を解決するには、運用コードに*シーム*を導入する必要があります。 1 つのアプローチとしては、インターフェイス内で制御する必要のあるコードをラップし、運用コードがそのインターフェイスに依存するようにします。

```csharp
public interface IDateTimeProvider
{
    DayOfWeek DayOfWeek();
}

public int GetDiscountedPrice(int price, IDateTimeProvider dateTimeProvider)
{
    if(dateTimeProvider.DayOfWeek() == DayOfWeek.Tuesday)
    {
        return price / 2;
    }
    else
    {
        return price;
    }
}
```

この時点で、テスト スイートは次のようになります。

```csharp
public void GetDiscountedPrice_ByDefault_ReturnsFullPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Monday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(2, actual);
}

public void GetDiscountedPrice_OnTuesday_ReturnsHalfPrice()
{
    var priceCalculator = new PriceCalculator();
    var dateTimeProviderStub = new Mock<IDateTimeProvider>();
    dateTimeProviderStub.Setup(dtp => dtp.DayOfWeek()).Returns(DayOfWeek.Tuesday);

    var actual = priceCalculator.GetDiscountedPrice(2, dateTimeProviderStub);

    Assert.Equals(1, actual);
}
```

これで、テスト スイートは `DateTime.Now` に対してフル コントロールを持ち、メソッドを呼び出すときに任意の値をスタブできるようになりました。
