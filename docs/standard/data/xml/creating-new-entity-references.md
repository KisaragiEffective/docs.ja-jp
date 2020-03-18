---
title: 新しいエンティティ参照の作成
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: a42f81b3-0403-4e34-b346-7d2129804e54
ms.openlocfilehash: 8c81aae89bbe5979dffdc47a369349bd2b3f2df7
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710987"
---
# <a name="creating-new-entity-references"></a>新しいエンティティ参照の作成
**CreateEntityReference** メソッドによって新しい **XmlEntityReference** ノードを作成できます。 XML ドキュメント オブジェクト モデル (DOM) は、参照されているエンティティ名が既に宣言されているかどうかを確認します。 宣言されている場合は、エンティティ宣言ノードから **XmlEntityReference** ノードの子ノードがコピーされます。 一致するエンティティ宣言がない場合、エンティティ参照ノードには、唯一の子として空のテキスト ノードが追加されます。 **XmlEntityReference** ノードの子ノードは別のノードのコピーなので、これらの子ノードは読み取り専用になり、変更はできません。  
  
 ノードがコピーされるとき、そのエンティティ参照が存在する位置のスコープに、名前空間が適用されていることがあります。 生成される要素ノードまたは属性ノードの構成は、この名前空間の影響を受けます。  
  
> [!NOTE]
> DOM は、ドキュメントに **EntityReference** ノードが挿入されたときだけ **EntityReference** に子ノードを追加します。 新しく作成された **EntityReference** ノードには、子ノードはありません。  
  
 **XmlDataDocument** は **XmlDocument** の派生クラスですが、**XmlDataDocument** はエンティティ参照の作成をサポートしていません。 **EntityReference** の子が読み取り専用であるためです。 **EntityReference** ノードの子の範囲は、複数の領域にまたがることができます。 この場合は、**EntityReference** の一部を含む領域と関連付けられている行の部分が読み取り専用になります。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
