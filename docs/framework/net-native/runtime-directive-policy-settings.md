---
title: ランタイム ディレクティブ ポリシーの設定
ms.date: 03/30/2017
ms.assetid: cb52b1ef-47fd-4609-b69d-0586c818ac9e
ms.openlocfilehash: 7a8933decaec45e8000f3f3d1717847f333deddd
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738494"
---
# <a name="runtime-directive-policy-settings"></a>ランタイム ディレクティブ ポリシーの設定

> [!NOTE]
> このトピックでは、プレリリース ソフトウェアである .NET Native Developer Preview について述べています。 プレビュー版は、[Microsoft Connect Web サイト](https://go.microsoft.com/fwlink/?LinkId=394611)からダウンロードできます (登録が必要です)。

.NET ネイティブのランタイム ディレクティブ ポリシー設定は、実行時に型と型のメンバーのメタデータが使用可能かどうかを決定します。 必要なメタデータがない場合、COM または Windows ランタイムへの .NET Framework 型のリフレクション、シリアル化と逆シリアル化、またはマーシャリングを利用する操作が失敗し、例外をスローする可能性があります。 最も一般的な例外は、[MissingMetadataException](missingmetadataexception-class-net-native.md) と、[MissingInteropDataException](missinginteropdataexception-class-net-native.md) (相互運用の場合) です。

実行時ポリシー設定は、ランタイム ディレクティブ (.rd.xml) ファイルによって制御されます。 各ランタイム ディレクティブは、アセンブリ ([\<Assembly>](assembly-element-net-native.md) 要素)、型 ([\<Type>](type-element-net-native.md) 要素)、またはメソッド ([\<Method>](method-element-net-native.md) 要素) などの特定のプログラム要素のポリシーを定義します。 ディレクティブには、次のセクションで説明する、リフレクション ポリシー種類、シリアル化ポリシー種類、および相互運用ポリシー種類を定義する属性が 1 つ以上含まれています。 属性の値はポリシー設定を定義します。

## <a name="policy-types"></a>ポリシーの種類

ランタイム ディレクティブ ファイルは、ポリシーの種類について、リフレクション、シリアル化、および相互運用の 3 つのカテゴリを認識します。

- リフレクション ポリシー種類は、実行時にリフレクションで使用できるメタデータを決定します。

  - `Activate` はコンストラクターへの実行時アクセスを制御して、インスタンスのアクティブ化を有効にします。

  - `Browse` は、プログラム要素に関する情報の照会を制御します。

  - `Dynamic` は、すべての型とメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。

  次の表に、リフレクション ポリシー種類と、この種類で使用できるプログラム要素を示します。

  |要素|アクティブ化|参照|動的|
  |-------------|--------------|------------|-------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)||✔️|✔️|
  |[\<Field>](field-element-net-native.md)||✔️|✔️|
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)||✔️|✔️|
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)||✔️|✔️|
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)||✔️|✔️|
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|

- シリアル化ポリシー種類は、実行時にシリアル化と逆シリアル化で使用できるメタデータを決定します。

  - `Serialize` は、コンストラクター、フィールド、およびプロパティへの実行時アクセスを制御し、Newtonsoft の JSON シリアライザーなどのサードパーティ ライブラリによって型インスタンスをシリアル化できるようにします。

  - `DataContractSerializer` はコンストラクター、フィールド、およびプロパティへの実行時アクセスを制御して、型インスタンスを <xref:System.Runtime.Serialization.DataContractSerializer> クラスによりシリアル化できるようにします。

  - `DataContractJsonSerializer` はコンストラクター、フィールド、およびプロパティへの実行時アクセスを制御して、型インスタンスを <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> クラスによりシリアル化できるようにします。

  - `XmlSerializer` はコンストラクター、フィールド、およびプロパティへの実行時アクセスを制御して、型インスタンスを <xref:System.Xml.Serialization.XmlSerializer> クラスによりシリアル化できるようにします。

  次の表に、シリアル化ポリシー種類と、この種類で使用できるプログラム要素を示します。

  |要素|シリアル化|DataContractSerializer|DataContractJsonSerializer|XmlSerializer|
  |-------------|---------------|----------------------------|--------------------------------|-------------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)|||||
  |[\<Field>](field-element-net-native.md)|✔️||||
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)|||||
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)|||||
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)|✔️||||
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|✔️|

- 相互運用ポリシー種類は、COM および Windows ランタイムに参照型、値型、および関数ポインターを渡すために実行時に使用できるメタデータを決定します。

  - `MarshalObject` は、参照型の COM および Windows ランタイムへのネイティブ マーシャリングを制御します。

  - `MarshalDelegate` は、関数ポインターとしてのデリゲート型のネイティブ マーシャリングを制御します。

  - `MarshalStructure` は、値型の COM および Windows ランタイムへのネイティブ マーシャリングを制御します。

  次の表に、相互運用ポリシー種類と、この種類で使用できるプログラム要素を示します。

  |要素|MarshalObject|MarshalDelegate|MarshalStructure|
  |-------------|-------------------|---------------------|----------------------|
  |[\<Application>](application-element-net-native.md)|✔️|✔️|✔️|
  |[\<Assembly>](assembly-element-net-native.md)|✔️|✔️|✔️|
  |[\<AttributeImplies>](attributeimplies-element-net-native.md)|✔️|✔️|✔️|
  |[\<Event>](event-element-net-native.md)||||
  |[\<Field>](field-element-net-native.md)||||
  |[\<GenericParameter>](genericparameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<ImpliesType>](impliestype-element-net-native.md)|✔️|✔️|✔️|
  |[\<Method>](method-element-net-native.md)||||
  |[\<MethodInstantiation>](methodinstantiation-element-net-native.md)||||
  |[\<Namespace>](namespace-element-net-native.md)|✔️|✔️|✔️|
  |[\<Parameter>](parameter-element-net-native.md)|✔️|✔️|✔️|
  |[\<Property>](property-element-net-native.md)||||
  |[\<Subtypes>](subtypes-element-net-native.md)|✔️|✔️|✔️|
  |[\<Type>](type-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|✔️|✔️|✔️|
  |[\<TypeParameter>](typeparameter-element-net-native.md)|✔️|✔️|✔️|

## <a name="policy-settings"></a>ポリシー設定

各ポリシーの種類は、次の表に示すいずれかの値に設定できます。 型のメンバーを表す要素は、他の要素とは異なる一連のポリシー設定をサポートしていることに注意してください。

|ポリシー設定|[説明]|`Assembly`、`Namespace`、`Type`、および `TypeInstantiation` 要素|`Event`、`Field`、`Method`、`MethodInstantiation`、および `Property` 要素|
|--------------------|-----------------|-----------------------------------------------------------------------|--------------------------------------------------------------------------------|
|`All`|.NET ネイティブ ツール チェーンが削除しないすべての型とメンバーのポリシーを有効にします。|✔️||
|`Auto`|そのプログラム要素のポリシーの種類に、既定のポリシーを使用する必要があることを指定します。 これは、そのポリシーの種類のポリシーを省略することと同じです。 `Auto` は通常、ポリシーが親要素から継承されることを示すために使用されます。|✔️|✔️|
|`Excluded`|特定のプログラム要素についてポリシーを無効にすることを指定します。 たとえば、次のランタイム ディレクティブは、<br /><br /> `<Type Name="BusinessClasses.Person" Browse="Excluded" Dynamic="Excluded" />`<br /><br /> `BusinessClasses.Person` オブジェクトの参照、および動的なインスタンス化と変更のいずれにも、`Person` クラスのメタデータを使用できないことを示しています。|✔️|✔️|
|`Included`|親型のメタデータが使用可能な場合に、ポリシーを有効にします。||✔️|
|`Public`|ツール チェーンによってパブリック型またはパブリック メンバーが不要と判断され、削除されない限り、それらの型またはメンバーのポリシーを有効にします。 この設定は、ツール チェーンによって不要であると判断された場合でもパブリック型とパブリック メンバーのメタデータを常に使用可能にする `Required Public` とは異なります。|✔️||
|`PublicAndInternal`|ツール チェーンでパブリックおよび内部の型またはメンバーが不要であると判断され、削除されない限り、それらの型またはメンバーのポリシーを有効にします。 この設定は、ツール チェーンによって不要と判断された場合でもパブリックおよび内部の型とメンバーのメタデータを常に使用可能にする `Required PublicAndInternal` とは異なります。|✔️||
|`Required`|メンバーが使用されていると見なされる場合でも、メンバーのポリシーを有効にし、そのメタデータを使用できるようにすることを指定します。||✔️|
|`Required Public`|パブリック型またはパブリック メンバーのポリシーを有効にして、パブリック型およびパブリック メンバーのメタデータが常に使用可能であるようにします。 この設定は、ツール チェーンが必要であると判断した場合にのみ、パブリック型とパブリック メンバーのメタデータを使用可能にする `Public` とは異なります。|✔️||
|`Required PublicAndInternal`|パブリックおよび内部の型またはメンバーのポリシーを有効にして、パブリックおよび内部の型とメンバーのメタデータが常に使用可能であるようにします。 この設定は、ツール チェーンが必要であると判断した場合にのみ、パブリックおよび内部の型とメンバーのメタデータを使用可能にする `PublicAndInternal` とは異なります。|✔️||
|`Required All`|使用されているかどうかに関係なく、すべての型とメンバーを保持し、そのポリシーを有効にするために、ツール チェーンを要求します。|✔️||

## <a name="see-also"></a>参照

- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
