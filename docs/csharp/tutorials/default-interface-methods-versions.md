---
title: C# で既定のインターフェイス メソッドを使用してインターフェイスを安全に更新する
description: この高度なチュートリアルでは、既存のインターフェイスを実装するすべてのクラスと構造体を損なうことなく、そのインターフェイスの定義に新しい機能を安全に追加する方法について説明します。
ms.date: 05/06/2019
ms.technlogy: csharp-advanced-concepts
ms.custom: mvc
ms.openlocfilehash: 650aea78b421783b3f249b3670578aa60e800ab2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156780"
---
# <a name="tutorial-update-interfaces-with-default-interface-methods-in-c-80"></a>チュートリアル: C# 8.0 で既定のインターフェイス メソッドを使用してインターフェイスを更新する

.NET Core 3.0 上の C# 8.0 以降では、インターフェイスのメンバーを宣言するときに実装を定義できます。 最も一般的なシナリオは、数え切れないほどのクライアントから既にリリースされ、使用されているインターフェイスにメンバーを安全に追加することです。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
>
> * 実装を含むメソッドを追加して、インターフェイスを安全に拡張します。
> * パラメーター化された実装を作成して、柔軟性を高めます。
> * 実装者がオーバーライドの形でより具体的な実装を提供できるようにします。

## <a name="prerequisites"></a>必須コンポーネント

お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。C# 8.0 コンパイラも実行されるようにします。 C# 8.0 コンパイラは [Visual Studio 2019 バージョン 16.3](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) または [.NET Core 3.0 SDK](https://dotnet.microsoft.com/download) 以降で使用できます。

## <a name="scenario-overview"></a>シナリオの概要

このチュートリアルは、カスタマー リレーションシップ ライブラリのバージョン 1 から始まります。 [GitHub 上の サンプル リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship)でスターター アプリケーションを入手できます。 このライブラリを構築した会社の目的は、既存のアプリケーションを使用している顧客がライブラリを採用することでした。 ライブラリのユーザーが実装できる最小限のインターフェイス定義が用意されました。 顧客向けのインターフェイス定義は次のとおりです。

[!code-csharp[InitialCustomerInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/ICustomer.cs?name=SnippetICustomerVersion1)]

注文を表す 2 つ目のインターフェイスを定義しました。

[!code-csharp[InitialOrderInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/starter/customer-relationship/IOrder.cs?name=SnippetIorderVersion1)]

チームは、これらのインターフェイスからユーザー向けのライブラリを構築し、顧客にとってより良いエクスペリエンスを作り出すことができました。 目標は、既存の顧客との関係を深め、新しい顧客との関係を改善することでした。

次回のリリースのためにライブラリをアップグレードする時期になりました。 求められている機能の 1 つは、注文数が多い顧客向けにロイヤルティ割引を有効にすることです。 この新しいロイヤルティ割引は、顧客が注文するたびに適用されます。 この特定の割引は、各顧客のプロパティです。 `ICustomer` の各実装で、ロイヤルティ割引に対して異なるルールを設定できます。

この機能を追加する最も自然な方法は、ロイヤルティ割引を適用するメソッドを使用して `ICustomer` インターフェイスを拡張することです。 この設計の提案から、経験豊富な開発者の間で次のような懸念が起こりました。「リリース済みのインターフェイスは変更できません。 これは破壊的変更です」 C# 8.0 では、インターフェイスをアップグレードするための "*既定のインターフェイス実装*" が追加されました。 ライブラリ作成者はインターフェイスに新しいメンバーを追加し、それらのメンバーに既定の実装を指定することができます。

既定のインターフェイス実装を使用すると、開発者がインターフェイスをアップグレードできるだけでなく、すべての実装者がその実装をオーバーライドできるようになります。 ライブラリのユーザーは、非破壊的変更として既定の実装を受け入れることができます。 ビジネス ルールが異なる場合は、オーバーライドできます。

## <a name="upgrade-with-default-interface-methods"></a>既定のインターフェイス メソッドを使用してアップグレードする

チームは、最も可能性の高い既定の実装、つまり顧客に対するロイヤルティ割引について合意しました。

アップグレードでは、2 つのプロパティを設定する機能を提供する必要があります。割引の対象となるために必要な注文数と、割引の割合です。 これは、既定のインターフェイス メソッドに最適なシナリオです。 `ICustomer` インターフェイスにメソッドを追加し、最も確実な実装を提供することができます。 すべての既存の実装とすべての新しい実装では、既定の実装を使用することも、独自の実装を提供することもできます。

まず、実装に新しいメソッドを追加します。

[!code-csharp[InitialOrderInterface](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionOne)]

ライブラリ作成者は、実装を確認する最初のテストを作成しました。

[!code-csharp[TestDefaultImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetTestDefaultImplementation)]

テストの次の部分に注目してください。

[!code-csharp[TestDefaultImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetHighlightCast)]

`SampleCustomer` から `ICustomer` へのキャストは必須です。 `SampleCustomer` クラスから `ComputeLoyaltyDiscount` の実装を提供する必要はありません。これは `ICustomer` インターフェイスによって提供されます。 ただし、`SampleCustomer` クラスはそのインターフェイスからメンバーを継承しません。 そのルールは変わっていません。 インターフェイスで宣言および実装されているメソッドを呼び出すには、変数をインターフェイスの型 (この例では `ICustomer`) 似する必要があります。

## <a name="provide-parameterization"></a>パラメーター化を提供する

なかなかのスタートです。 ただし、既定の実装は制限が厳しすぎます。 このシステムの多くの利用者は、異なる購入数のしきい値、異なる長さのメンバーシップ、または異なる割引率を選択する可能性があります。 これらのパラメーターを設定する方法を用意することで、より多くの顧客とって優れたアップグレード エクスペリエンスを提供できます。 既定の実装を制御するこれら 3 つのパラメーターを設定する静的メソッドを追加しましょう。

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetLoyaltyDiscountVersionTwo)]

このわずかなコード フラグメントには、新しい言語機能が多数見られます。 インターフェイスには、フィールドやメソッドなどの静的メンバーを含めることができるようになりました。 さまざまなアクセス修飾子も使用できます。 追加のフィールドはプライベートであり、新しいメソッドはパブリックです。 どの修飾子もインターフェイス メンバーに使用できます。

ロイヤルティ割引の計算に一般的な数式を使用する (ただしパラメーターは異なる) アプリケーションでは、カスタム実装を用意する必要がありません。静的メソッドを介して引数を設定できます。 たとえば、次のコードでは、メンバーシップが 1 か月を超えるすべての顧客に報酬を与える "顧客感謝" を設定します。

[!code-csharp[SetLoyaltyThresholds](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/Program.cs?name=SnippetSetLoyaltyThresholds)]

## <a name="extend-the-default-implementation"></a>既定の実装を拡張する

これまでに追加したコードでは、ユーザーが既定の実装のようなものを希望しているシナリオ、または関係のない一連のルールを指定するシナリオに便利な実装を提供しました。 最後の機能として、コードを少しリファクターして、ユーザーが既定の実装を基に構築したくなるようなシナリオに対応しましょう。

新規顧客を引き付けたいスタートアップ企業があるとします。 新規顧客の最初の注文には 50% 割引が提供されます。 それ以外の場合、既存の顧客は標準の割引を受けます。 このインターフェイスを実装するすべてのクラスが実装内でコードを再利用できるように、ライブラリ作成者は既定の実装を `protected static` メソッドに移行する必要があります。 インターフェイス メンバーの既定の実装では、この共有メソッドも呼び出されます。

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/ICustomer.cs?name=SnippetFinalVersion)]

このインターフェイスを実装するクラスの実装では、オーバーライドによって静的ヘルパー メソッドを呼び出し、そのロジックを拡張して "新規顧客" の割引を提供することができます。

[!code-csharp[VersionTwoImplementation](~/samples/snippets/csharp/tutorials/default-interface-members-versions/finished/customer-relationship/SampleCustomer.cs?name=SnippetOverrideAndExtend)]

[GitHub 上の サンプル リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/finished/customer-relationship)で完成したコード全体を確認できます。 [GitHub 上の サンプル リポジトリ](https://github.com/dotnet/samples/tree/master/csharp/tutorials/default-interface-members-versions/starter/customer-relationship)でスターター アプリケーションを入手できます。

これらの新機能は、新しいメンバーに妥当な既定の実装がある場合に、インターフェイスを安全に更新できることを意味します。 複数のクラスから実装できる 1 つの機能的なアイデアを表現するように、慎重にインターフェイスを設計してください。 その結果、同じ機能的なアイデアに対して新しい要件が見つかったときに、そのインターフェイス定義を簡単にアップグレードできるようになります。
