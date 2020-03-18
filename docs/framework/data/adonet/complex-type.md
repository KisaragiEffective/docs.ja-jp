---
title: 複合型
ms.date: 03/30/2017
ms.assetid: 63efbd23-11d4-4871-bc88-ad01b9837553
ms.openlocfilehash: e21ca90a7be8f2bd9be9483c66a1e95e6ba1bee2
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738538"
---
# <a name="complex-type"></a>複合型
*複合型*は、[エンティティ型](entity-type.md)またはその他の複合型に対して、構造化された豊富なプロパティを定義するためのテンプレートです。 各テンプレートには、以下が含まれています。  
  
- 一意の名前 (必須)  
  
    > [!NOTE]
    > 複合型の名前は、同じ名前空間のエンティティ型の名前と同じにすることはできません。  
  
- 1つ以上の[プロパティ](property.md)の形式のデータ。 (省略可能)  
  
    > [!NOTE]
    > 複合型のプロパティには、別の複合型を使用することができます。  
  
 複合型は、プリミティブ型のプロパティまたはその他の複合型の形式でデータ ペイロードを伝達できる点がエンティティ型と似ています。 ただし、複合型とエンティティ型の間にはいくつかの重要な違いがあります。  
  
- 複合型には ID がないため、独立して存在することができません。 複合型は、エンティティ型またはその他の複合型のプロパティとしてのみ存在できます。  
  
- 複合型は[アソシエーション](association-type.md)に参加できません。 アソシエーションの end に複合型を指定することはできません。したがって、[ナビゲーションプロパティ](navigation-property.md)を複合型に対して定義することはできません。  
  
## <a name="example"></a>例  
 [ADO.NET Entity Framework](./ef/index.md)は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれるドメイン固有言語 (DSL) を使用して概念モデルを定義します。 次の CSDL は、`StreetAddress`、`City`、`StateOrProvince`、`Country`、および `PostalCode` のプリミティブ型のプロパティの複合型 Address を定義しています。  
  
 [!code-xml[EDM_Example_Model#ComplexTypeExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books2.edmx#complextypeexample)]  
  
 エンティティ型のプロパティとして複合型 `Address` (上の図) を定義するには、エンティティ型の定義にプロパティの型を宣言する必要があります。 次の CSDL は、エンティティ型 (Publisher) に複合型として `Address` プロパティを宣言しています。  
  
 [!code-xml[EDM_Example_Model#EntityWithComplexType](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books3.edmx#entitywithcomplextype)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
