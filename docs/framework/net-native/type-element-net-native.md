---
title: <Type> 要素 (.NET ネイティブ)
ms.date: 03/30/2017
ms.assetid: 1e88d368-a886-4f1e-8eb6-6127979a9fce
ms.openlocfilehash: 4e88b49b82513079ddcf6f0bafe02d44235a406a
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73091853"
---
# <a name="type-element-net-native"></a>\<Type > 要素 (.NET ネイティブ)

クラスや構造体などの特定の型に実行時ポリシーを適用します。

## <a name="syntax"></a>構文

```xml
<Type Name="type_name"
      Activate="policy_type"
      Browse="policy_type"
      Dynamic="policy_type"
      Serialize="policy_type"
      DataContractSerializer="policy_setting"
      DataContractJsonSerializer="policy_setting"
      XmlSerializer="policy_setting"
      MarshalObject="policy_setting"
      MarshalDelegate="policy_setting"
      MarshalStructure="policy_setting" />
```

## <a name="attributes-and-elements"></a>属性および要素

以降のセクションでは、属性、子要素、および親要素について説明します。

### <a name="attributes"></a>属性

|属性|属性の型|説明|
|---------------|--------------------|-----------------|
|`Name`|全般|必須の属性です。 型名を指定します。|
|`Activate`|リフレクション|省略可能な属性です。 コンストラクターへの実行時アクセスを制御して、インスタンスのアクティブ化を有効にします。|
|`Browse`|リフレクション|省略可能な属性です。 プログラム要素に関する情報の照会を制御しますが、実行時アクセスは有効にしません。|
|`Dynamic`|リフレクション|省略可能な属性です。 コンストラクター、メソッド、フィールド、プロパティ、およびイベントを含むすべての型のメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。|
|`Serialize`|シリアル化|省略可能な属性です。 コンストラクター、フィールド、およびプロパティへの実行時アクセスを制御し、Newtonsoft の JSON シリアライザーなどのライブラリによって型インスタンスをシリアル化および逆シリアル化できるようにします。|
|`DataContractSerializer`|シリアル化|省略可能な属性です。 <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> クラスを使用するシリアル化のポリシーを制御します。|
|`DataContractJsonSerializer`|シリアル化|省略可能な属性です。 <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType> クラスを使用する JSON シリアル化のポリシーを制御します。|
|`XmlSerializer`|シリアル化|省略可能な属性です。 <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> クラスを使用する XML シリアル化のポリシーを制御します。|
|`MarshalObject`|Interop|省略可能な属性です。 Windows ランタイムと COM に参照型をマーシャリングするためのポリシーを制御します。|
|`MarshalDelegate`|Interop|省略可能な属性です。 ネイティブ コードへの関数ポインターとしてデリゲート型をマーシャリングするためのポリシーを制御します。|
|`MarshalStructure`|Interop|省略可能な属性です。 値型をネイティブ コードにマーシャリングするためのポリシーを制御します。|

## <a name="name-attribute"></a>Name 属性

|[値]|説明|
|-----------|-----------------|
|*type_name*|型名。 この `<Type>` 要素が [\<Namespace>](namespace-element-net-native.md) 要素または別の `<Type>` 要素のいずれかの子である場合、*type_name* には名前空間なしで型の名前を含めることができます。 それ以外の場合は、*type_name* には完全修飾型名を含める必要があります。|

## <a name="all-other-attributes"></a>その他すべての属性

|[値]|説明|
|-----------|-----------------|
|*policy_setting*|このポリシーの種類に適用する設定です。 指定できる値は、`All`、`Auto`、`Excluded`、`Public`、`PublicAndInternal`、`Required Public`、`Required PublicAndInternal`、および `Required All` です。 詳細については、「[ランタイム ディレクティブのポリシー設定](runtime-directive-policy-settings.md)」を参照してください。|

### <a name="child-elements"></a>子要素

|要素|説明|
|-------------|-----------------|
|[\<AttributeImplies>](attributeimplies-element-net-native.md)|含んでいる型が属性の場合は、その属性が適用されるコード要素の実行時ポリシーを定義します。|
|[\<Event>](event-element-net-native.md)|この型に属するイベントにリフレクション ポリシーを適用します。|
|[\<Field>](field-element-net-native.md)|この型に属するフィールドにリフレクション ポリシーを適用します。|
|[\<GenericParameter>](genericparameter-element-net-native.md)|ジェネリック型のパラメーターの型にポリシーを適用します。|
|[\<ImpliesType>](impliestype-element-net-native.md)|型にポリシーを適用します (それを含む `<Type>` 要素により表される型にそのポリシーが適用されている場合)。|
|[\<Method>](method-element-net-native.md)|この型に属するメソッドにリフレクション ポリシーを適用します。|
|[\<MethodInstantiation>](methodinstantiation-element-net-native.md)|この型に属する構築されたジェネリック メソッドにリフレクション ポリシーを適用します。|
|[\<Property>](property-element-net-native.md)|この型に属するプロパティにリフレクション ポリシーを適用します。|
|[\<Subtypes>](subtypes-element-net-native.md)|それを含む型から継承されたすべてのクラスに実行時ポリシーを適用します。|
|`<Type>`|入れ子になった型にリフレクション ポリシーを適用します。|
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|構築されたジェネリック型にリフレクション ポリシーを適用します。|

### <a name="parent-elements"></a>親要素

|要素|説明|
|-------------|-----------------|
|[\<Application>](application-element-net-native.md)|実行時にリフレクションで使用可能なメタデータを持つ、アプリケーション全体の型と型のメンバーのコンテナーとして機能します。|
|[\<Assembly>](assembly-element-net-native.md)|指定したアセンブリ内のすべての型にリフレクション ポリシーを適用します。|
|[\<Library>](library-element-net-native.md)|実行時にリフレクションに使用可能なメタデータを持つ型と型のメンバーを含むアセンブリを定義します。|
|[\<Namespace>](namespace-element-net-native.md)|名前空間内のすべての型にリフレクション ポリシーを適用します。|
|`<Type>`|型とそのすべてのメンバーにリフレクション ポリシーを適用します。|
|[\<TypeInstantiation>](typeinstantiation-element-net-native.md)|構築されたジェネリック型とそのすべてのメンバーにリフレクション ポリシーを適用します。|

## <a name="remarks"></a>Remarks

リフレクション、シリアル化、および相互運用属性はすべて省略可能です。 いずれも存在しない場合、`<Type>` 要素は、その子型が個々のメンバーのポリシーを定義するコンテナーとして機能します。

`<Type>` 要素が [\<Assembly>](assembly-element-net-native.md)、[\<Namespace>](namespace-element-net-native.md)、`<Type>`、[\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素の子である場合、親要素によって定義されたポリシー設定をオーバーライドします。

ジェネリック型の `<Type>` 要素は、独自のポリシーを持たないすべてのインスタンス化にそのポリシーを適用します。 構築されたジェネリック型のポリシーは、[\<TypeInstantiation>](typeinstantiation-element-net-native.md) 要素により定義されます。

型がジェネリック型の場合、アクサン グラーブ記号 (\`) の後ろにジェネリック パラメーターの数を付けたもので名前が修飾されます。 たとえば、`Name` クラスの `<Type>` 要素の <xref:System.Collections.Generic.List%601?displayProperty=nameWithType> 属性は、``Name="System.Collections.Generic.List`1"`` と示されます。

## <a name="example"></a>例

次の例では、リフレクションを使用して、<xref:System.Collections.Generic.List%601?displayProperty=nameWithType> クラスのフィールド、プロパティ、およびメソッドに関する情報を表示します。 この例の変数 `b` は <xref:Windows.UI.Xaml.Controls.TextBlock> コントロールです。 この例は単に型情報を取得するのみであるため、メタデータの可用性は `Browse` ポリシー設定により制御されます。

 [!code-csharp[ProjectN_Reflection#3](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/browsegenerictype1.cs#3)]

 <xref:System.Collections.Generic.List%601> クラスのメタデータは .NET ネイティブツールチェーンによって自動的に含まれないため、この例では、要求されたメンバー情報を実行時に表示できません。 必要なメタデータを提供するには、次の `<Type>` 要素をランタイム ディレクティブ ファイルに追加します。 親要素 [<Namespace\>](namespace-element-net-native.md) を指定しているため、`<Type>` 要素で完全修飾型名を指定する必要はないことに注意してください。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="*Application*" Dynamic="Required All" />
      <Namespace Name ="System.Collections.Generic" >
        <Type Name="List`1" Browse="Required All" />
     </Namespace>
   </Application>
</Directives>
```

## <a name="example"></a>例
 次の例では、リフレクションを使用して、<xref:System.Reflection.PropertyInfo> プロパティを表す <xref:System.String.Chars%2A?displayProperty=nameWithType> オブジェクトを取得します。 続けて、<xref:System.Reflection.PropertyInfo.GetValue%28System.Object%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドを使用して文字列の 7 番目の文字の値を取得し、文字列のすべての文字を表示します。 この例の変数 `b` は <xref:Windows.UI.Xaml.Controls.TextBlock> コントロールです。

 [!code-csharp[ProjectN_Reflection#1](../../../samples/snippets/csharp/VS_Snippets_CLR/projectn_reflection/cs/propertyinfo1.cs#1)]

 <xref:System.String> オブジェクトのメタデータは使用できないため、<xref:System.Reflection.PropertyInfo.GetValue%28System.Object%2CSystem.Object%5B%5D%29?displayProperty=nameWithType> メソッドを呼び出すと、.NET ネイティブツールチェーンを使用してコンパイルしたときに、実行時に <xref:System.NullReferenceException> 例外がスローされます。 例外を排除し、必要なメタデータを提供するには、次の `<Type>` 要素をランタイム ディレクティブ ファイルに追加します。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Assembly Name="*Application*" Dynamic="Required All" />
    <Type Name="System.String" Dynamic="Required Public"/> -->
  </Application>
</Directives>
```

## <a name="see-also"></a>関連項目

- [ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス](runtime-directives-rd-xml-configuration-file-reference.md)
- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
- [ランタイム ディレクティブ ポリシーの設定](runtime-directive-policy-settings.md)
