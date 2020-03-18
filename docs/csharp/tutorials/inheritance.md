---
title: C# での継承
description: C# ライブラリやアプリケーションでの継承の使用について学習します。
ms.date: 07/05/2018
ms.technology: csharp-fundamentals
ms.assetid: aeb68c74-0ea0-406f-9fbe-2ce02d47ef31
ms.openlocfilehash: b72badb7833e018dfcbf5d2583b17f17c800c382
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79156754"
---
# <a name="inheritance-in-c-and-net"></a>C# と .NET での継承

このチュートリアルでは、C# での継承について説明します。 継承は、オブジェクト指向プログラミング言語の一機能であり、特定の機能 (データおよび動作) を提供する基底クラスを定義し、その機能を継承またはオーバーライドする派生クラスを定義することができます。

## <a name="prerequisites"></a>必須コンポーネント

このチュートリアルでは、.NET Core SDK がインストールされていることを前提としています。 ダウンロードするには、[.NET Core ダウンロード](https://dotnet.microsoft.com/download) ページにアクセスしてください。 コード エディターも必要です。 このチュートリアルでは [Visual Studio Code](https://code.visualstudio.com) を使用していますが、任意のコード エディターを使用して構いません。

## <a name="running-the-examples"></a>例の実行

このチュートリアル内の例を作成して実行するには、コマンド ラインの [dotnet](../../core/tools/dotnet.md) ユーティリティを使用します。 それぞれの例について、次の手順に従います。

1. 例を格納するディレクトリを作成します。
1. コマンド プロンプトで [dotnet new コンソール](../../core/tools/dotnet-new.md) コマンドを入力し、新しい .NET Core プロジェクトを作成します。
1. 例にあるコードをコピーして、コード エディターに貼り付けます。
1. コマンド ラインから [dotnet restore](../../core/tools/dotnet-restore.md) コマンドを入力し、プロジェクトの依存関係を読み込みまたは復元します。

  [!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

1. [dotnet run](../../core/tools/dotnet-run.md) コマンドを入力して、例をコンパイルし実行します。

## <a name="background-what-is-inheritance"></a>背景: 継承とは何か

*継承*とは、オブジェクト指向プログラミングの基本的な属性の 1 つです。 親クラスの動作を再利用 (継承)、拡張、または変更する子クラスを定義することができます。 メンバーの継承元となるクラスを、*基底クラス*と呼びます。 基底クラスのメンバーを継承するクラスを、*派生クラス*と呼びます。

C# と .NET は*単一継承*のみをサポートしています。 つまり、1 つのクラスは、1 つのクラスからしか継承できないことになります。 ただし継承は推移的であり、一連の型の継承階層を定義することができます。 たとえば、`D` 型は `C` 型から継承でき、この C 型は `B` 型から継承され、この B 型は基底クラスである `A` 型から継承されます。 継承が推移的であるため、`A` 型のメンバーは `D` 型で使用できます。

基底クラスのすべてのメンバーが、派生クラスによって継承されるわけではありません。 以下のメンバーは継承されません。

- [静的コンスラクター](../programming-guide/classes-and-structs/static-constructors.md)。クラスの静的データを初期化するもの。

- [インスタンス コンストラクター](../programming-guide/classes-and-structs/constructors.md)。クラスの新しいインスタンスを作成するために呼び出すもの。 各クラスはそれ自身のコンストラクターを定義する必要があります。

- [ファイナライザー](../programming-guide/classes-and-structs/destructors.md)。ランタイムのガベージ コレクターによって呼び出され、クラスのインスタンスを破棄するもの。

他のすべての基底クラスのメンバーは派生クラスに継承されますが、それらが表示されるどうかはアクセシビリティに依存します。 メンバーのアクセシビリティは、次のとおり、派生したクラスの可視性に影響します。

- [プライベート](../language-reference/keywords/private.md) メンバーは、基底クラスで入れ子になっている派生クラスでのみ表示されます。 それ以外の場合、派生クラスでは表示されません。 次の例では、`A.B` は `A` から派生した入れ子になったクラスで、`C` は `A` から派生しています。 プライベートの `A.value` フィールドは A.B で表示されます。 しかし、`C.GetValue` メソッドからコメントを削除して例をコンパイルしようとすると、コンパイラ エラー CS0122 "'A.value' is inaccessible due to its protection level." ('A.value' はアクセスできない保護レベルになっています") が生成されます。

  [!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/private.cs#1)]

- [プロテクト](../language-reference/keywords/protected.md) メンバーは派生クラスでのみ表示されます。

- [内部](../language-reference/keywords/internal.md)メンバーは、基底クラスと同じアセンブリ内にある派生クラスでのみ表示されます。 基底クラスとは別のアセンブリにある派生クラスでは、表示されません。

- [パブリック](../language-reference/keywords/public.md) メンバーは派生クラスで表示され、派生クラスのパブリック インターフェイスの一部です。 パブリックの継承されたメンバーは、派生クラスで定義された場合と同様に呼び出すことができます。 次の例では、クラス `A` が `Method1` という名前のメソッドを定義し、クラス `B` がクラス `A` から継承します。 そこでこの例は、`Method1` を `B` 上のインスタンス メソッドであるかのように呼び出します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/basics.cs#1)]

派生クラスはまた、代替実装を行うことにより、継承されたメンバーを*オーバーライド*することができます。 メンバーをオーバーライドするためには、基底クラスのメンバーは [virtual](../language-reference/keywords/virtual.md) のキーワードでマークされている必要があります。 既定では基底クラスのメンバーは `virtual` としてマークされていないので、オーバーライドすることはできません。 次の例のように、非仮想メンバーをオーバーライドしようとすると、コンパイラ エラー CS0506 "\<member> cannot override inherited member \<member>" ("継承されたメンバー <member> が virtual、abstract、または override でマークされていないため、<member> でオーバーライドすることができません") が生成されます。

```csharp
public class A
{
    public void Method1()
    {
        // Do something.
    }
}

public class B : A
{
    public override void Method1() // Generates CS0506.
    {
        // Do something else.
    }
}
```

場合によっては、派生クラスは基底クラスの実装をオーバーライドする*必要があります*。 [abstract](../language-reference/keywords/abstract.md) キーワードでマークされた基底クラスのメンバーは、派生クラスによってオーバーライドされる必要があります。 次の例をコンパイルしようとすると、コンパイラ エラー CS0534 (「&lt;クラス&gt; は継承抽象メンバー &lt;メンバー&gt; を実装しません。」) が生成されます。これは、クラス `B` が `A.Method1` の実装を提供していないためです。

```csharp
public abstract class A
{
    public abstract void Method1();
}

public class B : A // Generates CS0534.
{
    public void Method3()
    {
        // Do something.
    }
}
```

継承は、クラスとインターフェイスにのみ適用されます。 その他の種類のカテゴリ (構造体、デリゲート、および列挙型) は、継承をサポートしていません。 これらのルールのため、次の例のようなコードをコンパイルしようとすると、コンパイラ エラー CS0527 "インターフェイス リストの型 'ValueType' はインターフェイスではありません。" が生成されます。 このエラー メッセージは、構造体が実装するインターフェイスを定義することはできても、継承はサポートされないことを示します。

```csharp
using System;

public struct ValueStructure : ValueType // Generates CS0527.
{
}
```

## <a name="implicit-inheritance"></a>暗黙的な継承

.NET 型システムの型はすべて、単一継承によって継承する型のほかに、<xref:System.Object> またはその派生型から暗黙的に継承します。 <xref:System.Object> の共通の機能はあらゆる型で使用できます。

暗黙的な継承とはどのようなものか、新しいクラス `SimpleClass` を定義してみましょう。単純な空のクラス定義です。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass.cs#1)]

次に、リフレクション (型のメタデータを検査して、その型の情報を取得できるもの) を使用して、`SimpleClass` 型に属するメンバーの一覧を取得します。 `SimpleClass` クラスにはメンバーをまだ定義していないにもかかわらず、この例の出力は、9 つのメンバーが存在することを示しています。 これらのメンバーのうちの 1 つは、パラメーターなし (既定) のコンストラクターで、C# コンパイラによって `SimpleClass` 型に自動的に提供されるものです。 あとの 8 つは <xref:System.Object> のメンバーで、この型から、.NET 型システムのすべてのクラスとインターフェイスが最終的に暗黙的に継承します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass.cs#2)]

<xref:System.Object> クラスからの暗黙的な継承により、`SimpleClass` クラスで以下のメソッドが使用可能になります。

- パブリック `ToString` メソッド。`SimpleClass` オブジェクトを文字列表記に変換し、完全修飾型名を返します。 ここで、`ToString` メソッドは文字列 "SimpleClass" を返します。

- 2 つのオブジェクトが等しいかどうか調べる 3 つのメソッド: パブリック インスタンス `Equals(Object)` メソッド、パブリック静的 `Equals(Object, Object)` メソッド、およびパブリック静的 `ReferenceEquals(Object, Object)` メソッド。 既定により、これらのメソッドは参照の等価性を調べます。つまり、等価であるためには、2 つのオブジェクトの変数が同じオブジェクトを参照している必要があります。

- パブリック `GetHashCode` メソッド。型インスタンスのハッシュされたコレクションでの使用を許可する値を計算します。

- パブリック `GetType` メソッド。`SimpleClass` 型を表す <xref:System.Type> オブジェクトを返します。

- 保護された <xref:System.Object.Finalize%2A> メソッド。オブジェクトのメモリがガベージ コレクターによって回収される前にアンマネージ リソースを解放するように設計されています。

- 保護された <xref:System.Object.MemberwiseClone%2A> メソッド。現在のオブジェクトの浅い複製を作成します。

暗黙的な継承によって、`SimpleClass` オブジェクトから任意の継承されたメンバーを、`SimpleClass` クラスで定義されたメンバーであるかのように呼び出すことができます。 たとえば、次の例では `SimpleClass.ToString` メソッドを呼び出しますが、これは `SimpleClass` が <xref:System.Object> から継承しています。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/simpleclass2.cs#1)]

次の表は、C# で作成できる型のカテゴリと、暗黙的に継承する元となる型の一覧です。 各々の基本型によって、暗黙的な派生型への継承を通して、異なるメンバーのセットが利用可能となります。

| 型のカテゴリ | 暗黙的な継承元                                                      |
| ------------- | ----------------------------------------------------------------------------- |
| class         | <xref:System.Object>                                                          |
| struct        | <xref:System.ValueType>、<xref:System.Object>                                 |
| enum          | <xref:System.Enum>、<xref:System.ValueType>、<xref:System.Object>             |
| delegate      | <xref:System.MulticastDelegate>、<xref:System.Delegate>、<xref:System.Object> |

## <a name="inheritance-and-an-is-a-relationship"></a>継承と "is a" 関係

継承は通常、基底クラスと 1 つまたは複数の派生クラスとの "is a" 関係を表現するのに使用します。ここで、派生クラスは基底クラスの特殊化されたバージョン、つまり基底クラスの 1 つの型です。 たとえば、`Publication` クラスはあらゆる種類の出版物を表しますが、`Book` クラスおよび `Magazine` クラスは特定の種類の出版物を表します。

> [!NOTE]
> 1 つのクラスまたは構造体は、1 つまたは複数のインターフェイスを実装できます。 インターフェイスの実装は、単一継承の回避策として、または構造体とともに継承を使用する方法として提示されることが多いですが、継承というよりは、インターフェイスとその実装型の間の別の関係 ("can do" 関係) を表すものとして意図されています。 インターフェイスは、その実装型で使用可能とする機能 (等価性を調べる機能、オブジェクトを比較または並べ替える機能、カルチャに依存した解析および書式設定のサポート機能など) のサブセットを定義します。

なお、"is a" 関係は、型とその型の特定のインスタンス化の間の関係も表します。 次の例では、`Automobile` は一意の読み取り専用プロパティを 3 つ持つクラスです。自動車の製造メーカーである `Make`、車種である `Model`、そして製造年である `Year` の 3 つです。 この `Automobile` クラスはまた、プロパティ値に割り当てられている引数があるコンストラクターを持ち、<xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドをオーバーライドして、`Automobile` クラスではなく `Automobile` インスタンスを一意に識別する文字列を生成します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/is-a.cs#1)]

この場合、特定の自動車メーカーと車種を表すために継承に依存すべきではありません。 たとえば、Packard Motor Car 社によって製造された自動車を表すのに、`Packard` という型を定義する必要はありません。 代わりに、次の例のように、`Automobile` オブジェクトを作成して、そのクラス コンストラクターに適切な値を渡すことによって同社の自動車を表すことができます。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/is-a.cs#2)]

継承に基づいた is-a 関係の適用が最も適しているのは、基底クラスと、基底クラスにメンバーを追加する派生クラス、または基底クラスにない追加機能が必要な派生クラスです。

## <a name="designing-the-base-class-and-derived-classes"></a>基底クラスと派生クラスの設計

基底クラスとその派生クラスを設計するプロセスについて説明します。 このセクションでは、基底クラス `Publication` を定義します。書籍、雑誌、新聞、ジャーナル、記事などの任意の種類の出版物を表します。さらに `Publication` から派生する `Book` クラスも定義します。 この例を拡張して、簡単に `Magazine`、`Journal`、`Newspaper`、および `Article` などの他の派生クラスを定義することができます。

### <a name="the-base-publication-class"></a>基底 Publication クラス

`Publication` クラスを設計するにあたり、次のように、設計についていくつか決定する必要があります。

- どのメンバーを基底クラス `Publication` に含めるか、`Publication` メンバーがメソッドの実装を提供するかどうか、`Publication` をその派生クラスのテンプレートとなる抽象基底クラスとするかどうか。

  ここで、`Publication` クラスはメソッドの実装を提供します。 「[抽象基底クラスとその派生クラスの設計](#abstract)」セクションには、抽象基底クラスを使用して、派生クラスがオーバーライドする必要があるメソッドを定義する例が含まれています。 派生クラスは、その派生型に適した任意の実装を提供することができます。

  コードを再利用する機能 (つまり、複数の派生クラスが基底クラスのメソッドの宣言と実装を共有し、それらをオーバーライドする必要がないこと) は、非抽象基底クラスの利点です。 そこで、コードが `Publication` 型の複数または非常に特殊化されたものによって共有される可能性が高い場合は、メンバーを `Publication` に追加します。 基底クラスの実装を効率的に提供できていないと、基底クラスでの単一の実装で済むところを、派生クラスでほぼ同一のメンバーの実装を行わなければいけないことになります。 複数の箇所で重複するコードを保守する必要が生じ、バグを引き起こす元となりえます。

  コードの再利用を最大化し、同時に論理的で直感的な継承階層を作成するために、`Publication` クラスには必ず、すべてもしくはほとんどの出版物に共通したデータと機能のみを含めるようにします。 そして派生クラスは、それ自身が表す特定の種類の出版物に固有のメンバーを実装します。

- クラス階層をどの程度まで拡張すべきか。 1 つの基底クラスと 1 つまたは複数の派生クラスではなく、3 つ以上のクラス階層を作るかどうか。 たとえば、`Publication` は `Periodical` の基底クラスになり得ますが、この Periodical は `Magazine`、`Journal`、および `Newspaper` の基底クラスです。

  この例では、`Publication` クラスと 1 つの派生クラス `Book` という小さな階層を使用します。 この例を拡張すると、`Magazine` や `Article` など、`Publication` から派生するクラスを簡単に追加で多く作成できます。

- 基底クラスのインスタンス化に意味があるのか。 意味がなければ、そのクラスには [abstract](../language-reference/keywords/abstract.md) キーワードを適用します。 それ以外の場合、`Publication` クラスはそのクラス コンストラクターを呼び出すことによってインスタンス化することができます。 `abstract` キーワードでマークされたクラスを、そのクラス コンストラクターへの直接呼び出しによってインスタンス化しようとすると、C# コンパイラはエラー CS0144 "Cannot create an instance of the abstract class or interface." ("抽象クラスまたはインターフェイスのインスタンスを作成できません") を生成します。 リフレクションを使用してクラスをインスタンス化しようとすると、そのリフレクション メソッドは <xref:System.MemberAccessException> をスローします。

  既定では、基底クラスはそのクラス コンストラクターを呼び出すことによってインスタンス化することができます。 クラス コンストラクターを明示的に定義する必要はありません。 基底クラスのソース コード内に存在しない場合、C# コンパイラは自動的に既定の (パラメーターなしの) コンストラクターを提供します。

  この例では、`Publication` クラスを [abstract](../language-reference/keywords/abstract.md) としてマークして、インスタンス化できないようにします。  `abstract` メソッドなしの `abstract` クラスは、このクラスが、いくつかの具象クラス (`Book`、`Journal` など) で共有される抽象概念を表すことを示します。

- 派生クラスで、特定のメンバーの基底クラス実装を継承する必要があるかどうか、基底クラス実装をオーバーライドするオプションがあるかどうか、または実装を提供する必要があるかどうか。 [abstract](../language-reference/keywords/abstract.md) キーワードを使用して、派生クラスで実装を提供するように強制します。 [virtual](../language-reference/keywords/virtual.md) キーワードを使用して、派生クラスによる基底クラス メソッドのオーバーライドを許可します。 既定では、基底クラスで定義されているメソッドはオーバーライド*できません*。

 `Publication` クラスには `abstract` メソッドはありませんが、クラス自体が `abstract` になります。

- 派生クラスが継承階層内の最後のクラスを表していて、それ自体が追加の派生クラスの基底クラスとして使用できないかどうか。 既定では、どのクラスも基底クラスとして使用できます。 [sealed](../language-reference/keywords/sealed.md) キーワードを適用すると、クラスが追加クラスの基底クラスとして使用できないことを示すことができます。 sealed クラスからの派生を試みると、コンパイラ エラー CS0509 "cannot derive from sealed type \<typeName>" ("シール型 typeName から派生することができません") が生成されます。

  この例では、派生クラスを `sealed` としてマークします。

次の例では、`Publication` のソース コードを、`Publication.PublicationType` プロパティに返される `PublicationType` 列挙型とともに示します。 <xref:System.Object> から継承したメンバーに加え、`Publication` クラスは次の一意のメンバーおよびメンバー オーバーライドを定義します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/base-and-derived.cs#1)]

- コンストラクター

  `Publication` クラスは `abstract` なので、次の例のようなコードから直接インスタンス化することはできません。

  ```csharp
  var publication = new Publication("Tiddlywinks for Experts", "Fun and Games",
                                    PublicationType.Book);
  ```

  ただし、`Book` クラスのソース コードが示すように、インスタンス コンストラクターは派生クラスのコンストラクターから直接呼び出すことができます。

- 出版物に関する 2 つのプロパティ

  `Title` は読み取り専用の <xref:System.String> プロパティで、`Publication` コンストラクターを呼び出すことでその値が提供されます。

  `Pages` は読み取り/書き込みの <xref:System.Int32> プロパティで、出版物の総ページ数を示します。 その値は `totalPages` というプライベート フィールドに格納されています。 正の数である必要があり、そうでなければ <xref:System.ArgumentOutOfRangeException> がスローされます。

- 出版社に関するメンバー

  2 つの読み取り専用プロパティ `Publisher` と `Type`。 これらの値はもともと `Publication` クラスのコンストラクターへの呼び出しによって提供されるものです。

- 出版に関するメンバー

  `Publish` および `GetPublicationDate` という 2 つのメソッドは、発行日を設定して返すものです。 `Publish` メソッドは、呼び出されるとプライベートの `published` フラグを `true` に設定し、渡された日付を引数としてプライベート `datePublished` フィールドに割り当てます。 `GetPublicationDate` メソッドは、`published` フラグが `false` の場合に文字列 "NYP" を返し、`true` の場合に `datePublished` フィールドの値を返します。

- 著作権に関するメンバー

  `Copyright` メソッドは、著作権者の名前および著作権年を引数として受け取り、`CopyrightName` および `CopyrightDate` プロパティに割り当てます。

- `ToString` メソッドのオーバーライド

  ある型が <xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドをオーバーライドしない場合、その型の完全修飾名を返しますが、これはインスタンスの区別にはほとんど役に立ちません。 `Publication` クラスは <xref:System.Object.ToString%2A?displayProperty=nameWithType> をオーバーライドして、`Title` プロパティの値を返します。

次の図は、基底の `Publication` クラスとその暗黙的に継承された <xref:System.Object> クラスの関係を表しています。

![Object および Publication クラス](media/publication-class.jpg)

### <a name="the-book-class"></a>`Book` クラス

`Book` クラスは、特定の種類の出版物としての本を表します。 次の例は、`Book` クラスのソース コードを示しています。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/base-and-derived.cs#2)]

`Publication` から継承したメンバーに加え、`Book` クラスは次の一意のメンバーおよびメンバー オーバーライドを定義します。

- 2 つのコンストラクター

  2 つの `Book` コンストラクターは、共通パラメーターを 3 つ共有しています。 *タイトル*および*出版社*の 2 つは、`Publication` コンストラクターのパラメーターに対応します。 3 つ目は*著者*で、パブリックの変更不可の `Author` プロパティに格納されています。 1 つのコンストラクターには *isbn* パラメーターが 1 つ含まれていて、`ISBN` 自動プロパティに格納されています。

  最初のコンストラクターは[この](../language-reference/keywords/this.md)キーワードを使用して、他のコンストラクターを呼び出します。 コンストラクター チェーンは、コンストラクターを定義する上で一般的なパターンです。 パラメーターが最も多いコンストラクターを呼び出すときに、パラメーターのより少ないコンストラクターが既定値を提供するものです。

  2 番目のコンストラクターは [base](../language-reference/keywords/base.md) キーワードを使用して、基底クラスのコンストラクターにタイトルと出版社名を渡します。 ソース コードで基底クラスのコンストラクターを明示的に呼び出さない場合、C# コンパイラは、基底クラスの既定またはパラメーターなしのコンストラクターへの呼び出しを自動的に提供します。

- 読み取り専用の `ISBN` プロパティ。`Book` オブジェクトの ISBN (一意の 10 ～ 13 桁の数字) を返します。 ISBN は `Book` コンストラクターの 1 つに引数として提供されます。 ISBN は、コンパイラで自動生成されるプライベート バッキング フィールドに格納されます。

- 読み取り専用の `Author` プロパティ。 著者名は両方の `Book` コンストラクターに引数として提供され、プロパティに格納されます。

- 価格に関する、2 つの読み取り専用の `Price` と `Currency` のプロパティ。 これらの値は、`SetPrice` メソッド呼び出しで引数として提供されます。 `Currency` プロパティは 3 桁の ISO 通貨記号 (たとえば米ドルの場合は USD) です。 ISO 通貨記号は <xref:System.Globalization.RegionInfo.ISOCurrencySymbol%2A> プロパティから取得できます。 これらのプロパティは両方とも外部では読み取り専用ですが、`Book` クラスのコードによって設定できます。

- `SetPrice` メソッド。`Price` プロパティおよび `Currency` プロパティの値を設定します。 これらの値は、それぞれ同じプロパティによって返されます。

- `ToString` メソッド (`Publication` から継承) へのオーバーライドと、<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> メソッドおよび <xref:System.Object.GetHashCode%2A> メソッド (<xref:System.Object> から継承) へのオーバーライド。

  オーバーライドされない限り、<xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> メソッドは参照の等価性を調べます。 つまり、2 つのオブジェクト変数は同じオブジェクトを参照している場合に等価であると見なされます。 一方、`Book` クラスでは、2 つの `Book` オブジェクトは同じ ISBN を持つ場合に等価であるはずです。

  <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> メソッドをオーバーライドする場合、<xref:System.Object.GetHashCode%2A> メソッドもオーバーライドする必要があります。このメソッドは、ランタイムで項目をハッシュされたコレクションに格納し効率的に取得するために使用する値を返すものです。 ハッシュ コードは、等価性のテストと一致する値を返します。 <xref:System.Object.Equals%28System.Object%29?displayProperty=nameWithType> をオーバーライドして、2 つの `Book` オブジェクトの ISBN プロパティが等しい場合に `true` を返すようにしたので、`ISBN` プロパティによって返される文字列の <xref:System.String.GetHashCode%2A> メソッドを呼び出すことにより計算されるハッシュ コードを返します。

次の図は、`Book` クラスとその基底クラスである `Publication` の関係を表しています。

![Publication クラスおよび Book クラス](media/book-class.jpg)

これで、次の例に示すように、`Book` オブジェクトをインスタンス化して、その一意のメンバーと継承されたメンバーの両方を呼び出し、`Publication` 型または `Book` 型のパラメーターを必要とするメソッドに引数として渡すことができるようになりました。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/use-publication.cs#1)]

## <a name="designing-abstract-base-classes-and-their-derived-classes"></a>抽象基底クラスとその派生クラスの設計
<a name="abstract"></a>

前述の例では、多くのメソッドの実装を提供する基底クラスを定義し、派生クラスがコードを共有できるようにしました。 しかし多くの場合、基底クラスが実装を提供する必要はありません。 むしろ基底クラスは、*抽象メソッド* を宣言する*抽象クラス* であり、各派生クラスで実装する必要があるメンバーを定義するテンプレートとして機能します。 通常、抽象基底クラスでは、派生型の実装はそれぞれその型に固有のものです。 クラスでは出版物に共通の機能の実装が提供されましたが、`Publication` オブジェクトをインスタンス化しても意味はないので、クラスは abstract キーワードでマークしました。

たとえば、2 次元の閉じた幾何学図形には、2 つのプロパティが含まれます。図形の内部の大きさである面積と、図形の辺に沿った長さである周です。 一方で、これらのプロパティの計算方法は、それぞれの図形によって違います。 たとえば、円周の計算式は、三角形の周の計算式とは異なります。 `Shape` クラスは、`abstract` メソッドがある `abstract` クラスです。 これは、派生クラスで同じ機能が共有されるものの、それらの派生クラスではその機能が異なる方法で実装されることを示します。

次の例では、`Area` と `Perimeter` という 2 つのプロパティを定義する、`Shape` という名前の抽象基底クラスを定義します。 クラスを [abstract](../language-reference/keywords/abstract.md) キーワードでマークするだけでなく、インスタンス メンバーもそれぞれ [abstract](../language-reference/keywords/abstract.md) キーワードでマークされます。 ここで `Shape` は <xref:System.Object.ToString%2A?displayProperty=nameWithType> メソッドもオーバーライドして、完全修飾名ではなく、その型の名前を返します。 そして `GetArea` と `GetPerimeter` の 2 つの静的メンバーを定義し、呼び出し元で任意の派生クラスのインスタンスの面積と周を簡単に取得できるようにします。 これらのメソッドのいずれかに派生クラスのインスタンスを渡すとき、ランタイムは派生クラスのメソッド オーバーライドを呼び出します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#1)]

ここで特定の図形を表す `Shape` から、いくつかのクラスを派生させることができます。 次の例では、`Triangle`、`Rectangle`、および `Circle` の 3 つのクラスを定義します。 これらのクラスはそれぞれ、その図形に一意の数式を使用して面積と周を計算します。 一部の派生クラスも、`Rectangle.Diagonal` や `Circle.Diameter` など、その図形に固有のプロパティを定義します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#2)]

次の例では、`Shape` から派生したオブジェクトを使用しています。 ここでは `Shape` から派生したオブジェクトの配列をインスタンス化して、`Shape` クラスの静的メソッドを呼び出します。これにより、返された `Shape` のプロパティ値がラップされます。 ランタイムは、派生型のオーバーライドされたプロパティから値を取得します。 この例ではまた、配列内の `Shape` オブジェクトをそれぞれの派生型にキャストし、キャストが成功すると、その特定の `Shape` サブクラスのプロパティを取得します。

[!code-csharp[Inheritance](../../../samples/snippets/csharp/tutorials/inheritance/shape.cs#3)]

## <a name="see-also"></a>関連項目

- [クラスとオブジェクト](../tour-of-csharp/classes-and-objects.md)
- [継承 (C# プログラミング ガイド)](../programming-guide/classes-and-structs/inheritance.md)
