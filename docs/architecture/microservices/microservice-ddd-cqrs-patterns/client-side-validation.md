---
title: クライアント側の検証 (プレゼンテーション層での検証)
description: コンテナー化された .NET アプリケーションの .NET マイクロサービス アーキテクチャ | クライアント側の検証の主な概念を調べる。
ms.date: 10/08/2018
ms.openlocfilehash: 4e72dcafafc3144a75afe1fd23a4a779f5667459
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68674359"
---
# <a name="client-side-validation-validation-in-the-presentation-layers"></a>クライアント側の検証 (プレゼンテーション層での検証)

実際のソースがドメイン モデルで、最終的にドメイン モデル レベルで検証が必要な場合でも、検証は引き続きドメイン モデル レベル (サーバー側) と UI (クライアント側) の両方で処理できます。

クライアント側の検証は、ユーザーにとって非常に便利です。 他の方法では、検証エラーが返される可能性のあるサーバーへのラウンド トリップを待つために必要な時間を節約できます。 ビジネス的に表現すると、1 回だけ見ればわずかな時間でも、それが毎日何百回も積み重なると、膨大な時間、費用、フラストレーションになります。 簡単かつ迅速な検証は、ユーザーの作業をいっそう効率的にし、入力と出力の品質が向上します。

ビュー モデルとドメイン モデルが異なる場合と同様に、ビュー モデルの検証とドメイン モデルの検証は似ていても目的は異なる場合があります。 DRY (Don’t Repeat Yourself) の原則を念頭に置いている場合は、コードの再利用はカップリングを意味することもある点を考慮してください。エンタープライズ アプリケーションでは、DRY 原則に従うよりも、サーバー側とクライアント側をカップリングしないことの方が重要です。

クライアント側の検証を使用する場合でも、サーバー API が攻撃ベクトルになる可能性があるため、サーバー コードに含まれるコマンドまたは入力 DTO を常に検証する必要があります。 通常、両方を実行するのが最善の方法です。なぜなら、クライアント アプリケーションがある場合、UX の観点から、事前対応型にして、ユーザーに無効な情報を入力させない方法が最善のためです。

したがって、通常、クライアント側のコードでは ViewModel を検証します。 サービスに送信する前に、クライアントの出力 DTO またはコマンドを検証することもできます。

クライアント側の検証の実装は、構築するクライアント アプリケーションの種類によって変わります。 実装は、検証対象のデータが、ほとんどのコードが .NET による Web MVC Web アプリケーションであるのか、検証が JavaScript または TypeScript でコーディングされている SPA Web アプリケーションであるのか、または Xamarin と C# でコーディングされているモバイル アプリであるのか、によって変わります。

## <a name="additional-resources"></a>その他の技術情報

### <a name="validation-in-xamarin-mobile-apps"></a>Xamarin モバイル アプリの検証

- **テキスト入力の検証とエラーの表示** \
  [https://developer.xamarin.com/recipes/ios/standard\_controls/text\_field/validate\_input/](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/validate_input/)

- **検証コールバック** \
  <https://developer.xamarin.com/samples/xamarin-forms/XAML/ValidationCallback/>

### <a name="validation-in-aspnet-core-apps"></a>ASP.NET Core アプリの検証

- **Rick Anderson の 検証の追加** \
  <https://docs.microsoft.com/aspnet/core/tutorials/first-mvc-app/validation>

### <a name="validation-in-spa-web-apps-angular-2-typescript-javascript"></a>SPA Web アプリの検証 (Angular 2、TypeScript、JavaScript)

- **Ado Kukic。Angular 2 フォームの検証** \
  <https://scotch.io/tutorials/angular-2-form-validation>

- **フォームの検証** \
  <https://angular.io/guide/form-validation>

- **検証。** Breeze ドキュメント。 \
  <https://breeze.github.io/doc-js/validation.html>

要約すると、検証に関する最も重要な概念は次のとおりです。

- エンティティと集計は、独自の一貫性を強制し、"常に有効" である必要があります。 集計ルートが、同じ集計内の複数エンティティの一貫性を担います。

- エンティティで無効な状態を入力する必要があると考えられる場合は、最終的なドメイン エンティティを作成するまで一時的な DTO を使用するなど、別のオブジェクト モデルの使用を検討してください。

- 集計などの複数の関連オブジェクトを作成する必要があり、すべてのオブジェクトが作成された後にのみ有効になる場合は、ファクトリ パターンの使用を検討してください。

- ほとんどの場合、クライアント側で冗長な検証を行うことをお勧めします。これは、アプリケーションを事前対応型にすることができるためです。

>[!div class="step-by-step"]
>[前へ](domain-model-layer-validations.md)
>[次へ](domain-events-design-implementation.md)
