---
title: マイクロサービスを基にしている複合 UI を作成する
description: マイクロサービス アーキテクチャは、バックエンド専用ではありません。 フロントエンドで使用してピーク ビューを取得します。
ms.date: 09/20/2018
ms.openlocfilehash: 1861d3bb6e5d4a0226aa8f3f72a2e0d3e83be56f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "72275743"
---
# <a name="creating-composite-ui-based-on-microservices"></a>マイクロサービスを基にしている複合 UI を作成する

マイクロサービス アーキテクチャは多くの場合、データおよびロジックを処理するサーバー側から始まりますが、通常、UI はモノリシックとして処理されます。 ただし、より高度なアプローチとして ([マイクロ フロントエンド](https://martinfowler.com/articles/micro-frontends.html)と呼ばれる)、マイクロサービスに基づいたアプリケーション UI を設計する方法もあります。 これは、サーバー上にマイクロサービスが置かれモノリシック クライアント アプリのみでマイクロサービスが利用されるというのでなく、マイクロサービスによって複合 UI が生成されることを意味します。 このアプローチの場合、ビルドするマイクロサービスは、ロジックおよび視覚的表現の両方を備えることができます。

図 4-20 に、モノリシック クライアント アプリケーションからマイクロサービスを利用するだけの簡単なアプローチを示します。 当然ながら、HTML および JavaScript の生成の間に ASP.NET MVC サービスを含めることが可能です。 図は簡単なものであり、マイクロサービスを利用している単一 (モノリシック) のクライアント UI が強調表示されています。またこれらのマイクロサービスでは、UI シェイプ (HTML および JavaScript) ではなく、ロジックとデータにのみ焦点を当てています。

![マイクロサービスに接続するモノリシック UI アプリの図。](./media/microservice-based-composite-ui-shape-layout/monolith-ui-consume-microservices.png)

**図 4-20** バックエンド マイクロサービスを利用するモノリシック UI アプリケーション

これに対し、複合 UI は正確に生成され、マイクロサービス自体で構成されます。 一部のマイクロサービスでは、UI の特定の領域のビジュアル シェイプをドライブしています。 その場合の主な違いは、クライアント UI コンポーネント (TypeScript クラスなど) がテンプレートに基づいており、それらのテンプレートのデータ シェイプ UI ViewModel が各マイクロサービスから取得されるということです。

クライアント アプリケーションの起動時、各クライアント UI コンポーネント (TypeScript クラスなど) は、特定のシナリオで ViewModels を提供できるインフラストラクチャ マイクロサービスに自らを登録します。 マイクロサービスがシェイプを変更すると、UI も変わります。

図 4-21 に、この複合 UI アプローチのバージョンを示します。 これは、異なる手法に基づく細分化された部分を集計する他のマイクロサービスが存在する可能性があるため、簡略化されています。 それは、従来の Web アプローチ (ASP.NET MVC) または SPA (シングル ページ アプリケーション) のどちらを構築しているかに依存します。

![多くのビュー モデルで構成された複合 UI の図。](./media/microservice-based-composite-ui-shape-layout/microservice-generate-composite-ui.png)

**図 4-21** バックエンド マイクロサービスによって成形された複合 UI アプリケーションの例

これらの UI コンポジション マイクロサービスのそれぞれは、小規模な API ゲートウェイに似ています。 ただし、この場合、それぞれが小規模な UI 領域を担当します。

マイクロサービスによってドライブされる複合 UI アプローチは、使用する UI テクノロジに応じて、直面する困難の度合いが異なる可能性があります。 たとえば、SPA またはネイティブ モバイル アプリをビルドするために使用する技法 (Xamarin アプリを開発する場合などで、このアプローチの場合は難易度がより高くなるため) が従来の Web アプリケーションをビルドするために使用する技法と異なる場合があります。

[eShopOnContainers](https://aka.ms/MicroservicesArchitecture) サンプル アプリケーションでは複数の理由からモノリシック UI アプローチを使用します。 まず、マイクロサービスとコンテナーの場合です。 複合 UI は高度な技法であり、UI の設計および開発するときには複雑な処理も求められます。 次に、eShopOnContainers でも Xamarin に基づいたネイティブ モバイル アプリが実現されます。この場合は、クライアント C\# 側での処理がより複雑になります。

次の参照情報を使用してマイクロサービスに基づく複合 UI に関する知識を深めることをお勧めします。

## <a name="additional-resources"></a>その他の技術情報

- **マイクロ フロントエンド (Martin Fowler のブログ)**  
  <https://martinfowler.com/articles/micro-frontends.html>
  
- **マイクロフロントエンド (Michael Geers サイト)**  
  <https://micro-frontends.org/>
  
- **ASP.NET を使用した複合 UI (特定のワークショップ)**  
  <https://github.com/Particular/Workshop/tree/master/demos/asp-net-core>

- **Ruben Oostinga。マイクロサービス アーキテクチャでのモノリシック フロントエンド**  
  <https://xebia.com/blog/the-monolithic-frontend-in-the-microservices-architecture/>

- **Mauro Servienti。優れた UI コンポジションの秘密**  
  <https://particular.net/blog/secret-of-better-ui-composition>

- **Viktor Farcic。フロントエンド Web コンポーネントをマイクロサービスに含める**  
  <https://technologyconversations.com/2015/08/09/including-front-end-web-components-into-microservices/>

- **マイクロサービス アーキテクチャでのフロントエンドの管理**  
  <https://allegro.tech/2016/03/Managing-Frontend-in-the-microservices-architecture.html>

>[!div class="step-by-step"]
>[前へ](microservices-addressability-service-registry.md)
>[次へ](resilient-high-availability-microservices.md)
