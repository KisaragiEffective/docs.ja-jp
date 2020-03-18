---
title: シリアル化のガイドライン
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- serialization, guidelines
- binary serialization, guidelines
ms.assetid: ebbeddff-179d-443f-bf08-9c373199a73a
ms.openlocfilehash: 067f32a026e3354e6c4256602ed17fd7d7bde0b8
ms.sourcegitcommit: 00aa62e2f469c2272a457b04e66b4cc3c97a800b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/28/2020
ms.locfileid: "78159794"
---
# <a name="serialization-guidelines"></a>シリアル化のガイドライン
このドキュメントには、シリアル化できるように API をデザインする際に考慮すべきガイドラインを示します。  

[!INCLUDE [binary-serialization-warning](../../../includes/binary-serialization-warning.md)]
  
 .NET には、さまざまなシリアル化シナリオに合わせて最適化できる 3 つの主なシリアル化テクノロジがあります。 これらのテクノロジ、およびテクノロジに関連した主な .NET 型を次の表に示します。  
  
|テクノロジ|関連するクラス|メモ|  
|----------------|----------------------|-----------|  
|データ コントラクトのシリアル化|<xref:System.Runtime.Serialization.DataContractAttribute><br /><br /> <xref:System.Runtime.Serialization.DataMemberAttribute><br /><br /> <xref:System.Runtime.Serialization.DataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.NetDataContractSerializer><br /><br /> <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer><br /><br /> <xref:System.Runtime.Serialization.ISerializable>|永続化全般<br /><br /> Web サービス<br /><br /> JSON|  
|XML シリアル化|<xref:System.Xml.Serialization.XmlSerializer>|XML 形式 <br />フル コントロール|  
|ランタイム シリアル化 (バイナリおよび SOAP)|<xref:System.SerializableAttribute><br /><br /> <xref:System.Runtime.Serialization.ISerializable><br /><br /> <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter><br /><br /> <xref:System.Runtime.Serialization.Formatters.Soap.SoapFormatter>|.NET リモート処理|  
  
 新しい型をデザインする際は、新しい型でどのテクノロジをサポートするかを決める必要があります。 次のガイドラインでその選択方法とサポートの提供方法について説明します。 このガイドラインは、アプリケーションまたはライブラリの実装時に使用するシリアル化テクノロジを決定するためのものではありません。 そのようなガイドラインは API のデザインには直接関係ないため、このトピックのスコープの範囲外となります。  
  
## <a name="guidelines"></a>ガイドライン  
  
- 新しい型をデザインする際にはシリアル化について考えてください。  
  
     シリアル化は、プログラムで型のインスタンスを永続化または変換しなければならないことがあるため、どのような型においてもデザイン上の重要な考慮事項です。  
  
### <a name="choosing-the-right-serialization-technology-to-support"></a>サポートする適切なシリアル化テクノロジの選択  
 任意の型で 0、または 1 つ以上のシリアル化テクノロジをサポートできます。  
  
- 使用する型のインスタンスを Web サービスで永続化させる、または使用する必要がある場合は、*データ コントラクトのシリアル化*をサポートすることを検討してください。  
  
- 型をシリアル化したときに作成される XML 形式の制御を強化する必要がある場合は、データ コントラクトのシリアル化の代わり、またはこれに加えて *XML シリアル化*をサポートすることを検討してください。  
  
     これは、データ コントラクトのシリアル化でサポートされていない XML 構築を使用して XML 属性を作成するような一部の相互運用シナリオで必要になることがあります。  
  
- 使用する型のインスタンスが .NET リモート処理境界を超えて移動する必要がある場合は、*ランタイムのシリアル化*をサポートすることを検討してください。  
  
- 一般的な永続化のためにランタイムのシリアル化や XML のシリアル化をサポートしないでください。 データ コントラクトのシリアル化を優先的に使用してください。  
  
#### <a name="supporting-data-contract-serialization"></a>データ コントラクトのシリアル化のサポート  
 <xref:System.Runtime.Serialization.DataContractAttribute> を型に適用し、<xref:System.Runtime.Serialization.DataMemberAttribute> をその型のメンバー (フィールドおよびプロパティ) に適用することによって、型でデータ コントラクトのシリアル化をサポートすることができます。  
  
 [!code-csharp[SerializationGuidelines#1](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#1)]
 [!code-vb[SerializationGuidelines#1](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#1)]  
  
1. 型を部分信頼で使用する場合は、型のデータ メンバーをパブリックにすることを検討してください。 完全な信頼では、データ コントラクト シリアライザーでパブリックではない型とメンバーのシリアル化と逆シリアル化を行うことが可能ですが、部分信頼の場合、パブリック メンバーのみをシリアル化および逆シリアル化できます。  
  
2. Data-MemberAttribute を持つすべてのプロパティに getter と setter を実装してください。 データ コントラクト シリアライザーでは、この型の getter と setter の両方がシリアル化可能と見なされる必要があります。 型を部分信頼で使用しない場合は、1 つまたは両方のプロパティ アクセサーを非パブリックにできます。  
  
     [!code-csharp[SerializationGuidelines#2](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#2)]
     [!code-vb[SerializationGuidelines#2](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#2)]  
  
3. 逆シリアル化されたインスタンスの初期化には、シリアル化コールバックを使用することを検討してください。  
  
     オブジェクトの逆シリアル化時にはコンストラクターは呼び出されません。 このため、通常の構築時に実行されるすべてのロジックは、*シリアル化のコールバック*の 1 つとして実装する必要があります。  
  
     [!code-csharp[SerializationGuidelines#3](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#3)]
     [!code-vb[SerializationGuidelines#3](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#3)]  
  
     <xref:System.Runtime.Serialization.OnDeserializedAttribute> 属性は最もよく使用されるコールバック属性です。 ファミリ内のその他の属性は、<xref:System.Runtime.Serialization.OnDeserializingAttribute>、<xref:System.Runtime.Serialization.OnSerializingAttribute>、および <xref:System.Runtime.Serialization.OnSerializedAttribute>です。 これらを使用して、逆シリアル化前、シリアル化前、およびシリアル化後に実行されるコールバックをマークすることができます。  
  
4. 複雑なオブジェクト グラフを逆シリアル化する場合は、使用する具象型を示す <xref:System.Runtime.Serialization.KnownTypeAttribute> を使用することを検討してください。  
  
     たとえば、逆シリアル化されたデータ メンバーの型を抽象クラスで表す場合、シリアライザーはインスタンス化してメンバーに割り当てる具象型を判断するために、*既知の型*の情報が必要になります。 属性を使用して既知の型を指定しない場合、明示的にシリアライザーに渡す (既知の型をシリアライザーのコンストラクターに渡します) か、構成ファイルで指定する必要があります。  
  
     [!code-csharp[SerializationGuidelines#4](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#4)]
     [!code-vb[SerializationGuidelines#4](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#4)]  
  
     既知の型のリストが静的にわからない場合 (**Person** クラスをコンパイルした場合)、**KnownTypeAttribute** で実行時に既知の型の一覧を返すメソッドを指すこともできます。  
  
5. シリアル化可能な型を作成または変更する場合は、上位互換性と下位互換性を考慮してください。  
  
     シリアル化した型の将来のバージョンは、現在のバージョンの型に逆シリアル化でき、その逆も可能であることを念頭に置いてください。 明示的なパラメーターを使用してデータ コントラクト属性にコントラクトを保存する特別な措置を取らない限り、データ メンバーはプライベートで内部データであっても、型の将来のバージョンで名前、型、順序を変更することはできないことを理解しておいてください。シリアル化可能な型に変更を加えるときは、シリアル化の互換性をテストしてください。 新しいバージョンを古いバージョンに逆シリアル化したり、その逆も試してみてください。  
  
6. 異なるバージョンの型の間でラウンドトリッピングができるように、<xref:System.Runtime.Serialization.IExtensibleDataObject> インターフェイスを実装することを検討してください。  
  
     インターフェイスを使用すると、ラウンドトリッピングの間にデータが失われないようにシリアライザーで確認することができます。 <xref:System.Runtime.Serialization.IExtensibleDataObject.ExtensionData%2A> プロパティにより、現在のバージョンでは未知の、将来使用される型の任意のデータが格納されます。 現在のバージョンを後で将来のバージョンにシリアル化または逆シリアル化するときに、**ExtensionData** プロパティ値を通じて、シリアル化されたストリーム内で追加データを使用できます。  
  
     [!code-csharp[SerializationGuidelines#5](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#5)]
     [!code-vb[SerializationGuidelines#5](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#5)]  
  
     詳細については、「[上位互換性のあるデータ コントラクト](../../../docs/framework/wcf/feature-details/forward-compatible-data-contracts.md)」を参照してください。  
  
#### <a name="supporting-xml-serialization"></a>XML シリアル化のサポート  
 データ コントラクトのシリアル化は .NET Framework の主な (既定の) シリアル化テクノロジですが、データ コントラクトのシリアル化ではサポートされないシリアル化シナリオがあります。 たとえば、シリアライザーによって作成または使用された XML の形状は完全に制御できません。 そのような微調整が必要な場合は、*XML シリアル化*を使用する必要があり、このシリアル化テクノロジをサポートする型を自分でデザインする必要があります。  
  
1. 作成された XML の形状を制御しなければならない強力な理由がない限り、XML シリアル化のために特別に型をデザインすることは避けてください。 このシリアル化テクノロジは、前のセクションで説明したデータ コントラクトのシリアル化よりも優先されます。  
  
     言い換えれば、XML シリアル化で使用する型であることがわかっている場合を除き、<xref:System.Xml.Serialization> 名前空間の属性を新しい型に適用しないでください。 **System.Xml.Serialization** を使用して、作成された XML の形状を制御する方法を次の例に示します。  
  
     [!code-csharp[SerializationGuidelines#6](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#6)]
     [!code-vb[SerializationGuidelines#6](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#6)]  
  
2. XML シリアル化属性を適用することで提供される、シリアル化された XML の形状をより細かく制御する場合は、<xref:System.Xml.Serialization.IXmlSerializable> インターフェイスを実装することを検討してください。 インターフェイスの2つのメソッド (<xref:System.Xml.Serialization.IXmlSerializable.ReadXml%2A> と <xref:System.Xml.Serialization.IXmlSerializable.WriteXml%2A>) を使用すると、シリアル化された XML ストリームを完全に制御できます。 また、<xref:System.Xml.Serialization.XmlSchemaProviderAttribute> 属性を適用することで、その型用に生成される XML スキーマを制御することもできます。  
  
#### <a name="supporting-runtime-serialization"></a>ランタイム シリアル化のサポート  
 *ランタイム シリアル化*は .NET リモート処理で使用されるテクノロジです。 .NET リモート処理を使用して型を変換する場合、ランタイム シリアル化がサポートされていることを確認する必要があります。  
  
 *ランタイム シリアル化*の基本的なサポートは <xref:System.SerializableAttribute> 属性を適用して提供できますが、より高度なシナリオでは単純な*ランタイム シリアル化可能パターン*の実装 (-<xref:System.Runtime.Serialization.ISerializable> を実装してシリアル化コンストラクターを指定) が必要になります。  
  
1. 使用する型で .NET リモート処理を使用する場合は、ランタイムのシリアル化をサポートすることを検討してください。 たとえば、<xref:System.AddIn> 名前空間は .NET リモート処理を使用するため、**System.AddIn** アドイン間で交換されるすべての型でランタイム シリアル化をサポートする必要があります。  
  
     [!code-csharp[SerializationGuidelines#7](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#7)]
     [!code-vb[SerializationGuidelines#7](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#7)]  
  
2. シリアル化プロセスを完全制御する場合は、*ランタイム シリアル化可能パターン*を実装することを検討してください。 たとえば、シリアル化または逆シリアル化されたデータを変換したいとします。  
  
     パターンは単純です。 必要な作業は、<xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装し、オブジェクトを逆シリアル化するときに使用する特別なコンストラクターを指定するだけです。  
  
     [!code-csharp[SerializationGuidelines#8](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#8)]
     [!code-vb[SerializationGuidelines#8](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#8)]  
  
3. このサンプルに示すように、シリアル化コンストラクターを保護し、型と名前を指定した 2 つのパラメーターを用意してください。  
  
     [!code-csharp[SerializationGuidelines#9](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#9)]
     [!code-vb[SerializationGuidelines#9](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#9)]  
  
4. ISerializable メンバーを明示的に実装してください。  
  
     [!code-csharp[SerializationGuidelines#10](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#10)]
     [!code-vb[SerializationGuidelines#10](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#10)]  
  
5. **ISerializable.GetObjectData** の実装にはリンク確認要求を適用してください。 こうすることで、完全に信頼できるコア、およびランタイムのシリアライザーだけがメンバーにアクセスできるようになります。  
  
     [!code-csharp[SerializationGuidelines#11](../../../samples/snippets/csharp/VS_Snippets_CFX/serializationguidelines/cs/source.cs#11)]
     [!code-vb[SerializationGuidelines#11](../../../samples/snippets/visualbasic/VS_Snippets_CFX/serializationguidelines/vb/source.vb#11)]  
  
## <a name="see-also"></a>参照

- [データ コントラクトの使用](../../../docs/framework/wcf/feature-details/using-data-contracts.md)
- [データ コントラクト シリアライザー](../../../docs/framework/wcf/feature-details/data-contract-serializer.md)
- [データ コントラクト シリアライザーでサポートされる型](../../../docs/framework/wcf/feature-details/types-supported-by-the-data-contract-serializer.md)
- [バイナリ シリアル化](binary-serialization.md)
- [.NET リモート処理](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507(v=vs.100))
- [XML シリアル化および SOAP シリアル化](xml-and-soap-serialization.md)
- [セキュリティとシリアル化](../../../docs/framework/misc/security-and-serialization.md)
