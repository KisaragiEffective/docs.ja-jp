---
title: エンティティ型
ms.date: 03/30/2017
ms.assetid: a6dee9ab-9e4a-48f2-a169-3f79cc15821c
ms.openlocfilehash: 1dafce5f7f95ba6f391c8742944f40a9afa7dcf8
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73737807"
---
# <a name="entity-type"></a>エンティティ型
*エンティティ型*は、ENTITY DATA MODEL (EDM) を使用してデータの構造を記述するための基本的な構成要素です。 概念モデルでのエンティティ型は、顧客や注文のように、トップレベル概念の構造を表します。 エンティティ型は、エンティティ型のインスタンスのテンプレートです。 各テンプレートには、次の情報が含まれています。  
  
- 一意の名前 (必須)  
  
- 1つ以上のプロパティによって定義される[エンティティキー](entity-key.md) 。 (必須)  
  
- [プロパティ](property.md)の形式のデータ。 (省略可能)  
  
- [アソシエーション](association-type.md)の一方の[端](association-end.md)からもう一方の端へのナビゲーションを可能にする[ナビゲーションプロパティ](navigation-property.md)。 (オプション)。  
  
 アプリケーションでは、エンティティ型のインスタンスが特定のオブジェクト (特定の顧客や注文など) を表します。 エンティティ型の各インスタンスは、エンティティ[セット](entity-set.md)内で一意の[エンティティキー](entity-key.md)を持つ必要があります。  
  
 2 つのエンティティ型のインスタンスは、型が同じであり、エンティティ キーの値が等しい場合にのみ、等価のインスタンスと見なされます。  
  
## <a name="example"></a>例  
 下のダイアグラムは、`Book`、`Publisher`、および `Author` という 3 つのエンティティ型の概念モデルを示しています。  
  
 ![3種類のエンティティを持つモデルの例](./media/entity-type/example-model-three-entity-types.gif)  
  
 各エンティティ型のエンティティ キーを構成するプロパティには、"(キー)" と示されています。  
  
 [ADO.NET Entity Framework](./ef/index.md)は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれるドメイン固有言語 (DSL) を使用して概念モデルを定義します。 次の CSDL は、上のダイアグラムに示された `Book` エンティティ型を定義しています。  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
- [facet](facet.md)
