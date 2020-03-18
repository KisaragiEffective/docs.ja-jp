---
title: JSON と XML 間のマッピング
ms.date: 03/30/2017
ms.assetid: 22ee1f52-c708-4024-bbf0-572e0dae64af
ms.openlocfilehash: 0dbe37a07024ae70e574b92582715d2d2ef52e5c
ms.sourcegitcommit: 09b4090b78f52fd09b0e430cd4b26576f1fdf96e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/17/2020
ms.locfileid: "76212080"
---
# <a name="mapping-between-json-and-xml"></a>JSON と XML 間のマッピング
<xref:System.Runtime.Serialization.Json.JsonReaderWriterFactory> によって作成されるリーダーとライターには、JSON (JavaScript Object Notation) コンテンツでの XML API が備わっています。 JSON は、JavaScript のオブジェクト リテラルのサブセットを使用してデータをエンコードします。 このファクトリによって生成されるリーダーとライターは、<xref:System.ServiceModel.Channels.WebMessageEncodingBindingElement> または <xref:System.ServiceModel.WebHttpBinding>を使用して Windows Communication Foundation (WCF) アプリケーションによって JSON コンテンツが送受信されるときにも使用されます。

JSON リーダーは JSON コンテンツで初期化されると、XML インスタンスで動作するテキストの XML リーダーと同じ方法で動作します。 テキストの XML リーダーでの一連の呼び出しによって特定の XML インスタンスが生成されると、JSON ライターは JSON コンテンツを書き出します。 このトピックでは、高度なシナリオで使用するために、この XML のインスタンスと JSON コンテンツ間のマッピングについて説明します。

内部的には、JSON は、WCF によって処理されるときに XML infoset として表されます。 マッピングは論理上の処理であるため、通常はこのような内部表現を考慮する必要はありません。JSON は通常、メモリで物理的に XML に変換されたり、XML から JSON に変換されたりすることはありません。 マッピングとは、XML API を使用して JSON コンテンツにアクセスすることを意味します。

WCF で JSON を使用する場合、通常のシナリオでは、<xref:System.ServiceModel.Description.WebScriptEnablingBehavior> の動作、または必要に応じて <xref:System.ServiceModel.Description.WebHttpBehavior> の動作によって、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> が自動的に接続されます。 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> は、JSON と XML 情報セット間のマッピングを認識し、直接 JSON を処理しているかのように動作します。 XML が次のマッピングに従うという了解の下に、任意の XML リーダーまたはライターで <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> を使用できます。

高度なシナリオでは、次のマッピングへの直接アクセスが必要になることがあります。 このようなシナリオが生じるのは、<xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> に依存することなく、独自の方法で JSON のシリアル化および逆シリアル化を行うとき、または JSON を含むメッセージに対して直接 <xref:System.ServiceModel.Channels.Message> 型を処理するときです。 JSON と XML 間のマッピングは、メッセージ ログにも使用されます。 WCF のメッセージログ機能を使用する場合、JSON メッセージは、次のセクションで説明するマッピングに従って XML としてログに記録されます。

マッピングの概念を明確にするために、次に JSON ドキュメントの例を示します。

```json
{"product":"pencil","price":12}
```

前述のリーダーのいずれか 1 つを使用してこの JSON ドキュメントを読み取るには、次の XML ドキュメントの読み取りに使用するシーケンスと同一の、<xref:System.Xml.XmlDictionaryReader> 呼び出しのシーケンスを使用します。

```xml
<root type="object">
    <product type="string">pencil</product>
    <price type="number">12</price>
</root>
```

さらに、この例の JSON メッセージが WCF によって受信され、ログに記録されている場合は、上記のログに XML フラグメントが表示されます。

## <a name="mapping-between-json-and-the-xml-infoset"></a>JSON と XML 情報セット間のマッピング

正式には、RFC 4627 で説明されているように、マッピングは、「 [Xml 情報セット](https://www.w3.org/TR/2004/REC-xml-infoset-20040204/)」で説明されているように、 [RFC](https://www.ietf.org/rfc/rfc4627.txt)で説明されている JSON 間 (特定の制限が緩和され、その他の制限が追加されている場合を除く) と xml infoset です このトピックでは、[角かっこ] の*情報項目*とフィールドの定義について説明します。

空の JSON ドキュメントは空の XML ドキュメントにマップされ、空の XML ドキュメントは空の JSON ドキュメントにマップされます。 XML から JSON へのマッピングでは、ドキュメントの後にある空白と末尾の空白文字は使用できません。

マッピングは、ドキュメント情報項目 (DII: Document Information Item) または要素情報項目 (EII: Element Information Item) のいずれか一方と JSON の間に定義されます。 EII、または DII の [document element] プロパティは、Root JSON Element と呼ばれます。 また、ドキュメント フラグメント (ルート要素を複数持つ XML) は、このマッピングではサポートされません。

例 : 次のドキュメントがあるとします。

```xml
<?xml version="1.0"?>
<root type="number">42</root>
```

また、次の要素があるとします。

```xml
<root type="number">42</root>
```

どちらにも JSON へのマッピングが存在します。 <`root`> 要素は、どちらの場合もルート JSON 要素です。

また、DII の場合は次の点を考慮する必要があります。

- [children] 一覧には、除外する必要のある項目が含まれています。 JSON からマッピングされた XML を読み取るときは、この事実に依存しないでください。

- [children] 一覧には、注釈情報項目が保持されません。

- [children] 一覧には、DTD 情報項目が保持されません。

- [Children] リストには、個人情報 (PI) 情報項目が保持されません (`<?xml…>` 宣言は PI 情報項目とは見なされません)

- [notations] セットは空です。

- [unparsed entities] セットは空です。

例 : 次のドキュメントでは [children] が PI とコメントを保持するため、JSON へのマッピングが存在しません。

```xml
<?xml version="1.0"?>
<!--comment--><?pi?>
<root type="number">42</root>
```

Root JSON Element としての EII には次の特性があります。

- [local name] は値 "root" を持ちます。

- [namespace name] は値を持ちません。

- [prefix] は値を持ちません。

- [children] には、後述するように内部要素である EII、または文字情報項目 (CII: Character Information Item) のいずれか一方が含まれる場合と、どちらも含まれない場合がありますが、両方とも含まれることはありません。

- [attributes] には、次のオプションの属性情報項目 (AII: Attribute Information Item) が含まれる場合があります。

- 後述の JSON Type Attribute ("type")。 この属性は、マップされた XML で JSON 型 (文字列、数値、ブール値、オブジェクト、配列、または null) を保持するのに使用します。

- 「」で説明されているように、データコントラクト名属性 ("\_\_type")。 この属性は、JSON Type Attribute が存在し、その [normalized value] が "object" であるときにのみ存在します。 この属性は、派生型がシリアル化されたり、基本型が要求されたりするポリモーフィックな場合などに、`DataContractJsonSerializer` によりデータ コントラクトの型情報を保持するために使用されます。 `DataContractJsonSerializer` を使用していなければ、この属性はほとんどの場合に無視されます。

- [スコープ内の名前空間] には、infoset 仕様によって義務付けられている `http://www.w3.org/XML/1998/namespace` の "xml" のバインドが含まれています。

- [children]、[attributes]、および[in-scope namespaces] は、あらかじめ指定した項目以外の項目を持つことができず、[namespace attributes] はメンバーを持つことができませんが、JSON からマップされた XML を読み取るときには、この事実に依存しないでください。

例 : 次のドキュメントでは [namespace attributes] が空ではないため、JSON へのマッピングが存在しません。

```xml
<?xml version="1.0"?>
<root xmlns:a="myattributevalue">42</root>
```

JSON Type Attribute としての AII には次の特性があります。

- [namespace name] は値を持ちません。
- [prefix] は値を持ちません。
- [local name] は "type" です。
- [normalized value] は、次のセクションで説明する型の値のいずれかになります。
- [specified] は `true` です。
- [attribute type] は値を持ちません。
- [references] は値を持ちません。

Data Contract Name Attribute としての AII には次の特性があります。

- [namespace name] は値を持ちません。
- [prefix] は値を持ちません。
- [local name] は "\_\_type" (2 つのアンダースコアと "type") です。
- [normalized value] は、任意の有効な Unicode 文字列です。この文字列から JSON へのマッピングは、次のセクションで説明します。
- [specified] は `true` です。
- [attribute type] は値を持ちません。
- [references] は値を持ちません。

Root JSON Element に含まれる内部要素、またはその他の内部要素には、次の特性があります。

- [ローカル名] には、後で説明するように任意の値を指定できます。
- [namespace name]、[prefix]、[children]、[attributes]、[namespace attributes]、および [in-scope namespaces] は、Root JSON Element と同じ規則に従います。

Root JSON Element および内部要素では、JSON Type Attribute により JSON へのマッピングと、[children] およびその解釈が定義されます。 属性の [正規化された値] では大文字と小文字が区別され、小文字にする必要があり、空白を含めることはできません。

|JSON 型属性の AII の [正規化された値]|対応する EII の許可された [children]|JSON へのマッピング|
|---------------------------------------------------------|---------------------------------------------------|---------------------|
|`string` (または JSON 型 AII なし)<br /><br /> `string` および JSON 型 AII なしは同じです。`string` を既定値にします。<br /><br /> その結果、`<root> string1</root>` が JSON `string` "string1" にマップされます。|0個以上の CIIs|JSON `string` (JSON RFC section 2.5)。 `char` はそれぞれ、CII の [character code] に対応する文字です。 CII が存在しない場合は、空の JSON `string` にマップされます。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="string">42</root>`<br /><br /> JSON フラグメントは "42" です。<br /><br /> XML から JSON へのマッピングでは、エスケープする必要がある文字はエスケープ文字にマップされ、その他はすべて、エスケープされない文字にマップされます。 "/" 文字は特殊です。 "/" は、("\\/" として書き出す必要はありませんが) エスケープされます。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="string">the "da/ta"</root>`<br /><br /> JSON フラグメントは "\\" da\\/ta\\"" です。<br /><br /> JSON から XML へのマッピングでは、エスケープ文字およびエスケープされない文字が、対応する [character code] に正しくマップされます。<br /><br /> 例 : JSON フラグメント "\u0041BC" が次の XML 要素にマップされます。<br /><br /> `<root type="string">ABC</root>`<br /><br /> 文字列は、XML にマップされない空白 (JSON RFC のセクション2の ' ws ') で囲むことができます。<br /><br /> 例 : JSON フラグメント           "ABC" (最初の二重引用符の前に空白があります) が次の XML 要素にマップされます。<br /><br /> `<root type="string">ABC</root>`<br /><br /> XML の空白文字は、JSON の空白文字にマップされます。<br /><br /> 例 : 次の XML 要素は JSON フラグメントにマップされます。<br /><br /> `<root type="string">  A BC      </root>`<br /><br /> JSON フラグメントは " A BC " です。|
|`number`|1 個以上の CII|JSON `number` (JSON RFC、セクション 2.4)。空白で囲まれている可能性があります。 数字と空白の組み合わせの各文字は、CII の [character code] に対応する文字です。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="number">    42</root>`<br /><br /> JSON フラグメントは    42 です<br /><br /> (空白は保持されます)。|
|`boolean`|4または 5 CIIs (`true` または `false`に対応)。追加の空白文字で囲まれている可能性があります。|文字列 "true" に対応する CII シーケンスは、リテラルの `true` にマップされ、文字列 "false" に対応する CII シーケンスは、リテラルの `false` にマップされます。 周囲の空白文字は保持されます。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="boolean"> false</root>`<br /><br /> JSON フラグメントは `false` です。|
|`null`|いずれも許可されません。|リテラルの `null`。 JSON から XML へのマッピングでは、XML にマップされない空白 (セクション2の ' ws ') で `null` を囲むことができます。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="null"/>`<br /><br /> または<br /><br /> `<root type="null"></root>`<br /><br /> :<br /><br /> いずれの場合も、JSON フラグメントは `Null` です。|
|`object`|0 個以上の EII|JSON RFC section 2.2 にあるように、`begin-object` (左中かっこ) の直後に、後述するように各 EII のメンバー レコードが続きます。 EII が複数個存在する場合、メンバー レコードの間に値区切り記号 (コンマ) が配置されます。 最後尾には、end-object (右中かっこ) が置かれます。<br /><br /> 例 : 次の要素は JSON フラグメントにマップされます。<br /><br /> `<root type="object">`<br /><br /> `<type1 type="string">aaa\</type1>`<br /><br /> `<type2 type="string">bbb\</type2>`<br /><br /> `</root >`<br /><br /> JSON フラグメントは `{"type1":"aaa","type2":"bbb"}` です。<br /><br /> XML から JSON へのマッピングに Data Contract Type Attribute が存在する場合は、最初に追加のメンバー レコードが挿入されます。 その名前は、データコントラクト型属性の [ローカル名] ("\_\_Type") であり、その値は属性の [正規化された値] です。 逆に、JSON から XML へのマッピングでは、最初のメンバーレコードの名前がデータコントラクト型属性の [local name] (つまり、"\_\_Type") の場合、対応するデータコントラクト型属性がマップされた XML に存在しますが、対応する EII は存在しません。 また、このような特定のマッピングが適用されるには、このメンバー レコードが最初に JSON オブジェクトに存在している必要があります。 これは、メンバー レコードの順序が重要ではない通常の JSON 処理とは異なっています。<br /><br /> 例:<br /><br /> 次の JSON フラグメントは XML にマップされます。<br /><br /> `{"__type":"Person","name":"John"}`<br /><br /> XML は次のコードです。<br /><br /> `<root type="object" __type="Person">   <name type="string">John</name> </root>`<br /><br /> \_\_型 AII が存在することに注意してくださいが、\_\_型 EII がありません。<br /><br /> ただし、次の例に示すように、JSON での順序が逆になる場合があります。<br /><br /> `{"name":"John","\_\_type":"Person"}`<br /><br /> このとき、対応する XML は次のとおりです。<br /><br /> `<root type="object">   <name type="string">John</name>   <__type type="string">Person</__type> </root>`<br /><br /> つまり、\__type は特別な意味を持たなくなり、AII ではなく通常どおりに EII にマップされます。<br /><br /> JSON 値にマップされるときの、AII の [normalized value] に対するエスケープ/エスケープ解除ルールは、このテーブルの "string" 行で指定された JSON 文字列に対するルールと同じです。<br /><br /> 例:<br /><br /> `<root type="object" __type="\abc" />`<br /><br /> これを前の例に適用すると、次の JSON にマップされます。<br /><br /> `{"__type":"\\abc"}`<br /><br /> XML から JSON へのマッピングでは、最初の EII の [local name] を "\_\_type" にすることはできません。<br /><br /> オブジェクトの XML から JSON へのマッピングでは空白 (`ws`) は生成されず、JSON から XML へのマッピングでは無視されます。<br /><br /> 例 : 次の JSON フラグメントは XML 要素にマップされます。<br /><br /> `{ "ccc" : "aaa", "ddd" :"bbb"}`<br /><br /> XML 要素を、次のコードで示します。<br /><br /> `<root type="object">    <ccc type="string">aaa</ccc>    <ddd type="string">bbb</bar> </root >`|
|アレイ|0 個以上の EII|JSON RFC section 2.3 にあるように、begin-array (左角かっこ) の直後に、後述する各 EII の配列レコードが続きます。 EII が複数個存在する場合、配列レコードの間に値区切り記号 (コンマ) が配置されます。 最後尾には、end-array が置かれます。<br /><br /> 例 : 次の XML 要素は JSON フラグメントにマップされます。<br /><br /> `<root type="array"/>    <item type="string">aaa</item>    <item type="string">bbb</item> </root >`<br /><br /> JSON フラグメントは `["aaa","bbb"]`<br /><br /> 配列の XML から JSON へのマッピングでは空白 (`ws`) が生成されず、JSON から XML へのマッピングでは無視されます。<br /><br /> 例: JSON フラグメント。<br /><br />`["aaa", "bbb"]`<br /><br /> マップされる XML 要素は、次のとおりです。<br /><br /> `<root type="array"/>    <item type="string">aaa</item>    <item type="string">bbb</item> </root >`|

メンバー レコードの動作は次のとおりです。

- JSON RFC section 2.2 で規定されるように、内部要素の [local name] は `string` の `member` 部分にマップされます。

例 : 次の要素は JSON フラグメントにマップされます。

```xml
<root type="object"/>
<myLocalName type="string">aaa</myLocalName>
</root >
```

次の JSON フラグメントが示されます。

```json
{"myLocalName":"aaa"}
```

- XML から JSON へのマッピングでは、JSON でエスケープする必要がある文字はエスケープされ、それ以外はエスケープされません。 "/" 文字は、エスケープする必要のない文字ですが、エスケープされます (JSON から XML へのマッピングではエスケープする必要はありません)。 これは、JSON の `DateTime` データに対する ASP.NET AJAX 形式をサポートするために必要な処理です。

- JSON から XML へのマッピングでは、すべての文字が (必要に応じて非エスケープ文字も含む) [local name] を生成する `string` を形成するために使用されます。

- 内部要素 [children] は `JSON Type Attribute` の場合と同じように、`Root JSON Element` に従って section 2.2 の値にマップされます。 EII の複数レベルの入れ子 (配列内の入れ子も含む) が許可されます。

例 : 次の要素は JSON フラグメントにマップされます。

```xml
<root type="object">
    <myLocalName1 type="string">myValue1</myLocalName1>
    <myLocalName2 type="number">2</myLocalName2>
    <myLocalName3 type="object">
        <myNestedName1 type="boolean">true</myNestedName1>
        <myNestedName2 type="null"/>
    </myLocalName3>
</root >
```

マップされる JSON フラグメントは次のとおりです。

```json
{"myLocalName1":"myValue1","myLocalName2":2,"myLocalName3":{"myNestedName1":true,"myNestedName2":null}}
```

> [!NOTE]
> 上記のマッピングには、XML エンコーディングの手順がありません。 したがって、WCF では、キー名のすべての文字が XML 要素名の有効な文字である JSON ドキュメントのみがサポートされます。 たとえば、JSON ドキュメント {"<": "a"} は、< が XML 要素の有効な名前ではないため、サポートされていません。

逆に、XML では有効であり、JSON では有効でない文字は、上記のマッピングに JSON のエスケープ/エスケープ解除の手順が含まれるため、問題が生じません。

Array Record の動作は次のとおりです。

- 内部要素の [local name] は "item" です。

- 内部要素の [children] は、Root JSON Element の場合と同じように、JSON Type Attribute に従って section 2.3 の値にマップされます。 EII の複数の入れ子 (オブジェクト内の入れ子も含む) が許可されます。

例 : 次の要素は JSON フラグメントにマップされます。

```xml
<root type="array"/>
    <item type="string">myValue1</item>
    <item type="number">2</item>
    <item type="array">
    <item type="boolean">true</item>
    <item type="null"/></item>
</root >
```

JSON フラグメントは次のとおりです。

```json
["myValue1",2,[true,null]]
```

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.Json.JsonReaderWriterFactory>
- <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer>
- [スタンドアロン JSON のシリアル化](stand-alone-json-serialization.md)
