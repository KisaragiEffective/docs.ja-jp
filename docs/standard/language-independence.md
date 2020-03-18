---
title: 言語への非依存性、および言語非依存コンポーネント
description: C#、C++/CLI、F#、IronPython、VB、Visual COBOL、PowerShell など、.NET でサポートされる多くの言語の中のいずれかで開発する方法を説明します。
ms.date: 07/22/2016
dev_langs:
- csharp
- vb
ms.technology: dotnet-standard
ms.assetid: 2dbed1bc-86f5-43cd-9a57-adbb1c5efba4
ms.openlocfilehash: e1f419dd57c1e90d7ebb57ef572f338a34d1c509
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "73423633"
---
# <a name="language-independence-and-language-independent-components"></a>言語への非依存性、および言語非依存コンポーネント

.NET は言語に依存しません。 つまり、開発者は、C#、F#、および Visual Basic などの .NET 実装を対象とする多くの言語の中のいずれかで開発できることを意味します。 .NET 実装用に開発されたクラス ライブラリの型とメンバーには、最初に記述された言語を知らなくてもアクセスできます。元の言語の規則に従う必要もありません。 コンポーネントを開発しているのであれば、コンポーネントの言語にかかわらず、すべての .NET アプリからそのコンポーネントにアクセスできます。

> [!NOTE]
> この記事の最初の部分では、言語に依存しないコンポーネント、つまり、どの言語で記述されたアプリからでも使用できるコンポーネントの作成について説明します。 また、複数の言語で記述されたソース コードから 1 つのコンポーネントまたはアプリを作成することもできます。この記事の 2 番目のパートにある「[言語間の相互運用性](#cross-language-interoperability)」を参照してください。

任意の言語で記述された他のオブジェクトと完全に対話するには、すべての言語に共通の機能だけを呼び出し側に公開するようにオブジェクトを実装する必要があります。 この共通の機能セットは、生成されたアセンブリに適用される規則のセットである、共通言語仕様 (CLS: Common Language Specification) によって定義されます。 共通言語仕様は、「[ECMA-335 Standard: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) の第 1 部の第 7 ～ 11 項で定義されています。

コンポーネントが共通言語仕様に準拠している場合は、CLS に準拠することが保証され、CLS をサポートするすべてのプログラミング言語で記述されたアセンブリのコードからアクセスできます。 コンパイル時にコンポーネントが共通言語仕様に準拠しているかどうかを確認するには、[CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 属性をソース コードに適用します。 詳細については、「[CLSCompliantAttribute 属性](#the-clscompliantattribute-attribute)」を参照してください。

この記事の内容:

* [CLS 準拠の規則](#cls-compliance-rules)

  * [型および型メンバーのシグネチャ](#types-and-type-member-signatures)

  * [名前付け規則](#naming-conventions)

  * [型変換](#type-conversion)

  * [配列](#arrays)

  * [インターフェイス](#interfaces)

  * [列挙型](#enumerations)

  * [一般的な型メンバー](#type-members-in-general)

  * [メンバーのアクセシビリティ](#member-accessibility)

  * [ジェネリック型とメンバー](#generic-types-and-members)

  * [コンストラクター](#constructors)

  * [プロパティ](#properties)

  * [イベント](#events)

  * [Overloads](#overloads)

  * [例外](#exceptions)

  * [属性](#attributes)

* [CLSCompliantAttribute 属性](#the-clscompliantattribute-attribute)

* [言語間の相互運用性](#cross-language-interoperability)

## <a name="cls-compliance-rules"></a>CLS 準拠の規則

ここでは、CLS に準拠したコンポーネントを作成するための規則について説明します。 規則規則の完全な一覧については、「[ECMA-335 Standard: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) の第 1 部の第 11 項を参照してください。

> [!NOTE]
> 共通言語仕様では、コンシューマー (プログラムによって CLS 準拠のコンポーネントにアクセスする開発者)、フレームワーク (言語コンパイラを使用して CLS 準拠のライブラリを作成する開発者)、およびエクステンダー (CLS 準拠のコンポーネントを作成する言語コンパイラ、コード パーサーなどのツールを作成する開発者) に適用する、CLS 準拠の各規則について説明します。 ここでは、フレームワークに適用するときの規則に焦点を当てます。 エクステンダーに適用する一部の規則は、[Reflection.Emit](xref:System.Reflection.Emit) を使用して作成されたアセンブリに適用されることもあります。

言語に依存しないコンポーネントをデザインするには、コンポーネントのパブリック インターフェイスに CLS 準拠の規則を適用するだけです。 プライベートな実装は仕様に準拠する必要はありません。

> [!IMPORTANT]
> CLS 準拠の規則は、コンポーネントのパブリック インターフェイスにのみ適用されます。プライベート実装には適用されません。

たとえば、[Byte](xref:System.Byte) 以外の符号なし整数は CLS に準拠していません。 次の例の `Person` クラスは型 [UInt16](xref:System.UInt16) の `Age` プロパティを公開するので、次のコードではコンパイラの警告が表示されます。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private UInt16 personAge = 0;

   public UInt16 Age
   { get { return personAge; } }
}
// The attempt to compile the example displays the following compiler warning:
//    Public1.cs(10,18): warning CS3003: Type of 'Person.Age' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As UInt16
      Get
         Return personAge
      End Get
   End Property
End Class
' The attempt to compile the example displays the following compiler warning:
'    Public1.vb(9) : warning BC40027: Return type of function 'Age' is not CLS-compliant.
'
'       Public ReadOnly Property Age As UInt16
'                                ~~~
```

Person クラスを CLS 準拠にするには、`Age` プロパティの型を `UInt16` から、CLS 準拠の 16 ビット符号付き整数である [Int16](xref:System.Int16) に変更します。 プライベート `personAge` フィールドの型を変更する必要はありません。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person
{
   private Int16 personAge = 0;

   public Int16 Age
   { get { return personAge; } }
}
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Person
   Private personAge As UInt16

   Public ReadOnly Property Age As Int16
      Get
         Return CType(personAge, Int16)
      End Get
   End Property
End Class
```

ライブラリのパブリック インターフェイスは、次の要素で構成されます。

* パブリック クラスの定義。

* パブリック クラスのパブリック メンバーの定義、および派生クラスからアクセスできるメンバー (つまり、protected メンバー) の定義。

* パブリック クラスのパブリック メソッドのパラメーターおよび戻り値の型、派生クラスからアクセスできるメソッドのパラメーターおよび戻り値の型。

CLS 準拠の規則を次の表に示します。 これらの規則のテキストは、「[ECMA-335 Standard: Common Language Infrastructure](https://www.ecma-international.org/publications/standards/Ecma-335.htm)」(標準の ECMA-335: 共通言語基盤) からの引用で、Ecma International が 2012 年の著作権を保有しています。 これらの規則の詳細については、以降のセクションを参照してください。

カテゴリ | 解決方法については、 | ルール | 規則番号
-------- | --- | ---- | -----------
ユーザー補助 | [メンバーのアクセシビリティ](#member-accessibility) | 継承されたメソッドをオーバーライドする場合、アクセシビリティは変更してはいけない。ただし、別のアセンブリから継承されたメソッドをアクセシビリティ `family-or-assembly` でオーバーライドする場合は除く。 この場合、オーバーライドのアクセシビリティは `family` にすること。 | 10
ユーザー補助 | [メンバーのアクセシビリティ](#member-accessibility) | 型およびメンバーの可視性およびアクセシビリティについて、メンバーのシグネチャに指定されている型は、そのメンバーが可視でアクセス可能な場合、必ず可視でアクセス可能でなければいけない。 たとえば、アセンブリ外部から参照できるパブリックなメソッドには、アセンブリ内部でだけ可視である型が引数として含まれていてはいけない。 メンバーのシグネチャに使用されているジェネリック型のインスタンスを構成する型の可視性およびアクセシビリティは、メンバーが可視でアクセス可能の場合、必ず可視でアクセス可能でなければいけない。 たとえば、アセンブリ外部から参照できるメンバーのシグネチャに指定されているジェネリック型のインスタンスに、アセンブリ内部でだけ可視である型の汎用引数が含まれていてはいけない。 | 12
配列 | [配列](#arrays) | 配列は、要素が CLS 準拠型で、すべての次元でインデックス番号が 0 から始まらなければならない。 項目が配列の場合、オーバーロードどうしを区別するには配列要素の型を必要とする。 オーバーロードが 2 つ以上の配列型に基づく場合、要素型は名前付きの型でなければいけない。 | 16
属性 | [属性](#attributes) | 属性は型 [System.Attribute](xref:System.Attribute) であるか、それから継承する型である必要がある。 | 41
属性 | [属性](#attributes) | CLS ではカスタム属性のエンコーディングのサブセットのみ使用できる。 これらのエンコーディングに表示される型 (第 4 部を参照) は、[System.Type](xref:System.Type)、[System.String](xref:System.String)、[System.Char](xref:System.Char)、[System.Boolean](xref:System.Boolean)、[System.Byte](xref:System.Byte)、[System.Int16](xref:System.Int16)、[System.Int32](xref:System.Int32)、[System.Int64](xref:System.Int64)、[System.Single](xref:System.Single)、[System.Double](xref:System.Double)、および CLS 準拠の基底の整数型に基づく任意の列挙型のみである。 | 34
属性 | [属性](#attributes) | CLS では、公開参照される必須の修飾子 (`modreq`、第 2 部を参照) は使用できない。ただし、認識しないオプションの修飾子 (`modopt`、第 2 部を参照) は使用できる。 | 35
コンストラクター | [コンストラクター](#constructors) | オブジェクト コンストラクターでは、継承しているインスタンス データへのアクセスが発生する前に、基底クラスのインスタンス コンストラクターを呼び出さなければいけない (コンストラクターが不要である値型は除く)。  | 21
コンストラクター | [コンストラクター](#constructors) | オブジェクト コンストラクターがオブジェクトの作成時以外で呼び出されてはならず、またオブジェクトが 2 度初期化されてもいけない。 | 22
列挙 | [列挙型](#enumerations) | enum の基になる型は組み込みの CLS 整数型、フィールド名は "value__" であり、そのフィールドには `RTSpecialName` のマークが付けられる。 |  7
列挙型 | [列挙型](#enumerations) | enum には 2 種類あり、[System.FlagsAttribute](xref:System.FlagsAttribute) カスタム属性 (第 4 部のライブラリを参照) の有無で区別する。 片方は名前付き整数値を表し、もう片方は名前付きビット フラグを表す。名前付きビット フラグは、それを組み合わせて名前のない値を生成できる。 `enum` の値は、指定した値に限定されない。 |  8
列挙 | [列挙型](#enumerations) | enum のリテラルな静的フィールドの型は、その enum 自体の型である。 |  9
イベント | [イベント](#events) | イベントを実装するメソッドは、メタデータ内で `SpecialName` のマークが付けられる。 |29
イベント | [イベント](#events) | イベントとイベントのアクセサーのアクセシビリティは同一である。 |30
イベント | [イベント](#events) | イベントの `add` メソッドおよび `remove` メソッドは、どちらもあってもなくてもよい。 |31
イベント | [イベント](#events) | `add` メソッドおよび `remove` メソッドは、それぞれパラメーターを 1 つ使用する。このパラメーターの型がイベントの型を規定する。また、パラメーターの型は [System.Delegate](xref:System.Delegate) の派生でなければいけない。 |32
イベント | [イベント](#events) | イベントは、特定の名前付けパターンに従わなくてはいけない。 CLS 規則 29 で触れられている SpecialName 属性は、適切な名前比較で無視され、識別子規則に従わなければいけない。  |33
例外 | [例外](#exceptions) | スローできるオブジェクト型は、[System.Exception](xref:System.Exception)、またはそれを継承する型である。 ただし、CLS 準拠のメソッドで他の型の例外のスローをブロックする必要はない。 | 40
全般 | [CLS 準拠の規則](#cls-compliance-rules) | CLS 規則は、型の構成部分のうち、その型を定義しているアセンブリの外部からアクセスまたは参照できる部分にのみ適用される。 | 1
全般 | [CLS 準拠の規則](#cls-compliance-rules) | CLS 非準拠型のメンバーを CLS 準拠と指定しない。 | 2
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | 入れ子になった型は、少なくともその外側の型と同じ数のジェネリック パラメーターを持つ。 入れ子にされた型のジェネリック パラメーターは、それを囲む型のジェネリック パラメーターと、位置によって対応します。  | 42
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | ジェネリック型の名前は、入れ子にされない型で宣言される型パラメーターの数をエンコードする必要がある。入れ子にされる場合は、上記の規則に従って、型に新しく組み込まれる型パラメーターの数をエンコードする必要がある。 | 43
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | ジェネリック型は必要な制約を再宣言して、基本型またはインターフェイスの制約がジェネリック型の制約で確実に満たされるようにする必要がある。 | 44
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | ジェネリック パラメーターの制約として使用される型は CLS に準拠する必要がある。 | 45
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | インスタンス化されたジェネリック型のメンバー (入れ子になった型も含む) の可視性およびアクセシビリティは、ジェネリック型の全体の宣言ではなく、特定のインスタンス化に対してスコープが設定される必要がある。 この場合でも、CLS 規則 12 の可視性規則とアクセシビリティ規則は同様に適用される。 | 46
ジェネリック | [ジェネリック型とメンバー](#generic-types-and-members) | 抽象メソッドまたは仮想ジェネリック メソッドごとに、既定の具体的な実装がある。 | 47
インターフェイス | [インターフェイス](#interfaces) | CLS 準拠のインターフェイスでは、CLS に準拠しないメソッドを実装するために、これらを定義する必要はない。 | 18
インターフェイス | [インターフェイス](#interfaces) | CLS 準拠インターフェイスでは、静的メソッドを定義してはいけない。また、フィールドも定義してはいけない。 | 19
メンバー | [一般的な型メンバー](#type-members-in-general) | グローバルで静的な (static) フィールドとメソッドは CLS 準拠ではありません。 | 36
メンバー | -- | 静的リテラル値の指定には、フィールド初期化メタデータを使用する。 CLS 準拠のリテラルには、そのリテラルと同じ型 (または、そのリテラルが `enum` の場合は基本型) の値がフィールド初期化メタデータに指定されている必要がある。 | 13
メンバー | [一般的な型メンバー](#type-members-in-general) | vararg 制約は CLS の一部ではなく、CLS でサポートする呼び出し規則だけが標準のマネージド呼び出し規則である。 | 15
名前付け規則 | [名前付け規則](#naming-conventions) | アセンブリは、識別子の頭文字および構成文字として使用できる文字セットを規定する Unicode Standard 3.0 の『Technical Report 15』の「Annex 7」に従う必要がある。詳細については、「[Unicode Normalization Forms](https://www.unicode.org/unicode/reports/tr15/tr15-18.html)」(Unicode 正規化形式) を参照。 識別子は、Unicode 正規形 C に定義されている標準形式で記述する必要がある。CLS で 2 つの識別子が同じと見なされるのは、小文字マッピング (Unicode のロケール非依存で 1 対 1 の小文字による対応付け) が同じ場合である。 つまり、CLS で 2 つの識別子が異なる場合、大文字小文字の違いだけではない。 ただし、継承された定義をオーバーライドする場合、CLI では元の宣言と厳密に同じエンコーディングの使用が求められる。 | 4
オーバーロード | [名前付け規則](#naming-conventions) | CLS 準拠のスコープに導入されるすべての名前は、完全に独立した種類である必要があります。ただし、名前が同じでオーバーロードによって解決できる場合を除きます。 CTS では 1 つの型でメソッドとフィールドに同じ名前を使用できるが、CLS では使用できない。 | 5
オーバーロード | [名前付け規則](#naming-conventions) | フィールドおよび入れ子になった型について、CTS ではシグネチャでの区別が可能だが、CLS では識別子の比較だけで区別できる必要がある。 CLS 規則 39 の指定を除き、識別子の比較により名前が同じであるメソッド、プロパティ、およびイベントでは、相違点は戻り値の型だけに限定されない。 | 6
オーバーロード | [Overloads](#overloads) | プロパティおよびメソッドのみオーバーロードできる。 | 37
オーバーロード | [Overloads](#overloads) |プロパティおよびメソッドは、パラメーターの数値と型にのみ基づいてオーバーロードできる。ただし、戻り値の型に基づいてオーバーロードできる変換演算子の `op_Implicit` と o`op_Explicit` は例外である。 | 38
オーバーロード | -- | 型で宣言された複数の CLS 準拠のメソッドに同じ名前が指定されている場合、特定の一連の型のインスタンス化において、これらのメソッドのパラメーターと戻り値の型は同じである。また、これらの型のインスタンス化で、すべてのメソッドをセマンティクス レベルで等価にする必要がある。 | 48
プロパティ | [プロパティ](#properties) | プロパティの getter メソッドおよび setter メソッドを実装するメソッドは、メタデータで `SpecialName` とマークする。 | 24
プロパティ | [プロパティ](#properties) | プロパティ アクセサーはすべて静的、すべて仮想、またはすべてインスタンスになる。 | 26
プロパティ | [プロパティ](#properties) | プロパティの型は、getter の戻り値の型であり、かつ setter の最後の引数の型でなければいけない。 プロパティのパラメーターの型は、getter へのパラメーターの型であり、かつ setter の最後のパラメーター以外のすべての型でなければいけない。 すべての型は CLS 準拠でなければならない。また、マネージド ポインターであってはいけない (つまり、参照渡しではいけない)。 | 27
プロパティ | [プロパティ](#properties) | プロパティは、特定の名前付けパターンに従わなくてはいけない。 CLS 規則 24 で触れられている `SpecialName` 属性は、適切な名前比較で無視され、識別子規則に従わなければいけない。 プロパティには getter メソッド、setter メソッド、またはこの両方が必ずなければいけない。 | 28
型変換 | [型変換](#type-conversion) | op_Implicit または op_Explicit が指定されている場合は、強制変換のための別の方法を用意する必要がある。 | 39
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | ボックス化された値型は CLS 準拠ではない。 | 3
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | シグネチャに出現するすべての型は CLS 準拠でなければいけない。 ジェネリック型のインスタンスを構成するすべての型は CLS 準拠でなければいけない。 | 11
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | 型指定された参照は CLS 準拠ではありません。 | 14
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | アンマネージ ポインター型は CLS 準拠ではない。 | 17
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | CLS 準拠のクラス、値型、およびインターフェイスでは、CLS に準拠しないメンバーの実装は不要である。 | 20
型 | [型および型メンバーのシグネチャ](#types-and-type-member-signatures) | [System.Object](xref:System.Object) は CLS 準拠である。 これ以外のあらゆる CLS 準拠クラスは CLS 準拠クラスの継承でなければならない。 | 23

### <a name="types-and-type-member-signatures"></a>型および型メンバーのシグネチャ

[System.Object](xref:System.Object) 型は CLS に準拠しており、.NET Framework 型システムのすべてのオブジェクト型の基本型です。 .NET Framework の継承は暗黙的また明示的に行われます。たとえば、[String](xref:System.String) クラスは `Object` クラスから暗黙的に継承します。また、[CultureNotFoundException](xref:System.Globalization.CultureNotFoundException) クラスは、[ArgumentException](xref:System.ArgumentException) クラスから明示的に継承し、これは [Exception](xref:System.Exception) クラスから明示的に継承します。 派生型を CLS 準拠にするには、その基本型も CLS に準拠している必要があります。

次の例は、基本型が CLS に準拠していない派生型を示しています。 これは、符号なし 32 ビット整数をカウンターとして使用する `Counter` 基底クラスを定義します。 クラスには、符号なし整数をラップすることでカウンター機能が用意されます。このため、クラスは CLS 非準拠としてマークされます。 結果として、派生クラス `NonZeroCounter` も CLS に準拠しなくなります。

```csharp
using System;

[assembly: CLSCompliant(true)]

[CLSCompliant(false)]
public class Counter
{
   UInt32 ctr;

   public Counter()
   {
      ctr = 0;
   }

   protected Counter(UInt32 ctr)
   {
      this.ctr = ctr;
   }

   public override string ToString()
   {
      return $"{ctr}). ";
   }

   public UInt32 Value
   {
      get { return ctr; }
   }

   public void Increment()
   {
      ctr += (uint) 1;
   }
}

public class NonZeroCounter : Counter
{
   public NonZeroCounter(int startIndex) : this((uint) startIndex)
   {
   }

   private NonZeroCounter(UInt32 startIndex) : base(startIndex)
   {
   }
}
// Compilation produces a compiler warning like the following:
//    Type3.cs(37,14): warning CS3009: 'NonZeroCounter': base type 'Counter' is not
//            CLS-compliant
//    Type3.cs(7,14): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>

<CLSCompliant(False)> _
Public Class Counter
   Dim ctr As UInt32

   Public Sub New
      ctr = 0
   End Sub

   Protected Sub New(ctr As UInt32)
      ctr = ctr
   End Sub

   Public Overrides Function ToString() As String
      Return $"{ctr}). "
   End Function

   Public ReadOnly Property Value As UInt32
      Get
         Return ctr
      End Get
   End Property

   Public Sub Increment()
      ctr += CType(1, UInt32)
   End Sub
End Class

Public Class NonZeroCounter : Inherits Counter
   Public Sub New(startIndex As Integer)
      MyClass.New(CType(startIndex, UInt32))
   End Sub

   Private Sub New(startIndex As UInt32)
      MyBase.New(CType(startIndex, UInt32))
   End Sub
End Class
' Compilation produces a compiler warning like the following:
'    Type3.vb(34) : warning BC40026: 'NonZeroCounter' is not CLS-compliant
'    because it derives from 'Counter', which is not CLS-compliant.
'
'    Public Class NonZeroCounter : Inherits Counter
'                 ~~~~~~~~~~~~~~
```

メソッドの戻り値の型、プロパティ型を含め、メンバー シグネチャに表示されるすべての型が CLS に準拠する必要があります。 さらに、ジェネリック型の場合は、次の要件もあります。

* ジェネリック型のインスタンスを構成するすべての型が、CLS に準拠する必要があります。

* ジェネリック パラメーターで制約として使用されるすべての型が、CLS に準拠する必要があります。

.NET [共通型システム](common-type-system.md)には、共通言語ランタイムが直接サポートする組み込み型がいくつか含まれ、アセンブリのメタデータで特別にエンコードされています。 これらの組み込み型のうち、次の表に示す型は CLS に準拠しています。

CLS 準拠型 | 説明
------------------ | -----------
[Byte](xref:System.Byte) | 8 ビット符号なし整数
[Int16](xref:System.Int16) | 16 ビット符号付き整数
[Int32](xref:System.Int32) | 32 ビット符号付き整数
[Int64](xref:System.Int64) | 64 ビット符号付き整数
[Single](xref:System.Single) | 単精度浮動小数点数値
[Double](xref:System.Double) | 倍精度浮動小数点数値
[Boolean](xref:System.Boolean) | true または false 値型
[Char](xref:System.Char) | UTF-16 でエンコードされたコード単位
[Decimal](xref:System.Decimal) | 10 進浮動小数点数以外
[IntPtr](xref:System.IntPtr) | プラットフォーム定義サイズのポインターまたはハンドル
[String](xref:System.String) | 0 個以上の Char オブジェクトのコレクション

次の表に示す組み込み型は CLS に準拠していません。

非準拠型 | 説明 | CLS に準拠する代替
------------------ | ----------- | -------------------------
[SByte](xref:System.SByte) | 8 ビット符号付き整数データ型 | [Int16](xref:System.Int16)
[UInt16](xref:System.UInt16) | 16 ビット符号なし整数 | [Int32](xref:System.Int32)
[UInt32](xref:System.UInt32) | 32 ビット符号なし整数 | [Int64](xref:System.Int64)
[UInt64](xref:System.UInt64) | 64 ビット符号なし整数 | [Int64](xref:System.Int64) (オーバーフローの可能性あり)、[BigInteger](xref:System.Numerics.BigInteger)、または [Double](xref:System.Double)
[UIntPtr](xref:System.UIntPtr) | 符号なしポインターまたはハンドル | [IntPtr](xref:System.IntPtr)

.NET Framework のクラス ライブラリまたはその他のクラス ライブラリには、CLS に準拠していない他の型が含まれる場合があります。次に例を示します。

* ボックス化された値型。 次の C# コード例では、`Value` という名前の型 `int`* のパブリック プロパティを持つクラスを作成します。 `int`* はボックス化された値型であるため、コンパイラは CLS 非準拠としてフラグを設定します。

```csharp
using System;

[assembly:CLSCompliant(true)]

public unsafe class TestClass
{
   private int* val;

   public TestClass(int number)
   {
      val = (int*) number;
   }

   public int* Value {
      get { return val; }
   }
}
// The compiler generates the following output when compiling this example:
//        warning CS3003: Type of 'TestClass.Value' is not CLS-compliant
```

* 型指定された参照。オブジェクトへの参照および型への参照を含む特別なコンストラクトです。

型が CLS に準拠していない場合は、*isCompliant* パラメータの値が `false` に指定された [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 属性をそれに適用する必要があります。 詳細については、「[CLSCompliantAttribute 属性](#the-clscompliantattribute-attribute)」を参照してください。

次の例は、メソッド シグネチャとジェネリック型のインスタンス化で発生する CLS 準拠の問題を示しています。 ここでは、`InvoiceItem` クラスを、型 [UInt32](xref:System.UInt32) のプロパティ、型 [Nullable(Of UInt32)](xref:System.Nullable%601) のプロパティ、および型 `UInt32` と `Nullable(Of UInt32)` のパラメーターが指定されたコンストラクターで定義します。 この例をコンパイルしようとすると、4 つのコンパイラの警告が表示されます。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<uint> qty;

   public InvoiceItem(uint sku, Nullable<uint> quantity)
   {
      itemId = sku;
      qty = quantity;
   }

   public Nullable<uint> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public uint InvoiceId
   {
      get { return invId; }
      set { invId = value; }
   }
}
// The attempt to compile the example displays the following output:
//    Type1.cs(13,23): warning CS3001: Argument type 'uint' is not CLS-compliant
//    Type1.cs(13,33): warning CS3001: Argument type 'uint?' is not CLS-compliant
//    Type1.cs(19,26): warning CS3003: Type of 'InvoiceItem.Quantity' is not CLS-compliant
//    Type1.cs(25,16): warning CS3003: Type of 'InvoiceItem.InvoiceId' is not CLS-compliant
```

```vb
<Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of UInteger)

   Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
      itemId = sku
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of UInteger)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As UInteger
      Get
         Return invId
      End Get
      Set
         invId = value
      End Set
   End Property
End Class
' The attempt to compile the example displays output similar to the following:
'    Type1.vb(13) : warning BC40028: Type of parameter 'sku' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                      ~~~
'    Type1.vb(13) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Sub New(sku As UInteger, quantity As Nullable(Of UInteger))
'                                                               ~~~~~~~~
'    Type1.vb(18) : warning BC40041: Type 'UInteger' is not CLS-compliant.
'
'       Public Property Quantity As Nullable(Of UInteger)
'                                               ~~~~~~~~
'    Type1.vb(27) : warning BC40027: Return type of function 'InvoiceId' is not CLS-compliant.
'
'       Public Property InvoiceId As UInteger
```

コンパイラの警告が表示されないようにするには、`InvoiceItem` パブリック インターフェイスの CLS 非準拠型を準拠型に置き換えます。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class InvoiceItem
{
   private uint invId = 0;
   private uint itemId = 0;
   private Nullable<int> qty;

   public InvoiceItem(int sku, Nullable<int> quantity)
   {
      if (sku <= 0)
         throw new ArgumentOutOfRangeException("The item number is zero or negative.");
      itemId = (uint) sku;

      qty = quantity;
   }

   public Nullable<int> Quantity
   {
      get { return qty; }
      set { qty = value; }
   }

   public int InvoiceId
   {
      get { return (int) invId; }
      set {
         if (value <= 0)
            throw new ArgumentOutOfRangeException("The invoice number is zero or negative.");
         invId = (uint) value; }
   }
}
```

```vb
Assembly: CLSCompliant(True)>

Public Class InvoiceItem

   Private invId As UInteger = 0
   Private itemId As UInteger = 0
   Private qty AS Nullable(Of Integer)

   Public Sub New(sku As Integer, quantity As Nullable(Of Integer))
      If sku <= 0 Then
         Throw New ArgumentOutOfRangeException("The item number is zero or negative.")
      End If
      itemId = CUInt(sku)
      qty = quantity
   End Sub

   Public Property Quantity As Nullable(Of Integer)
      Get
         Return qty
      End Get
      Set
         qty = value
      End Set
   End Property

   Public Property InvoiceId As Integer
      Get
         Return CInt(invId)
      End Get
      Set
         invId = CUInt(value)
      End Set
   End Property
End Class
```

ここで示した特定の型以外にも、CLS に準拠していないカテゴリはいくつかあります。 たとえば、アンマネージ ポインター型やアンマネージ関数ポインター型などです。 次の例では、整数へのポインターを使用して整数の配列を作成するので、コンパイラの警告が生成されます。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ArrayHelper
{
   unsafe public static Array CreateInstance(Type type, int* ptr, int items)
   {
      Array arr = Array.CreateInstance(type, items);
      int* addr = ptr;
      for (int ctr = 0; ctr < items; ctr++) {
          int value = *addr;
          arr.SetValue(value, ctr);
          addr++;
      }
      return arr;
   }
}
// The attempt to compile this example displays the following output:
//    UnmanagedPtr1.cs(8,57): warning CS3001: Argument type 'int*' is not CLS-compliant
```

CLS 準拠の抽象クラス (つまり、C# で `abstract` とマークされたクラス) については、そのクラスのすべてのメンバーも CLS に準拠にする必要があります。

### <a name="naming-conventions"></a>名前付け規則

一部のプログラミング言語は大文字と小文字が区別されるので、識別子 (名前空間、型、メンバーの名前など) は、大文字と小文字が違うだけで相違します。 2 つの識別子が同じと見なされるのは、小文字マッピングが同じ場合です。 次の C# コード例では、2 つのパブリック クラス、`Person` および `person` を定義します。 このクラスは大文字と小文字のみが異なるので、コンパイラは CLS 非準拠としてフラグを設定します。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Person : person
{

}

public class person
{

}
// Compilation produces a compiler warning like the following:
//    Naming1.cs(11,14): warning CS3005: Identifier 'person' differing
//                       only in case is not CLS-compliant
//    Naming1.cs(6,14): (Location of symbol related to previous warning)
```

名前空間、型、メンバーの名前など、プログラミング言語の識別子は [Unicode Standard 3.0 の Technical Report 15 の Annex 7](https://www.unicode.org/reports/tr15/tr15-18.html) に準拠する必要があります。 これによって、次のことが起こります。

* 識別子の最初の文字は Unicode の大文字と小文字、大文字と小文字の組み合わせ、修飾子文字、その他の文字、または文字数の番号を指定できます。 Unicode 文字のカテゴリの詳細については、「[System.Globalization.UnicodeCategory](xref:System.Globalization.UnicodeCategory) 列挙体」を参照してください。

* 2 文字目以降には、最初の文字で使用できる文字のほかに、非空白記号、空白結合記号、10 進数、接続符号、書式指定コードを使用できます。

識別子を比較する場合は、その前に書式設定コードを除外してから、識別子を Unicode 正規形 C に変換する必要があります。これは 1 つの文字を、UTF-16 でエンコードされた複数のコード単位で表すことができるからです。 同じコード単位を Unicode 正規形 C で生成する文字シーケンスは CLS に準拠していません。 次の例では、オングストローム文字 (U+212B) である `Å` という名前のプロパティを定義し、次に、上に丸が付く LATIN の大文字 A (U+00C5) である `Å` という名前のプロパティを定義します。 C# コンパイラは、ソース コードを CLS 非準拠としてフラグします。

```csharp
public class Size
{
   private double a1;
   private double a2;

   public double Å
   {
       get { return a1; }
       set { a1 = value; }
   }

   public double Å
   {
       get { return a2; }
       set { a2 = value; }
   }
}
// Compilation produces a compiler warning like the following:
//    Naming2a.cs(16,18): warning CS3005: Identifier 'Size.Å' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(10,18): (Location of symbol related to previous warning)
//    Naming2a.cs(18,8): warning CS3005: Identifier 'Size.Å.get' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(12,8): (Location of symbol related to previous warning)
//    Naming2a.cs(19,8): warning CS3005: Identifier 'Size.Å.set' differing only in case is not
//            CLS-compliant
//    Naming2a.cs(13,8): (Location of symbol related to previous warning)
```

```vb
<Assembly: CLSCompliant(True)>
Public Class Size
   Private a1 As Double
   Private a2 As Double

   Public Property Å As Double
       Get
          Return a1
       End Get
       Set
          a1 = value
       End Set
   End Property

   Public Property Å As Double
       Get
          Return a2
       End Get
       Set
          a2 = value
       End Set
   End Property
End Class
' Compilation produces a compiler warning like the following:
'    Naming1.vb(9) : error BC30269: 'Public Property Å As Double' has multiple definitions
'     with identical signatures.
'
'       Public Property Å As Double
'                       ~
```

特定のスコープ内のメンバー名 (アセンブリ内の名前空間、名前空間内の型、型内のメンバーなど) は一意である必要があります。ただし、オーバーロードによって解決される名前は除きます。 この要件は、共通型システムの要件よりも厳格です。共通型システムでは、スコープ内のメンバーは種類が異なっていれば、たとえば、種類がメソッドのメンバーとフィールドのメンバーは、同じ名前を持つことができます。 特に、型メンバーの場合は次の要件もあります。

* フィールドと入れ子になった型は名前でのみ識別されます。

* 名前が同じメソッド、プロパティ、およびイベントは、戻り値の型以外で区別できるようにする必要があります。

次の例は、メンバー名がスコープ内で一意でなければならない要件を示しています。 ここでは、`Converter` という名前の 4 つのメンバーを含む `Conversion` という名前のクラスを定義します。 3 つがメソッドで、1 つはプロパティです。 `Int64` パラメーターを含むメソッドには一意の名前が付けられますが、`Int32` パラメーターが指定された 2 つのメソッドには一意の名前は付けられません。これは戻り値がメンバーのシグネチャの一部と見なされないからです。 また、`Conversion` プロパティもこの要件に違反しています。プロパティの名前は、オーバーロードされたメソッドと同じにできないからです。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Converter
{
   public double Conversion(int number)
   {
      return (double) number;
   }

   public float Conversion(int number)
   {
      return (float) number;
   }

   public double Conversion(long number)
   {
      return (double) number;
   }

   public bool Conversion
   {
      get { return true; }
   }
}
// Compilation produces a compiler error like the following:
//    Naming3.cs(13,17): error CS0111: Type 'Converter' already defines a member called
//            'Conversion' with the same parameter types
//    Naming3.cs(8,18): (Location of symbol related to previous error)
//    Naming3.cs(23,16): error CS0102: The type 'Converter' already contains a definition for
//            'Conversion'
//    Naming3.cs(8,18): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Converter
   Public Function Conversion(number As Integer) As Double
      Return CDbl(number)
   End Function

   Public Function Conversion(number As Integer) As Single
      Return CSng(number)
   End Function

   Public Function Conversion(number As Long) As Double
      Return CDbl(number)
   End Function

   Public ReadOnly Property Conversion As Boolean
      Get
         Return True
      End Get
   End Property
End Class
' Compilation produces a compiler error like the following:
'    Naming3.vb(8) : error BC30301: 'Public Function Conversion(number As Integer) As Double'
'                    and 'Public Function Conversion(number As Integer) As Single' cannot
'                    overload each other because they differ only by return types.
'
'       Public Function Conversion(number As Integer) As Double
'                       ~~~~~~~~~~
'    Naming3.vb(20) : error BC30260: 'Conversion' is already declared as 'Public Function
'                     Conversion(number As Integer) As Single' in this class.
'
'       Public ReadOnly Property Conversion As Boolean
'                                ~~~~~~~~~~
```

個々の言語に一意のキーワードが含まれるので、共通言語ランタイムを対象にする言語も、キーワードと一致する識別子 (型名など) を参照するための機構を用意する必要があります。 たとえば、`case` は、C# と Visual Basic のキーワードです。 ただし、次の Visual Basic コード例では、左右の中かっこを使用して、`case` キーワードと `case` という名前のクラスを明確に区別できます。 それ以外の場合は、エラー メッセージ "キーワードは、識別子として有効ではありません" が表示され、コンパイルできません。

```vb
Public Class [case]
   Private _id As Guid
   Private name As String

   Public Sub New(name As String)
      _id = Guid.NewGuid()
      Me.name = name
   End Sub

   Public ReadOnly Property ClientName As String
      Get
         Return name
      End Get
   End Property
End Class
```

次の C# コード例では、@ シンボルを使用して `case` クラスをインスタンス化することで、識別子と言語キーワードを区別できます。 これがないと、C# コンパイラによって 2 つのエラー メッセージ "型が必要です" および "'case' は無効です" が表示されます。

```csharp
using System;

public class Example
{
   public static void Main()
   {
      @case c = new @case("John");
      Console.WriteLine(c.ClientName);
   }
}
```

### <a name="type-conversion"></a>型変換

共通言語仕様では、次の 2 つの変換演算子が定義されます。

* `op_Implicit`。データまたは精度の損失につながらない拡大変換に使用されます。 たとえば、[Decimal](xref:System.Decimal) 構造体には、整数型の値と [Char](xref:System.Char) 値を `Decimal` 値に変換できるように、オーバーロードされた `op_Implicit` 演算子が含まれます。

* `op_Explicit`。絶対値 (狭い範囲の値に変換される値) または精度の損失につながる可能性がある縮小変換に使用されます。 たとえば、`Decimal` 構造体には、[Double](xref:System.Double) 値と [Single](xref:System.Single) 値を `Decimal` に変換し、`Decimal` 値を整数値、`Double`、`Single`、および `Char` に変換できるように、オーバーロードされた `op_Explicit` 演算子が含まれます。

ただし、すべての言語で、演算子のオーバーロードまたはカスタム演算子の定義がサポートされているわけではありません。 これらの変換演算子を実装する場合は、他の方法で変換を実行する方法も用意する必要があります。 ここでは、`From`Xxx メソッドおよび`To`Xxx メソッドを用意することをお勧めします。

次の例では、CLS に準拠する暗黙的な変換と明示的な変換を定義します。 ここでは、符号付き倍精度浮動小数点数を表す `UDouble` クラスを作成します。 暗黙的な変換については、`UDouble` から `Double`、明示的な変換については、`UDouble` から `Single`、`Double` から `UDouble`、および `Single` から `UDouble` への例を示しています。 また、暗黙的な変換演算子の代替として `ToDouble` メソッドを、明示的な変換演算子の代替として `ToSingle`、`FromDouble`、`FromSingle` の各メソッドを定義します。

```csharp
using System;

public struct UDouble
{
   private double number;

   public UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      number = value;
   }

   public static readonly UDouble MinValue = (UDouble) 0.0;
   public static readonly UDouble MaxValue = (UDouble) Double.MaxValue;

   public static explicit operator Double(UDouble value)
   {
      return value.number;
   }

   public static implicit operator Single(UDouble value)
   {
      if (value.number > (double) Single.MaxValue)
         throw new InvalidCastException("A UDouble value is out of range of the Single type.");

      return (float) value.number;
   }

   public static explicit operator UDouble(double value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static implicit operator UDouble(float value)
   {
      if (value < 0)
         throw new InvalidCastException("A negative value cannot be converted to a UDouble.");

      return new UDouble(value);
   }

   public static Double ToDouble(UDouble value)
   {
      return (Double) value;
   }

   public static float ToSingle(UDouble value)
   {
      return (float) value;
   }

   public static UDouble FromDouble(double value)
   {
      return new UDouble(value);
   }

   public static UDouble FromSingle(float value)
   {
      return new UDouble(value);
   }
}
```

```vb
Public Structure UDouble
   Private number As Double

   Public Sub New(value As Double)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Sub New(value As Single)
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      number = value
   End Sub

   Public Shared ReadOnly MinValue As UDouble = CType(0.0, UDouble)
   Public Shared ReadOnly MaxValue As UDouble = Double.MaxValue

   Public Shared Widening Operator CType(value As UDouble) As Double
      Return value.number
   End Operator

   Public Shared Narrowing Operator CType(value As UDouble) As Single
      If value.number > CDbl(Single.MaxValue) Then
         Throw New InvalidCastException("A UDouble value is out of range of the Single type.")
      End If
      Return CSng(value.number)
   End Operator

   Public Shared Narrowing Operator CType(value As Double) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Narrowing Operator CType(value As Single) As UDouble
      If value < 0 Then
         Throw New InvalidCastException("A negative value cannot be converted to a UDouble.")
      End If
      Return New UDouble(value)
   End Operator

   Public Shared Function ToDouble(value As UDouble) As Double
      Return CType(value, Double)
   End Function

   Public Shared Function ToSingle(value As UDouble) As Single
      Return CType(value, Single)
   End Function

   Public Shared Function FromDouble(value As Double) As UDouble
      Return New UDouble(value)
   End Function

   Public Shared Function FromSingle(value As Single) As UDouble
      Return New UDouble(value)
   End Function
End Structure
```

### <a name="arrays"></a>配列

CLS 準拠の配列は、次の規則に従います。

* 配列の次元の下限値は 0 にする必要があります。 次の例では、下限が 1 の CLS 非準拠の配列を作成します。 [CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 属性の有無に関係なく、コンパイラでは、`Numbers.GetTenPrimes` メソッドによって返される配列が CLS に準拠していないことは検出されません。

  ```csharp
  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static Array GetTenPrimes()
    {
        Array arr = Array.CreateInstance(typeof(Int32), new int[] {10}, new int[] {1});
        arr.SetValue(1, 1);
        arr.SetValue(2, 2);
        arr.SetValue(3, 3);
        arr.SetValue(5, 4);
        arr.SetValue(7, 5);
        arr.SetValue(11, 6);
        arr.SetValue(13, 7);
        arr.SetValue(17, 8);
        arr.SetValue(19, 9);
        arr.SetValue(23, 10);

        return arr;
    }
  }
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As Array
        Dim arr As Array = Array.CreateInstance(GetType(Int32), {10}, {1})
        arr.SetValue(1, 1)
        arr.SetValue(2, 2)
        arr.SetValue(3, 3)
        arr.SetValue(5, 4)
        arr.SetValue(7, 5)
        arr.SetValue(11, 6)
        arr.SetValue(13, 7)
        arr.SetValue(17, 8)
        arr.SetValue(19, 9)
        arr.SetValue(23, 10)
        Return arr
     End Function
  End Class
  ```

* すべての配列の要素が、CLS 準拠の型で構成されている必要があります。 次の例では、CLS 非準拠の配列を返す 2 つのメソッドを定義します。 最初のメソッドは、[UInt32](xref:System.UInt32) 値の配列を返します。 2 番目のメソッドは [Int32](xref:System.Int32) 値と `UInt32` 値を含む[Object](xref:System.Object) 配列を返します。 最初の配列は `UInt32` 型であるため、コンパイラによって非準拠として識別されますが、2 番目の配列に CLS 非準拠の要素が含まれていることは認識されません。

  ```csharp
  using System;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static UInt32[] GetTenPrimes()
    {
        uint[] arr = { 1u, 2u, 3u, 5u, 7u, 11u, 13u, 17u, 19u };
        return arr;
    }

    public static Object[] GetFivePrimes()
    {
        Object[] arr = { 1, 2, 3, 5u, 7u };
        return arr;
    }
  }
  // Compilation produces a compiler warning like the following:
  //    Array2.cs(8,27): warning CS3002: Return type of 'Numbers.GetTenPrimes()' is not
  //            CLS-compliant
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Class Numbers
     Public Shared Function GetTenPrimes() As UInt32()
        Return { 1ui, 2ui, 3ui, 5ui, 7ui, 11ui, 13ui, 17ui, 19ui }
     End Function
     Public Shared Function GetFivePrimes() As Object()
        Dim arr() As Object = { 1, 2, 3, 5ui, 7ui }
        Return arr
     End Function
  End Class
  ' Compilation produces a compiler warning like the following:
  '    warning BC40027: Return type of function 'GetTenPrimes' is not CLS-compliant.
  ```

* 配列パラメーターを持つメソッドのオーバーロードの解決は、配列であるという事実とその要素型に基づきます。 したがって、次のオーバーロードされた `GetSquares` メソッドの定義は CLS に準拠しています。

  ```csharp
  using System;
  using System.Numerics;

  [assembly: CLSCompliant(true)]

  public class Numbers
  {
    public static byte[] GetSquares(byte[] numbers)
    {
        byte[] numbersOut = new byte[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++) {
            int square = ((int) numbers[ctr]) * ((int) numbers[ctr]);
            if (square <= Byte.MaxValue)
                numbersOut[ctr] = (byte) square;
            // If there's an overflow, assign MaxValue to the corresponding
            // element.
            else
                numbersOut[ctr] = Byte.MaxValue;

        }
        return numbersOut;
    }

    public static BigInteger[] GetSquares(BigInteger[] numbers)
  {
        BigInteger[] numbersOut = new BigInteger[numbers.Length];
        for (int ctr = 0; ctr < numbers.Length; ctr++)
            numbersOut[ctr] = numbers[ctr] * numbers[ctr];

       return numbersOut;
    }
  }
  ```

  ```vb
  Imports System.Numerics

  <Assembly: CLSCompliant(True)>

  Public Module Numbers
     Public Function GetSquares(numbers As Byte()) As Byte()
        Dim numbersOut(numbers.Length - 1) As Byte
        For ctr As Integer = 0 To numbers.Length - 1
           Dim square As Integer = (CInt(numbers(ctr)) * CInt(numbers(ctr)))
           If square <= Byte.MaxValue Then
              numbersOut(ctr) = CByte(square)
           ' If there's an overflow, assign MaxValue to the corresponding
           ' element.
           Else
              numbersOut(ctr) = Byte.MaxValue
           End If
        Next
        Return numbersOut
     End Function

     Public Function GetSquares(numbers As BigInteger()) As BigInteger()
         Dim numbersOut(numbers.Length - 1) As BigInteger
         For ctr As Integer = 0 To numbers.Length - 1
            numbersOut(ctr) = numbers(ctr) * numbers(ctr)
         Next
         Return numbersOut
     End Function
  End Module
  ```

### <a name="interfaces"></a>インターフェイス

CLS 準拠のインターフェイスでは、プロパティ、イベント、および仮想メソッド (実装のないメソッド) を定義できます。 次の項目は、このインターフェイスには指定できません。

* 静的メソッドまたは静的フィールド。 インターフェイスで静的メンバーを定義すると、C# コンパイラでコンパイラ エラーが発生します。

* フィールド。 インターフェイスでフィールドを定義すると、C# コンパイラでコンパイラ エラーが発生します。

* CLS に準拠していないメソッド。 たとえば、次のインターフェイス定義には、CLS 非準拠とマークされているメソッド、`INumber.GetUnsigned` が含まれています。 この例では、コンパイラの警告が生成されます。

  ```csharp
  using System;

  [assembly:CLSCompliant(true)]

  public interface INumber
  {
      int Length();
      [CLSCompliant(false)] ulong GetUnsigned();
  }
  // Attempting to compile the example displays output like the following:
  //    Interface2.cs(8,32): warning CS3010: 'INumber.GetUnsigned()': CLS-compliant interfaces
  //            must have only CLS-compliant members
  ```

  ```vb
  <Assembly: CLSCompliant(True)>

  Public Interface INumber
    Function Length As Integer
      <CLSCompliant(False)> Function GetUnsigned As ULong
    End Interface
    ' Attempting to compile the example displays output like the following:
    '    Interface2.vb(9) : warning BC40033: Non CLS-compliant 'function' is not allowed in a
    '    CLS-compliant interface.
    '
    '       <CLSCompliant(False)> Function GetUnsigned As ULong
    '                                      ~~~~~~~~~~~
  ```

  この規則のため、CLS に準拠している型は、CLS に準拠していないメンバーを実装する必要はありません。 CLS 準拠のフレームワークによって、CLS に非準拠のインターフェイスを実装するクラスが公開されている場合、そのフレームワークには、CLS 非準拠のすべてのメンバーの具象実装も用意する必要があります。

CLS 準拠の言語コンパイラでは、クラスによって、複数のインターフェイスにある同じ名前およびシグネチャを持つメンバーを個別に実装できるようにすることも必要です。 C# は明示的なインターフェイス実装をサポートしており、同じ名前を持つメソッドを別々に実装できます。 次の例は、明示的なインターフェイス実装として `Temperature` インターフェイスおよび `ICelsius` インターフェイスを実装する `IFahrenheit` クラスを定義することで、このシナリオを示しています。

```csharp
using System;

[assembly: CLSCompliant(true)]

public interface IFahrenheit
{
   decimal GetTemperature();
}

public interface ICelsius
{
   decimal GetTemperature();
}

public class Temperature : ICelsius, IFahrenheit
{
   private decimal _value;

   public Temperature(decimal value)
   {
      // We assume that this is the Celsius value.
      _value = value;
   }

   decimal IFahrenheit.GetTemperature()
   {
      return _value * 9 / 5 + 32;
   }

   decimal ICelsius.GetTemperature()
   {
      return _value;
   }
}
public class Example
{
   public static void Main()
   {
      Temperature temp = new Temperature(100.0m);
      ICelsius cTemp = temp;
      IFahrenheit fTemp = temp;
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        cTemp.GetTemperature());
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        fTemp.GetTemperature());
   }
}
// The example displays the following output:
//       Temperature in Celsius: 100.0 degrees
//       Temperature in Fahrenheit: 212.0 degrees
```

```vb
Assembly: CLSCompliant(True)>

Public Interface IFahrenheit
   Function GetTemperature() As Decimal
End Interface

Public Interface ICelsius
   Function GetTemperature() As Decimal
End Interface

Public Class Temperature : Implements ICelsius, IFahrenheit
   Private _value As Decimal

   Public Sub New(value As Decimal)
      ' We assume that this is the Celsius value.
      _value = value
   End Sub

   Public Function GetFahrenheit() As Decimal _
          Implements IFahrenheit.GetTemperature
      Return _value * 9 / 5 + 32
   End Function

   Public Function GetCelsius() As Decimal _
          Implements ICelsius.GetTemperature
      Return _value
   End Function
End Class

Module Example
   Public Sub Main()
      Dim temp As New Temperature(100.0d)
      Console.WriteLine("Temperature in Celsius: {0} degrees",
                        temp.GetCelsius())
      Console.WriteLine("Temperature in Fahrenheit: {0} degrees",
                        temp.GetFahrenheit())
   End Sub
End Module
' The example displays the following output:
'       Temperature in Celsius: 100.0 degrees
'       Temperature in Fahrenheit: 212.0 degrees
```

### <a name="enumerations"></a>列挙

CLS 準拠の列挙型は、次の規則に従う必要があります。

* 列挙体の基になる型は、組み込みの CLS 準拠の整数 ([Byte](xref:System.Byte)、[Int16](xref:System.Int16)、[Int32](xref:System.Int32)、または [Int64](xref:System.Int64)) である必要があります。 たとえば、次のコードでは、基になる型が [UInt32](xref:System.UInt32) の列挙体を定義しようとしますが、コンパイラの警告が生成されます。

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public enum Size : uint {
        Unspecified = 0,
        XSmall = 1,
        Small = 2,
        Medium = 3,
        Large = 4,
        XLarge = 5
    };

    public class Clothing
    {
        public string Name;
        public string Type;
        public string Size;
    }
    // The attempt to compile the example displays a compiler warning like the following:
    //    Enum3.cs(6,13): warning CS3009: 'Size': base type 'uint' is not CLS-compliant
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Enum Size As UInt32
       Unspecified = 0
       XSmall = 1
       Small = 2
       Medium = 3
       Large = 4
       XLarge = 5
    End Enum

    Public Class Clothing
       Public Name As String
       Public Type As String
       Public Size As Size
    End Class
    ' The attempt to compile the example displays a compiler warning like the following:
    '    Enum3.vb(6) : warning BC40032: Underlying type 'UInt32' of Enum is not CLS-compliant.
    '
    '    Public Enum Size As UInt32
    '                ~~~~
    ```

* 列挙型には、`Value__` 属性でマークされた `FieldAttributes.RTSpecialName` という名前の単一インスタンス フィールドが必要です。 これにより、フィールド値を暗黙的に参照できます。

* 列挙体には、その列挙体自体の型と同じ型を持つリテラルな静的フィールドが含まれます。 たとえば、`State` および `State.On` の値を持つ `State.Off` 列挙体を定義すると、`State.On` と `State.Off` は両方ともリテラルな静的フィールドで、その型は `State` です。

* 列挙体は 2 種類あります。

  * 同時に指定できない一連の名前付き整数値を表す列挙体。 この列挙体の型は、[System.FlagsAttribute](xref:System.FlagsAttribute) カスタム属性が存在しないことによって示されます。

  * 名前のない値を生成するために結合できる一連のビット フラグを表す列挙体。 この列挙体の型は、 [System.FlagsAttribute](xref:System.FlagsAttribute) カスタム属性が存在することによって示されます。

詳細については、[Enum](xref:System.Enum) 構造体のドキュメントを参照してください。

* 列挙体の値は、その列挙体の指定された値に限定されません。 つまり、列挙体の値の範囲は、その列挙体の基になる値の範囲です。 `Enum.IsDefined` メソッドを使用すると、指定された値が列挙体のメンバーかどうかを確認できます。

### <a name="type-members-in-general"></a>一般的な型メンバー

共通言語仕様では、すべてのフィールドとメソッドに、特定のクラスのメンバーとしてアクセスする必要があります。 したがって、グローバルな静的フィールドおよび静的メソッド (つまり、型とは別に定義された静的フィールドまたは静的メソッド) は CLS に準拠していません。 グローバル フィールドまたはグローバル メソッドをソース コードに追加しようとすると、C# コンパイラでコンパイラ エラーが発生します。

共通言語仕様では、標準のマネージド呼び出し規約のみがサポートされます。 アンマネージ呼び出し規約と、`varargs` キーワードでマークされた可変個引数リストを持つメソッドはサポートされません。 標準のマネージド呼び出し規則と互換性がある可変個引数リストについては、[ParamArrayAttribute](xref:System.ParamArrayAttribute) 属性、または `params` キーワード (C# の場合)、`ParamArray` キーワード (Visual Basic の場合) などの個々の言語の実装を使用します。

### <a name="member-accessibility"></a>メンバーのアクセシビリティ

継承されたメンバーをオーバーライドしても、そのメンバーのアクセシビリティは変更できません。 たとえば、基底クラスのパブリック メソッドは、派生クラスのプライベート メソッドではオーバーライドできません。 ただし例外が 1 つあります。あるアセンブリ内にある、別のアセンブリの型でオーバーライドされた `protected internal` メンバー (C# の場合) または `Protected Friend` メンバー (Visual Basic の場合) です。  この場合、オーバーライドのアクセシビリティは `Protected` です。

次の例は、[CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 属性が `true` に設定されており、`Animal` の派生クラス `Person` が、`Species` プロパティのアクセシビリティをパブリックからプライベートに変更しようとするときに発生するエラーを示しています。 アクセシビリティがパブリックに変更されると、コンパイルが正常に行われます。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class Animal
{
   private string _species;

   public Animal(string species)
   {
      _species = species;
   }

   public virtual string Species
   {
      get { return _species; }
   }

   public override string ToString()
   {
      return _species;
   }
}

public class Human : Animal
{
   private string _name;

   public Human(string name) : base("Homo Sapiens")
   {
      _name = name;
   }

   public string Name
   {
      get { return _name; }
   }

   private override string Species
   {
      get { return base.Species; }
   }

   public override string ToString()
   {
      return _name;
   }
}

public class Example
{
   public static void Main()
   {
      Human p = new Human("John");
      Console.WriteLine(p.Species);
      Console.WriteLine(p.ToString());
   }
}
// The example displays the following output:
//    error CS0621: 'Human.Species': virtual or abstract members cannot be private
```

```vb
<Assembly: CLSCompliant(True)>

Public Class Animal
   Private _species As String

   Public Sub New(species As String)
      _species = species
   End Sub

   Public Overridable ReadOnly Property Species As String
      Get
         Return _species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _species
   End Function
End Class

Public Class Human : Inherits Animal
   Private _name As String

   Public Sub New(name As String)
      MyBase.New("Homo Sapiens")
      _name = name
   End Sub

   Public ReadOnly Property Name As String
      Get
         Return _name
      End Get
   End Property

   Private Overrides ReadOnly Property Species As String
      Get
         Return MyBase.Species
      End Get
   End Property

   Public Overrides Function ToString() As String
      Return _name
   End Function
End Class

Public Module Example
   Public Sub Main()
      Dim p As New Human("John")
      Console.WriteLine(p.Species)
      Console.WriteLine(p.ToString())
   End Sub
End Module
' The example displays the following output:
'     'Private Overrides ReadOnly Property Species As String' cannot override
'     'Public Overridable ReadOnly Property Species As String' because
'      they have different access levels.
'
'         Private Overrides ReadOnly Property Species As String
```

メンバーにアクセスできる場合は必ず、そのメンバーのシグネチャの型にアクセスできなければなりません。 たとえば、これは、パブリック メンバーには、型がプライベート、プロテクト、または内部のパラメーターを含められないことを意味します。 次の例は、`StringWrapper` クラス コンストラクターが、文字列値のラップ方法を決定する内部 `StringOperationType` 列挙値を公開した場合に発生するコンパイラ エラーを示しています。

```csharp
using System;
using System.Text;

public class StringWrapper
{
   string internalString;
   StringBuilder internalSB = null;
   bool useSB = false;

   public StringWrapper(StringOperationType type)
   {
      if (type == StringOperationType.Normal) {
         useSB = false;
      }
      else {
         useSB = true;
         internalSB = new StringBuilder();
      }
   }

   // The remaining source code...
}

internal enum StringOperationType { Normal, Dynamic }
// The attempt to compile the example displays the following output:
//    error CS0051: Inconsistent accessibility: parameter type
//            'StringOperationType' is less accessible than method
//            'StringWrapper.StringWrapper(StringOperationType)'
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class StringWrapper

   Dim internalString As String
   Dim internalSB As StringBuilder = Nothing
   Dim useSB As Boolean = False

   Public Sub New(type As StringOperationType)
      If type = StringOperationType.Normal Then
         useSB = False
      Else
         internalSB = New StringBuilder()
         useSB = True
      End If
   End Sub

   ' The remaining source code...
End Class

Friend Enum StringOperationType As Integer
   Normal = 0
   Dynamic = 1
End Enum
' The attempt to compile the example displays the following output:
'    error BC30909: 'type' cannot expose type 'StringOperationType'
'     outside the project through class 'StringWrapper'.
'
'       Public Sub New(type As StringOperationType)
'                              ~~~~~~~~~~~~~~~~~~~
```

### <a name="generic-types-and-members"></a>ジェネリック型とメンバー

入れ子になった型には、少なくともその外側の型と同じ数のジェネリック パラメーターが必要です。 このジェネリック パラメーターは、外側の型のジェネリック パラメーターに位置によって対応します。 ジェネリック型に新しいジェネリック パラメーターを含めることもできます。

外側の型のジェネリック型パラメーターと、その入れ子になった型の関係は、個別の言語構文によって非表示になっていることがあります。 次の例では、入れ子になった 2 つのクラス、`Outer<T>` および `Inner1A` が、ジェネリック型 `Inner1B<U>` に含まれます。 各クラスが `Object.ToString` から継承した `ToString` メソッドへの呼び出しは、入れ子になった各クラスに、その外側のクラスの型パラメーターが含まれていることを示しています。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Outer<T>
{
   T value;

   public Outer(T value)
   {
      this.value = value;
   }

   public class Inner1A : Outer<T>
   {
      public Inner1A(T value) : base(value)
      {  }
   }

   public class Inner1B<U> : Outer<T>
   {
      U value2;

      public Inner1B(T value1, U value2) : base(value1)
      {
         this.value2 = value2;
      }
   }
}

public class Example
{
   public static void Main()
   {
      var inst1 = new Outer<String>("This");
      Console.WriteLine(inst1);

      var inst2 = new Outer<String>.Inner1A("Another");
      Console.WriteLine(inst2);

      var inst3 = new Outer<String>.Inner1B<int>("That", 2);
      Console.WriteLine(inst3);
   }
}
// The example displays the following output:
//       Outer`1[System.String]
//       Outer`1+Inner1A[System.String]
//       Outer`1+Inner1B`1[System.String,System.Int32]
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Outer(Of T)
   Dim value As T

   Public Sub New(value As T)
      Me.value = value
   End Sub

   Public Class Inner1A : Inherits Outer(Of T)
      Public Sub New(value As T)
         MyBase.New(value)
      End Sub
   End Class

   Public Class Inner1B(Of U) : Inherits Outer(Of T)
      Dim value2 As U

      Public Sub New(value1 As T, value2 As U)
         MyBase.New(value1)
         Me.value2 = value2
      End Sub
   End Class
End Class

Public Module Example
   Public Sub Main()
      Dim inst1 As New Outer(Of String)("This")
      Console.WriteLine(inst1)

      Dim inst2 As New Outer(Of String).Inner1A("Another")
      Console.WriteLine(inst2)

      Dim inst3 As New Outer(Of String).Inner1B(Of Integer)("That", 2)
      Console.WriteLine(inst3)
   End Sub
End Module
' The example displays the following output:
'       Outer`1[System.String]
'       Outer`1+Inner1A[System.String]
'       Outer`1+Inner1B`1[System.String,System.Int32]
```

ジェネリック型の名前は、フォーム *name*'*n* でエンコードされます。ここで、*name* は型の名前、 *`* は文字リテラル、*n* は型で宣言されたパラメーターの数、入れ子になったジェネリック型の場合は、新しく導入された型パラメーターの数です。 ジェネリック型の名前のエンコーディングは、主に、リフレクションを使用してライブラリ内の CLS 準拠のジェネリック型にアクセスする開発者が使用します。

制約がジェネリック型に適用される場合は、制約として使用されるすべての型も CLS に準拠している必要があります。 次の例では、CLS に準拠していない `BaseClass` という名前のクラスと、型パラメーターが `BaseCollection` から派生しなければならない `BaseClass` という名前のジェネリック クラスを定義します。 ただし、`BaseClass` が CLS に準拠していないので、コンパイラによって警告が生成されます。

```csharp
using System;

[assembly:CLSCompliant(true)]

[CLSCompliant(false)] public class BaseClass
{}

public class BaseCollection<T> where T : BaseClass
{}
// Attempting to compile the example displays the following output:
//    warning CS3024: Constraint type 'BaseClass' is not CLS-compliant
```

```vb
Assembly: CLSCompliant(True)>

<CLSCompliant(False)> Public Class BaseClass
End Class

Public Class BaseCollection(Of T As BaseClass)
End Class
' Attempting to compile the example displays the following output:
'    warning BC40040: Generic parameter constraint type 'BaseClass' is not
'    CLS-compliant.
'
'    Public Class BaseCollection(Of T As BaseClass)
'                                        ~~~~~~~~~
```

ジェネリック型がジェネリック基本型から派生している場合、そのジェネリック型は、基本型に対する制約も必ず満たすように制約を再宣言する必要があります。 次の例では、任意の数値型を表すことができる `Number<T>` を定義します。 また、浮動小数点値を表す `FloatingPoint<T>` クラスも定義します。 ただし、ソース コードはコンパイルされません。`Number<T>` (T は値型) の制約は `FloatingPoint<T>` に適用されないからです。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T>
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
// The attempt to compile the example displays the following output:
//       error CS0453: The type 'T' must be a non-nullable value type in
//               order to use it as parameter 'T' in the generic type or method 'Number<T>'
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
' The attempt to compile the example displays the following output:
'    error BC32105: Type argument 'T' does not satisfy the 'Structure'
'    constraint for type parameter 'T'.
'
'    Public Class FloatingPoint(Of T) : Inherits Number(Of T)
'                                                          ~
```

制約が `FloatingPoint<T>` クラスに追加されると、コンパイルが正常に行われます。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class Number<T> where T : struct
{
   // use Double as the underlying type, since its range is a superset of
   // the ranges of all numeric types except BigInteger.
   protected double number;

   public Number(T value)
   {
      try {
         this.number = Convert.ToDouble(value);
      }
      catch (OverflowException e) {
         throw new ArgumentException("value is too large.", e);
      }
      catch (InvalidCastException e) {
         throw new ArgumentException("The value parameter is not numeric.", e);
      }
   }

   public T Add(T value)
   {
      return (T) Convert.ChangeType(number + Convert.ToDouble(value), typeof(T));
   }

   public T Subtract(T value)
   {
      return (T) Convert.ChangeType(number - Convert.ToDouble(value), typeof(T));
   }
}

public class FloatingPoint<T> : Number<T> where T : struct
{
   public FloatingPoint(T number) : base(number)
   {
      if (typeof(float) == number.GetType() ||
          typeof(double) == number.GetType() ||
          typeof(decimal) == number.GetType())
         this.number = Convert.ToDouble(number);
      else
         throw new ArgumentException("The number parameter is not a floating-point number.");
   }
}
```

```vb
<Assembly:CLSCompliant(True)>

Public Class Number(Of T As Structure)
   ' Use Double as the underlying type, since its range is a superset of
   ' the ranges of all numeric types except BigInteger.
   Protected number As Double

   Public Sub New(value As T)
      Try
         Me.number = Convert.ToDouble(value)
      Catch e As OverflowException
         Throw New ArgumentException("value is too large.", e)
      Catch e As InvalidCastException
         Throw New ArgumentException("The value parameter is not numeric.", e)
      End Try
   End Sub

   Public Function Add(value As T) As T
      Return CType(Convert.ChangeType(number + Convert.ToDouble(value), GetType(T)), T)
   End Function

   Public Function Subtract(value As T) As T
      Return CType(Convert.ChangeType(number - Convert.ToDouble(value), GetType(T)), T)
   End Function
End Class

Public Class FloatingPoint(Of T As Structure) : Inherits Number(Of T)
   Public Sub New(number As T)
      MyBase.New(number)
      If TypeOf number Is Single Or
               TypeOf number Is Double Or
               TypeOf number Is Decimal Then
         Me.number = Convert.ToDouble(number)
      Else
         throw new ArgumentException("The number parameter is not a floating-point number.")
      End If
   End Sub
End Class
```

共通言語仕様では、入れ子になった型とプロテクト メンバーに対して従来のインスタンス化ごとのモデルが適用されます。 オープン ジェネリック型では、フィールドまたはメンバーのシグネチャに、入れ子になったプロテクト ジェネリック型の特定のインスタンス化が含まれている場合、これらのフィールドやメンバーは公開できません。 ジェネリックの基底クラスやインターフェイスについて特定のインスタンス化を拡張する非ジェネリック型では、フィールドまたはメンバーのシグネチャに、入れ子になったプロテクト ジェネリック型の別のインスタンス化が含まれている場合、これらのフィールドやメンバーは公開できません。

次の例では、ジェネリック型 `C1<T>` およびプロテクト クラス `C1<T>.N` を定義しています。 `C1<T>` には、2 つのメソッド `M1` と `M2` があります。 ただし、`M1` は、`C1<int>.N` オブジェクトを `C1<T>` から返そうとするので、CLS に準拠していません。 2 番目の `C2` クラスは、`C1<long>` から派生しています。 これには、`M3` と `M4` の 2 つのメソッドがあります。 `M3`は、`C1<int>.N` オブジェクトを `C1<long>` のサブクラスから返そうとするので、CLS に準拠していません。 言語コンパイラはさらに制限されている可能性があります。 この例では、Visual Basic で `M4` をコンパイルしようとするとエラーが表示されます。

```csharp
using System;

[assembly:CLSCompliant(true)]

public class C1<T>
{
   protected class N { }

   protected void M1(C1<int>.N n) { } // Not CLS-compliant - C1<int>.N not
                                      // accessible from within C1<T> in all
                                      // languages
   protected void M2(C1<T>.N n) { }   // CLS-compliant – C1<T>.N accessible
                                      // inside C1<T>
}

public class C2 : C1<long>
{
   protected void M3(C1<int>.N n) { }  // Not CLS-compliant – C1<int>.N is not
                                       // accessible in C2 (extends C1<long>)

   protected void M4(C1<long>.N n) { } // CLS-compliant, C1<long>.N is
                                       // accessible in C2 (extends C1<long>)
}
// Attempting to compile the example displays output like the following:
//       Generics4.cs(9,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
//       Generics4.cs(18,22): warning CS3001: Argument type 'C1<int>.N' is not CLS-compliant
```

```vb
<Assembly:CLSCompliant(True)>

Public Class C1(Of T)
   Protected Class N
   End Class

   Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
                                             ' accessible from within C1(Of T) in all
   End Sub                                   ' languages

   Protected Sub M2(n As C1(Of T).N)     ' CLS-compliant – C1(Of T).N accessible
   End Sub                               ' inside C1(Of T)
End Class

Public Class C2 : Inherits C1(Of Long)
   Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant – C1(Of Integer).N is not
   End Sub                                   ' accessible in C2 (extends C1(Of Long))

   Protected Sub M4(n As C1(Of Long).N)
   End Sub
End Class
' Attempting to compile the example displays output like the following:
'    error BC30508: 'n' cannot expose type 'C1(Of Integer).N' in namespace
'    '<Default>' through class 'C1'.
'
'       Protected Sub M1(n As C1(Of Integer).N)   ' Not CLS-compliant - C1<int>.N not
'                             ~~~~~~~~~~~~~~~~
'    error BC30389: 'C1(Of T).N' is not accessible in this context because
'    it is 'Protected'.
'
'       Protected Sub M3(n As C1(Of Integer).N)   ' Not CLS-compliant - C1(Of Integer).N is not
'
'                             ~~~~~~~~~~~~~~~~
'
'    error BC30389: 'C1(Of T).N' is not accessible in this context because it is 'Protected'.
'
'       Protected Sub M4(n As C1(Of Long).N)
'                             ~~~~~~~~~~~~~
```

### <a name="constructors"></a>コンストラクター

CLS 準拠のクラスと構造体のコンストラクターは、次の規則に従う必要があります。

* 派生クラスのコンストラクターは、継承されたインスタンス データにアクセスする前に、基底クラスのインスタンス コンストラクターを呼び出す必要があります。 これは、基底クラスのコンストラクターは派生クラスには継承されないからです。 この規則は、直接継承をサポートしない構造体に適用されません。

  次の例に示すように、コンパイラは、通常、CLS 準拠とは別にこの規則を適用します。 これにより、`Doctor` クラスから派生した `Person` クラスが作成されますが、`Doctor` クラスでは、継承されたインスタンス フィールドを初期化するための `Person` クラス コンストラクターは呼び出されません。

    ```csharp
    using System;

    [assembly: CLSCompliant(true)]

    public class Person
    {
    private string fName, lName, _id;

    public Person(string firstName, string lastName, string id)
    {
        if (String.IsNullOrEmpty(firstName + lastName))
            throw new ArgumentNullException("Either a first name or a last name must be provided.");

        fName = firstName;
        lName = lastName;
        _id = id;
    }

    public string FirstName
    {
        get { return fName; }
    }

    public string LastName
    {
        get { return lName; }
    }

    public string Id
    {
        get { return _id; }
    }

    public override string ToString()
    {
        return String.Format("{0}{1}{2}", fName,
                            String.IsNullOrEmpty(fName) ?  "" : " ",
                            lName);
    }
    }

    public class Doctor : Person
    {
    public Doctor(string firstName, string lastName, string id)
    {
    }

    public override string ToString()
    {
        return "Dr. " + base.ToString();
    }
    }
    // Attempting to compile the example displays output like the following:
    //    ctor1.cs(45,11): error CS1729: 'Person' does not contain a constructor that takes 0
    //            arguments
    //    ctor1.cs(10,11): (Location of symbol related to previous error)
    ```

    ```vb
    <Assembly: CLSCompliant(True)>

    Public Class Person
       Private fName, lName, _id As String

       Public Sub New(firstName As String, lastName As String, id As String)
          If String.IsNullOrEmpty(firstName + lastName) Then
             Throw New ArgumentNullException("Either a first name or a last name must be provided.")
          End If

          fName = firstName
          lName = lastName
          _id = id
       End Sub

       Public ReadOnly Property FirstName As String
          Get
             Return fName
          End Get
       End Property

       Public ReadOnly Property LastName As String
          Get
             Return lName
          End Get
       End Property

       Public ReadOnly Property Id As String
          Get
             Return _id
          End Get
       End Property

       Public Overrides Function ToString() As String
          Return String.Format("{0}{1}{2}", fName,
                               If(String.IsNullOrEmpty(fName), "", " "),
                               lName)
       End Function
    End Class

    Public Class Doctor : Inherits Person
       Public Sub New(firstName As String, lastName As String, id As String)
       End Sub

       Public Overrides Function ToString() As String
          Return "Dr. " + MyBase.ToString()
       End Function
    End Class
    ' Attempting to compile the example displays output like the following:
    '    Ctor1.vb(46) : error BC30148: First statement of this 'Sub New' must be a call
    '    to 'MyBase.New' or 'MyClass.New' because base class 'Person' of 'Doctor' does
    '    not have an accessible 'Sub New' that can be called with no arguments.
    '
    '       Public Sub New()
    '                  ~~~
    ````

* オブジェクト作成以外の目的で、オブジェクト コンストラクターを呼び出すことはできません。 また、オブジェクトを 2 度初期化することもできません。 たとえば、これは `Object.MemberwiseClone` でコンストラクターを呼び出す必要はないことを意味します。

### <a name="properties"></a>プロパティ

CLS 準拠型のプロパティは、次の規則に従う必要があります。

* プロパティには setter、getter、またはこの両方が必ず必要です。 アセンブリでは、これらは特殊なメソッドとして実装されます。つまり、アセンブリのメタデータで `SpecialName` とマークされた個別のメソッドとして (getter は `get`\_*propertyname*、setter は `set`\_*propertyname* という名前で) 表示されます。 C# コンパイラでは、<xref:System.CLSCompliantAttribute> 属性を適用しなくても、この規則が自動的に適用されます。

* プロパティの型は、プロパティ get アクセス操作子、および set アクセス操作子の最後の引数の戻り値の型です。 これらの型は CLS に準拠している必要があり、引数を参照によってプロパティに割り当てることはできません (つまり、マネージド ポインターにできません)。

* プロパティに get アクセス操作子と set アクセス操作子の両方がある場合は、両方が仮想、両方が静的、または両方がインスタンスである必要があります。 C# コンパイラは、プロパティ定義構文によって、この規則を自動的に適用します。

### <a name="events"></a>イベント

イベントは、名前と型によって定義されます。 イベントの型は、イベントの表示に使用されるデリゲートです。 たとえば、`DbConnection.StateChange` イベントは `StateChangeEventHandler` 型です。 イベント自体のほか、イベント名に基づく名前の 3 つのメソッドがイベントの実装を提供し、アセンブリのメタデータで `SpecialName` としてマークされています。

* イベント ハンドラーを追加するメソッド (`add`_*EventName*)。 たとえば、`DbConnection.StateChange` イベントのイベント サブスクリプション メソッドの名前は `add_StateChange` です。

* イベント ハンドラーを削除するメソッド (`remove`_*EventName*)。 たとえば、`DbConnection.StateChange` イベントの削除メソッドの名前は `remove_StateChange` です。

* イベントが発生したことを示すメソッド (`raise`\_*EventName*)。

> [!NOTE]
> イベントに関する共通言語仕様の規則は、ほとんどが言語コンパイラによって実装され、コンポーネント開発者が意識せずに使用できます。

イベントの追加、削除、発生の各メソッドには同じアクセシビリティが必要です。 また、すべてが、静的メソッド、インスタンス メソッド、仮想メソッドのいずれかでなければなりません。 イベントを追加および削除するメソッドには、イベント デリゲート型の型を持つパラメーターが 1 つあります。 追加メソッドおよび削除メソッドは両方とも存在するか、両方存在しないかのいずれかでなければなりません。

次の例では、2 つのポイントの気温の差がしきい値と等しいか、それを超えた場合に `Temperature` イベントを発生させる、`TemperatureChanged` という名前の CLS 準拠クラスを定義します。 `Temperature` クラスは、イベント ハンドラーを選択的に実行できるように、`raise_TemperatureChanged` メソッドを明示的に定義します。

```csharp
using System;
using System.Collections;
using System.Collections.Generic;

[assembly: CLSCompliant(true)]

public class TemperatureChangedEventArgs : EventArgs
{
   private Decimal originalTemp;
   private Decimal newTemp;
   private DateTimeOffset when;

   public TemperatureChangedEventArgs(Decimal original, Decimal @new, DateTimeOffset time)
   {
      originalTemp = original;
      newTemp = @new;
      when = time;
   }

   public Decimal OldTemperature
   {
      get { return originalTemp; }
   }

   public Decimal CurrentTemperature
   {
      get { return newTemp; }
   }

   public DateTimeOffset Time
   {
      get { return when; }
   }
}

public delegate void TemperatureChanged(Object sender, TemperatureChangedEventArgs e);

public class Temperature
{
   private struct TemperatureInfo
   {
      public Decimal Temperature;
      public DateTimeOffset Recorded;
   }

   public event TemperatureChanged TemperatureChanged;

   private Decimal previous;
   private Decimal current;
   private Decimal tolerance;
   private List<TemperatureInfo> tis = new List<TemperatureInfo>();

   public Temperature(Decimal temperature, Decimal tolerance)
   {
      current = temperature;
      TemperatureInfo ti = new TemperatureInfo();
      ti.Temperature = temperature;
      tis.Add(ti);
      ti.Recorded = DateTimeOffset.UtcNow;
      this.tolerance = tolerance;
   }

   public Decimal CurrentTemperature
   {
      get { return current; }
      set {
         TemperatureInfo ti = new TemperatureInfo();
         ti.Temperature = value;
         ti.Recorded = DateTimeOffset.UtcNow;
         previous = current;
         current = value;
         if (Math.Abs(current - previous) >= tolerance)
            raise_TemperatureChanged(new TemperatureChangedEventArgs(previous, current, ti.Recorded));
      }
   }

   public void raise_TemperatureChanged(TemperatureChangedEventArgs eventArgs)
   {
      if (TemperatureChanged == null)
         return;

      foreach (TemperatureChanged d in TemperatureChanged.GetInvocationList()) {
         if (d.Method.Name.Contains("Duplicate"))
            Console.WriteLine("Duplicate event handler; event handler not executed.");
         else
            d.Invoke(this, eventArgs);
      }
   }
}

public class Example
{
   public Temperature temp;

   public static void Main()
   {
      Example ex = new Example();
   }

   public Example()
   {
      temp = new Temperature(65, 3);
      temp.TemperatureChanged += this.TemperatureNotification;
      RecordTemperatures();
      Example ex = new Example(temp);
      ex.RecordTemperatures();
   }

   public Example(Temperature t)
   {
      temp = t;
      RecordTemperatures();
   }

   public void RecordTemperatures()
   {
      temp.TemperatureChanged += this.DuplicateTemperatureNotification;
      temp.CurrentTemperature = 66;
      temp.CurrentTemperature = 63;
   }

   internal void TemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }

   public void DuplicateTemperatureNotification(Object sender, TemperatureChangedEventArgs e)
   {
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature);
   }
}
```

```vb
Imports System.Collections
Imports System.Collections.Generic

<Assembly: CLSCompliant(True)>

Public Class TemperatureChangedEventArgs   : Inherits EventArgs
   Private originalTemp As Decimal
   Private newTemp As Decimal
   Private [when] As DateTimeOffset

   Public Sub New(original As Decimal, [new] As Decimal, [time] As DateTimeOffset)
      originalTemp = original
      newTemp = [new]
      [when] = [time]
   End Sub

   Public ReadOnly Property OldTemperature As Decimal
      Get
         Return originalTemp
      End Get
   End Property

   Public ReadOnly Property CurrentTemperature As Decimal
      Get
         Return newTemp
      End Get
   End Property

   Public ReadOnly Property [Time] As DateTimeOffset
      Get
         Return [when]
      End Get
   End Property
End Class

Public Delegate Sub TemperatureChanged(sender As Object, e As TemperatureChangedEventArgs)

Public Class Temperature
   Private Structure TemperatureInfo
      Dim Temperature As Decimal
      Dim Recorded As DateTimeOffset
   End Structure

   Public Event TemperatureChanged As TemperatureChanged

   Private previous As Decimal
   Private current As Decimal
   Private tolerance As Decimal
   Private tis As New List(Of TemperatureInfo)

   Public Sub New(temperature As Decimal, tolerance As Decimal)
      current = temperature
      Dim ti As New TemperatureInfo()
      ti.Temperature = temperature
      ti.Recorded = DateTimeOffset.UtcNow
      tis.Add(ti)
      Me.tolerance = tolerance
   End Sub

   Public Property CurrentTemperature As Decimal
      Get
         Return current
      End Get
      Set
         Dim ti As New TemperatureInfo
         ti.Temperature = value
         ti.Recorded = DateTimeOffset.UtcNow
         previous = current
         current = value
         If Math.Abs(current - previous) >= tolerance Then
            raise_TemperatureChanged(New TemperatureChangedEventArgs(previous, current, ti.Recorded))
         End If
      End Set
   End Property

   Public Sub raise_TemperatureChanged(eventArgs As TemperatureChangedEventArgs)
      If TemperatureChangedEvent Is Nothing Then Exit Sub

      Dim ListenerList() As System.Delegate = TemperatureChangedEvent.GetInvocationList()
      For Each d As TemperatureChanged In TemperatureChangedEvent.GetInvocationList()
         If d.Method.Name.Contains("Duplicate") Then
            Console.WriteLine("Duplicate event handler; event handler not executed.")
         Else
            d.Invoke(Me, eventArgs)
         End If
      Next
   End Sub
End Class

Public Class Example
   Public WithEvents temp As Temperature

   Public Shared Sub Main()
      Dim ex As New Example()
   End Sub

   Public Sub New()
      temp = New Temperature(65, 3)
      RecordTemperatures()
      Dim ex As New Example(temp)
      ex.RecordTemperatures()
   End Sub

   Public Sub New(t As Temperature)
      temp = t
      RecordTemperatures()
   End Sub

   Public Sub RecordTemperatures()
      temp.CurrentTemperature = 66
      temp.CurrentTemperature = 63

   End Sub

   Friend Shared Sub TemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 1: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub

   Friend Shared Sub DuplicateTemperatureNotification(sender As Object, e As TemperatureChangedEventArgs) _
          Handles temp.TemperatureChanged
      Console.WriteLine("Notification 2: The temperature changed from {0} to {1}", e.OldTemperature, e.CurrentTemperature)
   End Sub
End Class
```

### <a name="overloads"></a>Overloads

共通言語仕様では、次の要件がオーバーロード メンバーに適用されます。

* メンバーは、パラメーターの数および型に基づいてオーバーロードできます。 オーバーロードを区別するときに、呼び出し規約、戻り値の型、メソッドまたはそのパラメーターに適用されているカスタム修飾子、およびパラメーターが値渡しか参照渡しかは考慮されません。 例については、「[名前付け規則](#naming-conventions)」で、名前がスコープ内で一意であることを要求する要件のコードを参照してください。

* プロパティおよびメソッドのみオーバーロードできる。 フィールドとイベントはオーバーロードできません。

* ジェネリック メソッドは、ジェネリック パラメーターの数に基づいてオーバーロードできます。

> [!NOTE]
> `op_Explicit` 演算子および `op_Implicit` 演算子にはこの規則が適用されず、戻り値は、オーバーロード解決のためのメソッド シグネチャの一部として見なされません。 この 2 つの演算子は、そのパラメーターと戻り値の両方に基づいてオーバーロードできます。

### <a name="exceptions"></a>例外

例外オブジェクトは [System.Exception](xref:System.Exception)、または `System.Exception` の別の派生型から派生する必要があります。 次の例は、`ErrorClass` というカスタム クラスを例外処理に使用したときに発生するコンパイラ エラーを示しています。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
// Compilation produces a compiler error like the following:
//    Exceptions1.cs(26,16): error CS0155: The type caught or thrown must be derived from
//            System.Exception
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
' Compilation produces a compiler error like the following:
'    Exceptions1.vb(27) : error BC30665: 'Throw' operand must derive from 'System.Exception'.
'
'             Throw BadIndex
'             ~~~~~~~~~~~~~~
```

このエラーを修正するには、`ErrorClass` から継承するように `System.Exception` クラスを指定する必要があります。 また、Message プロパティはオーバーライドする必要があります。 次の例では、このエラーを修正して CLS 準拠の `ErrorClass` クラスを定義します。

```csharp
using System;

[assembly: CLSCompliant(true)]

public class ErrorClass : Exception
{
   string msg;

   public ErrorClass(string errorMessage)
   {
      msg = errorMessage;
   }

   public override string Message
   {
      get { return msg; }
   }
}

public static class StringUtilities
{
   public static string[] SplitString(this string value, int index)
   {
      if (index < 0 | index > value.Length) {
         ErrorClass badIndex = new ErrorClass("The index is not within the string.");
         throw badIndex;
      }
      string[] retVal = { value.Substring(0, index - 1),
                          value.Substring(index) };
      return retVal;
   }
}
```

```vb
Imports System.Runtime.CompilerServices

<Assembly: CLSCompliant(True)>

Public Class ErrorClass : Inherits Exception
   Dim msg As String

   Public Sub New(errorMessage As String)
      msg = errorMessage
   End Sub

   Public Overrides ReadOnly Property Message As String
      Get
         Return msg
      End Get
   End Property
End Class

Public Module StringUtilities
   <Extension()> Public Function SplitString(value As String, index As Integer) As String()
      If index < 0 Or index > value.Length Then
         Dim BadIndex As New ErrorClass("The index is not within the string.")
         Throw BadIndex
      End If
      Dim retVal() As String = { value.Substring(0, index - 1),
                                 value.Substring(index) }
      Return retVal
   End Function
End Module
```

### <a name="attributes"></a>属性

.NET Framework アセンブリでは、カスタム属性に拡張可能機構が用意されており、そのカスタム属性を格納し、アセンブリ、型、メンバー、メソッド パラメーターなどのプログラミング オブジェクトに関するメタデータを取得します。 カスタム属性は [System.Attribute](xref:System.Attribute)、または `System.Attribute` の派生型から派生する必要があります。

規則に違反する例を次に示します。 この例では、`NumericAttribute` から派生していない `System.Attribute` クラスを定義します。 コンパイラ エラーは、CLS 非準拠の属性が適用されている場合にのみ発生します。クラスが定義されているときではありません。

```csharp
using System;

[assembly: CLSCompliant(true)]

[AttributeUsageAttribute(AttributeTargets.Class | AttributeTargets.Struct)]
public class NumericAttribute
{
   private bool _isNumeric;

   public NumericAttribute(bool isNumeric)
   {
      _isNumeric = isNumeric;
   }

   public bool IsNumeric
   {
      get { return _isNumeric; }
   }
}

[Numeric(true)] public struct UDouble
{
   double Value;
}
// Compilation produces a compiler error like the following:
//    Attribute1.cs(22,2): error CS0616: 'NumericAttribute' is not an attribute class
//    Attribute1.cs(7,14): (Location of symbol related to previous error)
```

```vb
<Assembly: CLSCompliant(True)>

<AttributeUsageAttribute(AttributeTargets.Class Or AttributeTargets.Struct)> _
Public Class NumericAttribute
   Private _isNumeric As Boolean

   Public Sub New(isNumeric As Boolean)
      _isNumeric = isNumeric
   End Sub

   Public ReadOnly Property IsNumeric As Boolean
      Get
         Return _isNumeric
      End Get
   End Property
End Class

<Numeric(True)> Public Structure UDouble
   Dim Value As Double
End Structure
' Compilation produces a compiler error like the following:
'    error BC31504: 'NumericAttribute' cannot be used as an attribute because it
'    does not inherit from 'System.Attribute'.
'
'    <Numeric(True)> Public Structure UDouble
'     ~~~~~~~~~~~~~
```

CLS 準拠の属性のコンストラクターまたはプロパティは、次の型のみを公開できます。

* [Boolean](xref:System.Boolean)

* [Byte](xref:System.Byte)

* [Char](xref:System.Char)

* [Double](xref:System.Double)

* [Int16](xref:System.Int16)

* [Int32](xref:System.Int32)

* [Int64](xref:System.Int64)

* [Single](xref:System.Single)

* [String](xref:System.String)

* [Type](xref:System.Type)

* 基になる型が `Byte`、`Int16`、`Int32`、または `Int64` である列挙体の型。

次の例では、[Attribute](xref:System.Attribute) から派生する `DescriptionAttribute` クラスを定義します。 クラス コンストラクターには型 `Descriptor` のパラメーターがあるので、クラスは CLS に準拠していません。 C# コンパイラが警告を生成しますが、コンパイルは成功することに注意してください。

```csharp
using System;

[assembly:CLSCompliantAttribute(true)]

public enum DescriptorType { type, member };

public class Descriptor
{
   public DescriptorType Type;
   public String Description;
}

[AttributeUsage(AttributeTargets.All)]
public class DescriptionAttribute : Attribute
{
   private Descriptor desc;

   public DescriptionAttribute(Descriptor d)
   {
      desc = d;
   }

   public Descriptor Descriptor
   { get { return desc; } }
}
// Attempting to compile the example displays output like the following:
//       warning CS3015: 'DescriptionAttribute' has no accessible
//               constructors which use only CLS-compliant types
```

```vb
<Assembly:CLSCompliantAttribute(True)>

Public Enum DescriptorType As Integer
   Type = 0
   Member = 1
End Enum

Public Class Descriptor
   Public Type As DescriptorType
   Public Description As String
End Class

<AttributeUsage(AttributeTargets.All)> _
Public Class DescriptionAttribute : Inherits Attribute
   Private desc As Descriptor

   Public Sub New(d As Descriptor)
      desc = d
   End Sub

   Public ReadOnly Property Descriptor As Descriptor
      Get
         Return desc
      End Get
   End Property
End Class
```

## <a name="the-clscompliantattribute-attribute"></a>CLSCompliantAttribute 属性

[CLSCompliantAttribute](xref:System.CLSCompliantAttribute) 属性は、プログラム要素が共通言語仕様でコンパイルされているかどうかを示すために使用されます。 `CLSCompliantAttribute.CLSCompliantAttribute(Boolean)` コンストラクターには、プログラム要素が CLS に準拠しているかどうかを示す 1 つの必須パラメーター、*isCompliant* が含まれます。

コンパイル時に、CLS 準拠が前提とされる非準拠要素が検出され、警告が出力されます。 非準拠として明示的に宣言された型またはメンバーに対しては、警告は出力されません。

コンポーネント開発者は、次の 2 とおりの目的で `CLSCompliantAttribute` 属性を使用できます。

* コンポーネントによって公開されたパブリック インターフェイスの CLS 準拠部分と CLS 非準拠部分を定義する。 この属性を使用して特定のプログラム要素を CLS 準拠としてマークすると、.NET Framework を対象とするすべてのツールおよび言語から、これらの要素に必ずアクセスできるようになります。

* コンポーネント ライブラリのパブリック インターフェイスが CLS に準拠するプログラム要素のみを公開するように保証する。 要素が CLS 非準拠の場合は、通常、警告が表示されます。

> [!WARNING]
> 言語コンパイラでは、`CLSCompliantAttribute` 属性が使用されているかどうかに関係なく、CLS 準拠の規則が適用される場合があります。 たとえば、インターフェイスに `*static` メンバーを定義すると CLS の規則に違反します。 ただし、インターフェイスに `*static` メンバーを定義した場合、C# コンパイラはエラー メッセージを表示し、アプリのコンパイルに失敗します。

`CLSCompliantAttribute` 属性は、値 `AttributeTargets.All` が指定された [AttributeUsageAttribute](xref:System.AttributeUsageAttribute) 属性でマークされます。 この値を使用すると、`CLSCompliantAttribute` 属性を、アセンブリ、モジュール、型 (クラス、構造体、列挙体、インターフェイス、およびデリゲート)、型パラメーター (コンストラクター、メソッド、プロパティ、フィールド、およびイベント)、パラメーター、ジェネリック パラメーター、戻り値など、すべてのプログラム要素に適用できます。 ただし、実際は、アセンブリ、型、および型メンバーだけに属性を適用することをお勧めします。 そうしないと、属性は、コンパイラによってライブラリのパブリック インターフェイスで非準拠パラメーター、ジェネリック パラメーター、または戻り値が検出されたときに必ず無視され、コンパイラ警告が引き続き生成されます。

`CLSCompliantAttribute` 属性の値は、内包型プログラム要素によって継承されます。 たとえば、アセンブリが CLS 準拠としてマークされている場合は、その型も CLS に準拠します。 型が CLS 準拠としてマークされている場合は、その入れ子になった型とメンバーも CLS に準拠します。

継承された準拠状況を明示的にオーバーライドするには、`CLSCompliantAttribute` 属性を、格納されているプログラム要素に適用します。 たとえば、*isCompliant* 値が `false` に指定された `CLSCompliantAttribute` 属性を使用すると、準拠アセンブリで非準拠型を定義できます。また、*isCompliant* 値が `true` に指定された属性を使用すると、非準拠アセンブリで準拠型を定義できます。 準拠型で非準拠メンバーを定義することもできます。 ただし、非準拠型は準拠メンバーを持つことができないので、*isCompliant* 値が `true` に指定された属性では非準拠型から継承をオーバーライドできません。

コンポーネントを開発するときは必ず、`CLSCompliantAttribute` 属性を使用して、アセンブリ、その型、およびそのメンバーが CLS に準拠しているかどうかを指定することをお勧めします。

CLS 準拠のコンポーネントを作成するには:

1. `CLSCompliantAttribute` を使用して、CLS 準拠としてアセンブリをマークします。

2. アセンブリ内の CLS に準拠していない公開された型を、非準拠としてマークします。

3. CLS 準拠の型の公開されたメンバーを、非準拠としてマークします。

4. CLS 非準拠メンバーの CLS 準拠の代替を指定します。

非準拠の型とメンバーすべてを正常にマークした場合、非準拠の警告は出力されません。 ただし、どのメンバーが CLS に準拠していないかを提示し、CLS 準拠の代替を製品ドキュメントに示すことをお勧めします。

次の例では、`CLSCompliantAttribute` 属性を使用して、CLS 準拠のアセンブリと、2 つの CLS 非準拠のメンバーを持つ型、`CharacterUtilities` を定義します。 両メンバーとも `CLSCompliant(false)` 属性でタグ付けされているので、警告は生成されません。 また、クラスには、両方のメソッド用の CLS 準拠の代替も用意されています。 通常、CLS 準拠の代替を用意するには、2 つのオーバーロードを `ToUTF16` メソッドに追加するだけです。 ただし、戻り値に基づいてメソッドをオーバーロードできないので、CLS 準拠のメソッドの名前は、非準拠のメソッドの名前とは異なります。

```csharp
using System;
using System.Text;

[assembly:CLSCompliant(true)]

public class CharacterUtilities
{
   [CLSCompliant(false)] public static ushort ToUTF16(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return Convert.ToUInt16(s[0]);
   }

   [CLSCompliant(false)] public static ushort ToUTF16(Char ch)
   {
      return Convert.ToUInt16(ch);
   }

   // CLS-compliant alternative for ToUTF16(String).
   public static int ToUTF16CodeUnit(String s)
   {
      s = s.Normalize(NormalizationForm.FormC);
      return (int) Convert.ToUInt16(s[0]);
   }

   // CLS-compliant alternative for ToUTF16(Char).
   public static int ToUTF16CodeUnit(Char ch)
   {
      return Convert.ToInt32(ch);
   }

   public bool HasMultipleRepresentations(String s)
   {
      String s1 = s.Normalize(NormalizationForm.FormC);
      return s.Equals(s1);
   }

   public int GetUnicodeCodePoint(Char ch)
   {
      if (Char.IsSurrogate(ch))
         throw new ArgumentException("ch cannot be a high or low surrogate.");

      return Char.ConvertToUtf32(ch.ToString(), 0);
   }

   public int GetUnicodeCodePoint(Char[] chars)
   {
      if (chars.Length > 2)
         throw new ArgumentException("The array has too many characters.");

      if (chars.Length == 2) {
         if (! Char.IsSurrogatePair(chars[0], chars[1]))
            throw new ArgumentException("The array must contain a low and a high surrogate.");
         else
            return Char.ConvertToUtf32(chars[0], chars[1]);
      }
      else {
         return Char.ConvertToUtf32(chars.ToString(), 0);
      }
   }
}
```

```vb
Imports System.Text

<Assembly:CLSCompliant(True)>

Public Class CharacterUtilities
   <CLSCompliant(False)> Public Shared Function ToUTF16(s As String) As UShort
      s = s.Normalize(NormalizationForm.FormC)
      Return Convert.ToUInt16(s(0))
   End Function

   <CLSCompliant(False)> Public Shared Function ToUTF16(ch As Char) As UShort
      Return Convert.ToUInt16(ch)
   End Function

   ' CLS-compliant alternative for ToUTF16(String).
   Public Shared Function ToUTF16CodeUnit(s As String) As Integer
      s = s.Normalize(NormalizationForm.FormC)
      Return CInt(Convert.ToInt16(s(0)))
   End Function

   ' CLS-compliant alternative for ToUTF16(Char).
   Public Shared Function ToUTF16CodeUnit(ch As Char) As Integer
      Return Convert.ToInt32(ch)
   End Function

   Public Function HasMultipleRepresentations(s As String) As Boolean
      Dim s1 As String = s.Normalize(NormalizationForm.FormC)
      Return s.Equals(s1)
   End Function

   Public Function GetUnicodeCodePoint(ch As Char) As Integer
      If Char.IsSurrogate(ch) Then
         Throw New ArgumentException("ch cannot be a high or low surrogate.")
      End If
      Return Char.ConvertToUtf32(ch.ToString(), 0)
   End Function

   Public Function GetUnicodeCodePoint(chars() As Char) As Integer
      If chars.Length > 2 Then
         Throw New ArgumentException("The array has too many characters.")
      End If
      If chars.Length = 2 Then
         If Not Char.IsSurrogatePair(chars(0), chars(1)) Then
            Throw New ArgumentException("The array must contain a low and a high surrogate.")
         Else
            Return Char.ConvertToUtf32(chars(0), chars(1))
         End If
      Else
         Return Char.ConvertToUtf32(chars.ToString(), 0)
      End If
   End Function
End Class
```

ライブラリではなくアプリを開発している場合 (つまり、他のアプリ開発者が使用できる型またはメンバーを公開しない場合)、アプリが使用するプログラム要素は、そのプログラム要素が使用する言語でサポートされていない場合に CLS に準拠する必要があります。 この場合、CLS 非準拠の要素を使用しようとすると、言語コンパイラによってエラーが生成されます。

## <a name="cross-language-interoperability"></a>言語間の相互運用性

言語に依存しないということは、いくつか意味があります。 1 つには、ある言語で記述された型を、別の言語で記述されたアプリからシームレスに使用できることを意味します。 また、複数の言語で記述されたコードを 1 つの .NET .NET Framework アセンブリにまとめることもできます。ここでは、この点について焦点を当てて説明します。

次の例では、`NumericLib` および `StringLib` という 2 つのクラスを含む Utilities.dll という名前のクラス ライブラリを作成して言語間の相互運用性を示します。 `NumericLib` クラスは C# で記述され、`StringLib` クラスは Visual Basic で記述されています。 以下は `StringUtil.vb` のソース コードで、`StringLib` クラスに `ToTitleCase` という単一のメンバーが含まれます。

```vb
Imports System.Collections.Generic
Imports System.Runtime.CompilerServices

Public Module StringLib
   Private exclusions As List(Of String)

   Sub New()
      Dim words() As String = { "a", "an", "and", "of", "the" }
      exclusions = New List(Of String)
      exclusions.AddRange(words)
   End Sub

   <Extension()> _
   Public Function ToTitleCase(title As String) As String
      Dim words() As String = title.Split()
      Dim result As String = String.Empty

      For ctr As Integer = 0 To words.Length - 1
         Dim word As String = words(ctr)
         If ctr = 0 OrElse Not exclusions.Contains(word.ToLower()) Then
            result += word.Substring(0, 1).ToUpper() + _
                      word.Substring(1).ToLower()
         Else
            result += word.ToLower()
         End If
         If ctr <= words.Length - 1 Then
            result += " "
         End If
      Next
      Return result
   End Function
End Module
```

以下は NumberUtil.cs のソース コードで、`NumericLib` および `IsEven` という 2 つのメンバーを持つ `NearZero` クラスを定義しています。

```csharp
using System;

public static class NumericLib
{
   public static bool IsEven(this IConvertible number)
   {
      if (number is Byte ||
          number is SByte ||
          number is Int16 ||
          number is UInt16 ||
          number is Int32 ||
          number is UInt32 ||
          number is Int64)
         return ((long) number) % 2 == 0;
      else if (number is UInt64)
         return ((ulong) number) %2 == 0;
      else
         throw new NotSupportedException("IsEven called for a non-integer value.");
   }

   public static bool NearZero(double number)
   {
      return number < .00001;
   }
}
```

単一のアセンブリに 2 つのクラスをパッケージ化するには、モジュールにコンパイルする必要があります。 Visual Basic のソース コード ファイルをモジュールにコンパイルするには、次のコマンドを使用します。

```console
vbc /t:module StringUtil.vb
```

C# のソース コード ファイルをモジュールにコンパイルするには、次のコマンドを使用します。

```console
csc /t:module NumberUtil.cs
```

次に、リンク ツール (Link.exe) を使用して 2 つのモジュールをアセンブリにコンパイルします。

```console
link numberutil.netmodule stringutil.netmodule /out:UtilityLib.dll /dll
```

次の例では、その後 `NumericLib.NearZero` メソッドおよび `StringLib.ToTitleCase` メソッドを呼び出します。 Visual Basic コードと C# コードは、両方のクラスのメソッドにアクセスできることに注意してください。

```csharp
using System;

public class Example
{
   public static void Main()
   {
      Double dbl = 0.0 - Double.Epsilon;
      Console.WriteLine(NumericLib.NearZero(dbl));

      string s = "war and peace";
      Console.WriteLine(s.ToTitleCase());
   }
}
// The example displays the following output:
//       True
//       War and Peace
```

```vb
Module Example
   Public Sub Main()
      Dim dbl As Double = 0.0 - Double.Epsilon
      Console.WriteLine(NumericLib.NearZero(dbl))

      Dim s As String = "war and peace"
      Console.WriteLine(s.ToTitleCase())
   End Sub
End Module
' The example displays the following output:
'       True
'       War and Peace
```

Visual Basic コードをコンパイルするには、次のコマンドを使用します。

```console
vbc example.vb /r:UtilityLib.dll
```

C# でコンパイルするには、コンパイラの名前を vbc から csc に変更し、ファイル拡張子を .vb から .cs に変更します。

```console
csc example.cs /r:UtilityLib.dll
```
