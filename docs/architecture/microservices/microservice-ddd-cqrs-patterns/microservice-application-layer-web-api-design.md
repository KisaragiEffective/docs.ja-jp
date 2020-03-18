---
title: マイクロサービス アプリケーション レイヤーと Web API を設計する
description: '.NET マイクロサービス: コンテナー化された .NET アプリケーションのアーキテクチャ | アプリケーション レイヤーを設計するための SOLID の原則の概要。'
ms.date: 10/08/2018
ms.openlocfilehash: 3c3b9f74e76e01deafa1f97de5d3250d57716014
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68676519"
---
# <a name="design-the-microservice-application-layer-and-web-api"></a>マイクロサービス アプリケーション レイヤーと Web API を設計する

## <a name="use-solid-principles-and-dependency-injection"></a>SOLID の原則と依存関係の挿入を使用する

SOLID の原則は、最新のミッション クリティカル アプリケーション (DDD のパターンを使用したマイクロサービスの開発など) で使用する重要な技法です。 SOLID は、次の 5 つの基本原則をグループ化する頭文字で構成されています。

- 単一責任 (Single Responsibility) の原則

- 開放/閉鎖 (Open/closed) の原則

- リスコフの置換 (Liskov substitution) 原則

- インターフェイス分離 (Interface Segregation) の原則

- 依存関係逆転 (Dependency Inversion) の原則

SOLID はむしろ、アプリケーションまたはマイクロサービス内部レイヤーの設計方法と、それらの間の依存関係の分離方法について述べています。 ドメインには無関係ですが、アプリケーションの技術的な設計には関係しています。 最後の原則、つまり依存関係逆転原則に従って、インフラストラクチャ レイヤーを他のレイヤーから分離できます。それにより、DDD レイヤーの優れた分離実装が可能になります。

依存関係挿入 (DI) は、依存関係逆転原則を実装する 1 つの方法です。 オブジェクトとその依存関係の間の疎結合を実現するための手法です。 クラスがそのアクションを実行するために必要なオブジェクトは、コラボレーターを直接インスタンス化したり、静的参照を使用 (つまり、新しいものを使用) するのではなく、クラスに提供 (または "挿入") されます。 ほとんどの場合、クラスはコンストラクターを使って依存関係を宣言することで、明示的な依存関係の原則に従うことができます。 通常、依存関係の挿入は特定の制御の反転 (Inversion of Control: IoC) コンテナーに基づきます。 ASP.NET Core には簡単な組み込み IoC コンテナーが備わっていますが、Autofac や Ninject などのお気に入りの IoC コンテナーを使用することもできます。

SOLID の原則に従うことで、クラスは必然的に小さく、十分に考慮された、簡単にテストできるものになる可能性が高くなります。 しかし、クラスに挿入されている依存関係が多すぎるかどうかはどのように把握できますか。 コンストラクターによって DI を使用している場合、コンストラクターのパラメーター数を確認するだけで簡単にわかります。 依存関係が多すぎる場合、一般に、クラスで行おうとしていることが多すぎるサイン ([コードのにおい](https://deviq.com/code-smells/)) であり、単一責任の原則に違反する可能性があります。

SOLID の詳細を取り上げた別のガイドがあります。 そのため、このガイドで必要となるのは、これらのトピックに関する最小限の知識のみです。

#### <a name="additional-resources"></a>その他の技術情報

- **SOLID: 基本的な OOP 原則** \
  <https://deviq.com/solid/>

- **制御の反転コンテナーと依存関係の挿入パターン** \
  <https://martinfowler.com/articles/injection.html>

- **Steve Smith。new は接着剤である** \
  <https://ardalis.com/new-is-glue>

> [!div class="step-by-step"]
> [前へ](nosql-database-persistence-infrastructure.md)
> [次へ](microservice-application-layer-implementation-web-api.md)
