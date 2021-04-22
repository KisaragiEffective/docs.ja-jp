---
title: データ コントラクトのコレクション型
description: データ コントラクト モデルによる .NET Framework のコレクションの処理方法と、WCF によるコレクション型のデータ シリアル化のサポート方法について説明します。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- collection types [WCF], data contracts
- data contracts [WCF], collection types
- collection types [WCF]
ms.assetid: 9b45b28e-0a82-4ea3-8c33-ec0094aff9d5
ms.openlocfilehash: 83acf1f74bf3cb117f3f94743eda32d3f2cc4b82
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245181"
---
# <a name="collection-types-in-data-contracts"></a>データ コントラクトのコレクション型

*"コレクション"* は、特定の型の項目のリストです。 .NET Framework では、このようなリストは、配列や他のさまざまな型 (ジェネリック List、ジェネリック <xref:System.ComponentModel.BindingList%601>、<xref:System.Collections.Specialized.StringCollection>、または <xref:System.Collections.ArrayList>) を使用して表すことができます。 たとえば、コレクションでは指定された顧客のアドレスのリストを保持できます。 これらのコレクションは、実際の型に関係なく、 *リスト コレクション* と呼びます。

コレクションには、ある項目 ("キー") と別の項目 ("値") の関連付けを表す特殊な形式のものがあります。 これらは、.NET Framework では <xref:System.Collections.Hashtable> やジェネリック ディクショナリなどの型によって表されます。 たとえば、関連付けコレクションでは、都市 ("キー") をその人口 ("値") に関連付けることができます。 これらのコレクションは、実際の型に関係なく、 *ディクショナリ コレクション* と呼びます。

データ コントラクト モデルでは、コレクションは特別な扱いを受けます。

配列やジェネリック コレクションを含め、 <xref:System.Collections.IEnumerable> インターフェイスを実装する型はコレクションとして認識されます。 これらの中で、 <xref:System.Collections.IDictionary> インターフェイスまたはジェネリック <xref:System.Collections.Generic.IDictionary%602> インターフェイスを実装する型がディクショナリ コレクションであり、他はすべてリスト コレクションです。

`Add` というメソッドとパラメーターなしのコンストラクターを用意するなど、コレクション型のその他の要件については、以降のセクションで詳しく説明します。 これにより、コレクション型を確実にシリアル化および逆シリアル化できます。 これは、直接サポートされないコレクションもあることを意味します。たとえば、ジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> (パラメーターなしのコンストラクターを持たないため) などは直接サポートされません。 これらの制限を回避する方法については、このトピックで後述する「コレクション インターフェイス型と読み取り専用コレクションの使用」を参照してください。

コレクションに含まれる型は、データ コントラクト型である必要があります。それ以外の場合は、シリアル化可能な型であることが必要です。 詳細については、「[データ コントラクト シリアライザーでサポートされる型](types-supported-by-the-data-contract-serializer.md)」を参照してください。

有効なコレクションと見なされるものと見なされないもの、およびコレクションがシリアル化される方法の詳細については、このトピックの「コレクションの高度な規則」でコレクションのシリアル化に関する情報を参照してください。

## <a name="interchangeable-collections"></a>交換可能なコレクション

同じ型のすべてのリスト コレクションは、同じデータ コントラクトを持つと見なされます (このトピックで後述するように、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を使用してカスタマイズされている場合を除きます)。 たとえば、次のデータ コントラクトは等価です。

[!code-csharp[c_collection_types_in_data_contracts#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#0)]
[!code-vb[c_collection_types_in_data_contracts#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#0)]

この 2 つのデータ コントラクトでは、いずれも次のコードに似た XML が生成されます。

```xml
<PurchaseOrder>
    <customerName>...</customerName>
    <items>
        <Item>...</Item>
        <Item>...</Item>
        <Item>...</Item>
        ...
    </items>
    <comments>
        <string>...</string>
        <string>...</string>
        <string>...</string>
        ...
    </comments>
</PurchaseOrder>
```

コレクションの可換性により、たとえば、サーバーではパフォーマンスを確保するように最適化されたコレクション型を使用し、クライアントではユーザー インターフェイス コンポーネントにバインドするよう設計されたコレクション型を使用することが可能です。

リスト コレクションと同様に、( <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性によってカスタマイズされている場合を除き) キーおよび値の型が同じであるすべてのディクショナリ コレクションは、同じデータ コントラクトを持つと見なされます。

コレクションの等価性に関する限り、重要となるのはデータ コントラクト型のみであり、.NET 型は影響しません。 つまり、Type1 と Type2 のデータ コントラクトが等価であれば、Type1 のコレクションは Type2 のコレクションと等価と見なされます。

非ジェネリック コレクションは、 `Object`型のジェネリック コレクションと同じデータ コントラクトを持つと見なされます (たとえば、 <xref:System.Collections.ArrayList> と <xref:System.Collections.Generic.List%601> のジェネリック `Object` のデータ コントラクトは同じです)。

## <a name="using-collection-interface-types-and-read-only-collections"></a>コレクション インターフェイス型と読み取り専用コレクションの使用

コレクション インターフェイス型 (<xref:System.Collections.IEnumerable>、 <xref:System.Collections.IDictionary>、ジェネリック <xref:System.Collections.Generic.IDictionary%602>、またはこれらのインターフェイスから派生したインターフェイス) も、実際のコレクション型のコレクション データ コントラクトと等価のコレクション データ コントラクトを持つと見なされます。 したがって、シリアル化する型をコレクション インターフェイス型として宣言できます。この結果は、実際のコレクション型を使用した場合と同じになります。 たとえば、次のデータ コントラクトは等価です。

[!code-csharp[c_collection_types_in_data_contracts#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#1)]
[!code-vb[c_collection_types_in_data_contracts#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#1)]

シリアル化の実行時には、宣言された型がインターフェイスの場合、使用される実際のインスタンスの型はそのインターフェイスを実装する任意の型になります。 前述の制限 (パラメーターなしのコンストラクターと `Add` メソッドを持つ) は適用されません。 たとえば、ジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> 型のデータ メンバーを直接宣言できない場合でも、Customer2 のアドレスを Address のジェネリック <xref:System.Collections.ObjectModel.ReadOnlyCollection%601>のインスタンスに設定できます。

逆シリアル化の実行時には、宣言された型がインターフェイスの場合、宣言されたインターフェイスを実装する型がシリアル化エンジンによって選択され、インスタンス化されます。 この場合、既知の型機構 (「[既知のデータ コントラクト型](data-contract-known-types.md)」を参照) は無効になります。型の選択は WCF に組み込まれています。

## <a name="customizing-collection-types"></a>コレクション型のカスタマイズ

コレクション型は、複数の用途を持つ <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を使用することによってカスタマイズできます。

コレクション型をカスタマイズすると、コレクションの可換性が損なわれるため、一般に、できるだけこの属性を適用しないようにすることをお勧めします。 この問題に関する詳細については、このトピックで後述する「コレクションの高度な規則」を参照してください。

### <a name="collection-data-contract-naming"></a>コレクション データ コントラクトの名前付け

コレクション型の名前付け規則は、「 [Data Contract Names](data-contract-names.md)」で説明する通常のデータ コントラクト型の名前付けの規則に似ていますが、重要な違いがいくつかあります。

- 名前をカスタマイズする際に、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性ではなく、 <xref:System.Runtime.Serialization.DataContractAttribute> 属性を使用します。 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性にも、 `Name` プロパティと `Namespace` プロパティがあります。

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用しない場合、コレクション型の既定の名前と名前空間は、コレクションに含まれる型の名前と名前空間によって決まります。 これらは、コレクション型自体の名前と名前空間の影響を受けません。 型の一例を次に示します。

  ```csharp
  public CustomerList1 : Collection<string> {}
  public StringList1 : Collection<string> {}
  ```

これらの型のデータ コントラクト名は、"CustomerList1" や "StringList1" ではなく、どちらも "ArrayOfstring" になります。 つまり、ルート レベルでこれらの型のどちらをシリアル化しても、次のコードのような XML が生成されます。

```xml
<ArrayOfstring>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</ArrayOfstring>
```

この名前付け規則が選択されたのは、文字列のリストを表すカスタマイズされていないすべての型が同じデータ コントラクトと XML 表現を持つことができるようにするためです。 これにより、コレクションの可換性が実現されています。 この例では、CustomerList1 と StringList1 は完全に交換可能です。

ただし、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用すると、この属性でプロパティが設定されていない場合でも、コレクションはカスタマイズされたコレクション データ コントラクトになります。 この場合、コレクション データ コントラクトの名前と名前空間は、コレクション型自体によって決まります。 型の一例を次に示します。

[!code-csharp[c_collection_types_in_data_contracts#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#2)]
[!code-vb[c_collection_types_in_data_contracts#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#2)]

シリアル化すると、生成される XML は次のようになります。

```xml
<CustomerList2>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</CustomerList2>
```

これは、カスタマイズされていない型の XML 表現と等価ではなくなっていることに注意してください。

- `Name` プロパティと `Namespace` プロパティを使用して、名前付けをさらにカスタマイズできます。 次のクラスを参照してください。

  [!code-csharp[c_collection_types_in_data_contracts#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#3)]
  [!code-vb[c_collection_types_in_data_contracts#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#3)]

生成される XML は次のようになります。

```xml
<cust_list>
    <string>...</string>
    <string>...</string>
    <string>...</string>
    ...
</cust_list>
```

詳細については、このトピックで後述する「コレクションの高度な規則」を参照してください。

### <a name="customizing-the-repeating-element-name-in-list-collections"></a>リスト コレクション内の反復される要素名のカスタマイズ

リスト コレクションには、反復されるエントリが含まれています。 通常、反復される各エントリは、コレクションに含まれる型のデータ コントラクト名に従って名前が付けられた要素として表されます。

`CustomerList` の各例では、コレクションに文字列が含まれていました。 文字列プリミティブ型のデータ コントラクト名は "string" であるため、反復される要素は "\<string>" でした。

ただし、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> 属性の <xref:System.Runtime.Serialization.CollectionDataContractAttribute> プロパティを使用すると、この反復される要素名をカスタマイズできます。 型の一例を次に示します。

[!code-csharp[c_collection_types_in_data_contracts#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#4)]
[!code-vb[c_collection_types_in_data_contracts#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#4)]

生成される XML は次のようになります。

```xml
<CustomerList4>
    <customer>...</customer>
    <customer>...</customer>
    <customer>...</customer>
    ...
</CustomerList4>
```

反復される要素の名前空間は、コレクション データ コントラクトの名前空間と常に同じです。前述のように、コレクション データ コントラクトの名前空間は、 `Namespace` プロパティを使用してカスタマイズできます。

### <a name="customizing-dictionary-collections"></a>ディクショナリ コレクションのカスタマイズ

ディクショナリ コレクションは、基本的にエントリのリストです。各エントリにはキーが含まれ、その後に値が続きます。 通常のリストと同様に、反復される要素に対応する要素名は、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ItemName%2A> プロパティを使用して変更できます。

また、キーと値を表す要素名は、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> プロパティと <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> プロパティを使用して変更できます。 これらの要素の名前空間は、コレクション データ コントラクトの名前空間と同じです。

型の一例を次に示します。

[!code-csharp[c_collection_types_in_data_contracts#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#5)]
[!code-vb[c_collection_types_in_data_contracts#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#5)]

シリアル化すると、生成される XML は次のようになります。

```xml
<CountriesOrRegionsWithCapitals>
    <entry>
        <countryorregion>USA</countryorregion>
        <capital>Washington</capital>
    </entry>
    <entry>
        <countryorregion>France</countryorregion>
        <capital>Paris</capital>
    </entry>
    ...
</CountriesOrRegionsWithCapitals>
```

ディクショナリ コレクションの詳細については、このトピックで後述される「コレクションの高度な規則」を参照してください。

## <a name="collections-and-known-types"></a>コレクションと既知の型

コレクション型を、他のコレクションまたはコレクション インターフェイスの代わりにポリモーフィックに使用する場合は、既知の型に追加する必要はありません。 たとえば、 <xref:System.Collections.IEnumerable> 型のデータ メンバーを宣言し、このデータ メンバーを使用して <xref:System.Collections.ArrayList>のインスタンスを送信する場合、 <xref:System.Collections.ArrayList> を既知の型に追加する必要はありません。

コレクションを、コレクション型以外の型の代わりにポリモーフィックに使用する場合は、コレクションを既知の型に追加する必要があります。 たとえば、 `Object` 型のデータ メンバーを宣言し、このデータ メンバーを使用して <xref:System.Collections.ArrayList>のインスタンスを送信する場合は、 <xref:System.Collections.ArrayList> を既知の型に追加する必要があります。

この場合、等価のコレクションをポリモーフィックにシリアル化することはできません。 たとえば、前述の例で、 <xref:System.Collections.ArrayList> を既知の型のリストに追加すると、 `Array of Object` クラスが等価のデータ コントラクトを持っていても、これを割り当てることはできなくなります。 これは、コレクション型以外の型のシリアル化における、通常の既知の型の動作と変わりません。しかし、コレクションが等価であることはごく一般的なことであるため、コレクションに関しては、この動作を理解しておくことが特に重要となります。

シリアル化では、指定されたデータ コントラクトの特定のスコープで許可される既知の型は 1 つだけであり、等価のコレクションはすべて同じデータ コントラクトを持ちます。 つまり、前述の例では、同じスコープで <xref:System.Collections.ArrayList> と `Array of Object` の両方を既知の型に追加することはできません。 先ほども説明したように、これはコレクション型以外の型での既知の型の動作と同じですが、コレクションではこの動作を理解しておくことが特に重要です。

コレクションの内容に既知の型が必要になることもあります。 たとえば、 <xref:System.Collections.ArrayList> に `Type1` と `Type2`のインスタンスを実際に含める場合、この両方の型を既知の型に追加する必要があります。

コレクションと既知の型を使用して、適切に構築されたオブジェクト グラフの例を次に示します (この例には、多少不自然な部分があります。実際のアプリケーションでは、通常、次のデータ メンバーを `Object`として定義することはないため、既知の型やポリモーフィズムの問題が発生することはありません)。

[!code-csharp[c_collection_types_in_data_contracts#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#6)]
[!code-vb[c_collection_types_in_data_contracts#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#6)]

逆シリアル化では、宣言された型がコレクション型の場合、実際に送信された型に関係なく、宣言された型がインスタンス化されます。 宣言された型がコレクション インターフェイスの場合は、既知の型に関係なく、インスタンス化される型がデシリアライザーによって選択されます。

また、逆シリアル化では、宣言された型がコレクション型ではないときにコレクション型を送信する場合、既知の型リストから一致するコレクション型が選択されます。 逆シリアル化では、コレクション インターフェイス型を既知の型のリストに追加できます。 この場合、逆シリアル化エンジンによって、インスタンス化する型が再度選択されます。

## <a name="collections-and-the-netdatacontractserializer-class"></a>コレクションと NetDataContractSerializer クラス

<xref:System.Runtime.Serialization.NetDataContractSerializer> クラスの使用時には、配列ではないカスタマイズされていないコレクション型 ( <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を使用していないコレクション型) は、その特殊な意味を失います。

<xref:System.SerializableAttribute> 属性でマークされたカスタマイズされていないコレクション型でも、 <xref:System.Runtime.Serialization.NetDataContractSerializer> 属性または <xref:System.SerializableAttribute> インターフェイスの規則に従って、 <xref:System.Runtime.Serialization.ISerializable> クラスによってシリアル化できます。

<xref:System.Runtime.Serialization.NetDataContractSerializer> クラスを使用している場合でも、カスタマイズされたコレクション型、コレクション インターフェイス、および配列は、コレクションとして扱われます。

## <a name="collections-and-schema"></a>コレクションとスキーマ

すべての等価のコレクションは、XML スキーマ定義言語 (XSD: XML Schema Definition Language) スキーマで同様に表現されます。 このため、生成されたクライアント コードでもサーバーと同じコレクション型になることは通常ありません。 たとえば、サーバーで Integer データ メンバーのジェネリック <xref:System.Collections.Generic.List%601> を含むデータ コントラクトを使用していても、生成されたクライアント コードでは、この同じデータ メンバーが整数の配列になることがあります。

ディクショナリ コレクションは、それがディクショナリであることを示す WCF 固有のスキーマ注釈でマークされます。そうしないと、キーと値を持つエントリが含まれた単純なリストと区別できなくなります。 データ コントラクト スキーマでのコレクションの表現方法の正確な記述については、「 [Data Contract Schema Reference](data-contract-schema-reference.md)」を参照してください。

既定では、コードをインポートする際に、カスタマイズされていないコレクションの型は生成されません。 リスト コレクション型のデータ メンバーは配列としてインポートされ、ディクショナリ コレクション型のデータ メンバーはジェネリック ディクショナリとしてインポートされます。

ただし、カスタマイズされたコレクションの場合、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性でマークされた個別の型が生成されます (スキーマにおけるカスタマイズされたコレクション型とは、既定の名前空間、名前、反復される要素名、またはキーと値の要素名を使用しない型のことです)。これらの型は、リスト型のジェネリック <xref:System.Collections.Generic.List%601> およびディクショナリ型のジェネリック ディクショナリから派生した空の型です。

たとえば、サーバーで次のような型を使用するとします。

[!code-csharp[c_collection_types_in_data_contracts#7](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#7)]
[!code-vb[c_collection_types_in_data_contracts#7](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#7)]

スキーマをエクスポートし、再度インポートすると、生成されるクライアント コードは次のようになります (わかりやすくするために、プロパティの代わりにフィールドを示しています)。

[!code-csharp[c_collection_types_in_data_contracts#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#8)]
[!code-vb[c_collection_types_in_data_contracts#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#8)]

生成されるコードで、既定の型と異なる型を使用することが必要になる場合があります。 たとえば、データ メンバーをユーザー インターフェイス コンポーネントにバインドしやすくするために、データ メンバーの通常の配列ではなく、ジェネリック <xref:System.ComponentModel.BindingList%601> を使用することがあります。

生成するコレクション型を選択するには、スキーマをインポートするときに、使用するコレクション型のリストを <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A> オブジェクトの <xref:System.Runtime.Serialization.ImportOptions> プロパティに渡します。 これらの型は、 *"参照されるコレクション型"* と呼ばれます。

ジェネリック型を参照する場合は、完全なオープン ジェネリックまたは完全なクローズ ジェネリックであることが必要です。

> [!NOTE]
> Svcutil.exe ツールを使用すると、 **/collectionType** コマンド ライン スイッチ (短縮形: **/ct**) を使用して、この参照を実現できます。 **/reference** スイッチ (短縮形: **/r**) を使用して、参照されるコレクション型のアセンブリも指定する必要があることに注意してください。 型がジェネリックの場合は、型の後に逆引用符とジェネリック パラメーターの数を指定する必要があります。 逆引用符 (\`) は、単一引用符 (‘) と混同しないでください。 参照されるコレクション型を複数指定するには、 **/collectionType** スイッチを複数回使用します。

たとえば、すべてのリストをジェネリック <xref:System.Collections.Generic.List%601>としてインポートするには、次のように指定します。

```console
svcutil.exe MyService.wsdl MyServiceSchema.xsd /r:C:\full_path_to_system_dll\System.dll /ct:System.Collections.Generic.List`1
```

コレクションをインポートするときに、参照されるコレクション型のこのリストがスキャンされ、最も一致するコレクションが見つかると、そのコレクションがデータ メンバー型 (カスタマイズされていないコレクションの場合)、または派生元となる基本型 (カスタマイズされたコレクションの場合) として使用されます。 ディクショナリはディクショナリと照合され、リストはリストと照合されます。

たとえば、参照される型のリストにジェネリック <xref:System.ComponentModel.BindingList%601> と <xref:System.Collections.Hashtable> を追加した場合、前述の例で生成されるクライアント コードは次のようになります。

[!code-csharp[c_collection_types_in_data_contracts#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#9)]
[!code-vb[c_collection_types_in_data_contracts#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#9)]

参照されるコレクション型の一部として、コレクション インターフェイス型を指定できますが、無効なコレクション型 ( `Add` メソッドまたはパブリック コンストラクターを持たないコレクション型など) を指定することはできません。

クローズ ジェネリック型は、合致度が最も高いと見なされます (非ジェネリック型は、 `Object`のクローズ ジェネリックと同等に扱われます)。 たとえば、 <xref:System.Collections.Generic.List%601> のジェネリック <xref:System.DateTime>、ジェネリック <xref:System.ComponentModel.BindingList%601> (オープン ジェネリック)、および <xref:System.Collections.ArrayList> が参照されるコレクション型である場合、次のようなコードが生成されます。

[!code-csharp[c_collection_types_in_data_contracts#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#10)]
[!code-vb[c_collection_types_in_data_contracts#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#10)]

リスト コレクションの場合、以下の表内のケースだけがサポートされます。

|参照される型|参照される型で実装されるインターフェイス|例|型の処理|
|---------------------|----------------------------------------------|-------------|----------------------|
|非ジェネリックまたはクローズ ジェネリック (任意の数のパラメーター)|非ジェネリック|`MyType : IList`<br /><br /> or<br /><br /> `MyType<T> : IList`<br /><br /> ここでは、T= `int`|`Object` のクローズ ジェネリック (例 : `IList<object>`)|
|非ジェネリックまたはクローズ ジェネリック (コレクション型と必ずしも一致するわけではない任意の数のパラメーター)|クローズ ジェネリック|`MyType : IList<string>`<br /><br /> or<br /><br /> `MyType<T> : IList<string>` ここでは、T=`int`|クローズ ジェネリック (例 : `IList<string>`)|
|任意の数のパラメーターを持つクローズ ジェネリック|型のパラメーターのいずれかを使用するオープン ジェネリック|`MyType<T,U,V> : IList<U>`<br /><br /> ここでは、T=`int`、U=`string`、V=`bool`|クローズ ジェネリック (例 : `IList<string>`)|
|パラメーターを 1 つ持つオープン ジェネリック|型のパラメーターを使用するオープン ジェネリック|`MyType<T> : IList<T>`、T はオープン|オープン ジェネリック (例 : `IList<T>`)|

型が複数のリスト コレクション インターフェイスを実装している場合、以下の制限が適用されます。

- 型が、異なる型のジェネリック <xref:System.Collections.Generic.IEnumerable%601> (またはその派生インターフェイス) を複数回実装している場合、その型は有効な参照されるコレクション型とは見なされず、無視されます。 これは、一部の実装が無効であったり、オープン ジェネリックを使用していたりする場合も該当します。 たとえば、型が <xref:System.Collections.Generic.IEnumerable%601> のジェネリック `int` と、T のジェネリック <xref:System.Collections.Generic.IEnumerable%601> を実装している場合、 `int` の参照されるコレクションとしても、その他の型の参照されるコレクションとしても使用されることはありません。この場合、型が `Add` を受け入れる `int` メソッドと T 型のパラメーターを受け入れる `Add` メソッドのいずれか、または両方を持っているかどうかは関係ありません。

- 型がジェネリック コレクション インターフェイスと <xref:System.Collections.IList>を実装している場合、ジェネリック コレクション インターフェイスが <xref:System.Object>型のクローズ ジェネリックでない限り、参照されるコレクション型として使用されることはありません。

ディクショナリ コレクションの場合、以下の表内のケースだけがサポートされます。

|参照される型|参照される型で実装されるインターフェイス|例|型の処理|
|---------------------|----------------------------------------------|-------------|---------------------|
|非ジェネリックまたはクローズ ジェネリック (任意の数のパラメーター)|<xref:System.Collections.IDictionary>|`MyType : IDictionary`<br /><br /> or<br /><br /> `MyType<T> : IDictionary` ここでは、T=`int`|クローズ ジェネリック `IDictionary<object,object>`|
|クローズ ジェネリック (任意の数のパラメーター)|<xref:System.Collections.Generic.IDictionary%602>、クローズ|`MyType<T> : IDictionary<string, bool>` ここでは、T=`int`|クローズ ジェネリック (例 : `IDIctionary<string,bool>`)|
|クローズ ジェネリック (任意の数のパラメーター)|ジェネリック <xref:System.Collections.Generic.IDictionary%602>、キーまたは値の一方がクローズ。もう一方はオープンで、型のパラメーターのいずれかを使用|`MyType<T,U,V> : IDictionary<string,V>` ここでは、T=`int`、U=`float`、V=`bool`<br /><br /> or<br /><br /> `MyType<Z> : IDictionary<Z,bool>` ここでは、Z=`string`|クローズ ジェネリック (例 : `IDictionary<string,bool>`)|
|クローズ ジェネリック (任意の数のパラメーター)|ジェネリック <xref:System.Collections.Generic.IDictionary%602>、キーと値の両方がオープンであり、それぞれ型のパラメーターのいずれかを使用|`MyType<T,U,V> : IDictionary<V,U>` ここでは、T=`int`、U=`bool`、V=`string`|クローズ ジェネリック (例 : `IDictionary<string,bool>`)|
|オープン ジェネリック (2 つのパラメーター)|ジェネリック <xref:System.Collections.Generic.IDictionary%602>、オープン、型のジェネリック パラメーターの両方をその出現順に使用|`MyType<K,V> : IDictionary<K,V>`、K と V は共にオープン|オープン ジェネリック (例 : `IDictionary<K,V>`)|

型が <xref:System.Collections.IDictionary> とジェネリック <xref:System.Collections.Generic.IDictionary%602>の両方を実装している場合、ジェネリック <xref:System.Collections.Generic.IDictionary%602> だけが考慮されます。

部分的なジェネリック型の参照はサポートされていません。

重複は許可されていません。たとえば、 <xref:System.Collections.Generic.List%601> のジェネリック `Integer` と、 `Integer` のジェネリック コレクションの両方を <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A>に追加することはできません。これは、スキーマで整数のリストが見つかったときに、どちらを使用するかを判断できなくなるためです。 もっとも、このような重複が検出されるのは、それが問題になるような型がスキーマ中にある場合だけです。 たとえば、インポートしているスキーマ中に整数のリストがない場合に、 <xref:System.Collections.Generic.List%601> のジェネリック `Integer` と `Integer` のジェネリック コレクションの両方が <xref:System.Runtime.Serialization.ImportOptions.ReferencedCollectionTypes%2A>にあっても問題はありませんが、いずれも使用されません。

## <a name="advanced-collection-rules"></a>コレクションの高度な規則

### <a name="serializing-collections"></a>コレクションのシリアル化

コレクションのシリアル化規則を以下に示します。

- コレクション型は、組み合わせる (コレクションのコレクションを持つ) ことができます。 ジャグ配列は、コレクションのコレクションとして扱われます。 多次元配列はサポートされていません。

- バイト配列と <xref:System.Xml.XmlNode> の配列は、コレクションではなく、プリミティブとして扱われる特殊な配列の型です。 バイト配列をシリアル化すると、バイトごとの個別の要素ではなく、Base64 でエンコードされたデータのチャンクを含む単一の XML 要素が生成されます。 <xref:System.Xml.XmlNode> の配列の処理方法の詳細については、「[データ コントラクトの XML および ADO.NET の種類](xml-and-ado-net-types-in-data-contracts.md)」を参照してください。 これらの特殊な型もコレクションに参加できます。バイト配列の配列の場合、複数の XML 要素が生成され、各要素に Base64 でエンコードされたデータのチャンクが含まれます。

- <xref:System.Runtime.Serialization.DataContractAttribute> 属性をコレクション型に適用した場合、型はコレクションとしてではなく、通常のデータ コントラクト型として扱われるようになります。

- コレクション型が <xref:System.Xml.Serialization.IXmlSerializable> インターフェイスを実装している場合、次の規則が適用されます。 `myType:IList<string>, IXmlSerializable`型と仮定します。

  - 宣言型が `IList<string>`の場合、この型はリストとしてシリアル化されます。

  - 宣言型が `myType`の場合、この型は `IXmlSerializable`としてシリアル化されます。

  - 宣言型が `IXmlSerializable`の場合は、既知の型のリストに `IXmlSerializable`を追加しているときにのみ、この型が `myType` としてシリアル化されます。

- コレクションは、以下の表に示すメソッドを使用してシリアル化および逆シリアル化されます。

|コレクション型の実装|シリアル化で呼び出されるメソッド|逆シリアル化で呼び出されるメソッド|
|--------------------------------|-----------------------------------------|-------------------------------------------|
|ジェネリック <xref:System.Collections.Generic.IDictionary%602>|`get_Keys`, `get_Values`|ジェネリック Add|
|<xref:System.Collections.IDictionary>|`get_Keys`, `get_Values`|`Add`|
|ジェネリック <xref:System.Collections.Generic.IList%601>|ジェネリック <xref:System.Collections.Generic.IList%601> インデクサー|ジェネリック Add|
|ジェネリック <xref:System.Collections.Generic.ICollection%601>|列挙子|ジェネリック Add|
|<xref:System.Collections.IList>|<xref:System.Collections.IList> インデクサー|`Add`|
|ジェネリック <xref:System.Collections.Generic.IEnumerable%601>|`GetEnumerator`|適切な型 (ジェネリック パラメーターの型、またはその基本型のいずれか) のパラメーターを 1 つ受け取る `Add` という非静的メソッド。 このようなメソッドは、シリアル化と逆シリアル化の両方で、シリアライザーがコレクション型をコレクションとして処理するために必要になります。|
|<xref:System.Collections.IEnumerable> (およびこのインターフェイスから派生した <xref:System.Collections.ICollection>)|`GetEnumerator`|`Add` 型のパラメーターを 1 つ受け取る `Object`という非静的メソッド。 このようなメソッドは、シリアル化と逆シリアル化の両方で、シリアライザーがコレクション型をコレクションとして処理するために必要になります。|

前の表に示すコレクション インターフェイスは、優先度の高いものから順に並べられています。 たとえば、型が <xref:System.Collections.IList> とジェネリック <xref:System.Collections.Generic.IEnumerable%601>の両方を実装している場合、 <xref:System.Collections.IList> の規則に従って、コレクションがシリアル化および逆シリアル化されます。

- 逆シリアル化では、すべてのコレクションが、まず、パラメーターなしのコンストラクターを呼び出して型のインスタンスを作成することによって逆シリアル化されます。パラメーターなしのコンストラクターは、シリアル化と逆シリアル化の両方で、シリアライザーがコレクション型をコレクションとして処理するために必要となります。

- 同じジェネリック コレクション インターフェイスが複数回実装されており (型が <xref:System.Collections.Generic.ICollection%601> のジェネリック `Integer` と、 <xref:System.Collections.Generic.ICollection%601> のジェネリック <xref:System.String>の両方を実装している場合など)、優先度がより高いインターフェイスが見つからない場合は、コレクションが有効なコレクションとして扱われません。

- コレクション型には <xref:System.SerializableAttribute> 属性を適用でき、 <xref:System.Runtime.Serialization.ISerializable> インターフェイスを実装できます。 これらはいずれも無視されます。 ただし、型がコレクション型の要件を完全に満たしていない場合 ( `Add` メソッドがないなど)、その型はコレクション型と見なされないため、 <xref:System.SerializableAttribute> 属性と <xref:System.Runtime.Serialization.ISerializable> インターフェイスを使用して、型をシリアル化できるかどうかが判断されます。

- コレクションをカスタマイズするために、コレクションに <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用した場合、上記の <xref:System.SerializableAttribute> 代替機構は除去されます。 カスタマイズされたコレクションがコレクション型の要件を満たしていない場合には、代わりに <xref:System.Runtime.Serialization.InvalidDataContractException> 例外がスローされます。 多くの場合、この例外文字列には、指定された型が有効なコレクションと見なされない理由を説明する情報 (`Add` メソッドがない、パラメーターなしのコンストラクターがないなど) が含まれるため、<xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用することは、しばしばデバッグに役立ちます。

### <a name="collection-naming"></a>コレクションの名前付け

コレクションの名前付け規則を以下に示します。

- Namespace を使用してオーバーライドしていない場合、すべてのディクショナリ コレクション データ コントラクト、およびプリミティブ型を含むリスト コレクション データ コントラクトの既定の名前空間は、`http://schemas.microsoft.com/2003/10/Serialization/Arrays` です。 このために、組み込みの XSD 型に割り当てられた型と、 `char`、 `Timespan`、および `Guid` の各型がプリミティブと見なされます。

- Namespace を使用してオーバーライドしていない場合、プリミティブ型以外の型を含むコレクション型の既定の名前空間は、コレクションに含まれる型のデータ コントラクト名前空間と同じです。

- Name を使用してオーバーライドしていない場合、リスト コレクション データ コントラクトの既定の名前は、文字列 "ArrayOf" とコレクションに含まれる型のデータ コントラクト名を組み合わせたものです。 たとえば、Integers のジェネリック List のデータ コントラクト名は "ArrayOfint" です。 `Object` のデータ コントラクト名は "anyType" であることに留意してください。したがって、 <xref:System.Collections.ArrayList> のような非ジェネリック リストのデータ コントラクト名は、"ArrayOfanyType" になります。

`Name`を使用してオーバーライドしていない場合、ディクショナリ コレクション データ コントラクトの既定の名前は、文字列 "ArrayOfKeyValueOf"、キーの型のデータ コントラクト名、および値の型のデータ コントラクト名をこの順番で組み合わせたものです。 たとえば、String と Integer のジェネリック ディクショナリのデータ コントラクト名は、"ArrayOfKeyValueOfstringint" になります。 また、キーの型と値の型がいずれもプリミティブ型ではない場合は、キーおよび値の型のデータ コントラクト名前空間の名前空間ハッシュが名前に付加されます。 名前空間ハッシュの詳細については、「[データ コントラクト名](data-contract-names.md)」を参照してください。

各ディクショナリ コレクション データ コントラクトには、ディクショナリの 1 エントリを表すコンパニオン データ コントラクトが存在します。 コンパニオン データ コントラクトの名前は、"ArrayOf" プレフィックスを除いたディクショナリ データ コントラクトの名前と同じです。また、名前空間は、ディクショナリ データ コントラクトの名前空間と同じです。 たとえば、"ArrayOfKeyValueOfstringint" ディクショナリ データ コントラクトの場合、"KeyValueofstringint" データ コントラクトがディクショナリの 1 エントリを表します。 次のセクションで説明するように、このデータ コントラクトの名前は、 `ItemName` プロパティを使用してカスタマイズできます。

「 [Data Contract Names](data-contract-names.md)」に記載されたジェネリック型の名前付け規則は、コレクション型にすべて適用されます。つまり、Name 内の中かっこを使用してジェネリック型パラメーターを示すことができます。 ただし、かっこ内の数値は、コレクションに含まれる型ではなく、ジェネリック パラメーターを指すことに注意してください。

## <a name="collection-customization"></a>コレクションのカスタマイズ

<xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を次のように使用することは禁止されています。次のように使用すると、 <xref:System.Runtime.Serialization.InvalidDataContractException> 例外が発生します。

- <xref:System.Runtime.Serialization.DataContractAttribute> 属性が既に適用されている型、またはその派生型のいずれかに <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用する。

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute> インターフェイスを実装する型に <xref:System.Xml.Serialization.IXmlSerializable> 属性を適用する。

- コレクション型以外の型に <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を適用する。

- ディクショナリ型以外の型に適用された <xref:System.Runtime.Serialization.CollectionDataContractAttribute.KeyName%2A> 属性で、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute.ValueName%2A> または <xref:System.Runtime.Serialization.CollectionDataContractAttribute> の設定を試みる。

### <a name="polymorphism-rules"></a>ポリモーフィズム規則

既に説明したように、 <xref:System.Runtime.Serialization.CollectionDataContractAttribute> 属性を使用してコレクションをカスタマイズすると、コレクションの可換性を妨げることがあります。 カスタマイズされた 2 つのコレクション型を等価と見なすことができるのは、それらの名前、名前空間、項目名、およびキーと値の名前 (ディクショナリ コレクションの場合) が一致する場合だけです。

カスタマイズが原因で、あるコレクション データ コントラクトを使用する必要があるところに、誤って別のコレクション データ コントラクトが使用される可能性があります。 このような状況は回避する必要があります。 次の型を参照してください。

[!code-csharp[c_collection_types_in_data_contracts#11](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_collection_types_in_data_contracts/cs/program.cs#11)]
[!code-vb[c_collection_types_in_data_contracts#11](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_collection_types_in_data_contracts/vb/program.vb#11)]

この場合、 `Marks1` のインスタンスは `testMarks`に割り当てることができます。 ただし、 `Marks2` は使用しないようにする必要があります。そのデータ コントラクトは、 `IList<int>` データ コントラクトと等価とは見なされないからです。 このデータ コントラクト名は、"ArrayOfint" ではなく "Marks2" であり、反復される要素名は、"\<int>" ではなく "\<mark>" になります。

以下の表に、コレクションのポリモーフィックな割り当てに適用される規則を示します。

|宣言された型|カスタマイズされていないコレクションの割り当て|カスタマイズされたコレクションの割り当て|
|-------------------|--------------------------------------------|---------------------------------------|
|Object|コントラクト名はシリアル化されます。|コントラクト名はシリアル化されます。<br /><br /> カスタマイズが使用されます。|
|コレクション インターフェイス|コントラクト名はシリアル化されません。|コントラクト名はシリアル化されません。<br /><br /> カスタマイズは使用されません。\*|
|カスタマイズされていないコレクション|コントラクト名はシリアル化されません。|コントラクト名はシリアル化されます。<br /><br /> カスタマイズが使用されます**。|
|カスタマイズされたコレクション|コントラクト名はシリアル化されます。 カスタマイズは使用されません。\*\*|コントラクト名はシリアル化されます。<br /><br /> 割り当てられた型のカスタマイズが使用されます。\*\*|

\* <xref:System.Runtime.Serialization.NetDataContractSerializer> クラスでは、このケースでカスタマイズが使用されます。 また、 <xref:System.Runtime.Serialization.NetDataContractSerializer> クラスは、このケースで実際の型名もシリアル化するため、逆シリアル化も予期されたとおりに機能します。

\*\* これらのケースでは、スキーマが無効なインスタンスが生成されます。したがって、これらのケースは避けるようにしてください。

コントラクト名をシリアル化する場合、割り当てるコレクション型が既知の型リストに含まれている必要があります。 逆に言うと、名前をシリアル化しない場合は、既知の型リストに型を追加する必要はありません。

派生型の配列は、基本型の配列に代入できます。 この場合、反復される各要素について、派生型のコントラクト名がシリアル化されます。 たとえば、 `Book` 型が `LibraryItem`型から派生している場合、 `Book` の配列を `LibraryItem`の配列に代入できます。 これは、他のコレクション型には適用されません。 たとえば、 `Generic List of Book` を `Generic List of LibraryItem`に代入することはできません。 ただし、 `Generic List of LibraryItem` インスタンスを含む `Book` を代入することはできます。 配列の場合も、配列ではない場合も、 `Book` は既知の型リストに含まれている必要があります。

## <a name="collections-and-object-reference-preservation"></a>コレクションとオブジェクト参照の保存

シリアライザーがオブジェクト参照を保存するモードで機能している場合、オブジェクト参照の保存はコレクションにも適用されます。 具体的には、コレクション全体と、コレクションに含まれる個々の項目の両方のオブジェクト ID が保存されます。 ディクショナリの場合、キーと値のペア オブジェクトと、個々のキー オブジェクトおよび値オブジェクトのオブジェクト ID が保存されます。

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.Serialization.CollectionDataContractAttribute>
