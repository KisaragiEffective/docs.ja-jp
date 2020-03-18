---
title: アソシエーション セット End
ms.date: 03/30/2017
ms.assetid: fe4bf1d3-047a-4a37-98c5-a66e70811346
ms.openlocfilehash: f7ec1ca6fcdf299b9fcfc78f299ea4c6c5267cd3
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738615"
---
# <a name="association-set-end"></a>アソシエーション セット End
*アソシエーションセット end*は、[アソシエーションセット](association-set.md)の末尾にある[エンティティ型](entity-type.md)と[エンティティセット](entity-set.md)を識別します。 アソシエーション セット End はアソシエーション セットの一部として定義されます。アソシエーション セットには、アソシエーション セット End が 2 つ必要です。  
  
 アソシエーション セット End の定義には、次の情報が含まれます。  
  
- アソシエーション セットに含まれるエンティティ型の 1 つ。 (必須)  
  
- アソシエーション セットに含まれるエンティティ型のエンティティ セット。 (必須)  
  
## <a name="example"></a>例  
 下のダイアグラムは、`WrittenBy` および `PublishedBy` という 2 つのアソシエーションの概念モデルを示しています。  
  
 ![3種類のエンティティを持つモデルの例](./media/association-set-end/example-model-three-entity-types.gif)  
  
 次のダイアグラムには、上の概念モデルに基づくアソシエーション セット (`PublishedBy`) と 2 つのエンティティ セット (`Books` および `Publishers`) を示しています。 アソシエーション セット End は `Books` および `Publishers` エンティティ セットです。 `Books` エンティティセット内の Bi は、実行時に `Book` エンティティ型のインスタンスを表します。 同様に、Pj は、`Publishers` エンティティセット内の `Publisher` インスタンスを表します。 BiPj は、`PublishedBy` アソシエーションセット内の `PublishedBy` 関連付けのインスタンスを表します。  
  
 ![セットの例を示すスクリーンショット。](./media/association-set-end/sets-example-association.gif)  
  
 [ADO.NET Entity Framework](./ef/index.md)は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれる DSL を使用して概念モデルを定義します。 次の CSDL は、上のダイアグラムの各アソシエーションに対して 1 つのアソシエーション セットを持つエンティティ コンテナーを定義しています。 アソシエーション セット End はアソシエーション セット定義の一部として定義されています。  
  
 [!code-xml[EDM_Example_Model#EntityContainerExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entitycontainerexample)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
