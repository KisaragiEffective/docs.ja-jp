---
title: 'Entity Data Model: 名前空間'
ms.date: 03/30/2017
ms.assetid: 98ab4226-bb9f-44e7-af46-61a9b1a4e47c
ms.openlocfilehash: a403bcd459005ed9cea4a455fcde40c31c8caac9
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73738443"
---
# <a name="entity-data-model-namespaces"></a>Entity Data Model: 名前空間
Entity Data Model (EDM) の名前空間は、[エンティティ型](entity-type.md)、[複合型](complex-type.md)、および[アソシエーション](association-type.md)の抽象コンテナーです。 EDM の名前空間はプログラミング言語の名前空間と似ており、その中に含まれるオブジェクトのコンテキストを指定し、同じ名前を持つ (しかし、別の名前空間に含まれる) オブジェクトを明確に区別する手段になります。  
  
## <a name="example"></a>例  
 [ADO.NET Entity Framework](./ef/index.md)は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれるドメイン固有言語 (DSL) を使用して概念モデルを定義します。 次の CSDL コードは、名前空間を使用して異なる概念モデルに定義された型を識別します。 この例は、`Publisher` 名前空間からインポートされた複合型のプロパティ (`Address`) を持つエンティティ型 (`ExtendedBooksModel`) を定義します。 `Using` 要素は、名前空間がインポートされていることを示します。 さらに、`Address` プロパティの型は、完全修飾名 (`ExtendedBooksModel.Address`) で定義されており、この型が `ExtendedBooksModel` 名前空間で定義されていることを示します。  
  
 [!code-xml[EDM_Example_Model#ImportedNamespace](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books6.edmx#importednamespace)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
