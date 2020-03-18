---
title: XML 型サポートの実装に関するメモ
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 26b071f3-1261-47ef-8690-0717f5cd93c1
ms.openlocfilehash: 40ab0f746ef82ccd195fc6b873f5c8edb255f868
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75709869"
---
# <a name="xml-type-support-implementation-notes"></a>XML 型サポートの実装に関するメモ
このトピックでは、認識しておく必要があるいくつかの実装上の詳細について説明します。  
  
## <a name="list-mappings"></a>リストのマッピング  
 <xref:System.Collections.IList>、<xref:System.Collections.ICollection>、<xref:System.Collections.IEnumerable>、**Type[]** 、<xref:System.String> 型は、XML スキーマ定義言語 (XSD) のリスト型を表現するために使用されます。  
  
## <a name="union-mappings"></a>ユニオンのマッピング  
 ユニオン型は <xref:System.Xml.Schema.XmlAtomicValue> 型または <xref:System.String> 型を使用して表現されます。 したがって、変換前の型または変換後の型は常に <xref:System.String> または <xref:System.Xml.Schema.XmlAtomicValue> のどちらかである必要があります。  
  
 <xref:System.Xml.Schema.XmlSchemaDatatype> オブジェクトがリスト型を表す場合、このオブジェクトは入力文字列値をリストまたは複数のオブジェクトに変換します。 <xref:System.Xml.Schema.XmlSchemaDatatype> が共用体型を表す場合は、共用体のメンバーの型として入力値が解析されます。 解析が失敗した場合は、変換が成功するか他のメンバーの型がなくなるまで、ユニオンの次のメンバーを使用して変換が試行されます。変換を試行するメンバーの型がなくなると、例外がスローされます。  
  
## <a name="differences-between-clr-and-xml-data-types"></a>CLR 型と XML データ型の違い  
 CLR 型と XML データ型の間で発生する可能性のある不一致とその処理方法を次に示します。  
  
> [!NOTE]
> `xs` プレフィックスは、<https://www.w3.org/2001/XMLSchema> と名前空間の URI にマップされます。
  
### <a name="systemtimespan-and-xsduration"></a>System.TimeSpan と xs:duration  
 `xs:duration` 型は、値は異なるが同等の継続時間値として、部分的な順序付けが行われます。 これは、たとえば 1 か月 (P1M) という `xs:duration` 型の値が、32 日 (P32D) より小さく、27 日 (P27D) より大きく、28 日、29 日または 30 日と同等であることを意味します。  
  
 <xref:System.TimeSpan> クラスは、この部分的な順序付けをサポートしません。 その代わりに、1 年および 1 か月に特定の日数 (それぞれ 365 日と 30 日) を使用します。  
  
 `xs:duration` 型の詳細については、W3C の 「[XML Schema Part 2: Datatypes Recommendation](https://www.w3.org/TR/xmlschema-2/)」(XML スキーマ 第 2 部: データ型の推奨事項) を参照してください。
  
### <a name="xstime-gregorian-date-types-and-systemdatetime"></a>xs:time、グレオリオ暦の日付、および System.DateTime  
 `xs:time` 値が <xref:System.DateTime> オブジェクトにマップされている場合、<xref:System.DateTime.MinValue> フィールドは <xref:System.DateTime> オブジェクトの日付プロパティ (<xref:System.DateTime.Year%2A>、<xref:System.DateTime.Month%2A>、<xref:System.DateTime.Day%2A> など) を可能な最小の <xref:System.DateTime> 値に初期化するために使用されます。  
  
 同様に、`xs:gMonth`、`xs:gDay`、`xs:gYear`、`xs:gYearMonth`、および `xs:gMonthDay` のインスタンスは、<xref:System.DateTime> オブジェクトにマップされます。 <xref:System.DateTime> オブジェクトの未使用のプロパティは、<xref:System.DateTime.MinValue> の値で初期化されます。  
  
> [!NOTE]
> 内容が <xref:System.DateTime.Year%2A?displayProperty=nameWithType> として型指定されている場合は、`xs:gMonthDay` 値は信頼できません。 この場合、<xref:System.DateTime.Year%2A?displayProperty=nameWithType> 値は常に 1,904 に設定されています。  
  
### <a name="xsanyuri-and-systemuri"></a>xs:anyURI および System.Uri  
 相対 URI を表す `xs:anyURI` のインスタンスが <xref:System.Uri> にマップされている場合、<xref:System.Uri> オブジェクトには基本 URI がありません。  
  
## <a name="see-also"></a>関連項目

- [System.Xml クラスでの型のサポート](../../../../docs/standard/data/xml/type-support-in-the-system-xml-classes.md)
