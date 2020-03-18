---
title: 保持されずに展開されるエンティティ参照
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: ffd97806-ab43-4538-8de2-5828bfbbde57
ms.openlocfilehash: ae3db77d7659b7e1d36a9bccf7143f52c536dbbf
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710935"
---
# <a name="entity-references-are-expanded-and-not-preserved"></a>保持されずに展開されるエンティティ参照
エンティティ参照が展開され、それが表すテキストに置き換えられる場合には、**XmlEntityReference** ノードは作成されません。 この場合はエンティティ宣言が解析され、宣言のコンテンツから作成されたノードが **XmlEntityReference** の代わりにコピーされます。 したがって、`&publisher;` の例では、`&publisher;` は保存されず、代わりに **XmlText** ノードが作成されます。  
  
 ![展開されたツリー構造](../../../../docs/standard/data/xml/media/xmlentityref-expanded-nodes.gif "xmlentityref_expanded_nodes")  
エンティティ参照が展開される場合のツリー構造  
  
 `B` や `<` などの文字エンティティは保持されません。 文字エンティティは常に展開され、テキスト ノードとして表されます。  
  
 **XmlEntityReference** ノードと、それに付随するエンティティ参照の子ノードを保持するには、**EntityHandling** フラグを **ExpandCharEntities** に設定します。 それ以外の場合は、**EntityHandling** フラグを既定の **ExpandEntities** のままにしておきます。 その場合、DOM ではエンティティ参照ノードが認識されません。 エンティティ参照ノードは、エンティティ宣言の子ノードのコピーに置き換えられます。  
  
 エンティティ参照を保持しない場合の副作用の 1 つとして、そのドキュメントを保存して別のアプリケーションに渡したとき、受け取る側のアプリケーションでは、エンティティ参照によってノードが生成されたことを識別できないという点があります。 しかし、エンティティ参照を保持する場合は、受け取り側アプリケーションがエンティティ参照を認識し、子ノードを読み取ります。 子ノードが、エンティティ宣言に含まれていた情報を表していることは明らかです。 たとえば、エンティティ参照が保持される場合、DOM の構造は理論的に次のようになります。  
  
 XmlElement: publisher  
  
 XmlEntityReference: `&publisher;`  
  
 XmlText: Microsoft Press  
  
 DOM でエンティティ参照が展開される場合は、次のようなツリー構造になります。既定のメソッドでは、エンティティ参照が展開されます。  
  
 XmlElement: publisher  
  
 XmlText: Microsoft Press  
  
 エンティティ参照がないため、受け取る側のアプリケーションでは、"Microsoft Press" を含む **XmlText** ノードがエンティティ宣言から作成されたことを識別できません。  
  
 エンティティを解決できないリーダーを使用すると、**Load** メソッドは、エンティティ参照を見つけたときに例外をスローします。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
