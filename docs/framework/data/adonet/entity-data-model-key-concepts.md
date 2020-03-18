---
title: Entity Data Model キーの概念
ms.date: 03/30/2017
ms.assetid: c635a16d-6674-45aa-9344-dcb7df992bab
ms.openlocfilehash: b3a2f8eb9fa5b83eec5ba7c2a56c348d050d1938
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2019
ms.locfileid: "73737819"
---
# <a name="entity-data-model-key-concepts"></a>Entity Data Model キーの概念
Entity Data Model (EDM) では、*エンティティ型*、*アソシエーション型*、および*プロパティ*という3つの主要概念を使用して、データの構造を記述します。 これらは、EDM の実装においてデータ構造を記述する上で最も重要な概念です。  
  
## <a name="entity-type"></a>エンティティ型  
 [エンティティ型](entity-type.md)は、Entity Data Model でデータの構造を記述するための基本的な構成要素です。 概念モデルでは、エンティティ型は[プロパティ](property.md)から作成され、ビジネスアプリケーション内の顧客や注文などの最上位レベルの概念の構造を記述します。 コンピューター プログラムのクラス定義がクラスのインスタンスのテンプレートになるように、エンティティ型はエンティティのテンプレートになります。 エンティティは、特定のオブジェクト (特定の顧客や注文など) を表します。 各エンティティには、[エンティティセット](entity-set.md)内で一意の[エンティティキー](entity-key.md)が必要です。  エンティティ セットは、特定のエンティティ型のインスタンスのコレクションです。 エンティティセット (および[アソシエーションセット](association-set.md)) は、[エンティティコンテナー](entity-container.md)に論理的にグループ化されます。  
  
 エンティティ型では継承がサポートされており、1 つのエンティティ型を別のエンティティ型から派生させることができます。 詳細については、「 [Entity Data Model: 継承](entity-data-model-inheritance.md)」を参照してください。  
  
## <a name="association-type"></a>アソシエーション型  
 [アソシエーション型](association-type.md)(アソシエーションとも呼ばれます) は、Entity Data Model 内のリレーションシップを記述するための基本的な構成要素です。 概念モデルでは、アソシエーションが 2 つのエンティティ型 (Customer や Order など) の間のリレーションシップを表します。 すべてのアソシエーションには、アソシエーションに関連するエンティティ型を指定する2つの[アソシエーション end](association-end.md)があります。 各アソシエーション end は、アソシエーションのその end に存在できるエンティティの数を示す[アソシエーション end の多重度](association-end-multiplicity.md)も指定します。 アソシエーション end の多重度には、1 (1)、0または 1 (0 ..1)、または多数 (\*) の値を指定できます。 アソシエーションの一方の end のエンティティには、[ナビゲーションプロパティ](navigation-property.md)を使用して、またはエンティティ型で公開されている場合は外部キーを使用してアクセスできます。 詳細については、「[外部キーのプロパティ](foreign-key-property.md)」を参照してください。  
  
 アプリケーションでは、アソシエーションのインスタンスが特定のアソシエーション (Customer のインスタンスと Order のインスタンスの間のアソシエーションなど) を表します。 アソシエーションインスタンスは、[アソシエーションセット](association-set.md)に論理的にグループ化されます。 アソシエーションセット (および[エンティティセット](entity-set.md)) は、[エンティティコンテナー](entity-container.md)に論理的にグループ化されます。  
  
## <a name="property"></a>property  
 [エンティティ型](entity-type.md)には、その構造と特性を定義する[プロパティ](property.md)が含まれています。 たとえば、Customer エンティティ型には、CustomerId、Name、Address などのプロパティがあります。  
  
 概念モデルのプロパティは、コンピューター プログラムのクラスに定義されるプロパティに似ています。 クラスのプロパティがクラスの構造を定義し、オブジェクトに関する情報を伝達するのと同様に、概念モデルのプロパティはエンティティ型の構造を定義し、エンティティ型のインスタンスに関する情報を伝達します。  
  
 プロパティには、プリミティブ データ (文字列、整数、ブール値など) または構造化データ (複合型) を含めることができます。 詳細については、「 [Entity Data Model: プリミティブデータ型](entity-data-model-primitive-data-types.md)」を参照してください。  
  
## <a name="representations-of-a-conceptual-model"></a>概念モデルの表現  
 *概念モデル*は、エンティティおよびリレーションシップとしての一部のデータ構造の特定の表現です。 概念モデルを表現する 1 つの手段として、ダイアグラムを使用することができます。 下のダイアグラムは、3 つのエンティティ型 (`Book`、`Publisher`、および `Author`) と 2 つのアソシエーション (`PublishedBy` および `WrittenBy`) の概念モデルを表しています。  
  
 ![3種類のエンティティを持つ概念モデルを示す図](./media/entity-data-model-key-concepts/conceptual-model-entity-types-associations.gif)  
  
 しかし、この表現には、モデルに関する詳細を伝える上でいくつかの欠点があります。 たとえば、プロパティ型とエンティティ セットの情報はダイアグラムに示されていません。 ドメイン固有言語 (DSL) を使用すると、概念モデルの詳細情報をさらに明確に伝えることができます。 [ADO.NET Entity Framework](./ef/index.md)は、概念*スキーマ定義言語*([CSDL](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) と呼ばれる XML ベースの DSL を使用して概念モデルを定義します。 以下に、上のダイアグラムに示されている概念モデルの CSDL 定義を示します。  
  
 [!code-xml[EDM_Example_Model#EDMExampleCSDL](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#edmexamplecsdl)]  
  
## <a name="see-also"></a>関連項目

- [Entity Data Model](entity-data-model.md)
