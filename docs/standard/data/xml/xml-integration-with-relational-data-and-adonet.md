---
title: XML とリレーショナル データおよび ADO.NET との統合
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: f6ebb1a1-f2ca-49b9-92c9-0150940cf6e6
ms.openlocfilehash: 30b788c77a2352d0d02ee772ab3f428381facd9f
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78155621"
---
# <a name="xml-integration-with-relational-data-and-adonet"></a>XML とリレーショナル データおよび ADO.NET との統合
**XmlDocument** の派生クラスである **XmlDataDocument** クラスには XML データが格納されます。 **XmlDataDocument** の利点は、リレーショナル データと階層データとを仲介できることです。 **DataSet** に連結できるのは **XmlDocument** であり、どちらのクラスも、それぞれが格納しているデータが変更されたときに、変更内容の同期をとることができます。 **DataSet** に連結した **XmlDocument** では XML をリレーショナル データと統合できるため、データ表現は XML でもリレーショナル形式でもかまいません。 両方の処理ができ、一方のデータ表現だけに制限されることもありません。  
  
 2 つの形式でデータが使用できる利点は次のとおりです。  
  
- XML ドキュメントの構造部分はデータセットに対応付けることができ、効率的な格納、インデックス付け、および検索ができる。  
  
- リレーション形式で格納された XML データに対して、カーソル モデルを使用して、効率的な変換、検証、および移動ができる。 **XmlDocument** モデルに XML が格納されている場合よりも、リレーショナル構造の方が効率的に処理できる場合があります。  
  
- **DataSet** に XML の一部を格納できる。 **XPath** または **XslTransform** を使用して、目的の要素や属性だけを **DataSet** に格納できます。 この場合は、抽出された部分的なデータに対して変更を行い、その変更を **XmlDataDocument** 内のデータに反映させることができます。  
  
 SQL サーバーから **DataSet** に読み込まれたデータを変換することもできます。 また、.NET Framework のクラスの形式で管理されている WinForm および WebForm のコントロールを、XML 入力ストリームからデータを読み込んだ **DataSet** に連結することもできます。  
  
 **XslTransform** のサポートの他に、**XmlDataDocument** は、リレーショナル データに対する **XPath** クエリと検証もサポートします。  基本的には、すべての XML サービスをリレーショナル データで利用でき、コントロールの連結やコード生成などのすべてのリレーショナル機能を XML に基づく構造化データで、XML の厳密性を損なうことなく利用できます。  
  
 **XmlDataDocument** は **XmlDocument** から継承されているため、W3C DOM が実装されています。 **XmlDataDocument** は、**DataSet** に関連付けられており、そのデータの一部を格納していますが、**XmlDocument** として使用する場合に制限や特別な対応が必要になることはありません。 **XmlDocument** を使用しているコードは、**XmlDataDocument** を使用する場合でも、修正せずに動作させることができます。 **DataSet** は、テーブル、列、リレーション、および制約を定義することで、データをリレーショナル形式で提示できる、スタンドアロンかつインメモリのユーザー データ ストアです。  
  
 XML データと **DataSet** の関係と、XML データと **XmlDataDocument** の関連との違いを次の図に示します。
  
 ![XML DataSet との関連の違いを示す図。](./media/xml-integration-with-relational-data-and-adonet/xml-integration-relational-data-adodotnet.gif)  
  
 この図では、XML データは **DataSet** に直接読み込むことができ、XML をリレーショナルな方法で直接操作できることが示されています。 また、XML を DOM の派生クラスである **XmlDataDocument** に読み込み、その後、**DataSet** と同期させることができることも示されています。 **DataSet** と **XmlDataDocument** は、1 つのデータ セットを基にして同期がとられているため、一方のデータ ストアのデータを変更すると、他方のストアに反映されます。  
  
 **XmlDataDocument** は、**XmlDocument** のすべての編集機能と移動機能を継承します。 状況によっては、XML を直接 **DataSet** に読み込む方法よりも、**XmlDataDocument** とその継承された機能を使用して **DataSet** と同期する方法の方が適している場合があります。 **DataSet** にデータを読み込む場合に、どちらの方法を採用すればよいかを判断するための条件を次の表に示します。  
  
|XML を DataSet に直接読み込む方がよい場合|XmlDataDocument と DataSet の同期をとる方がよい場合|  
|----------------------------------------------|-----------------------------------------------------------|  
|**DataSet** のデータに対するクエリで、XPath よりも SQL を使用する方が簡単である。|**DataSet** のデータに対して XPath クエリが必要である。|  
|ソース XML 内での要素の順序を保つ必要がない。|ソース XML 内での要素の順序を保つ必要がある。|  
|要素間の空白や形式をソース XML で保持する必要がない。|空白や形式をソース XML で保持する必要がある。|  
  
 **DataSet** に XML を直接読み込んだり、DataSet から XML を書き出す方法については、「[XML からの DataSet の読み込み](../../../../docs/framework/data/adonet/dataset-datatable-dataview/loading-a-dataset-from-xml.md)」と「[XML データとしての DataSet の書き込み](../../../../docs/framework/data/adonet/dataset-datatable-dataview/writing-dataset-contents-as-xml-data.md)」を参照してください。  
  
 **XmlDataDocument** から **DataSet** にデータを読み込む方法については、[Dataset と XML ドキュメントの同期](../../../../docs/framework/data/adonet/dataset-datatable-dataview/dataset-and-xmldatadocument-synchronization.md)に関するページを参照してください。  
  
## <a name="see-also"></a>参照

- [DataSet での XML の使用](../../../../docs/framework/data/adonet/dataset-datatable-dataview/using-xml-in-a-dataset.md)
