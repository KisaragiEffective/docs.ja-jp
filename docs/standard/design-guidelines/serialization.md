---
title: シリアル化
ms.date: 10/22/2008
ms.technology: dotnet-standard
ms.assetid: bebb27ac-9712-4196-9931-de19fc04dbac
ms.openlocfilehash: d29a7bc9f304b8d19e8af593166b8d847117424f
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76743631"
---
# <a name="serialization"></a>シリアル化
シリアル化は、オブジェクトを、簡単に永続化または転送できる形式に変換するプロセスです。 たとえば、オブジェクトをシリアル化し、HTTP を使用してインターネット経由で転送し、ターゲットコンピューターで逆シリアル化することができます。

 .NET Framework には、さまざまなシリアル化シナリオ用に最適化された3つの主要なシリアル化テクノロジがあります。 これらのテクノロジ、およびテクノロジに関連した主な Framework 型を次の表に示します。

|**テクノロジ名**|**主要な型**|**シナリオ**|
|-------------------------|--------------------|-------------------|
|**データコントラクトのシリアル化**|<xref:System.Runtime.Serialization.DataContractAttribute> <br /> <xref:System.Runtime.Serialization.DataMemberAttribute> <br /> <xref:System.Runtime.Serialization.DataContractSerializer> <br /> <xref:System.Runtime.Serialization.NetDataContractSerializer> <br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> <br /> <xref:System.Runtime.Serialization.ISerializable>|永続化全般<br />Web サービス<br />JSON|
|**XML シリアル化**|<xref:System.Xml.Serialization.XmlSerializer>|Xml の構造を完全に制御する XML 形式|
|**ランタイムのシリアル化 (バイナリおよび SOAP)**|<xref:System.SerializableAttribute> <br /> <xref:System.Runtime.Serialization.ISerializable> <br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> <br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|.NET リモート処理|

 新しい型を設計する際には、シリアル化について考え✔️ます。

## <a name="choosing-the-right-serialization-technology-to-support"></a>サポートする適切なシリアル化テクノロジの選択
 型のインスタンスを永続化したり、Web サービスで使用したりする必要がある場合は、データコントラクトのシリアル化をサポートする✔️ます。

 型をシリアル化するときに生成される XML 形式をより詳細に制御する必要がある場合は、データコントラクトのシリアル化に加えて、またはデータコントラクトのシリアル化の代わりに XML シリアル化をサポートすることを✔️してください。

 これは、データコントラクトのシリアル化でサポートされていない XML コンストラクトを使用する必要がある場合 (XML 属性を生成する場合など) に必要になることがあります。

 型のインスタンスが .NET リモート処理の境界を越えて移動する必要がある場合は、ランタイムシリアル化のサポートを検討✔️。

 ❌、一般的な永続化のためだけにランタイムシリアル化または XML シリアル化をサポートしないようにします。 代わりに、データコントラクトのシリアル化を優先します。

## <a name="supporting-data-contract-serialization"></a>データ コントラクトのシリアル化のサポート
 型に <xref:System.Runtime.Serialization.DataContractAttribute> を適用し、型のメンバー (フィールドおよびプロパティ) に <xref:System.Runtime.Serialization.DataMemberAttribute> を適用することで、データコントラクトのシリアル化をサポートできます。

 型を部分信頼で使用できる場合は、型のデータメンバーをパブリックとしてマークする✔️ます。

 完全信頼では、データコントラクトシリアライザーは、非パブリックな型とメンバーをシリアル化および逆シリアル化できますが、部分信頼でシリアル化および逆シリアル化できるのはパブリックメンバーだけです。

 <xref:System.Runtime.Serialization.DataMemberAttribute>を持つすべてのプロパティに getter と setter を実装✔️ます。 データコントラクトシリアライザーでは、型の getter と setter の両方がシリアル化可能と見なされる必要があります。 (.NET Framework 3.5 SP1 では、一部のコレクションプロパティを取得専用にすることができます)。型が部分信頼で使用されない場合、プロパティアクセサーの一方または両方を非パブリックにすることができます。

 逆シリアル化されたインスタンスの初期化には、シリアル化コールバックの使用を検討✔️。

 オブジェクトの逆シリアル化時にはコンストラクターは呼び出されません。 (規則には例外があります。 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> でマークされたコレクションのコンストラクターは、逆シリアル化中に呼び出されます)。したがって、通常の構築時に実行されるすべてのロジックは、シリアル化コールバックの1つとして実装する必要があります。

 `OnDeserializedAttribute` は、最もよく使用されるコールバック属性です。 その他の属性には、<xref:System.Runtime.Serialization.OnDeserializingAttribute>、<xref:System.Runtime.Serialization.OnSerializingAttribute>、および <xref:System.Runtime.Serialization.OnSerializedAttribute> があります。 これらを使用して、逆シリアル化前、シリアル化前、およびシリアル化後に実行されるコールバックをマークすることができます。

 複合オブジェクトグラフを逆シリアル化するときに使用する具象型を示すために、<xref:System.Runtime.Serialization.KnownTypeAttribute> を使用することを✔️検討してください。

 シリアル化可能な型を作成または変更するときに、後方互換性と上位互換性を考慮する✔️。

 シリアル化した型の将来のバージョンは、現在のバージョンの型に逆シリアル化でき、その逆も可能であることを念頭に置いてください。

 データコントラクト属性への明示的なパラメーターを使用してコントラクトを保持する特別な注意を払う場合を除き、データメンバー (プライベートおよび内部でも) では、将来のバージョンでも、名前、型、および順序を変更することはできないことを理解してください.

 シリアル化可能な型に変更を加えるときに、シリアル化の互換性をテストします。 新しいバージョンを古いバージョンに逆シリアル化したり、その逆も試してみてください。

 型の異なるバージョン間でのラウンドトリップを可能にするために、<xref:System.Runtime.Serialization.IExtensibleDataObject> を実装する✔️を検討してください。

 インターフェイスを使用すると、ラウンドトリッピングの間にデータが失われないようにシリアライザーで確認することができます。 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A?displayProperty=nameWithType> プロパティは、現在のバージョンでは不明な型の将来のバージョンからデータを格納するために使用されます。そのため、データメンバーに格納することはできません。 現在のバージョンを後でシリアル化および逆シリアル化した後に、シリアル化されたストリームで使用できるようになります。

## <a name="supporting-xml-serialization"></a>XML シリアル化のサポート
 データコントラクトのシリアル化は .NET Framework の主要な (既定の) シリアル化テクノロジですが、データコントラクトのシリアル化でサポートされていないシリアル化のシナリオがあります。 たとえば、シリアライザーによって作成または使用された XML の形状は完全に制御できません。 このような細かい制御が必要な場合は、XML シリアル化を使用する必要があり、このシリアル化テクノロジをサポートするために型をデザインする必要があります。

 生成される XML の構造を厳密に制御する理由がない限り、XML シリアル化専用の型を設計することは避けてください。 ❌ このシリアル化テクノロジは、前のセクションで説明したデータ コントラクトのシリアル化よりも優先されます。

 XML シリアル化属性を適用するよりも、シリアル化された XML の構造をさらに細かく制御する場合は、<xref:System.Xml.Serialization.IXmlSerializable> インターフェイスを実装することを検討してください✔️。 インターフェイスの2つのメソッド (<xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> と <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>) を使用すると、シリアル化された XML ストリームを完全に制御できます。 また、`XmlSchemaProviderAttribute`を適用することによって、型に対して生成される XML スキーマを制御することもできます。

## <a name="supporting-runtime-serialization"></a>ランタイム シリアル化のサポート
 ランタイムシリアル化は、.NET リモート処理で使用されるテクノロジです。 .NET リモート処理を使用して型が転送されると思われる場合は、ランタイムシリアル化をサポートしていることを確認する必要があります。

 ランタイムシリアル化の基本的なサポートは、<xref:System.SerializableAttribute>を適用することによって実現できます。さらに高度なシナリオでは、単純なランタイムシリアル化可能パターンを実装します (<xref:System.Runtime.Serialization.ISerializable> を実装し、シリアル化コンストラクターを提供します)。

 型が .NET リモート処理で使用される場合は、ランタイムシリアル化のサポートを検討✔️。 たとえば、<xref:System.AddIn?displayProperty=nameWithType> 名前空間は .NET リモート処理を使用するため、`System.AddIn` のアドイン間で交換されるすべての型でランタイムシリアル化をサポートする必要があります。

 シリアル化プロセスを完全に制御する必要がある場合は、ランタイムシリアル化可能パターンを実装することを✔️してください。 たとえば、シリアル化または逆シリアル化されたデータを変換したいとします。

 パターンは単純です。 必要な作業は、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装し、オブジェクトを逆シリアル化するときに使用する特別なコンストラクターを指定するだけです。

 このサンプルで示すように、シリアル化コンストラクターを保護し、2つのパラメーターを型指定して名前を指定して、この例では✔️します。

```csharp
[Serializable]
public class Person : ISerializable
{
    protected Person(SerializationInfo info, StreamingContext context)
    {
        // ...
    }
}
```

 ✔️ <xref:System.Runtime.Serialization.ISerializable> メンバーを明示的に実装します。

 ✔️ <xref:System.Runtime.Serialization.ISerializable.GetObjectData%2A?displayProperty=nameWithType> の実装にリンク確認要求を適用します。 これにより、完全に信頼されたコアとランタイムシリアライザーだけがメンバーにアクセスできるようになります。

 *©2005、2009 Microsoft Corporation の部分。すべての権限が予約されています。*

 *2008 年 10 月 22 日に Microsoft Windows Development シリーズの一部として、Addison-Wesley Professional によって発行された、Krzysztof Cwalina および Brad Abrams による「[Framework Design Guidelines: Conventions, Idioms, and Patterns for Reusable .NET Libraries, 2nd Edition](https://www.informit.com/store/framework-design-guidelines-conventions-idioms-and-9780321545619)」 (フレームワーク デザイン ガイドライン: 再利用可能な .NET ライブラリの規則、用法、パターン、第 2 版) から Pearson Education, Inc. の許可を得て再印刷されています。*

## <a name="see-also"></a>参照

- [フレームワーク デザインのガイドライン](../../../docs/standard/design-guidelines/index.md)
- [使用方法のガイドライン](../../../docs/standard/design-guidelines/usage-guidelines.md)
