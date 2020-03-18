---
title: 外部リソースの解決
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: ad3fa320-4b8f-4e5c-b549-01157591007a
ms.openlocfilehash: 05cc41cef7da07581d4f0ec8e584858b913d1a80
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710272"
---
# <a name="resolving-external-resources"></a>外部リソースの解決
**XmlDocument** クラスでは、外部のドキュメント型定義 (DTD)、エンティティ、スキーマなど、XML データのインラインでないリソースを検索するときに **XmlDocument** の **XmlResolver** プロパティを使用します。 これらのリソースは、ネットワーク上やローカル ドライブ上にあり、URI (Uniform Resource Identifier) で識別できます。 そのため、**XmlDocument** は、ドキュメント内にある **EntityReference** ノードを解決し、外部の DTD またはスキーマに基づいてドキュメントを検証することができます。  
  
## <a name="fully-trusted-xmldocument"></a>完全に信頼されている XmlDocument  
 **XmlResolver** プロパティは、**XmlDocument.Load** メソッドの機能に影響を与えることがあります。 **XmlDocument** オブジェクトが完全に信頼されている場合の **XmlDocument.XmlResolver** プロパティの動作を次の表に示します。 Load への入力が **TextReader**、**String**、**Stream**、または **URI** である場合の **XmlDocument.Load** メソッドが対象です。 **XmlDocument** が **XmlReader** から読み込まれた場合の **Load** メソッドは対象外です。  
  
|XmlResolver プロパティ|関数|メモ|  
|--------------------------|--------------|-----------|  
|プロパティが、以前作成され、ユーザーの設定したプロパティを持っている **XmlResolver** クラスに設定されている。|**XmlDocument** は、ファイル名を解決するために指定された **XmlResolver** を使用し、DTD、エンティティ、スキーマなどの外部リソースへの参照を解決します。<br /><br /> **XmlResolver** は、**XmlDocument** のノードの追加や編集に必要な外部リソースを解決するときにも使用されます。|**XmlDocument** に指定された **XmlResolver** は、外部リソースの検索と解決が必要な場合に常に使用されるリゾルバーです。|  
|プロパティが **null** (Microsoft Visual Basic .NET では **Nothing**) に設定されている。|外部のスキーマや DTD の検索など、外部リソースを必要とする機能はサポートされていません。 外部エンティティの解決も行われず、解決が必要なエンティティ ノードの挿入などの編集機能もサポートされません。|**XmlDocument** は、ファイルを匿名で読み込みます。他のリソースを解決しようとはしません。|  
|プロパティが設定されておらず、既定状態のままである。|ファイル名を解決したり、外部の DTD、エンティティ、スキーマを検索するときは、NULL 資格情報を持つ **XmlUrlResolver** のインスタンスが作成され、**XmlDocument** によって使用されます。ノードを編集するときは、**null** 資格情報が使用されます。||  
  
 **Load** への入力が **XmlReader** であり、**XmlDocument** が完全に信頼されている場合の **XmlDocument.Load** メソッドを次の表に示します。  
  
|XmlResolver プロパティ|関数|メモ|  
|--------------------------|--------------|-----------|  
|**XmlDocument** で使用される **XmlResolver** クラスが、**XmlReader** で使用されるクラスと同じである。|**XmlDocument** は、**XmlReader** に割り当てられた **XmlResolver** を使用します。<br /><br /> **XmlReader**  から **XmlResolver** を取得しているため、**XmlDocument** の信頼レベルにかかわらず、**XmlDocument.Resolver** プロパティは設定できません。 **XmlDocument** の **XmlResolver** プロパティを設定することによって **XmlReaders** の **XmlResolver** の設定をオーバーライドすることはできません。|**XmlReader** は、**XmlTextReader**、**XmlValidatingReader**、または独自に作成したリーダーにすることができます。 使用するリーダーでエンティティ解決がサポートされている場合は、外部エンティティが解決されます。 渡されたリーダーでエンティティ参照がサポートされていない場合は、エンティティ参照は解決されません。|  
  
## <a name="semi-trusted-xmldocument"></a>信頼性の高くない XmlDocument  
 オブジェクトの信頼性が高くない場合の **XmlDocument.XmlResolver** プロパティの動作を次の表に示します。 Load への入力が **TextReader**、**String**、**Stream**、または **URI** である場合の **XmlDocument.Load** メソッドが対象です。 **XmlDocument** が **XmlReader** から読み込まれた場合の **Load** メソッドは対象外です。  
  
|XmlResolver プロパティ|関数|メモ|  
|--------------------------|--------------|-----------|  
|信頼性が高くないシナリオでは、**XmlResolver** プロパティは null 以外に設定できない。|ファイル名を解決したり、外部の DTD、エンティティ、スキーマを検索するときは、**null** 資格情報を持つ **XmlUrlResolver** のインスタンスが作成され、**XmlDocument** によって使用されます。ノードを編集するときは、**null** 資格情報が使用されます。|この動作は、**XmlResolver** プロパティが設定されておらず、既定状態のままである場合と同じ動作になります。<br /><br /> **XmlDocument** では、すべての処理で、匿名のアクセス許可を使用します。|  
|プロパティが **null** (Microsoft Visual Basic .NET では **Nothing**) に設定されている。|外部のスキーマや DTD の検索など、外部リソースを必要とする機能はサポートされていません。 外部エンティティの解決も行われず、解決が必要なエンティティ ノードの挿入などの編集機能もサポートされません。|プロパティが **null** の場合の動作は、**XmlDocument** の信頼性の高低に関係なく、同じです。|  
|プロパティが設定されておらず、既定状態のままである。|ファイル名を解決したり、外部の DTD、エンティティ、スキーマを検索するときは、**null** 資格情報を持つ **XmlUrlResolver** のインスタンスが作成され、**XmlDocument** によって使用されます。ノードを編集するときは、**null** 資格情報が使用されます。|**XmlDocument** では、すべての処理で、匿名のアクセス許可を使用します。|  
  
 **Load** への入力が **XmlReader** であり、**XmlDocument** の信頼性が高くない場合の **XmlDocument.Load** メソッドを次の表に示します。  
  
|XmlResolver プロパティ|関数|メモ|  
|--------------------------|--------------|-----------|  
|**XmlDocument** で使用される **XmlResolver** クラスが、**XmlReader** で使用されるクラスと同じである。|**XmlDocument** は、**XmlReader** に割り当てられた **XmlResolver** を使用します。<br /><br /> **XmlReader**  から **XmlResolver** を取得しているため、**XmlDocument** の信頼レベルにかかわらず、**XmlDocument.Resolver** プロパティは設定できません。 **XmlDocument**の**xmlresolver**プロパティを設定することにより、 **xmlreaders** **xmlresolver**の設定をオーバーライドすることはできません。|**XmlTextReader**、検証用の <xref:System.Xml.XmlReader>、または独自に作成したリーダーを **XmlTextReader** として使用できます。 使用するリーダーでエンティティ解決がサポートされている場合は、外部エンティティが解決されます。 渡されたリーダーでエンティティ参照がサポートされていない場合は、エンティティ参照は解決されません。|  
  
 XmlResolver が正しい資格情報を持つように設定すれば、外部リソースにアクセスできます。  
  
> [!NOTE]
> **XmlResolver** プロパティを取得する方法はありません。 これは、資格情報が設定されている **XmlResolver** をユーザーが再利用するのを防ぐためです。 また、**XmlTextReader** または検証用の <xref:System.Xml.XmlReader> を使用して、リゾルバーが設定されている **XmlDocument** を読み込んだ場合、**XmlDocument** は、セキュリティ リスクを回避するために、**Load** フェーズの後、これらのリーダーのリゾルバーをキャッシュしません。  
  
 詳細については、<xref:System.Xml.XmlResolver> のリファレンス ページの「解説」を参照してください。  
  
## <a name="see-also"></a>関連項目

- [XML ドキュメント オブジェクト モデル (DOM)](../../../../docs/standard/data/xml/xml-document-object-model-dom.md)
