---
title: アソシエーション End
ms.date: 03/30/2017
ms.assetid: 2c345213-0296-4d90-ac6d-cef179798a75
ms.openlocfilehash: 489802ca18708e076c0cd5dd380ad1361916ad5f
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73732357"
---
# <a name="association-end"></a>アソシエーション End
アソシエーション*end*は、アソシエーションの一方の end の[エンティティ型](entity-type.md)と、アソシエーションのその end に存在できるエンティティ型のインスタンスの[数を識別](association-type.md)します。 アソシエーション End はアソシエーションの一部として定義され、アソシエーションには必ず 2 つのアソシエーション End が必要です。 [ナビゲーションプロパティ](navigation-property.md)を使用すると、1つのアソシエーションエンドからもう一方のアソシエーション end へのナビゲーションを行うことができます。  
  
 アソシエーション End の定義には、次の情報が含まれます。  
  
- アソシエーションに含まれるエンティティ型の 1 つ。 (必須)  
  
    > [!NOTE]
    > アソシエーションでは、各アソシエーション End に同じエンティティ型を指定することができます。 これにより、自己アソシエーションが作成されます。  
  
- アソシエーションの1つの end に存在できるエンティティ型インスタンスの数を示す[アソシエーション end の多重度](association-end-multiplicity.md)。 アソシエーション end の多重度には、1 (1)、0または 1 (0 ..1)、または多数 (\*) の値を指定できます。  
  
- アソシエーション End の名前。 (オプション)。  
  
- アソシエーション End に実行する CASCADE ON DELETE などの操作の情報。 (オプション)。  
  
## <a name="example"></a>例  
 下のダイアグラムは、`PublishedBy` および `WrittenBy` という 2 つのアソシエーションの概念モデルを示しています。 `PublishedBy` アソシエーションのアソシエーション End は `Book` および `Publisher` のエンティティ型です。 `Publisher` end の多重度は 1 (1) で、`Book` end の多重度は多数 (\*) であり、パブリッシャーが多数のブックを発行し、1つの出版社によって書籍が発行されていることを示します。  
  
 ![3種類のエンティティを持つモデルの例](./media/association-end/example-model-three-entity-types.gif)  
  
 ADO.NET Entity Framework は、概念スキーマ定義言語 ([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれるドメイン固有言語 (DSL) を使用して概念モデルを定義します。 次の CSDL は、上のダイアグラムに示された `PublishedBy` アソシエーションを定義します。 各アソシエーション End の型、名前、および多重度は、XML 属性 (それぞれ `Type` 属性、`Role` 属性、および `Multiplicity` 属性) で指定されます。 アソシエーション End に実行する操作に関するオプション情報は、XML 要素 (`OnDelete` 要素) に指定します。 この場合、出版社を削除すると、関連付けられたすべての書籍も削除されます。  
  
 [!code-xml[EDM_Example_Model#AssociationEnd](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books3.edmx#associationend)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model キーの概念](entity-data-model-key-concepts.md)
- [Entity Data Model](entity-data-model.md)
