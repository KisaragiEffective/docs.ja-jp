---
title: 参照整合性制約
ms.date: 03/30/2017
ms.assetid: 3d3ba44b-4302-40d8-a7a9-62932e0395e5
ms.openlocfilehash: ad35df7bcca62ffdbc3842b0817b22c5482a3d4d
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738377"
---
# <a name="referential-integrity-constraint"></a>参照整合性制約
Entity Data Model (EDM) の*参照整合性制約*は、リレーショナルデータベースの参照整合性制約に似ています。 データベーステーブルの列が別のテーブルの主キーを参照できるのと同じように、[エンティティ型](entity-type.md)の[プロパティ](property.md)(またはプロパティ) は、別のエンティティ型の[エンティティキー](entity-key.md)を参照できます。 参照されるエンティティ型は、制約の*プリンシパル end*と呼ばれます。 プリンシパル end を参照するエンティティ型は、制約の*依存 end*と呼ばれます。  
  
 参照整合性制約は、2つのエンティティ型間の[アソシエーション](association-type.md)の一部として定義されます。 参照整合性制約の定義には、次の情報を指定します。  
  
- 制約のプリンシパル End。 (エンティティ キーが依存 End により参照されるエンティティ型)  
  
- プリンシパル End のエンティティ キー。  
  
- 制約の依存 End。 (プリンシパル End のエンティティ キーを参照するプロパティを持つエンティティ型)  
  
- 依存 End の参照プロパティ。  
  
 EDM の参照整合性制約の目的は、常に有効なアソシエーションが存在することを確認するためです。 詳細については、「[外部キーのプロパティ](foreign-key-property.md)」を参照してください。  
  
## <a name="example"></a>例  
 下のダイアグラムは、`WrittenBy` および `PublishedBy` という 2 つのアソシエーションの概念モデルを示しています。 `Book` エンティティ型には、`PublisherId` というプロパティがあります。`Publisher` アソシエーションに参照整合性制約を定義するときに、このプロパティは `PublishedBy` エンティティ型のエンティティ キーを参照します。  
  
 ![RefConstraintModel](./media/referential-integrity-constraint/reference-constraint-model.gif "参照制約モデルの例")  
  
 [ADO.NET Entity Framework](./ef/index.md)は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれるドメイン固有言語 (DSL) を使用して概念モデルを定義します。 次の CSDL は、上の概念モデルに示された `PublishedBy` アソシエーションの参照整合性制約を定義しています。  
  
 [!code-xml[EDM_Example_Model#RefConstraint](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books4.edmx#refconstraint)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
