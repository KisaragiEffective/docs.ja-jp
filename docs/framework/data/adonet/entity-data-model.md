---
title: エンティティ データ モデル
ms.date: 03/30/2017
ms.assetid: 2dda3d5b-4582-4ba0-a91d-fcd7a1498137
ms.openlocfilehash: ed834c57104e9f03ac337f6c1d30a0498bd42a06
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738414"
---
# <a name="entity-data-model"></a>エンティティ データ モデル
Entity Data Model (EDM) は、格納される形式に関係なく、データ構造を記述する一連の概念です。 EDM は、1976 年に Peter Chen により記述されたエンティティ リレーションシップ モデルを取り入れていますが、これを土台にして利用法が拡張されています。  
  
 EDM は、データを多くの形式で格納する場合につきまとう問題に対応しています。 たとえば、データをリレーショナル データベース、テキスト ファイル、XML ファイル、スプレッドシート、およびレポートに格納している企業について考えてみます。 この状況は、データ モデリング、アプリケーション設計、データ アクセスに深刻な問題を提示しています。 データ指向のアプリケーションを設計する場合、データ アクセス、ストレージ、およびスケーラビリティの効率性を損なうことなく、効率的で保守性に優れたコードを作成することが問題になります。 データがリレーショナル構造の場合は、データ アクセス、ストレージ、およびスケーラビリティの効率性が非常に高くなるものの、効率的で保守性に優れたコードを書くことが難しくなります。 データがオブジェクト構造の場合は、これが反対になり、効率的で保守性に優れたコードを作成しやすくなる一方で、データ アクセス、ストレージ、およびスケーラビリティの効率性が損なわれます。 これらの要因の間で適切なバランスを取れる場合でも、データを 1 つの形式から別の形式に移動する際に新しい問題が発生します。 Entity Data Model は、ストレージ スキーマに依存しないエンティティとリレーションシップでデータ構造を記述することで、これらの問題に対応しています。 このため、アプリケーションの設計と開発でデータの格納形式が問題になりません。 さらに、エンティティおよびリレーションシップによりアプリケーションで使用されるデータ構造 (格納形式ではなく) が記述されるため、アプリケーションの進化に伴ってこれらを進化させることができます。  
  
 `conceptual model` は、データ構造をエンティティおよびリレーションシップとして表現したもので、一般的には、EDM の概念を実装するドメイン固有言語 (DSL) で記述されます。 [概念スキーマ定義言語 (CSDL)](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)は、このようなドメイン固有言語の一例です。 概念モデルで記述されるエンティティおよびリレーションシップは、アプリケーションにおけるオブジェクトとアソシエーションの抽象化と考えることができます。 これにより開発者は、ストレージ スキーマを気にすることなく概念モデルに集中でき、効率的で保守性に優れたコードを書けるようになります。 同時にストレージ スキーマの設計者は、データ アクセス、ストレージ、およびスケーラビリティの効率性に集中できます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 このセクションのトピックでは、Entity Data Model の概念について説明します。 EDM を実装する DSL には、ここで解説した概念を含める必要があります。 [ADO.NET Entity Framework](./ef/index.md)は CSDL を使用して概念モデルを定義することに注意してください。 詳細については、「 [CSDL Specification](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)」を参照してください。  
  
 [Entity Data Model キーの概念](entity-data-model-key-concepts.md)  
  
 [Entity Data Model: 名前空間](entity-data-model-namespaces.md)  
  
 [Entity Data Model: プリミティブ データ型](entity-data-model-primitive-data-types.md)  
  
 [Entity Data Model: 継承](entity-data-model-inheritance.md)  
  
 [アソシエーション End](association-end.md)  
  
 [アソシエーション End の多重度](association-end-multiplicity.md)  
  
 [アソシエーション セット](association-set.md)  
  
 [アソシエーション セット End](association-set-end.md)  
  
 [アソシエーション型](association-type.md)  
  
 [複合型](complex-type.md)  
  
 [エンティティ コンテナー](entity-container.md)  
  
 [エンティティ キー](entity-key.md)  
  
 [エンティティ セット](entity-set.md)  
  
 [エンティティ型](entity-type.md)  
  
 [facet](facet.md)  
  
 [外部キーのプロパティ](foreign-key-property.md)  
  
 [モデル宣言関数](model-declared-function.md)  
  
 [モデル定義関数](model-defined-function.md)  
  
 [ナビゲーション プロパティ](navigation-property.md)  
  
 [プロパティ](property.md)  
  
 [参照整合性制約](referential-integrity-constraint.md)  
  
## <a name="see-also"></a>関連項目

- [ADO.NET Entity Data Model ツール](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb399249(v=vs.100))
- [.edmx ファイルの概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/cc982042(v=vs.100))
- [CSDL 仕様](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)
