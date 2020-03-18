---
title: 単純型を推論するときの規則
ms.date: 03/30/2017
ms.technology: dotnet-standard
ms.assetid: 394624d6-4da0-430a-8a88-46efe40f14de
ms.openlocfilehash: 17429e77f7764873e607a8feaa62da1cc6e014a4
ms.sourcegitcommit: 5f236cd78cf09593c8945a7d753e0850e96a0b80
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/07/2020
ms.locfileid: "75710233"
---
# <a name="rules-for-inferring-simple-types"></a>単純型を推論するときの規則
<xref:System.Xml.Schema.XmlSchemaInference> クラスが属性と要素のデータ型を推論する方法を説明します。  
  
 <xref:System.Xml.Schema.XmlSchemaInference> クラスは、属性と要素のデータ型を単純型として推論します。 このセクションでは、推論される可能性がある型、複数の異なる値を 1 つの型に調整する方法、スキーマ定義 `xsi` 属性の取り扱いについて説明します。  
  
## <a name="inferred-types"></a>推論型  
 <xref:System.Xml.Schema.XmlSchemaInference> クラスは、要素値と属性値を単純型として推論し、生成されるスキーマに型属性をインクルードします。 推論型はすべて単純型です。 生成されるスキーマの一部として基本型またはファセットがインクルードされることはありません。  
  
 XML ドキュメントで値が検出されるたびに、その値が個別に調べられます。 値が調べられるときに、その値の型が推論されます。 属性または要素の型が既に推論されており、その属性または要素に対して新たに検出された値が現在推論されている型と一致しない場合、<xref:System.Xml.Schema.XmlSchemaInference> クラスは、一定の規則に基づいてその型を昇格させます。 型を昇格させるときの規則については、このトピックの「型の昇格」のセクションで説明します。  
  
 生成されるスキーマに含まれる可能性がある推論型を次の表に示します。  
  
|単純型|説明|  
|-----------------|-----------------|  
|boolean|true、false、0、1|  
|byte|-128 ～ 127 の整数|  
|unsignedByte|0 ～ 255 の整数|  
|short|-32768 ～ 32767 の整数|  
|unsignedShort|0 ～ 65535 の整数|  
|int|-2147483648 ～ 2147483647 の整数|  
|unsignedInt|0 ～ 4294967295 の整数|  
|long|-9223372036854775808 ～ 9223372036854775807 の整数|  
|unsignedLong|0 ～ 18446744073709551615 の整数|  
|整数|先頭に "-" 記号が付く可能性がある有限桁の数値。|  
|decimal|精度が 0 ～ 28 桁の数値。|  
|フローティング|小数点数。指数を表す整数値を伴った、省略可能な "E" または "e" を付ける表記法もあります。 仮数部の範囲は -16777216 ～ 16777216 です。 指数部の範囲は -149 ～ 104 です。<br /><br /> float では、無限大および非数を表す特殊値が使用可能です。 float の特殊値は、0、-0、INF、-INF、NaN です。|  
|二重線|仮数部の範囲が -9007199254740992 ～ 9007199254740992 で、指数部の範囲が -1075 ～ 970 である点を除いて、float と同じです。<br /><br /> double では、無限大および非数を表す特殊値が使えます。 float の特殊値は、0、-0、INF、-INF、NaN です。|  
|duration|W3C の持続時間形式|  
|dateTime|W3C の日付時刻形式|  
|time|W3C の時刻形式|  
|date|年に使用できる値は 0001 ～ 9999 に制限されています。|  
|gYearMonth|W3C のグレゴリオ暦の年月形式|  
|string|1 つ以上の Unicode 文字|  
  
## <a name="type-promotion"></a>型の上位変換  
 <xref:System.Xml.Schema.XmlSchemaInference> クラスは属性値と要素値を 1 つずつ調べます。 値が検出されると、最も制限の厳しい、符号なし型が推論されます。 属性または要素の型が推論されている状態で、現在推論されている型と一致しない新しい値が検出されると、推論型が現在の推論型と新しい値の両方に当てはまる新しい型に昇格します。 <xref:System.Xml.Schema.XmlSchemaInference> クラスは、推論型を昇格させるときに前の値を考慮します。  
  
 たとえば、2 つの XML ドキュメントから取得された次の XML フラグメントを見てみましょう。  
  
 `<MyElement1 attr1="12" />`  
  
 `<MyElement1 attr1="52344" />`  
  
 最初に `attr1` の値が検出されると、`attr1` の型は、値 `unsignedByte` に基づいて `12` と推論されます。 `attr1` の 2 番目の値が検出されると、現在推論されている型 `unsignedShort` と現在の値 `unsignedByte` に基づいて、型が `52344` に昇格します。  
  
 次に、2 つの XML ドキュメントから取得された次の XML を見てみましょう。  
  
 `<MyElement2 attr2="0" />`  
  
 `<MyElement2 attr2="true" />`  
  
 最初に `attr2` の値が検出されると、`attr2` の型は、値 `unsignedByte` に基づいて `0` と推論されます。 `attr2` の 2 番目の値が検出されると、現在推論されている型 `string` と現在の値 `unsignedByte` に基づいて、型が `true` に昇格します。これは、<xref:System.Xml.Schema.XmlSchemaInference> クラスが推論型を昇格させるときに前の値を考慮するためです。 ただし、`attr2` の 2 つのインスタンスが、上記の説明のように 2 つの別々の XML ドキュメントで検出されたのではなく、同じ XML ドキュメントで検出された場合、`attr2` は `boolean` と推論されます。  
  
### <a name="ignored-attributes-from-the-httpswwww3org2001xmlschema-instance-namespace"></a>無視される <https://www.w3.org/2001/XMLSchema-instance> 名前空間の属性

スキーマの推論で無視されるスキーマ定義属性を次に示します。  
  
|属性|説明|  
|---------------|-----------------|  
|`xsi:type`|`xsi:type` が指定されている要素が検出された場合、`xsi:type` は無視されます。|  
|`xsi:nil`|`xsi:nil` 属性を持った要素が検出されると、推論されるスキーマ内の要素宣言の値が `nillable="true"` に設定されます。 `xsi:nil` 属性が `true` に設定された要素は子要素を持つことができません。|  
|`xsi:schemaLocation`|`xsi:schemaLocation` は検出されても無視されます。|  
|`xsi:noNamespaceSchemaLocation`|`xsi:noNamespaceSchemaLocation` は検出されても無視されます。|  
  
## <a name="see-also"></a>関連項目

- [XML スキーマ オブジェクト モデル (SOM)](../../../../docs/standard/data/xml/xml-schema-object-model-som.md)
- [XML ドキュメントからのスキーマの推論](../../../../docs/standard/data/xml/inferring-schemas-from-xml-documents.md)
- [スキーマのノード型および構造を推論するときの規則](../../../../docs/standard/data/xml/rules-for-inferring-schema-node-types-and-structure.md)
