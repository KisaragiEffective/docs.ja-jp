---
title: 言語への非依存性、および言語非依存コンポーネント
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- language interoperability
- Common Language Specification
- cross-language interoperability
- CLS
- runtime, language interoperability
- common language runtime, language interoperability
ms.assetid: 4f0b77d0-4844-464f-af73-6e06bedeafc6
ms.openlocfilehash: 689ca9f7278dcf91b12bc62b5255a968388bb9f8
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398785"
---
# <a name="language-independence-and-language-independent-components"></a>言語への非依存性、および言語非依存コンポーネント

.NET Framework は言語に依存しません。 つまり、C++/CLI、Eiffel、F#、IronPython、IronRuby、PowerBuilder、Visual Basic、Visual COBOL、Windows PowerShell など、.NET Framework を対象とする多くの言語の 1 つを開発に使用できます。 .NET Framework 用に開発されたクラス ライブラリの型とメンバーには、最初に記述された言語を知らなくてもアクセスできます。元の言語の規則に従う必要もありません。 コンポーネントを開発しているのであれば、コンポーネントの言語にかかわらず、すべての .NET Framework アプリからそのコンポーネントにアクセスできます。

> [!NOTE]
> この記事の最初の部分では、言語に依存しないコンポーネント、つまり、どの言語で記述されたアプリからでも使用できるコンポーネントの作成について説明します。 また、複数の言語で記述されたソース コードから 1 つのコンポーネントまたはアプリを作成することもできます。この記事の 2 番目のパートにある「[言語間の相互運用性](#CrossLang)」を参照してください。

任意の言語で記述された他のオブジェクトと完全に対話するには、すべての言語に共通の機能だけを呼び出し側に公開するようにオブジェクトを実装する必要があります。 この共通の機能セットは、生成されたアセンブリに適用される規則のセットである、共通言語仕様 (CLS: Common Language Specification) によって定義されます。 共通言語仕様は、「[Standard ECMA-335: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) の第 1 部の第 7 ～ 11 項で定義されています。

コンポーネントが共通言語仕様に準拠している場合は、CLS に準拠することが保証され、CLS をサポートするすべてのプログラミング言語で記述されたアセンブリのコードからアクセスできます。 コンパイル時にコンポーネントが共通言語仕様に準拠しているかどうかを確認するには、<xref:System.CLSCompliantAttribute> 属性をソース コードに適用します。 詳細については、「[CLSCompliantAttribute 属性](#CLSAttribute)」を参照してください。

この記事の内容:

- [CLS 準拠の規則](#Rules)

  - [型および型メンバーのシグネチャ](#Types)

  - [名前付け規則](#naming)

  - [型変換](#conversion)

  - [配列](#arrays)

  - [インターフェイス](#Interfaces)

  - [列挙体](#enums)

  - [一般的な型メンバー](#members)

  - [メンバーのアクセシビリティ](#MemberAccess)

  - [ジェネリック型とメンバー](#Generics)

  - [コンストラクター](#ctors)

  - [Properties](#properties)

  - [イベント](#events)

  - [オーバーロード](#overloads)

  - [例外](#exceptions)

  - [属性](#attributes)

- [CLSCompliantAttribute 属性](#CLSAttribute)

- [言語間の相互運用性](#CrossLang)

<a name="Rules"></a>

## <a name="cls-compliance-rules"></a>CLS 準拠の規則

ここでは、CLS に準拠したコンポーネントを作成するための規則について説明します。 規則の一覧については、「[ECMA-335 Standard: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) の第 1 部の第 11 項を参照してください。

> [!NOTE]
> 共通言語仕様では、コンシューマー (プログラムによって CLS 準拠のコンポーネントにアクセスする開発者)、フレームワーク (言語コンパイラを使用して CLS 準拠のライブラリを作成する開発者)、およびエクステンダー (CLS 準拠のコンポーネントを作成する言語コンパイラ、コード パーサーなどのツールを作成する開発者) に適用する、CLS 準拠の各規則について説明します。 ここでは、フレームワークに適用するときの規則に焦点を当てます。 エクステンダーに適用する一部の規則は、Reflection.Emit を使用して作成されたアセンブリに適用されることもあります。

言語に依存しないコンポーネントをデザインするには、コンポーネントのパブリック インターフェイスに CLS 準拠の規則を適用するだけです。 プライベートな実装は仕様に準拠する必要はありません。

> [!IMPORTANT]
> CLS 準拠の規則は、コンポーネントのパブリック インターフェイスにのみ適用されます。プライベート実装には適用されません。

たとえば、<xref:System.Byte> 以外の符号なし整数は CLS に準拠していません。 次の例の `Person` クラスは型 `Age` の <xref:System.UInt16> プロパティを公開するので、次のコードではコンパイラの警告が表示されます。

[!code-csharp[Conceptual.CLSCompliant#1](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/public1.cs#1)]
[!code-vb[Conceptual.CLSCompliant#1](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/public1.vb#1)]

`Person` クラスを CLS 準拠にするには、`Age` プロパティの型を <xref:System.UInt16> から、CLS 準拠の 16 ビット符号付き整数である <xref:System.Int16> に変更します。 プライベート `personAge` フィールドの型を変更する必要はありません。

[!code-csharp[Conceptual.CLSCompliant#2](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/public2.cs#2)]
[!code-vb[Conceptual.CLSCompliant#2](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/public2.vb#2)]

ライブラリのパブリック インターフェイスは、次の要素で構成されます。

- パブリック クラスの定義。

- パブリック クラスのパブリック メンバーの定義、および派生クラスからアクセスできるメンバー (つまり、protected メンバー) の定義。

- パブリック クラスのパブリック メソッドのパラメーターおよび戻り値の型、派生クラスからアクセスできるメソッドのパラメーターおよび戻り値の型。

CLS 準拠の規則を次の表に示します。 この規則は、「[ECMA-335 Standard: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) からの引用で、Ecma International が 2012 年の著作権を保有しています。 これらの規則の詳細については、以降のセクションを参照してください。

|カテゴリ|参照先|規則|規則番号|
|--------------|---------|----------|-----------------|
|アクセシビリティ|[メンバーのアクセシビリティ](#MemberAccess)|継承されたメソッドをオーバーライドする場合、アクセシビリティは変更してはいけない。ただし、別のアセンブリから継承されたメソッドをアクセシビリティ `family-or-assembly` でオーバーライドする場合は除く。 この場合、オーバーライドのアクセシビリティは `family` にすること。|10|
|アクセシビリティ|[メンバーのアクセシビリティ](#MemberAccess)|型およびメンバーの可視性およびアクセシビリティについて、メンバーのシグネチャに指定されている型は、そのメンバーが可視でアクセス可能な場合、必ず可視でアクセス可能でなければいけない。 たとえば、アセンブリ外部から参照できるパブリックなメソッドには、アセンブリ内部でだけ可視である型が引数として含まれていてはいけない。 メンバーのシグネチャに使用されているジェネリック型のインスタンスを構成する型の可視性およびアクセシビリティは、メンバーが可視でアクセス可能の場合、必ず可視でアクセス可能でなければいけない。 たとえば、アセンブリ外部から参照できるメンバーのシグネチャに指定されているジェネリック型のインスタンスに、アセンブリ内部でだけ可視である型の汎用引数が含まれていてはいけない。|12|
|配列|[配列](#arrays)|配列は、要素が CLS 準拠型で、すべての次元でインデックス番号が 0 から始まらなければならない。 項目が配列の場合、オーバーロードどうしを区別するには配列要素の型を必要とする。 オーバーロードが 2 つ以上の配列型に基づく場合、要素型は名前付きの型でなければいけない。|16|
|属性|[属性](#attributes)|属性の型 <xref:System.Attribute?displayProperty=nameWithType>、またはそれを継承する型である。|41|
|属性|[属性](#attributes)|CLS ではカスタム属性のエンコーディングのサブセットのみ使用できる。 これらのエンコーディングに表示される型 (第 4 部を参照) は、<xref:System.Type?displayProperty=nameWithType>、<xref:System.String?displayProperty=nameWithType>、<xref:System.Char?displayProperty=nameWithType>、<xref:System.Boolean?displayProperty=nameWithType>、<xref:System.Byte?displayProperty=nameWithType>、<xref:System.Int16?displayProperty=nameWithType>、<xref:System.Int32?displayProperty=nameWithType>、<xref:System.Int64?displayProperty=nameWithType>、<xref:System.Single?displayProperty=nameWithType>、<xref:System.Double?displayProperty=nameWithType>、CLS 準拠の基底の整数型に基づく列挙型のみである。|34|
|属性|[属性](#attributes)|CLS では、公開参照される必須の修飾子 (`modreq`、第 2 部を参照) は使用できない。ただし、認識しないオプションの修飾子 (`modopt`、第 2 部を参照) は使用できる。|35|
|コンストラクター|[コンストラクター](#ctors)|オブジェクト コンストラクターでは、継承しているインスタンス データへのアクセスが発生する前に、基底クラスのインスタンス コンストラクターを呼び出さなければいけない (コンストラクターが不要である値型は除く)。|21|
|コンストラクター|[コンストラクター](#ctors)|オブジェクト コンストラクターがオブジェクトの作成時以外で呼び出されてはならず、またオブジェクトが 2 度初期化されてもいけない。|22|
|列挙体|[列挙体](#enums)|enum の基になる型は組み込みの CLS 整数型、フィールド名は "value__" であり、そのフィールドには `RTSpecialName` のマークが付けられる。|7|
|列挙体|[列挙体](#enums)|enum には 2 種類あり、<xref:System.FlagsAttribute?displayProperty=nameWithType> カスタム属性 (第 4 部のライブラリを参照) の有無で区別する。 片方は名前付き整数値を表し、もう片方は名前付きビット フラグを表す。名前付きビット フラグは、それを組み合わせて名前のない値を生成できる。 `enum` の値は、指定した値に限定されない。|8|
|列挙体|[列挙体](#enums)|enum のリテラルな静的フィールドの型は、その enum 自体の型である。|9|
|events|[イベント](#events)|イベントを実装するメソッドは、メタデータ内で `SpecialName` のマークが付けられる。|29|
|events|[イベント](#events)|イベントとイベントのアクセサーのアクセシビリティは同一である。|30|
|events|[イベント](#events)|イベントの `add` メソッドおよび `remove` メソッドは、どちらもあってもなくてもよい。|31|
|events|[イベント](#events)|`add` メソッドおよび `remove` メソッドは、それぞれパラメーターを 1 つ使用する。このパラメーターの型がイベントの型を規定する。また、パラメーターの型は <xref:System.Delegate?displayProperty=nameWithType> の派生でなければいけない。|32|
|events|[イベント](#events)|イベントは、特定の名前付けパターンに従わなくてはいけない。 CLS 規則 29 で触れられている `SpecialName` 属性は、適切な名前比較で無視され、識別子規則に従わなければいけない。|33|
|例外|[例外](#exceptions)|スローできるオブジェクト型は、<xref:System.Exception?displayProperty=nameWithType>、またはそれを継承する型である。 ただし、CLS 準拠のメソッドで他の型の例外のスローをブロックする必要はない。|40|
|全般|[CLS 準拠: 規則](#Rules)|CLS 規則は、型の構成部分のうち、その型を定義しているアセンブリの外部からアクセスまたは参照できる部分にのみ適用される。|1|
|全般|[CLS 準拠: 規則](#Rules)|CLS 非準拠型のメンバーを CLS 準拠と指定しない。|2|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|入れ子になった型は、少なくともその外側の型と同じ数のジェネリック パラメーターを持つ。 入れ子にされた型のジェネリック パラメーターは、それを囲む型のジェネリック パラメーターと、位置によって対応します。|42|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|ジェネリック型の名前は、入れ子にされない型で宣言される型パラメーターの数をエンコードする必要がある。入れ子にされる場合は、上記の規則に従って、型に新しく組み込まれる型パラメーターの数をエンコードする必要がある。|43|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|ジェネリック型は必要な制約を再宣言して、基本型またはインターフェイスの制約がジェネリック型の制約で確実に満たされるようにする必要がある。|4444|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|ジェネリック パラメーターの制約として使用される型は CLS に準拠する必要がある。|45|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|インスタンス化されたジェネリック型のメンバー (入れ子になった型も含む) の可視性およびアクセシビリティは、ジェネリック型の全体の宣言ではなく、特定のインスタンス化に対してスコープが設定される必要がある。 この場合でも、CLS 規則 12 の可視性規則とアクセシビリティ規則は同様に適用される。|46|
|ジェネリック|[ジェネリック型とメンバー](#Generics)|抽象メソッドまたは仮想ジェネリック メソッドごとに、既定の具体的な実装がある。|47|
|インターフェイス|[インターフェイス](#Interfaces)|CLS 準拠のインターフェイスでは、CLS に準拠しないメソッドを実装するために、これらを定義する必要はない。|18|
|インターフェイス|[インターフェイス](#Interfaces)|CLS 準拠インターフェイスでは、静的メソッドを定義してはいけない。また、フィールドも定義してはいけない。|19|
|メンバー|[一般的な型メンバー](#members)|グローバルで静的な (static) フィールドとメソッドは CLS 準拠ではありません。|36|
|メンバー|--|静的リテラル値の指定には、フィールド初期化メタデータを使用する。 CLS 準拠のリテラルには、そのリテラルと同じ型 (または、そのリテラルが `enum` の場合は基本型) の値がフィールド初期化メタデータに指定されている必要がある。|13|
|メンバー|[一般的な型メンバー](#members)|vararg 制約は CLS の一部ではなく、CLS でサポートする呼び出し規則だけが標準のマネージド呼び出し規則である。|15|
|名前付け規則|[名前付け規則](#naming)|アセンブリは、識別子の頭文字および構成文字として使用できる文字セットを規定する Unicode Standard 3.0 の『Technical Report 15』の「Annex 7」に従う必要がある。詳細については、「<https://www.unicode.org/unicode/reports/tr15/tr15-18.html>」を参照。 識別子は、Unicode 正規形 C に定義されている標準形式で記述する必要がある。CLS で 2 つの識別子が同じと見なされるのは、小文字マッピング (Unicode のロケール非依存で 1 対 1 の小文字による対応付け) が同じ場合である。 つまり、CLS で 2 つの識別子が異なる場合、大文字小文字の違いだけではない。 ただし、継承された定義をオーバーライドする場合、CLI では元の宣言と厳密に同じエンコーディングの使用が求められる。|4|
|オーバーロード|[名前付け規則](#naming)|CLS 準拠のスコープに導入されるすべての名前は、完全に独立した種類である必要があります。ただし、名前が同じでオーバーロードによって解決できる場合を除きます。 CTS では 1 つの型でメソッドとフィールドとに同じ名前を使用できるが、CLS では使用できない。|5|
|オーバーロード|[名前付け規則](#naming)|フィールドおよび入れ子になった型について、CTS ではシグネチャでの区別が可能だが、CLS では識別子の比較だけで区別できる必要がある。 CLS 規則 39 の指定を除き、識別子の比較により名前が同じであるメソッド、プロパティ、およびイベントでは、相違点は戻り値の型だけに限定されない。|6|
|オーバーロード|[オーバーロード](#overloads)|プロパティおよびメソッドのみオーバーロードできる。|37|
|オーバーロード|[オーバーロード](#overloads)|プロパティおよびメソッドは、パラメーターの数値と型にのみ基づいてオーバーロードできる。ただし、戻り値の型に基づいてオーバーロードできる変換演算子の `op_Implicit` と o`op_Explicit` は例外である。|38|
|オーバーロード|--|型で宣言された複数の CLS 準拠のメソッドに同じ名前が指定されている場合、特定の一連の型のインスタンス化において、これらのメソッドのパラメーターと戻り値の型は同じである。また、これらの型のインスタンス化で、すべてのメソッドをセマンティクス レベルで等価にする必要がある。|48|
|型|[型および型メンバーのシグネチャ](#Types)|<xref:System.Object?displayProperty=nameWithType> は CLS 準拠である。 これ以外のあらゆる CLS 準拠クラスは CLS 準拠クラスの継承でなければならない。|23|
|Properties|[Properties](#properties)|プロパティの getter メソッドおよび setter メソッドを実装するメソッドは、メタデータで `SpecialName` とマークする。|24|
|Properties|[Properties](#properties)|プロパティ アクセサーはすべて静的、すべて仮想、またはすべてインスタンスになる。|26|
|Properties|[Properties](#properties)|プロパティの型は、getter の戻り値の型であり、かつ setter の最後の引数の型でなければいけない。 プロパティのパラメーターの型は、getter へのパラメーターの型であり、かつ setter の最後のパラメーター以外のすべての型でなければいけない。 すべての型は CLS 準拠でなければならない。また、マネージド ポインターであってはいけない。つまり、参照渡しではいけない。|27|
|Properties|[Properties](#properties)|プロパティは、特定の名前付けパターンに従わなくてはいけない。 CLS 規則 24 で触れられている `SpecialName` 属性は、適切な名前比較で無視され、識別子規則に従わなければいけない。 プロパティには getter メソッド、setter メソッド、またはこの両方が必ずなければいけない。|28|
|型変換|[型変換](#conversion)|`op_Implicit` または `op_Explicit` が指定されている場合は、強制変換のための別の方法を用意する必要がある。|39|
|型|[型および型メンバーのシグネチャ](#Types)|ボックス化された値型は CLS 準拠ではない。|3|
|型|[型および型メンバーのシグネチャ](#Types)|シグネチャに出現するすべての型は CLS 準拠でなければいけない。 ジェネリック型のインスタンスを構成するすべての型は CLS 準拠でなければいけない。|11|
|型|[型および型メンバーのシグネチャ](#Types)|型指定された参照は CLS 準拠ではありません。|14|
|型|[型および型メンバーのシグネチャ](#Types)|アンマネージ ポインター型は CLS 準拠ではない。|17|
|型|[型および型メンバーのシグネチャ](#Types)|CLS 準拠のクラス、値型、およびインターフェイスでは、CLS に準拠しないメンバーの実装は不要である。|20|

<a name="Types"></a>

### <a name="types-and-type-member-signatures"></a>型および型メンバーのシグネチャ

<xref:System.Object?displayProperty=nameWithType> 型は CLS に準拠しており、.NET Framework 型システムのすべてのオブジェクト型の基本型です。 .NET Framework の継承は暗黙的また明示的に行われます。たとえば、<xref:System.String> クラスは <xref:System.Object> クラスから暗黙的に継承します。また、<xref:System.Globalization.CultureNotFoundException> クラスは、<xref:System.ArgumentException> クラスから明示的に継承し、これは <xref:System.SystemException> クラスから明示的に継承します。そして、このクラスは <xref:System.Exception> クラスから明示的に継承します。 派生型を CLS 準拠にするには、その基本型も CLS に準拠している必要があります。

次の例は、基本型が CLS に準拠していない派生型を示しています。 これは、符号なし 32 ビット整数をカウンターとして使用する `Counter` 基底クラスを定義します。 クラスには、符号なし整数をラップすることでカウンター機能が用意されます。このため、クラスは CLS 非準拠としてマークされます。 結果として、派生クラス `NonZeroCounter` も CLS に準拠しなくなります。

[!code-csharp[Conceptual.CLSCompliant#12](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type3.cs#12)]
[!code-vb[Conceptual.CLSCompliant#12](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type3.vb#12)]

メソッドの戻り値の型、プロパティ型を含め、メンバー シグネチャに表示されるすべての型が CLS に準拠する必要があります。 さらに、ジェネリック型の場合は、次の要件もあります。

- ジェネリック型のインスタンスを構成するすべての型が、CLS に準拠する必要があります。

- ジェネリック パラメーターで制約として使用されるすべての型が、CLS に準拠する必要があります。

.NET Framework の[共通型システム](../../docs/standard/base-types/common-type-system.md)には、共通言語ランタイムが直接サポートする組み込み型がいくつか含まれ、アセンブリのメタデータで特別にエンコードされています。 これらの組み込み型のうち、次の表に示す型は CLS に準拠しています。

|CLS 準拠型|[説明]|
|-------------------------|-----------------|
|<xref:System.Byte>|8 ビット符号なし整数|
|<xref:System.Int16>|16 ビット符号付き整数|
|<xref:System.Int32>|32 ビット符号付き整数|
|<xref:System.Int64>|64 ビット符号付き整数|
|<xref:System.Single>|単精度浮動小数点数値|
|<xref:System.Double>|倍精度浮動小数点数値|
|<xref:System.Boolean>|`true` or `false` 値型|
|<xref:System.Char>|UTF-16 でエンコードされたコード単位|
|<xref:System.Decimal>|10 進浮動小数点数以外|
|<xref:System.IntPtr>|プラットフォーム定義サイズのポインターまたはハンドル|
|<xref:System.String>|0 個以上の <xref:System.Char> オブジェクトのコレクション|

次の表に示す組み込み型は CLS に準拠していません。

|非準拠型|[説明]|CLS に準拠する代替|
|-------------------------|-----------------|--------------------------------|
|<xref:System.SByte>|8 ビット符号付き整数データ型|<xref:System.Int16>|
|<xref:System.TypedReference>|オブジェクトとそのランタイム型へのポインター|なし|
|<xref:System.UInt16>|16 ビット符号なし整数|<xref:System.Int32>|
|<xref:System.UInt32>|32 ビット符号なし整数|<xref:System.Int64>|
|<xref:System.UInt64>|64 ビット符号なし整数|<xref:System.Int64> (オーバーフローの可能性あり)、<xref:System.Numerics.BigInteger>、または <xref:System.Double>|
|<xref:System.UIntPtr>|符号なしポインターまたはハンドル|<xref:System.IntPtr>|

.NET Framework のクラス ライブラリまたはその他のクラス ライブラリには、CLS に準拠していない他の型が含まれる場合があります。次に例を示します。

- ボックス化された値型。 次の C# コード例では、`int*` という名前の型 `Value` のパブリック プロパティを持つクラスを作成します。 `int*` はボックス化された値型であるため、コンパイラは CLS 非準拠としてフラグを設定します。

  [!code-csharp[Conceptual.CLSCompliant#26](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/box2.cs#26)]

- 型指定された参照。オブジェクトへの参照および型への参照を含む特別なコンストラクトです。 型指定された参照は、.NET Framework では <xref:System.TypedReference> クラスによって表されます。

型が CLS に準拠していない場合は、<xref:System.CLSCompliantAttribute> 値が `isCompliant` に指定された `false` 属性を適用する必要があります。 詳細については、「[CLSCompliantAttribute 属性](#CLSAttribute)」を参照してください。

次の例は、メソッド シグネチャとジェネリック型のインスタンス化で発生する CLS 準拠の問題を示しています。 ここでは、`InvoiceItem` クラスを、型 <xref:System.UInt32> のプロパティ、型 `Nullable(Of UInt32)` のプロパティ、および型 <xref:System.UInt32> と `Nullable(Of UInt32)` のパラメーターが指定されたコンストラクターで定義します。 この例をコンパイルしようとすると、4 つのコンパイラの警告が表示されます。

[!code-csharp[Conceptual.CLSCompliant#3](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type1.cs#3)]
[!code-vb[Conceptual.CLSCompliant#3](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type1.vb#3)]

コンパイラの警告が表示されないようにするには、`InvoiceItem` パブリック インターフェイスの CLS 非準拠型を準拠型に置き換えます。

[!code-csharp[Conceptual.CLSCompliant#4](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/type2.cs#4)]
[!code-vb[Conceptual.CLSCompliant#4](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/type2.vb#4)]

ここで示した特定の型以外にも、CLS に準拠していないカテゴリはいくつかあります。 たとえば、アンマネージ ポインター型やアンマネージ関数ポインター型などです。 次の例では、整数へのポインターを使用して整数の配列を作成するので、コンパイラの警告が生成されます。

[!code-csharp[Conceptual.CLSCompliant#5](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/unmanagedptr1.cs#5)]

CLS 準拠の抽象クラス (つまり、`abstract` (C# の場合) または `MustInherit` (Visual Basic の場合) とマークされたクラス) については、そのクラスのすべてのメンバーも CLS に準拠にする必要があります。

<a name="naming"></a>

### <a name="naming-conventions"></a>名前付け規則

一部のプログラミング言語は大文字と小文字が区別されるので、識別子 (名前空間、型、メンバーの名前など) は、大文字と小文字が違うだけで相違します。 2 つの識別子が同じと見なされるのは、小文字マッピングが同じ場合です。 次の C# コード例では、2 つのパブリック クラス、`Person` および `person` を定義します。 このクラスは大文字と小文字のみが異なるので、コンパイラは CLS 非準拠としてフラグを設定します。

[!code-csharp[Conceptual.CLSCompliant#16](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming1.cs#16)]

名前空間、型、メンバーの名前など、プログラミング言語の識別子は [Unicode Standard 3.0 の Technical Report 15 の Annex 7](https://www.unicode.org/reports/tr15/tr15-18.html) に準拠する必要があります。 これは、次のことを意味します。

- 識別子の最初の文字は Unicode の大文字と小文字、大文字と小文字の組み合わせ、修飾子文字、その他の文字、または文字数の番号を指定できます。 Unicode 文字のカテゴリの詳細については、「<xref:System.Globalization.UnicodeCategory?displayProperty=nameWithType> 列挙体」を参照してください。

- 2 文字目以降には、最初の文字で使用できる文字のほかに、非空白記号、空白結合記号、10 進数、接続符号、書式指定コードを使用できます。

識別子を比較する場合は、その前に書式設定コードを除外してから、識別子を Unicode 正規形 C に変換する必要があります。これは 1 つの文字を、UTF-16 でエンコードされた複数のコード単位で表すことができるからです。 同じコード単位を Unicode 正規形 C で生成する文字シーケンスは CLS に準拠していません。 次の例では、最初にオングストローム文字 (U+212B) である `Å` という名前のプロパティを定義し、次に、上に丸が付く LATIN の大文字 A (U+00C5) である `Å` という名前のプロパティを定義します。 C# と Visual Basic の両方のコンパイラが、CLS 非準拠としてソース コードにフラグを設定します。

[!code-csharp[Conceptual.CLSCompliant#17](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming1.cs#17)]
[!code-vb[Conceptual.CLSCompliant#17](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/naming1.vb#17)]

特定のスコープ内のメンバー名 (アセンブリ内の名前空間、名前空間内の型、型内のメンバーなど) は一意である必要があります。ただし、オーバーロードによって解決される名前は除きます。 この要件は、共通型システムの要件よりも厳格です。共通型システムでは、スコープ内のメンバーは種類が異なっていれば、たとえば、種類がメソッドのメンバーとフィールドのメンバーは、同じ名前を持つことができます。 特に、型メンバーの場合は次の要件もあります。

- フィールドと入れ子になった型は名前でのみ識別されます。

- 名前が同じメソッド、プロパティ、およびイベントは、戻り値の型以外で区別できるようにする必要があります。

次の例は、メンバー名がスコープ内で一意でなければならない要件を示しています。 ここでは、`Converter` という名前の 4 つのメンバーを含む `Conversion` という名前のクラスを定義します。 3 つがメソッドで、1 つはプロパティです。 <xref:System.Int64> パラメーターを含むメソッドには一意の名前が付けられますが、<xref:System.Int32> パラメーターが指定された 2 つのメソッドには一意の名前は付けられません。これは戻り値がメンバーのシグネチャの一部と見なされないからです。 また、`Conversion` プロパティもこの要件に違反しています。プロパティの名前は、オーバーロードされたメソッドと同じにできないからです。

[!code-csharp[Conceptual.CLSCompliant#19](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/naming3.cs#19)]
[!code-vb[Conceptual.CLSCompliant#19](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/naming3.vb#19)]

個々の言語に一意のキーワードが含まれるので、共通言語ランタイムを対象にする言語も、キーワードと一致する識別子 (型名など) を参照するための機構を用意する必要があります。 たとえば、`case` は、C# と Visual Basic のキーワードです。 ただし、次の Visual Basic コード例では、左右の中かっこを使用して、`case` キーワードと `case` という名前のクラスを明確に区別できます。 それ以外の場合は、エラー メッセージ "キーワードは、識別子として有効ではありません" が表示され、コンパイルできません。

[!code-vb[Conceptual.CLSCompliant#22](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/keyword1.vb#22)]

次の C# コード例では、`case` シンボルを使用して `@` クラスをインスタンス化することで、識別子と言語キーワードを区別できます。 これがないと、C# コンパイラによって 2 つのエラー メッセージ "型が必要です" および "'case' は無効です" が表示されます。

[!code-csharp[Conceptual.CLSCompliant#23](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/keyword2.cs#23)]

<a name="conversion"></a>

### <a name="type-conversion"></a>型変換

共通言語仕様では、次の 2 つの変換演算子が定義されます。

- `op_Implicit`。データまたは精度の損失につながらない拡大変換に使用されます。 たとえば、<xref:System.Decimal> 構造体には、整数型の値と `op_Implicit` 値を <xref:System.Char> 値に変換できるように、オーバーロードされた <xref:System.Decimal> 演算子が含まれます。

- `op_Explicit`。絶対値 (狭い範囲の値に変換される値) または精度の損失につながる可能性がある縮小変換に使用されます。 たとえば、<xref:System.Decimal> 構造体には、`op_Explicit` 値と <xref:System.Double> 値を <xref:System.Single> に変換し、<xref:System.Decimal> 値を整数値、<xref:System.Decimal>、<xref:System.Double>、および <xref:System.Single> に変換できるように、オーバーロードされた <xref:System.Char> 演算子が含まれます。

ただし、すべての言語で、演算子のオーバーロードまたはカスタム演算子の定義がサポートされているわけではありません。 これらの変換演算子を実装する場合は、他の方法で変換を実行する方法も用意する必要があります。 ここでは、`From`*Xxx* メソッドと `To`*Xxx* メソッドを用意することをお勧めします。

次の例では、CLS に準拠する暗黙的な変換と明示的な変換を定義します。 ここでは、符号付き倍精度浮動小数点数を表す `UDouble` クラスを作成します。 暗黙的な変換については、`UDouble` から <xref:System.Double>、明示的な変換については、`UDouble` から <xref:System.Single>、<xref:System.Double> から `UDouble`、および <xref:System.Single> から `UDouble` への例を示しています。 また、暗黙的な変換演算子の代替として `ToDouble` メソッドを、明示的な変換演算子の代替として `ToSingle`、`FromDouble`、`FromSingle` の各メソッドを定義します。

[!code-csharp[Conceptual.CLSCompliant#15](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/convert1.cs#15)]
[!code-vb[Conceptual.CLSCompliant#15](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/convert1.vb#15)]

<a name="arrays"></a>

### <a name="arrays"></a>配列

CLS 準拠の配列は、次の規則に従います。

- 配列の次元の下限値は 0 にする必要があります。 次の例では、下限が 1 の CLS 非準拠の配列を作成します。 <xref:System.CLSCompliantAttribute> 属性の有無に関係なく、コンパイラでは、`Numbers.GetTenPrimes` メソッドによって返される配列が CLS に準拠していないことは検出されません。

  [!code-csharp[Conceptual.CLSCompliant#8](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array1.cs#8)]
  [!code-vb[Conceptual.CLSCompliant#8](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array1.vb#8)]

- すべての配列の要素が、CLS 準拠の型で構成されている必要があります。 次の例では、CLS 非準拠の配列を返す 2 つのメソッドを定義します。 最初のメソッドは、<xref:System.UInt32> 値の配列を返します。 2 番目のメソッドは <xref:System.Object> 値と <xref:System.Int32> 値を含む <xref:System.UInt32> 配列を返します。 最初の配列は <xref:System.UInt32> 型であるため、コンパイラによって非準拠として識別されますが、2 番目の配列に CLS 非準拠の要素が含まれていることは認識されません。

  [!code-csharp[Conceptual.CLSCompliant#9](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array2.cs#9)]
  [!code-vb[Conceptual.CLSCompliant#9](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array2.vb#9)]

- 配列パラメーターを持つメソッドのオーバーロードの解決は、配列であるという事実とその要素型に基づきます。 したがって、次のオーバーロードされた `GetSquares` メソッドの定義は CLS に準拠しています。

  [!code-csharp[Conceptual.CLSCompliant#10](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/array3.cs#10)]
  [!code-vb[Conceptual.CLSCompliant#10](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/array3.vb#10)]

<a name="Interfaces"></a>

### <a name="interfaces"></a>インターフェイス

CLS 準拠のインターフェイスでは、プロパティ、イベント、および仮想メソッド (実装のないメソッド) を定義できます。 次の項目は、このインターフェイスには指定できません。

- 静的メソッドまたは静的フィールド。 インターフェイスで静的メンバーを定義すると、C# と Visual Basic の両方のコンパイラでコンパイラ エラーが発生します。

- フィールド。 インターフェイスでフィールドを定義すると、C# と Visual Basic の両方のコンパイラでコンパイラ エラーが発生します。

- CLS に準拠していないメソッド。 たとえば、次のインターフェイス定義には、CLS 非準拠とマークされているメソッド、`INumber.GetUnsigned` が含まれています。 この例では、コンパイラの警告が生成されます。

  [!code-csharp[Conceptual.CLSCompliant#6](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/interface2.cs#6)]
  [!code-vb[Conceptual.CLSCompliant#6](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/interface2.vb#6)]

  この規則のため、CLS に準拠している型は、CLS に準拠していないメンバーを実装する必要はありません。 CLS 準拠のフレームワークによって、CLS に非準拠のインターフェイスを実装するクラスが公開されている場合、そのフレームワークには、CLS 非準拠のすべてのメンバーの具象実装も用意する必要があります。

CLS 準拠の言語コンパイラでは、クラスによって、複数のインターフェイスにある同じ名前およびシグネチャを持つメンバーを個別に実装できるようにすることも必要です。  C# と Visual Basic の両方が[明示的なインターフェイス実装](../csharp/programming-guide/interfaces/explicit-interface-implementation.md)をサポートしており、同じ名前を持つメソッドを別々に実装できます。 Visual Basic では `Implements` キーワードもサポートされているので、特定のメンバーが実装するインターフェイスおよびメンバーを明示的に指定できます。 次の例は、明示的なインターフェイス実装として `Temperature` インターフェイスおよび `ICelsius` インターフェイスを実装する `IFahrenheit` クラスを定義することで、このシナリオを示しています。

[!code-csharp[Conceptual.CLSCompliant#24](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/eii1.cs#24)]
[!code-vb[Conceptual.CLSCompliant#24](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/eii1.vb#24)]

<a name="enums"></a>

### <a name="enumerations"></a>列挙体

CLS 準拠の列挙型は、次の規則に従う必要があります。

- 列挙体の基になる型は、組み込みの CLS 準拠の整数 (<xref:System.Byte>、<xref:System.Int16>、<xref:System.Int32>、または <xref:System.Int64>) である必要があります。 たとえば、次のコードでは、基になる型が <xref:System.UInt32> の列挙体を定義しようとしますが、コンパイラの警告が生成されます。

  [!code-csharp[Conceptual.CLSCompliant#7](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/enum3.cs#7)]
  [!code-vb[Conceptual.CLSCompliant#7](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/enum3.vb#7)]

- 列挙型には、`Value__` 属性でマークされた <xref:System.Reflection.FieldAttributes.RTSpecialName?displayProperty=nameWithType> という名前の単一インスタンス フィールドが必要です。 これにより、フィールド値を暗黙的に参照できます。

- 列挙体には、その列挙体自体の型と同じ型を持つリテラルな静的フィールドが含まれます。 たとえば、`State` および `State.On` の値を持つ `State.Off` 列挙体を定義すると、`State.On` と `State.Off` は両方ともリテラルな静的フィールドで、その型は `State` です。

- 列挙体は 2 種類あります。

  - 同時に指定できない一連の名前付き整数値を表す列挙体。 この列挙体の型は、<xref:System.FlagsAttribute?displayProperty=nameWithType> カスタム属性が存在しないことによって示されます。

  - 名前のない値を生成するために結合できる一連のビット フラグを表す列挙体。 この列挙体の型は、<xref:System.FlagsAttribute?displayProperty=nameWithType> カスタム属性が存在することによって示されます。

  詳細については、<xref:System.Enum> 構造体のドキュメントを参照してください。

- 列挙体の値は、その列挙体の指定された値に限定されません。 つまり、列挙体の値の範囲は、その列挙体の基になる値の範囲です。 <xref:System.Enum.IsDefined%2A?displayProperty=nameWithType> メソッドを使用すると、指定された値が列挙体のメンバーかどうかを確認できます。

<a name="members"></a>

### <a name="type-members-in-general"></a>一般的な型メンバー

共通言語仕様では、すべてのフィールドとメソッドに、特定のクラスのメンバーとしてアクセスする必要があります。 したがって、グローバルな静的フィールドおよび静的メソッド (つまり、型とは別に定義された静的フィールドまたは静的メソッド) は CLS に準拠していません。 グローバル フィールドまたはグローバル メソッドをソース コードに追加しようとすると、C# と Visual Basic の両方のコンパイラでコンパイラ エラーが発生します。

共通言語仕様では、標準のマネージド呼び出し規約のみがサポートされます。 アンマネージ呼び出し規約と、`varargs` キーワードでマークされた可変個引数リストを持つメソッドはサポートされません。 標準のマネージド呼び出し規約と互換性がある可変個引数リストについては、<xref:System.ParamArrayAttribute> キーワード (C# の場合)、`params` キーワード (Visual Basic の場合) など、`ParamArray` 属性または個々の言語の実装を使用します。

<a name="MemberAccess"></a>

### <a name="member-accessibility"></a>メンバーのアクセシビリティ

継承されたメンバーをオーバーライドしても、そのメンバーのアクセシビリティは変更できません。 たとえば、基底クラスのパブリック メソッドは、派生クラスのプライベート メソッドではオーバーライドできません。 ただし例外が 1 つあります。あるアセンブリ内にある、別のアセンブリの型でオーバーライドされた `protected internal` メンバー (C# の場合) または `Protected Friend` メンバー (Visual Basic の場合) です。 この場合、オーバーライドのアクセシビリティは `Protected` です。

次の例は、<xref:System.CLSCompliantAttribute> 属性が `true` で、`Human` の派生クラス `Animal` が、`Species` プロパティのアクセシビリティをパブリックからプライベートに変更しようとするときに発生するエラーを示しています。 アクセシビリティがパブリックに変更されると、コンパイルが正常に行われます。

[!code-csharp[Conceptual.CLSCompliant#28](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/accessibility1.cs#28)]
[!code-vb[Conceptual.CLSCompliant#28](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/accessibility1.vb#28)]

メンバーにアクセスできる場合は必ず、そのメンバーのシグネチャの型にアクセスできなければなりません。 たとえば、これは、パブリック メンバーには、型がプライベート、プロテクト、または内部のパラメーターを含められないことを意味します。 次の例は、`StringWrapper` クラス コンストラクターが、文字列値のラップ方法を決定する内部 `StringOperationType` 列挙値を公開した場合に発生するコンパイラ エラーを示しています。

[!code-csharp[Conceptual.CLSCompliant#27](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/accessibility3.cs#27)]
[!code-vb[Conceptual.CLSCompliant#27](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/accessibility3.vb#27)]

<a name="Generics"></a>

### <a name="generic-types-and-members"></a>ジェネリック型とメンバー

入れ子になった型には、少なくともその外側の型と同じ数のジェネリック パラメーターが必要です。 このジェネリック パラメーターは、外側の型のジェネリック パラメーターに位置によって対応します。 ジェネリック型に新しいジェネリック パラメーターを含めることもできます。

外側の型のジェネリック型パラメーターと、その入れ子になった型の関係は、個別の言語構文によって非表示になっていることがあります。 次の例では、入れ子になった 2 つのクラス、`Outer<T>` および `Inner1A` が、ジェネリック型 `Inner1B<U>` に含まれます。 各クラスが `ToString` から継承した <xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドへの呼び出しは、入れ子になった各クラスに、その外側のクラスの型パラメーターが含まれていることを示しています。

[!code-csharp[Conceptual.CLSCompliant#29](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/nestedgenerics2.cs#29)]
[!code-vb[Conceptual.CLSCompliant#29](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/nestedgenerics2.vb#29)]

ジェネリック型の名前は、フォーム *name\`n* でエンコードされます。ここで、*name* は型の名前、\` は文字リテラル、*n* は型で宣言されたパラメーターの数、入れ子になったジェネリック型の場合は、新しく導入された型パラメーターの数です。 ジェネリック型の名前のエンコーディングは、主に、リフレクションを使用してライブラリ内の CLS 準拠のジェネリック型にアクセスする開発者が使用します。

制約がジェネリック型に適用される場合は、制約として使用されるすべての型も CLS に準拠している必要があります。 次の例では、CLS に準拠していない `BaseClass` という名前のクラスと、型パラメーターが `BaseCollection` から派生しなければならない `BaseClass` という名前のジェネリック クラスを定義します。 ただし、`BaseClass` が CLS に準拠していないので、コンパイラによって警告が生成されます。

[!code-csharp[Conceptual.CLSCompliant#34](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics5.cs#34)]
[!code-vb[Conceptual.CLSCompliant#34](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics5.vb#34)]

ジェネリック型がジェネリック基本型から派生している場合、そのジェネリック型は、基本型に対する制約も必ず満たすように制約を再宣言する必要があります。 次の例では、任意の数値型を表すことができる `Number<T>` を定義します。 また、浮動小数点値を表す `FloatingPoint<T>` クラスも定義します。 ただし、ソース コードはコンパイルされません。`Number<T>` (T は値型) の制約は `FloatingPoint<T>` に適用されないからです。

[!code-csharp[Conceptual.CLSCompliant#30](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics2a.cs#30)]
[!code-vb[Conceptual.CLSCompliant#30](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics2a.vb#30)]

制約が `FloatingPoint<T>` クラスに追加されると、コンパイルが正常に行われます。

[!code-csharp[Conceptual.CLSCompliant#31](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics2.cs#31)]
[!code-vb[Conceptual.CLSCompliant#31](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics2.vb#31)]

共通言語仕様では、入れ子になった型とプロテクト メンバーに対して従来のインスタンス化ごとのモデルが適用されます。 オープン ジェネリック型では、フィールドまたはメンバーのシグネチャに、入れ子になったプロテクト ジェネリック型の特定のインスタンス化が含まれている場合、これらのフィールドやメンバーは公開できません。 ジェネリックの基底クラスやインターフェイスについて特定のインスタンス化を拡張する非ジェネリック型では、フィールドまたはメンバーのシグネチャに、入れ子になったプロテクト ジェネリック型の別のインスタンス化が含まれている場合、これらのフィールドやメンバーは公開できません。

次の例では、ジェネリック型 `C1<T>` (Visual Basic の場合は `C1(Of T)`)、およびプロテクト クラス `C1<T>.N` (Visual Basic の場合は `C1(Of T).N`) を定義します。 `C1<T>` には、2 つのメソッド `M1` と `M2` があります。 ただし、`M1` は、`C1<int>.N` (`C1(Of Integer).N`) オブジェクトを C1\<T> (または `C1(Of T)`) から返そうとするので、CLS に準拠していません。 2 番目の `C2` クラスは、`C1<long>` (`C1(Of Long)`) から派生しています。 これには、`M3` と `M4` の 2 つのメソッドがあります。 `M3` は、`C1<int>.N` (`C1(Of Integer).N`) オブジェクトを `C1<long>` のサブクラスから返そうとするので、CLS に準拠していません。 言語コンパイラはさらに制限されている可能性があります。 この例では、Visual Basic で `M4` をコンパイルしようとするとエラーが表示されます。

[!code-csharp[Conceptual.CLSCompliant#32](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/generics4.cs#32)]
[!code-vb[Conceptual.CLSCompliant#32](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/generics4.vb#32)]

<a name="ctors"></a>

### <a name="constructors"></a>コンストラクター

CLS 準拠のクラスと構造体のコンストラクターは、次の規則に従う必要があります。

- 派生クラスのコンストラクターは、継承されたインスタンス データにアクセスする前に、基底クラスのインスタンス コンストラクターを呼び出す必要があります。 これは、基底クラスのコンストラクターは派生クラスには継承されないからです。 この規則は、直接継承をサポートしない構造体に適用されません。

  次の例に示すように、コンパイラは、通常、CLS 準拠とは別にこの規則を適用します。 これにより、`Doctor` クラスから派生した `Person` クラスが作成されますが、`Doctor` クラスでは、継承されたインスタンス フィールドを初期化するための `Person` クラス コンストラクターは呼び出されません。

  [!code-csharp[Conceptual.CLSCompliant#11](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/ctor1.cs#11)]
  [!code-vb[Conceptual.CLSCompliant#11](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/ctor1.vb#11)]

- オブジェクト作成以外の目的で、オブジェクト コンストラクターを呼び出すことはできません。 また、オブジェクトを 2 度初期化することもできません。 たとえば、これは <xref:System.Object.MemberwiseClone%2A?displayProperty=nameWithType> メソッド、および <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter.Deserialize%2A?displayProperty=nameWithType> などの逆シリアル化メソッドでは、コンストラクターを呼び出してはいけないことを意味します。

<a name="properties"></a>

### <a name="properties"></a>Properties

CLS 準拠型のプロパティは、次の規則に従う必要があります。

- プロパティには setter、getter、またはこの両方が必ず必要です。 アセンブリでは、これらは特殊なメソッドとして実装されます。つまり、個別のメソッドとして (getter は `get_`*propertyname*、setter は `set_`*propertyname* という名前で) 表示され、アセンブリのメタデータでは `SpecialName` としてマークされます。 C# コンパイラおよび Visual Basic コンパイラでは、この規則が自動的に適用されます。<xref:System.CLSCompliantAttribute> 属性を適用する必要はありません。

- プロパティの型は、プロパティ get アクセス操作子、および set アクセス操作子の最後の引数の戻り値の型です。 これらの型は CLS に準拠している必要があり、引数を参照によってプロパティに割り当てることはできません (つまり、マネージド ポインターにできません)。

- プロパティに get アクセス操作子と set アクセス操作子の両方がある場合は、両方が仮想、両方が静的、または両方がインスタンスである必要があります。 C# コンパイラおよび Visual Basic コンパイラでは、この規則がプロパティ定義構文によって自動的に適用されます。

<a name="events"></a>

### <a name="events"></a>events

イベントは、名前と型によって定義されます。 イベントの型は、イベントの表示に使用されるデリゲートです。 たとえば、<xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントは <xref:System.ResolveEventHandler> 型です。 イベント自体のほか、イベント名に基づく名前の 3 つのメソッドがイベントの実装を提供し、アセンブリのメタデータで `SpecialName` としてマークされています。

- イベント ハンドラーを追加するメソッド (`add_`*EventName*)。 たとえば、<xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントのイベント サブスクリプション メソッドの名前は `add_AssemblyResolve` です。

- イベント ハンドラーを削除するメソッド (`remove_`*EventName*)。 たとえば、<xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> イベントの削除メソッドの名前は `remove_AssemblyResolve` です。

- イベントが発生したことを示すメソッド (`raise_`*EventName*)。

> [!NOTE]
> イベントに関する共通言語仕様の規則は、ほとんどが言語コンパイラによって実装され、コンポーネント開発者が意識せずに使用できます。

イベントの追加、削除、発生の各メソッドには同じアクセシビリティが必要です。 また、すべてが、静的メソッド、インスタンス メソッド、仮想メソッドのいずれかでなければなりません。 イベントを追加および削除するメソッドには、イベント デリゲート型の型を持つパラメーターが 1 つあります。 追加メソッドおよび削除メソッドは両方とも存在するか、両方存在しないかのいずれかでなければなりません。

次の例では、2 つのポイントの気温の差がしきい値と等しいか、それを超えた場合に `Temperature` イベントを発生させる、`TemperatureChanged` という名前の CLS 準拠クラスを定義します。 `Temperature` クラスは、イベント ハンドラーを選択的に実行できるように、`raise_TemperatureChanged` メソッドを明示的に定義します。

[!code-csharp[Conceptual.CLSCompliant#20](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/event1.cs#20)]
[!code-vb[Conceptual.CLSCompliant#20](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/event1.vb#20)]

<a name="overloads"></a>

### <a name="overloads"></a>Shadows

共通言語仕様では、次の要件がオーバーロード メンバーに適用されます。

- メンバーは、パラメーターの数および型に基づいてオーバーロードできます。 オーバーロードを区別するときに、呼び出し規約、戻り値の型、メソッドまたはそのパラメーターに適用されているカスタム修飾子、およびパラメーターが値渡しか参照渡しかは考慮されません。 例については、「[名前付け規則](#naming)」で、名前がスコープ内で一意であることを要求する要件のコードを参照してください。

- プロパティおよびメソッドのみオーバーロードできる。 フィールドとイベントはオーバーロードできません。

- ジェネリック メソッドは、ジェネリック パラメーターの数に基づいてオーバーロードできます。

> [!NOTE]
> `op_Explicit` 演算子および `op_Implicit` 演算子にはこの規則が適用されず、戻り値は、オーバーロード解決のためのメソッド シグネチャの一部として見なされません。 この 2 つの演算子は、そのパラメーターと戻り値の両方に基づいてオーバーロードできます。

<a name="exceptions"></a>

### <a name="exceptions"></a>例外

例外オブジェクトは <xref:System.Exception?displayProperty=nameWithType>、または <xref:System.Exception?displayProperty=nameWithType> の別の派生型から派生する必要があります。 次の例は、`ErrorClass` というカスタム クラスを例外処理に使用したときに発生するコンパイラ エラーを示しています。

[!code-csharp[Conceptual.CLSCompliant#13](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/exceptions1.cs#13)]
[!code-vb[Conceptual.CLSCompliant#13](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/exceptions1.vb#13)]

このエラーを修正するには、`ErrorClass` から継承するように <xref:System.Exception?displayProperty=nameWithType> クラスを指定する必要があります。 また、`Message` プロパティはオーバーライドする必要があります。 次の例では、このエラーを修正して CLS 準拠の `ErrorClass` クラスを定義します。

[!code-csharp[Conceptual.CLSCompliant#14](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/exceptions2.cs#14)]
[!code-vb[Conceptual.CLSCompliant#14](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/exceptions2.vb#14)]

<a name="attributes"></a>

### <a name="attributes"></a>属性

.NET Framework アセンブリでは、カスタム属性に拡張可能機構が用意されており、そのカスタム属性を格納し、アセンブリ、型、メンバー、メソッド パラメーターなどのプログラミング オブジェクトに関するメタデータを取得します。 カスタム属性は <xref:System.Attribute?displayProperty=nameWithType>、または <xref:System.Attribute?displayProperty=nameWithType> の派生型から派生する必要があります。

規則に違反する例を次に示します。 この例では、`NumericAttribute` から派生していない <xref:System.Attribute?displayProperty=nameWithType> クラスを定義します。 コンパイラ エラーは、CLS 非準拠の属性が適用されている場合にのみ発生します。クラスが定義されているときではありません。

[!code-csharp[Conceptual.CLSCompliant#18](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/attribute1.cs#18)]
[!code-vb[Conceptual.CLSCompliant#18](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/attribute1.vb#18)]

CLS 準拠の属性のコンストラクターまたはプロパティは、次の型のみを公開できます。

- <xref:System.Boolean>

- <xref:System.Byte>

- <xref:System.Char>

- <xref:System.Double>

- <xref:System.Int16>

- <xref:System.Int32>

- <xref:System.Int64>

- <xref:System.Single>

- <xref:System.String>

- <xref:System.Type>

- 基になる型が <xref:System.Byte>、<xref:System.Int16>、<xref:System.Int32>、または <xref:System.Int64> である列挙体の型。

次の例では、`DescriptionAttribute` から派生する <xref:System.Attribute> クラスを定義します。 クラス コンストラクターには型 `Descriptor` のパラメーターがあるので、クラスは CLS に準拠していません。 C# コンパイラでは警告が出力されますが、コンパイルは正常に行われます。一方、Visual Basic コンパイラでは警告もエラーも出力されません。

[!code-csharp[Conceptual.CLSCompliant#33](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/attribute2.cs#33)]
[!code-vb[Conceptual.CLSCompliant#33](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/attribute2.vb#33)]

<a name="CLSAttribute"></a>

## <a name="the-clscompliantattribute-attribute"></a>CLSCompliantAttribute 属性

<xref:System.CLSCompliantAttribute> 属性は、プログラム要素が共通言語仕様でコンパイルされているかどうかを示すために使用されます。 <xref:System.CLSCompliantAttribute.%23ctor%28System.Boolean%29?displayProperty=nameWithType> コンストラクターには、プログラム要素が CLS に準拠しているかどうかを示す 1 つの必須パラメーター、`isCompliant` が含まれます。

コンパイル時に、CLS 準拠が前提とされる非準拠要素が検出され、警告が出力されます。 非準拠として明示的に宣言された型またはメンバーに対しては、警告は出力されません。

コンポーネント開発者は、次の 2 とおりの目的で <xref:System.CLSCompliantAttribute> 属性を使用できます。

- コンポーネントによって公開されたパブリック インターフェイスの CLS 準拠部分と CLS 非準拠部分を定義する。 この属性を使用して特定のプログラム要素を CLS 準拠としてマークすると、.NET Framework を対象とするすべてのツールおよび言語から、これらの要素に必ずアクセスできるようになります。

- コンポーネント ライブラリのパブリック インターフェイスが CLS に準拠するプログラム要素のみを公開するように保証する。 要素が CLS 非準拠の場合は、通常、警告が表示されます。

> [!WARNING]
> 言語コンパイラでは、<xref:System.CLSCompliantAttribute> 属性が使用されているかどうかに関係なく、CLS 準拠の規則が適用される場合があります。 たとえば、インターフェイスの静的メンバーを定義すると CLS の規則に違反します。 この点に関して、インターフェイスで `static` メンバー (C# の場合) または `Shared` メンバー (Visual Basic の場合) を定義すると、C# と Visual Basic の両方のコンパイラでエラー メッセージが表示され、アプリはコンパイルされません。

<xref:System.CLSCompliantAttribute> 属性は、値 <xref:System.AttributeUsageAttribute> が指定された <xref:System.AttributeTargets.All?displayProperty=nameWithType> 属性でマークされます。 この値を使用すると、<xref:System.CLSCompliantAttribute> 属性を、アセンブリ、モジュール、型 (クラス、構造体、列挙体、インターフェイス、およびデリゲート)、型パラメーター (コンストラクター、メソッド、プロパティ、フィールド、およびイベント)、パラメーター、ジェネリック パラメーター、戻り値など、すべてのプログラム要素に適用できます。 ただし、実際は、アセンブリ、型、および型メンバーだけに属性を適用することをお勧めします。 そうしないと、属性は、コンパイラによってライブラリのパブリック インターフェイスで非準拠パラメーター、ジェネリック パラメーター、または戻り値が検出されたときに必ず無視され、コンパイラ警告が引き続き生成されます。

<xref:System.CLSCompliantAttribute> 属性の値は、内包型プログラム要素によって継承されます。 たとえば、アセンブリが CLS 準拠としてマークされている場合は、その型も CLS に準拠します。 型が CLS 準拠としてマークされている場合は、その入れ子になった型とメンバーも CLS に準拠します。

継承された準拠状況を明示的にオーバーライドするには、<xref:System.CLSCompliantAttribute> 属性を、格納されているプログラム要素に適用します。 たとえば、<xref:System.CLSCompliantAttribute> 値が `isCompliant` に指定された `false` 属性を使用すると、準拠アセンブリで非準拠型を定義できます。また、`isCompliant` 値が `true` に指定された属性を使用すると、非準拠アセンブリで準拠型を定義できます。 準拠型で非準拠メンバーを定義することもできます。 ただし、非準拠型は準拠メンバーを持つことができないので、`isCompliant` 値が `true` に指定された属性では非準拠型から継承をオーバーライドできません。

コンポーネントを開発するときは必ず、<xref:System.CLSCompliantAttribute> 属性を使用して、アセンブリ、その型、およびそのメンバーが CLS に準拠しているかどうかを指定することをお勧めします。

CLS 準拠のコンポーネントを作成するには:

1. <xref:System.CLSCompliantAttribute> を使用して、CLS 準拠としてアセンブリをマークします。

2. アセンブリ内の CLS に準拠していない公開された型を、非準拠としてマークします。

3. CLS 準拠の型の公開されたメンバーを、非準拠としてマークします。

4. CLS 非準拠メンバーの CLS 準拠の代替を指定します。

非準拠の型とメンバーすべてを正常にマークした場合、非準拠の警告は出力されません。 ただし、どのメンバーが CLS に準拠していないかを提示し、CLS 準拠の代替を製品ドキュメントに示すことをお勧めします。

次の例では、<xref:System.CLSCompliantAttribute> 属性を使用して、CLS 準拠のアセンブリと、2 つの CLS 非準拠のメンバーを持つ型、`CharacterUtilities` を定義します。 両メンバーとも `CLSCompliant(false)` 属性でタグ付けされているので、警告は生成されません。 また、クラスには、両方のメソッド用の CLS 準拠の代替も用意されています。 通常、CLS 準拠の代替を用意するには、2 つのオーバーロードを `ToUTF16` メソッドに追加するだけです。 ただし、戻り値に基づいてメソッドをオーバーロードできないので、CLS 準拠のメソッドの名前は、非準拠のメソッドの名前とは異なります。

[!code-csharp[Conceptual.CLSCompliant#35](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.clscompliant/cs/indicator3.cs#35)]
[!code-vb[Conceptual.CLSCompliant#35](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.clscompliant/vb/indicator3.vb#35)]

ライブラリではなくアプリを開発している場合 (つまり、他のアプリ開発者が使用できる型またはメンバーを公開しない場合)、アプリが使用するプログラム要素は、そのプログラム要素が使用する言語でサポートされていない場合に CLS に準拠する必要があります。 この場合、CLS 非準拠の要素を使用しようとすると、言語コンパイラによってエラーが生成されます。

<a name="CrossLang"></a>

## <a name="cross-language-interoperability"></a>言語間の相互運用性

言語に依存しないということは、いくつか意味があります。 たとえば、ある言語で記述された型を、別の言語で記述されたアプリからシームレスに利用することができます。これについては、記事「[言語への非依存性、および言語非依存コンポーネント](../../docs/standard/language-independence-and-language-independent-components.md)」で説明されています。 また、複数の言語で記述されたコードを 1 つの .NET .NET Framework アセンブリにまとめることもできます。ここでは、この点について焦点を当てて説明します。

次の例では、`NumericLib` および `StringLib` という 2 つのクラスを含む Utilities.dll という名前のクラス ライブラリを作成して言語間の相互運用性を示します。 `NumericLib` クラスは C# で記述され、`StringLib` クラスは Visual Basic で記述されています。 以下は StringUtil.vb のソース コードで、`ToTitleCase` クラスに `StringLib` という単一のメンバーが含まれます。

[!code-vb[Conceptual.CrossLanguage#1](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.crosslanguage/vb/stringutil.vb#1)]

以下は NumberUtil.cs のソース コードで、`NumericLib` および `IsEven` という 2 つのメンバーを持つ `NearZero` クラスを定義しています。

[!code-csharp[Conceptual.CrossLanguage#2](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.crosslanguage/cs/numberutil.cs#2)]

単一のアセンブリに 2 つのクラスをパッケージ化するには、モジュールにコンパイルする必要があります。 Visual Basic のソース コード ファイルをモジュールにコンパイルするには、次のコマンドを使用します。

```console
vbc /t:module StringUtil.vb
```

Visual Basic コンパイラのコマンド ライン構文の詳細については、「[コマンド ラインからのビルド](../visual-basic/reference/command-line-compiler/building-from-the-command-line.md)」を参照してください。

C# のソース コード ファイルをモジュールにコンパイルするには、次のコマンドを使用します。

```console
csc /t:module NumberUtil.cs
```

C# コンパイラのコマンド ライン構文の詳細については、「[csc.exe を使用したコマンド ラインからのビルド](../csharp/language-reference/compiler-options/command-line-building-with-csc-exe.md)」を参照してください。

次に、[リンカー オプション](/cpp/build/reference/linker-options)を使用して 2 つのモジュールをアセンブリにコンパイルします。

```console
link numberutil.netmodule stringutil.netmodule /out:UtilityLib.dll /dll
```

次の例では、その後 `NumericLib.NearZero` メソッドおよび `StringLib.ToTitleCase` メソッドを呼び出します。 Visual Basic コードと C# コードは、両方のクラスのメソッドにアクセスできることに注意してください。

[!code-csharp[Conceptual.CrossLanguage#3](../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.crosslanguage/cs/useutilities1.cs#3)]
[!code-vb[Conceptual.CrossLanguage#3](../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.crosslanguage/vb/useutilities1.vb#3)]

Visual Basic コードをコンパイルするには、次のコマンドを使用します。

```console
vbc example.vb /r:UtilityLib.dll
```

C# でコンパイルするには、コンパイラの名前を **vbc** から **csc** に変更し、ファイル拡張子を .vb から .cs に変更します。

```console
csc example.cs /r:UtilityLib.dll
```

## <a name="see-also"></a>参照

- <xref:System.CLSCompliantAttribute>
