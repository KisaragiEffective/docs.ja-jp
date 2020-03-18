---
title: ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス
ms.date: 03/30/2017
ms.assetid: 8241523f-d8e1-4fb6-bf6a-b29bfe07b38a
ms.openlocfilehash: e74d34693446cca645003a9f93bc1777849e3182
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738411"
---
# <a name="runtime-directives-rdxml-configuration-file-reference"></a>ランタイム ディレクティブ (rd.xml) 構成ファイル リファレンス

ランタイム ディレクティブ (.rd.xml) ファイルは、指定されたプログラム要素をリフレクションに使用できるかどうかを示す XML 構成ファイルです。 ランタイム ディレクティブ ファイルの例を次に示します。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
    <Namespace Name="Contoso.Cloud.AppServices" Serialize="Required Public" />
    <Namespace Name="ContosoClient.ViewModels" Serialize="Required Public" />
    <Namespace Name="ContosoClient.DataModel" Serialize="Required Public" />
    <Namespace Name="Contoso.Reader.UtilityLib" Serialize="Required Public" />

    <Namespace Name="System.Collections.ObjectModel" >
      <TypeInstantiation Name="ObservableCollection"
            Arguments="ContosoClient.DataModel.ProductItem" Serialize="Public" />
      <TypeInstantiation Name="ReadOnlyObservableCollection"
            Arguments="ContosoClient.DataModel.ProductGroup" Serialize="Public" />
    </Namespace>
  </Application>
</Directives>
```

## <a name="the-structure-of-a-runtime-directives-file"></a>ランタイム ディレクティブ ファイルの構造

ランタイム ディレクティブ ファイルは `http://schemas.microsoft.com/netfx/2013/01/metadata` 名前空間を使用します。

ルート要素は [Directives](directives-element-net-native.md) 要素です。 これには、次の構造に示すように、0 個以上の [Library](library-element-net-native.md) 要素と 0 または 1 個の [Application](application-element-net-native.md) 要素を含めることができます。 [Application](application-element-net-native.md) 要素の属性は、アプリケーション全体のランタイム リフレクション ポリシーを定義できるか、子要素のコンテナーとして機能できます。 一方、[Library](library-element-net-native.md) 要素は単にコンテナーです。 [Application](application-element-net-native.md) 要素と [Library](library-element-net-native.md) 要素の子は、リフレクションで使用できる型、メソッド、フィールド、プロパティ、およびイベントを定義します。

参照情報については、次の構造から要素を選択するか、「[ランタイム ディレクティブ要素](runtime-directive-elements.md)」を参照してください。 次の階層で、省略記号は再帰構造を示します。 角かっこ内の情報は、その要素が省略可能または必須のいずれであるか、および使用される場合に許可されるインスタンスの数 (1 つまたは複数) を示します。

- [Directives](directives-element-net-native.md) [1:1]
  - [Application](application-element-net-native.md) [0:1]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [名前空間](namespace-element-net-native.md)[0: M]。 。 。
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [名前空間](namespace-element-net-native.md)[0: M]。 。 。
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md) (それを含む型のサブクラス) [O:1]
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
      - [AttributeImplies](attributeimplies-element-net-native.md) (それを含む型が属性) [O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (構築されたジェネリック メソッド) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md) (構築されたジェネリック型) [0:M]
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
      - [Method](method-element-net-native.md) [0:M]
        - [Parameter](parameter-element-net-native.md) [0:M]
        - [TypeParameter](typeparameter-element-net-native.md) [0:M]
        - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (構築されたジェネリック メソッド) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
  - [Library](library-element-net-native.md) [0:M]
    - [Assembly](assembly-element-net-native.md) [0:M]
      - [名前空間](namespace-element-net-native.md)[0: M]。 。 。
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
    - [Namespace](namespace-element-net-native.md) [0:M]
      - [名前空間](namespace-element-net-native.md)[0: M]。 。 。
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
    - [Type](type-element-net-native.md) [0:M]
      - [Subtypes](subtypes-element-net-native.md) (それを含む型のサブクラス) [O:1]
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
      - [AttributeImplies](attributeimplies-element-net-native.md) (それを含む型が属性) [O:1]
      - [GenericParameter](genericparameter-element-net-native.md) [0:M]
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (構築されたジェネリック メソッド) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]
    - [TypeInstantiation](typeinstantiation-element-net-native.md) (構築されたジェネリック型) [0:M]
      - [「](type-element-net-native.md) [0: M]」と入力します。 。 。
      - [Typeinstantiation インスタンス化](typeinstantiation-element-net-native.md)(構築されたジェネリック型) [0: M]。 。 。
      - [Method](method-element-net-native.md) [0:M]
      - [MethodInstantiation](methodinstantiation-element-net-native.md) (構築されたジェネリック メソッド) [0:M]
      - [Property](property-element-net-native.md) [0:M]
      - [Field](field-element-net-native.md) [0:M]
      - [Event](event-element-net-native.md) [0:M]

[Application](application-element-net-native.md) 要素は属性を持たないか、「[ランタイム ディレクティブとポリシー](#Directives)」セクションで説明しているポリシー属性を持つことができます。

[Library](library-element-net-native.md) 要素は、ライブラリまたはアセンブリの名前をファイル拡張子なしで指定する、`Name` 属性 1 つを持ちます。 たとえば、次の [Library](library-element-net-native.md) 要素は、Extensions.dll という名前のアセンブリに適用されます。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
  <Application>
     <!-- Child elements go here -->
  </Application>
  <Library Name="Extensions">
     <!-- Child elements go here -->
  </Library>
</Directives>
```

<a name="Directives"></a>

## <a name="runtime-directives-and-policy"></a>ランタイム ディレクティブとポリシー

[Application](application-element-net-native.md) 要素自体と [Library](library-element-net-native.md) 要素と [Application](application-element-net-native.md) 要素の子要素はポリシーを表します。つまり、アプリがプログラム要素にリフレクションを適用できる方法を定義します。 ポリシーの種類は要素の属性 (たとえば、`Serialize`) により定義されます。 ポリシーの値は属性の値 (たとえば、`Serialize="Required"`) により定義されます。

要素の属性により指定されるすべてのポリシーは、そのポリシーの値を指定しないすべての子要素に適用されます。 たとえば、[Type](type-element-net-native.md) 要素で指定されたポリシーは、ポリシーが明示的に指定されていない、その要素に含まれる型とメンバーすべてに適用されます。

[Application](application-element-net-native.md)、[Assembly](assembly-element-net-native.md)、[AttributeImplies](attributeimplies-element-net-native.md)、[Namespace](namespace-element-net-native.md)、[Subtypes](subtypes-element-net-native.md)、および [Type](type-element-net-native.md) 要素で表すことができるポリシーは、個々のメンバーについて ([Method](method-element-net-native.md)、[Property](property-element-net-native.md)、[Field](field-element-net-native.md)、および [Event](event-element-net-native.md) 要素で) 表すことができるポリシーとは異なります。

### <a name="specifying-policy-for-assemblies-namespaces-and-types"></a>アセンブリ、名前空間、型に対するポリシーの指定

[Application](application-element-net-native.md)、[Assembly](assembly-element-net-native.md)、[AttributeImplies](attributeimplies-element-net-native.md)、[Namespace](namespace-element-net-native.md)、[Subtypes](subtypes-element-net-native.md)、および [Type](type-element-net-native.md) 要素は、次のポリシーの種類をサポートしています。

- [https://login.microsoftonline.com/consumers/](`Activate`) コンストラクターへの実行時アクセスを制御して、インスタンスのアクティブ化を有効にします。

- [https://login.microsoftonline.com/consumers/](`Browse`) プログラム要素に関する情報の照会を制御しますが、実行時アクセスは有効にしません。

- [https://login.microsoftonline.com/consumers/](`Dynamic`) コンストラクター、メソッド、フィールド、プロパティ、およびイベントを含むすべての型のメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。

- [https://login.microsoftonline.com/consumers/](`Serialize`) コンストラクター、フィールド、およびプロパティへの実行時アクセスを制御し、Newtonsoft の JSON シリアライザーなどのサードパーティ ライブラリによって型インスタンスをシリアル化および逆シリアル化できるようにします。

- [https://login.microsoftonline.com/consumers/](`DataContractSerializer`) <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> クラスを使用するシリアル化のポリシーを制御します。

- [https://login.microsoftonline.com/consumers/](`DataContractJsonSerializer`) <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType> クラスを使用する JSON シリアル化のポリシーを制御します。

- [https://login.microsoftonline.com/consumers/](`XmlSerializer`) <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType> クラスを使用する XML シリアル化のポリシーを制御します。

- [https://login.microsoftonline.com/consumers/](`MarshalObject`) WinRT と COM に参照型をマーシャリングするためのポリシーを制御します。

- [https://login.microsoftonline.com/consumers/](`MarshalDelegate`) ネイティブ コードへの関数ポインターとしてデリゲート型をマーシャリングするためのポリシーを制御します。

- [`MarshalStructure`]。 ネイティブ コードに構造体をマーシャリングするためのポリシーを制御します。

これらのポリシーの種類に関連付けられている設定を次に示します。

- [https://login.microsoftonline.com/consumers/](`All`) ツール チェーンが削除しないすべての型とメンバーに対するポリシーを有効にします。

- [https://login.microsoftonline.com/consumers/](`Auto`) 既定の動作を使用します。 (親要素などによってポリシーがオーバーライドされない限り、ポリシーを指定しないことは、そのポリシーを `Auto` に設定することと同じです。)

- [https://login.microsoftonline.com/consumers/](`Excluded`) プログラム要素のポリシーを無効にします。

- [https://login.microsoftonline.com/consumers/](`Public`) ツール チェーンがメンバーが不要なために削除すると判断した場合を除き、パブリック型またはメンバーのポリシーを有効にします。 (後者の場合は、`Required Public` を使用して、メンバーが保持されており、リフレクション機能があることを確認する必要があります。)

- [https://login.microsoftonline.com/consumers/](`PublicAndInternal`) パブリックおよび内部型またはメンバーがツール チェーンによって削除されていない場合、それらのポリシーを有効にします。

- [https://login.microsoftonline.com/consumers/](`Required Public`) 使用されているかどうかに関係なく、パブリック型とメンバーを保持し、それらのポリシーを有効にするためにツール チェーンを要求します。

- [https://login.microsoftonline.com/consumers/](`Required PublicAndInternal`) 使用されているかどうかに関係なく、パブリックおよび内部両方の型とメンバーを保持し、それらのポリシーを有効にするためにツール チェーンを要求します。

- [https://login.microsoftonline.com/consumers/](`Required All`) 使用されているかどうかに関係なく、すべての型とメンバーを保持し、それらのポリシーを有効にするために、ツール チェーンを要求します。

たとえば、次のランタイム ディレクティブ ファイルは、DataClasses.dll アセンブリ内のすべての型とメンバーのポリシーを定義します。 これは、すべてのパブリック プロパティのシリアル化のリフレクションを有効にし、すべての型と型のメンバーの参照を有効にし、すべての型のアクティブ化を (`Dynamic` 属性により) 有効にして、すべてのパブリック型とメンバーのリフレクションを有効にします。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"
                Browse="All" Activate="PublicAndInternal"
                Dynamic="Public"  />
   </Application>
   <Library Name="UtilityLibrary">
     <!-- Child elements go here -->
   </Library>
</Directives>
```

### <a name="specifying-policy-for-members"></a>メンバーのポリシーの指定

[Property](property-element-net-native.md) 要素と [Field](field-element-net-native.md) 要素は次のポリシーの種類をサポートしています。

- `Browse`: このメンバーに関する情報の照会を制御しますが、実行時アクセスは有効にしません。

- `Dynamic`: コンストラクター、メソッド、フィールド、プロパティ、およびイベントを含むすべての型のメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。 また、それを含む型に関する情報の照会も制御します。

- `Serialize`: メンバーへの実行時アクセスを制御し、Newtonsoft の JSON シリアライザーなどのライブラリによって型インスタンスをシリアル化および逆シリアル化できるようにします。 このポリシーは、コンストラクター、フィールド、およびプロパティに適用できます。

[Method](method-element-net-native.md) 要素と [Event](event-element-net-native.md) 要素は次のポリシーの種類をサポートしています。

- `Browse`: このメンバーに関する情報の照会を制御しますが、実行時アクセスは有効にしません。

- `Dynamic`: コンストラクター、メソッド、フィールド、プロパティ、およびイベントを含むすべての型のメンバーへの実行時アクセスを制御して、動的プログラミングを有効にします。 また、それを含む型に関する情報の照会も制御します。

 これらのポリシーの種類に関連付けられている設定を次に示します。

- `Auto`: 既定の動作を使用します。 (何かにオーバーライドされない限り、ポリシーを指定しないことは、そのポリシーを `Auto` に設定することと同じです。)

- `Excluded`: メンバーのメタデータを含めません。

- `Included`: 親型が出力に存在する場合、ポリシーを有効にします。

- `Required`: このメンバーが未使用と見なされる場合でもこれを保持し、そのメンバーのポリシーを有効にするためにツール チェーンを要求します。

## <a name="runtime-directives-file-semantics"></a>ランタイム ディレクティブ ファイルのセマンティクス

ポリシーは、上位レベルと下位レベルの要素両方に同時に定義できます。 たとえば、アセンブリと、そのアセンブリに含まれる型の一部にポリシーを定義できます。 特定の下位レベル要素が表されていない場合、その親のポリシーを継承します。 たとえば、`Assembly` 要素は存在するが `Type` 要素は存在しない場合、`Assembly` 要素で指定されるポリシーはアセンブリ内のすべての型に適用されます。 複数の要素が同じプログラム要素にポリシーを適用することもできます。 たとえば、個別の [Assembly](assembly-element-net-native.md) 要素が、同じアセンブリの同じポリシー要素を別々に定義するとします。 次のセクションで、このような場合に特定の型のポリシーがどのように解決されるかを説明します。

ジェネリック型またはメソッドの [Type](type-element-net-native.md) 要素または [Method](method-element-net-native.md) 要素は、独自のポリシーを持たないすべてのインスタンス化にそのポリシーを適用します。 たとえば、`Type` のポリシーを指定する <xref:System.Collections.Generic.List%601> 要素は、構築された特定のジェネリック型 (`List<Int32>` など) について `TypeInstantiation` 要素によってオーバーライドされない限り、そのジェネリック型のすべての構築されたインスタンスに適用されます。 そうでない場合、要素は指定されたプログラム要素のポリシーを定義します。

要素があいまいな場合、エンジンが一致を検索し、完全一致が見つかるとそれを使用します。 複数の一致が見つかった場合は警告またはエラーになります。

### <a name="if-two-directives-apply-policy-to-the-same-program-element"></a>2 つのディレクティブが同じプログラム要素にポリシーを適用する場合

別のランタイム ディレクティブ ファイルにある 2 つの要素が同じプログラム要素 (アセンブリや型など) の同じポリシーの種類を異なる値に設定しようとした場合、次のように競合が解決されます。

1. `Excluded` 要素が存在する場合はこれが優先されます。

2. `Required` は `Required` 以外より優先されます。

3. `All` は `PublicAndInternal` (`Public` より優先) より優先されされます。

4. 明示的な設定はいずれも `Auto` より優先されます。

たとえば、1 つのプロジェクトに次の 2 つのランタイム ディレクティブ ファイルが含まれている場合、DataClasses.dll のシリアル化ポリシーは `Required Public` と `All` の両方に設定されます。 この場合、シリアル化ポリシーは `Required All` として解決されます。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public"/>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="All" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

ただし、1 つのランタイム ディレクティブ ファイル内の 2 つのディレクティブが同じプログラム要素に同じポリシーの種類を設定しようとした場合、XML スキーマ定義ツールからエラー メッセージが表示されます。

### <a name="if-child-and-parent-elements-apply-the-same-policy-type"></a>子要素と親要素が同じポリシーの種類を適用する場合

`Excluded` 設定を含め、子要素はその親要素をオーバーライドします。 オーバーライドは、`Auto` を指定する必要がある主な理由です。

次の例では、`DataClasses` にはあるが `DataClasses.ViewModels` にはないものすべてのシリアル化ポリシー設定が `Required Public` になり、`DataClasses` と `DataClasses.ViewModels` の両方にあるものすべてのシリアル化ポリシー設定は `All` になります。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="if-open-generics-and-instantiated-elements-apply-the-same-policy-type"></a>オープン ジェネリックとインスタンス化された要素が同じポリシーの種類を適用する場合

次の例では、`Dictionary<int,int>` には、特に `Browse` ポリシーを割り当てる理由がエンジンにある場合にのみ、`Browse` ポリシーが割り当てられます (通常は、これが既定の動作です)。それ以外の <xref:System.Collections.Generic.Dictionary%602> のすべてのインスタンス化では、そのメンバーのすべてが参照可能になります。

```xml
<Directives xmlns="http://schemas.microsoft.com/netfx/2013/01/metadata">
   <Application>
      <Assembly Name="DataClasses" Serialize="Required Public" >
         <Namespace Name="DataClasses.ViewModels" Serialize="All" />
      </Assembly>
      <Namespace Name="DataClasses.Generics" />
      <Type Name="Dictionary" Browse="All" />
      <TypeInstantiation Name="Dictionary"
                         Arguments="System.Int32,System.Int32" Browse="Auto" />
   </Application>
   <Library Name="DataClasses">
      <!-- any other elements -->
   </Library>
</Directives>
```

### <a name="how-policy-is-inferred"></a>ポリシーの推論方法

ポリシーの種類ごとに、そのポリシーの種類の存在が他の構造体にどう影響するかを決定する一連のルールが異なります。

#### <a name="the-effect-of-browse-policy"></a>Browse ポリシーの効果

`Browse` ポリシーを型に適用すると、ポリシーが次のように変更されます。

- 型の基本型が `Browse` ポリシーでマークされます。

- 型がインスタンス化されたジェネリックである場合、インスタンス化されていないその型が `Browse` ポリシーでマークされます。

- 型がデリゲートの場合、型の `Invoke` メソッドが `Dynamic` ポリシーでマークされます。

- 型の各インターフェイスが `Browse` ポリシーでマークされます。

- 型に適用されている各属性の型が `Browse` ポリシーでマークされます。

- 型がジェネリックの場合、各制約型が `Browse` ポリシーでマークされます。

- 型がジェネリックの場合、その型のインスタンス化対象である型が `Browse` ポリシーでマークされます。

メソッドに `Browse` ポリシーを適用すると、ポリシーが次のように変更されます。

- メソッドの各パラメーター型が `Browse` ポリシーでマークされます。

- メソッドの戻り型が `Browse` ポリシーでマークされます。

- メソッドを含む型が `Browse` ポリシーでマークされます。

- メソッドがインスタンス化されたジェネリック メソッドである場合、インスタンス化されていないジェネリック メソッドが `Browse` ポリシーでマークされます。

- メソッドに適用される各属性の型が `Browse` ポリシーでマークされます。

- メソッドがジェネリックの場合、各制約型が `Browse` ポリシーでマークされます。

- メソッドがジェネリックの場合、そのメソッドのインスタンス化対象である型が `Browse` ポリシーでマークされます。

フィールドに `Browse` ポリシーを適用すると、ポリシーが次のように変更されます。

- フィールドに適用される各属性の型が `Browse` ポリシーでマークされます。

- フィールドの型が `Browse` ポリシーでマークされます。

- フィールドが属する型が `Browse` ポリシーでマークされます。

#### <a name="the-effect-of-dynamic-policy"></a>Dynamic ポリシーの効果

`Dynamic` ポリシーを型に適用すると、ポリシーが次のように変更されます。

- 型の基本型が `Dynamic` ポリシーでマークされます。

- 型がインスタンス化されたジェネリックである場合、インスタンス化されていないその型が `Dynamic` ポリシーでマークされます。

- 型がデリゲート型の場合、型の `Invoke` メソッドが `Dynamic` ポリシーでマークされます。

- 型の各インターフェイスが `Browse` ポリシーでマークされます。

- 型に適用されている各属性の型が `Browse` ポリシーでマークされます。

- 型がジェネリックの場合、各制約型が `Browse` ポリシーでマークされます。

- 型がジェネリックの場合、その型のインスタンス化対象である型が `Browse` ポリシーでマークされます。

メソッドに `Dynamic` ポリシーを適用すると、ポリシーが次のように変更されます。

- メソッドの各パラメーター型が `Browse` ポリシーでマークされます。

- メソッドの戻り型が `Dynamic` ポリシーでマークされます。

- メソッドを含む型が `Dynamic` ポリシーでマークされます。

- メソッドがインスタンス化されたジェネリック メソッドである場合、インスタンス化されていないジェネリック メソッドが `Browse` ポリシーでマークされます。

- メソッドに適用される各属性の型が `Browse` ポリシーでマークされます。

- メソッドがジェネリックの場合、各制約型が `Browse` ポリシーでマークされます。

- メソッドがジェネリックの場合、そのメソッドのインスタンス化対象である型が `Browse` ポリシーでマークされます。

- メソッドを `MethodInfo.Invoke` により呼び出すことができ、<xref:System.Reflection.MethodInfo.CreateDelegate%2A?displayProperty=nameWithType> によりデリゲートの作成が可能になります。

フィールドに `Dynamic` ポリシーを適用すると、ポリシーが次のように変更されます。

- フィールドに適用される各属性の型が `Browse` ポリシーでマークされます。

- フィールドの型が `Dynamic` ポリシーでマークされます。

- フィールドが属する型が `Dynamic` ポリシーでマークされます。

#### <a name="the-effect-of-activation-policy"></a>Activation ポリシーの効果

型に Activation ポリシーを適用すると、ポリシーが次のように変更されます。

- 型がインスタンス化されたジェネリックである場合、インスタンス化されていないその型が `Browse` ポリシーでマークされます。

- 型がデリゲート型の場合、型の `Invoke` メソッドが `Dynamic` ポリシーでマークされます。

- 型のコンストラクターが `Activation` ポリシーでマークされます。

メソッドに `Activation` ポリシーを適用すると、ポリシーが次のように変更されます。

- コンストラクターを <xref:System.Reflection.ConstructorInfo.Invoke%2A?displayProperty=nameWithType> メソッドと <xref:System.Activator.CreateInstance%2A?displayProperty=nameWithType> メソッドにより呼び出すことができます。 メソッドに関しては、`Activation` ポリシーはコンストラクターにのみ影響します。

フィールドに `Activation` ポリシーを適用しても効果はありません。

#### <a name="the-effect-of-serialize-policy"></a>Serialize ポリシーの効果

`Serialize` ポリシーにより、一般的なリフレクション ベースのシリアライザーを使用できるようになります。 ただし、Microsoft では、Microsoft 以外のシリアライザーの正確なリフレクション アクセス パターンを認識していないため、このポリシーが常に効果的であるとは限りません。

`Serialize` ポリシーを型に適用すると、ポリシーが次のように変更されます。

- 型の基本型が `Serialize` ポリシーでマークされます。

- 型がインスタンス化されたジェネリックである場合、インスタンス化されていないその型が `Browse` ポリシーでマークされます。

- 型がデリゲート型の場合、型の `Invoke` メソッドが `Dynamic` ポリシーでマークされます。

- 型が列挙の場合、型の配列が `Serialize` ポリシーでマークされます。

- 型が <xref:System.Collections.Generic.IEnumerable%601> を実装する場合、`T` が `Serialize` ポリシーでマークされます。

- 型が <xref:System.Collections.Generic.IEnumerable%601>、<xref:System.Collections.Generic.IList%601>、<xref:System.Collections.Generic.ICollection%601>、<xref:System.Collections.Generic.IReadOnlyCollection%601>、または <xref:System.Collections.Generic.IReadOnlyList%601> の場合、`T[]` と <xref:System.Collections.Generic.List%601> が `Serialize` ポリシーでマークされますが、インターフェイス型のメンバーは `Serialize` ポリシーでマークされません。

- 型が <xref:System.Collections.Generic.List%601> の場合、型のどのメンバーも `Serialize` ポリシーでマークされません。

- 型が <xref:System.Collections.Generic.IDictionary%602> の場合、<xref:System.Collections.Generic.Dictionary%602> が `Serialize` ポリシーでマークされます。 ただし、型のどのメンバーも `Serialize` ポリシーでマークされません。

- 型が <xref:System.Collections.Generic.Dictionary%602> の場合、型のどのメンバーも `Serialize` ポリシーでマークされません。

- 型が <xref:System.Collections.Generic.IDictionary%602> を実装している場合、`TKey` と `TValue` が `Serialize` ポリシーでマークされます。

- 各コンストラクター、各プロパティ アクセサー、および各フィールドが `Serialize` ポリシーでマークされます。

メソッドに `Serialize` ポリシーを適用すると、ポリシーが次のように変更されます。

- それを含む型が `Serialize` ポリシーでマークされます。

- メソッドの戻り型が `Serialize` ポリシーでマークされます。

フィールドに `Serialize` ポリシーを適用すると、ポリシーが次のように変更されます。

- それを含む型が `Serialize` ポリシーでマークされます。

- フィールドの型が `Serialize` ポリシーでマークされます。

#### <a name="the-effect-of-xmlserializer-datacontractserializer-and-datacontractjsonserializer-policies"></a>XmlSerializer、DataContractSerializer、DataContractJsonSerializer の各ポリシーの影響

リフレクションベースのシリアライザーを対象とする `Serialize` ポリシーとは異なり、<xref:System.Xml.Serialization.XmlSerializer>、<xref:System.Runtime.Serialization.DataContractSerializer>、および <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer> のポリシーを使用して、.NET ネイティブツールチェーンで認識されているシリアライザーのセットを有効にします。 これらのシリアライザーはリフレクションを使用して実装されるのではなく、実行時にシリアル化可能な型のセットが、リフレクション可能な型と同様の方法で決定されます。

これらのポリシーのいずれかを型に適用すると、対応するシリアライザーで型をシリアル化できるようになります。 また、シリアル化が必要であることをシリアル化エンジンが静的に決定できる、すべての型もシリアル化されます。

これらのポリシーは、メソッドとフィールドには影響しません。

詳細については、「[Windows ストア アプリの .NET ネイティブへの移行](migrating-your-windows-store-app-to-net-native.md)」の「シリアライザーの違い」セクションを参照してください。

## <a name="see-also"></a>参照

- [ランタイム ディレクティブ要素](runtime-directive-elements.md)
- [リフレクションおよび .NET ネイティブ](reflection-and-net-native.md)
