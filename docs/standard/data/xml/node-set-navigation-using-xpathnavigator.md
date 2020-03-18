---
title: XPathNavigator を使用するノード セットのナビゲーション
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 1a954b41-7173-40bc-8544-d430f209b1e5
ms.openlocfilehash: 91115af03b635d7660721fac5ce8bd749953e4ff
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710571"
---
# <a name="node-set-navigation-using-xpathnavigator"></a>XPathNavigator を使用するノード セットのナビゲーション
<xref:System.Xml.XPath.XPathDocument> クラスのノード セット ナビゲーション メソッドを使用して、<xref:System.Xml.XmlDocument> または <xref:System.Xml.XPath.XPathNavigator> オブジェクト内のノード間を移動できます。 すべてのノード間の移動、または <xref:System.Xml.XPath.XPathNavigator> クラスの選択メソッドによって返される選択されたノード セット間を移動できます。  
  
## <a name="element-node-set-navigation"></a>要素ノード セットのナビゲーション  
 <xref:System.Xml.XPath.XPathNavigator> クラスは、要素ノードの移動に使用されるいくつかのメソッドを提供します。 利用可能なナビゲーション メソッドおよびその移動方法を次の表に示します。これには、属性および名前空間のノードのナビゲートに使用されるメソッドは含まれません。  
  
 <xref:System.Xml.XPath.XPathNavigator> オブジェクトでノードを選択する方法については、「[XPathNavigator を使用した XML データの選択、評価、および照合](../../../../docs/standard/data/xml/selecting-evaluating-and-matching-xml-data-using-xpathnavigator.md)」を参照してください。 属性ノードと名前空間ノードをナビゲートする方法については、「[XPathNavigator を使用する属性と名前空間のナビゲーション](../../../../docs/standard/data/xml/attribute-and-namespace-node-navigation-using-xpathnavigator.md)」を参照してください。  
  
|メソッド|説明|  
|------------|-----------------|  
|<xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>|<xref:System.Xml.XPath.XPathNavigator> を指定された <xref:System.Xml.XPath.XPathNavigator> と同じ位置に移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの子ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの最初の兄弟ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの最初の子ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToFollowing%2A>|<xref:System.Xml.XPath.XPathNavigator> をドキュメント順で指定された要素に移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>|<xref:System.Xml.XPath.XPathNavigator> を、与えられた `ID` に一致する値の <xref:System.String> 型の属性を持つノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの次の兄弟ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの親ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A>|<xref:System.Xml.XPath.XPathNavigator> を現在のノードの前の兄弟ノードに移動します。|  
|<xref:System.Xml.XPath.XPathNavigator.MoveToRoot%2A>|<xref:System.Xml.XPath.XPathNavigator> を XML ドキュメントのルート ノードに移動します。|  
  
## <a name="comments-and-processing-instruction-node-navigation"></a>コメントおよび処理命令ノードのナビゲーション  
 次の <xref:System.Xml.XPath.XPathNavigator> クラスのメソッドは XML ドキュメント内の他のノードからコメントまたは処理命令に移動するのに有効です。  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveTo%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToNext%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToPrevious%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirst%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToFirstChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToChild%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToParent%2A>  
  
- <xref:System.Xml.XPath.XPathNavigator.MoveToId%2A>  
  
## <a name="see-also"></a>関連項目

- <xref:System.Xml.XmlDocument>
- <xref:System.Xml.XPath.XPathDocument>
- <xref:System.Xml.XPath.XPathNavigator>
- [XPath データ モデルを使用した XML データの処理](../../../../docs/standard/data/xml/process-xml-data-using-the-xpath-data-model.md)
- [XPathNavigator を使用する属性と名前空間のナビゲーション](../../../../docs/standard/data/xml/attribute-and-namespace-node-navigation-using-xpathnavigator.md)
- [XpathNavigator を使用した XML データの抽出](../../../../docs/standard/data/xml/extract-xml-data-using-xpathnavigator.md)
- [厳密に型指定された XML データへの XPathNavigator を使用したアクセス](../../../../docs/standard/data/xml/accessing-strongly-typed-xml-data-using-xpathnavigator.md)
