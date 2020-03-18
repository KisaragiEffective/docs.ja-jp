---
title: 名前空間の概要 (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: 16283322-8238-4918-ab11-802ac6748eb7
ms.openlocfilehash: d5f176a5561b77126ef23af996ab1e4bf51092ad
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "68868826"
---
# <a name="namespaces-overview-linq-to-xml"></a>名前空間の概要 (LINQ to XML)

この記事では、名前空間、<xref:System.Xml.Linq.XName> クラス、<xref:System.Xml.Linq.XNamespace> クラスについて説明します。

## <a name="xml-names"></a>XML 名

XML 名は、多くの場合、XML プログラミングを複雑にしている原因です。 XML 名は、XML 名前空間 (XML 名前空間 URI) とローカル名で構成されます。 XML 名前空間は、.NET Framework ベースのプログラムにおける名前空間と似ています。 これを使用すると、要素と属性の名前を一意に修飾できます。 このため、XML ドキュメントのさまざまな部分の間で、名前の競合を回避するのに役立ちます。 XML 名前空間を宣言した後は、その名前空間内でのみ一意であるローカル名を選択できます。

XML 名には、XML *名前空間プレフィックス*という側面もあります。 XML プレフィックスは、XML 名の複雑さの主要な原因です。 このプレフィックスを使用すると、XML 名前空間に対するショートカットを作成できるため、XML ドキュメントがより簡潔でわかりやすくなります。 ただし XML プレフィックスはコンテキストによって意味が異なり、これが複雑さの原因となります。 たとえば、XML プレフィックス `aw` に関連付けられている XML 名前空間が、同じ XML ツリー内でも部分によって異なる可能性があります。

C# で [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] を使用する利点の 1 つは、XML プレフィックスを使用する必要がないという点です。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] で XML ドキュメントを読み込んで解析するときに、各 XML プレフィックスは、対応する XML 名前空間に解決されます。 以後、名前空間を使用するドキュメントの操作時には、ほとんどの場合、名前空間プレフィックスではなく名前空間 URI を通して名前空間にアクセスします。 開発者が [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] で XML 名を操作するときは、必ず完全修飾 XML 名 (つまり XML 名前空間とローカル名) を操作します。 ただし、[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] では、必要に応じて名前空間プレフィックスを操作したり制御したりできます。

[!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] で XML 名を表すクラスは <xref:System.Xml.Linq.XName> です。 [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] API では XML 名がよく使用されており、XML 名が必要な場合には必ず <xref:System.Xml.Linq.XName> パラメーターが存在します。 しかし、<xref:System.Xml.Linq.XName> を直接操作することはほとんどありません。 <xref:System.Xml.Linq.XName> には、文字列からの暗黙の変換が含まれています。

詳細については、「 <xref:System.Xml.Linq.XNamespace> および <xref:System.Xml.Linq.XName>」を参照してください。
