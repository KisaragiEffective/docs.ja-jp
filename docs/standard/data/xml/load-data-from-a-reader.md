---
title: リーダーからのデータの読み込み
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 7e74918c-bc72-4977-a49b-e1520a6d8f60
ms.openlocfilehash: 90a66e04bda4fb2ee4216e8aabd631afb2f28dd0
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710714"
---
# <a name="load-data-from-a-reader"></a>リーダーからのデータの読み込み
<xref:System.Xml.XmlDocument.Load%2A> メソッドに <xref:System.Xml.XmlReader> パラメーターを渡して XML ドキュメントを読み込むと、他の形式からデータを読み込む場合と比較して、その動作に違いがあります。 リーダーが初期状態のとき、<xref:System.Xml.XmlDocument.Load%2A> メソッドは、リーダーからのコンテンツ全体を使用して、リーダー内のすべてのデータから XML ドキュメント オブジェクト モデル (DOM) を構築します。  
  
 既にリーダーがドキュメント内のあるノード上に位置している場合、リーダーが <xref:System.Xml.XmlDocument.Load%2A> メソッドに渡されると、<xref:System.Xml.XmlDocument.Load%2A> は現在のノードと、現在の深さを閉じる終了タグまで、その兄弟すべてをメモリに読み込もうとします。 <xref:System.Xml.XmlDocument.Load%2A> はリーダーからの XML が整形式かどうかを検証するので、<xref:System.Xml.XmlDocument.Load%2A> が成功するかどうかは、読み込みを試みた時点でリーダーが位置していたノードに依存します。 XML が整形式でない場合、<xref:System.Xml.XmlDocument.Load%2A> は例外をスローします。 たとえば、次のノード セットは、ルート レベルの要素が 2 つ含まれ、XML が整形式ではないため、<xref:System.Xml.XmlDocument.Load%2A> は例外をスローします。  
  
- Comment ノード、Element ノード、Element ノード、EndElement ノードの順序のノード セット  
  
 次のノード セットは、ルート レベルの要素が存在しないため、不完全な DOM が作成されます。  
  
- Comment ノード、ProcessingInstruction ノード、Comment ノード、EndElement ノードの順序のノード セット  
  
 この場合、例外はスローされず、データが読み込まれます。 これらのノードの最上位にルート要素を追加すると、エラーなしで保存できる整形式の XML を作成できます。  
  
 ドキュメントのルート レベルとしては無効なリーフ ノード (たとえば空白ノードや属性ノード) にリーダーが位置している場合、リーダーはルートとして使用できるノードに移動するまで読み込みを続行します。 ドキュメントの読み込みは、この位置から開始されます。  
  
 既定では、<xref:System.Xml.XmlDocument.Load%2A> は、ドキュメント型定義 (DTD) またはスキーマ検証を使用して、XML が有効かどうかを検証しません。 チェックするのは、XML が整形式かどうかだけです。 検証を行うには、<xref:System.Xml.XmlReader> クラスを使用して <xref:System.Xml.XmlReaderSettings> を作成する必要があります。 <xref:System.Xml.XmlReader> クラスは、DTD またはスキーマ定義言語 (XSD) のスキーマを使用して検証を強制できます。 <xref:System.Xml.ValidationType> クラスの <xref:System.Xml.XmlReaderSettings> プロパティによって、<xref:System.Xml.XmlReader> インスタンスが検証を強制するかどうかが決まります。 XML データの検証の詳細については、<xref:System.Xml.XmlReader> のリファレンス ページの「解説」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
