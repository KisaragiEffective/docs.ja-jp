---
title: 'チュートリアル: 型プロバイダーの作成'
description: いくつかの単純型プロバイダー F#を調べてF#基本的な概念を説明することにより、3.0 で独自の型プロバイダーを作成する方法について説明します。
ms.date: 11/04/2019
ms.openlocfilehash: 8df893669b8ee04bad366dbe42a55c83d1f5a8fe
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73968371"
---
# <a name="tutorial-create-a-type-provider"></a>チュートリアル: 型プロバイダーの作成

の型プロバイダー機構F#は、情報豊富なプログラミングをサポートするための重要な部分です。 このチュートリアルでは、基本的な概念を示すために単純な型プロバイダーをいくつか作成する過程を通して、独自の型プロバイダーを作成する方法を説明します。 のF#型プロバイダー機構の詳細については、「[型プロバイダー](index.md)」を参照してください。

F#エコシステムには、一般的に使用されるインターネットおよびエンタープライズデータサービス用のさまざまな種類のプロバイダーが含まれています。 (例:

- Fsharp.core には、JSON、XML、CSV、HTML ドキュメント形式の型プロバイダーが含まれてい[ます。](https://fsharp.github.io/FSharp.Data/)

- [Sqlprovider](https://fsprojects.github.io/SQLProvider/)は、これらのデータソースに対するオブジェクトマッピングとF# LINQ クエリを使用して、SQL データベースへの厳密に型指定されたアクセスを提供します。

- [Fsharp.core](https://fsprojects.github.io/FSharp.Data.SqlClient/)には、の t-sql F#のコンパイル時チェック埋め込み用の型プロバイダーのセットが含まれています。

- [Fsharp.core](https://fsprojects.github.io/FSharp.Data.TypeProviders/)は、SQL、Entity Framework、OData、および WSDL データサービスにアクセスするための .NET Framework プログラミングでのみ使用するための、以前の種類のプロバイダーのセットです。

必要に応じて、カスタム型プロバイダーを作成することも、他のユーザーが作成した型プロバイダーを参照することもできます。 たとえば、組織に 1 つのデータ サービスを用意し、そのデータ サービスにより、増加し続ける名前付きデータセット群と各データセットの安定したデータ スキーマを提供することができます。 スキーマを読み取り、最新のデータセットを厳密に型指定してプログラマに提示する型プロバイダーを作成できます。

## <a name="before-you-start"></a>開始前の作業

型プロバイダー メカニズムは、主に F# のプログラミング作業にデータとサービスの安定した情報空間を提供するために設計されています。

このメカニズムは、プログラム ロジックに合うようにプログラムの実行中にスキーマが変わるような情報空間を提供するためのものではありません。 また、このメカニズムは言語内メタプログラミングでいくつか有効な使用法がありますが、この分野のために設計されたものではありません。 このメカニズムは型プロバイダーの作成が非常に高い価値をもたらし必要な場合にのみ使用してください。

スキーマを使用できない場合は、型プロバイダーを作成しないでください。 同様に、通常の (または既存の) .NET ライブラリが十分に機能する場合は、型プロバイダーを作成しないでください。

開始する前に、次の検討事項を確認してください。

- 情報ソースのスキーマがあるか。 その場合、F# および .NET の型システムへのマッピングはどうか。

- 既存の (動的に型指定された) API を実装の開始点として使用できるか。

- 型プロバイダーを作成する理由として十分な使用が見込まれるか。 通常の .NET ライブラリでニーズに対応できるか。

- 使用するスキーマがどの程度変わるか。

- スキーマがコーディング中に変わるか。

- スキーマがコーディング セッション間で変わるか。

- スキーマがプログラムの実行中に変わるか。

型プロバイダーはスキーマが実行時とコンパイル済みコードの有効期間中に安定している場合に最も適しています。

## <a name="a-simple-type-provider"></a>単純な型プロバイダー

このサンプルは HelloWorldTypeProvider です。これは、 [ F#型プロバイダー SDK](https://github.com/fsprojects/FSharp.TypeProviders.SDK/)の `examples` ディレクトリのサンプルに似ています。 このプロバイダーは、次のコードが示すように、F# シグネチャ構文を使用して `Type1` 以外の詳細を省略することで、消去型 100 個を含む "型空間" を使用可能にします。 消去型の詳細については、このトピックで後述する「[指定された型の消去に関する詳細](#details-about-erased-provided-types)」を参照してください。

```fsharp
namespace Samples.HelloWorldTypeProvider

type Type1 =
    /// This is a static property.
    static member StaticProperty : string

    /// This constructor takes no arguments.
    new : unit -> Type1

    /// This constructor takes one argument.
    new : data:string -> Type1

    /// This is an instance property.
    member InstanceProperty : int

    /// This is an instance method.
    member InstanceMethod : x:int -> char

    nested type NestedType =
        /// This is StaticProperty1 on NestedType.
        static member StaticProperty1 : string
        …
        /// This is StaticProperty100 on NestedType.
        static member StaticProperty100 : string

type Type2 =
…
…

type Type100 =
…
```

指定された型とメンバーのセットは静的で既知であることに注意してください。 この例では、スキーマに依存する型を指定するプロバイダーの機能を利用しません。 型プロバイダーの実装は次のコードでその概要を示し、詳細については、このトピックの後半のセクションで説明します。

> [!WARNING]
> このコードとオンラインサンプルには違いがある場合があります。

```fsharp
namespace Samples.FSharp.HelloWorldTypeProvider

open System
open System.Reflection
open ProviderImplementation.ProvidedTypes
open FSharp.Core.CompilerServices
open FSharp.Quotations

// This type defines the type provider. When compiled to a DLL, it can be added
// as a reference to an F# command-line compilation, script, or project.
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =

  // Inheriting from this type provides implementations of ITypeProvider
  // in terms of the provided types below.
  inherit TypeProviderForNamespaces(config)

  let namespaceName = "Samples.HelloWorldTypeProvider"
  let thisAssembly = Assembly.GetExecutingAssembly()

  // Make one provided type, called TypeN.
  let makeOneProvidedType (n:int) =
  …
  // Now generate 100 types
  let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]

  // And add them to the namespace
  do this.AddNamespace(namespaceName, types)

[<assembly:TypeProviderAssembly>]
do()
```

このプロバイダーを使用するには、Visual Studio の別のインスタンスをF#開き、スクリプトを作成した後、次のコードに示すように、#r を使用してスクリプトからプロバイダーへの参照を追加します。

```fsharp
#r @".\bin\Debug\Samples.HelloWorldTypeProvider.dll"

let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")

let obj2 = Samples.HelloWorldTypeProvider.Type1("some other data")

obj1.InstanceProperty
obj2.InstanceProperty

[ for index in 0 .. obj1.InstanceProperty-1 -> obj1.InstanceMethod(index) ]
[ for index in 0 .. obj2.InstanceProperty-1 -> obj2.InstanceMethod(index) ]

let data1 = Samples.HelloWorldTypeProvider.Type1.NestedType.StaticProperty35
```

型プロバイダーが生成した `Samples.HelloWorldTypeProvider` という名前空間で型を探します。

プロバイダーを再コンパイルする前に、そのプロバイダーの DLL を使用する Visual Studio と F# Interactive のすべてのインスタンスを閉じる必要があります。 これを行わない場合、出力の DLL がロックされているためビルド エラーが発生します。

print ステートメントを使ってこのプロバイダーをデバッグするには、プロバイダーの問題を明らかにするスクリプトを作成して、次のコードを使用します。

```console
fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

Visual Studio を使用してこのプロバイダーをデバッグするには、管理者資格情報を使用して Visual Studio の開発者コマンドプロンプトを開き、次のコマンドを実行します。

```console
devenv.exe /debugexe fsc.exe -r:bin\Debug\HelloWorldTypeProvider.dll script.fsx
```

別の方法として、Visual Studio を開き、[デバッグ] メニューを開き、[`Debug/Attach to process…`] をクリックして、スクリプトを編集している別の `devenv` プロセスにアタッチします。 この方法を使用すると、2 番目のインスタンス (IntelliSense およびその他の機能をすべて備えている) に式を対話形式で入力できるので、型プロバイダー内の特定のロジックを簡単に対象とすることができます。

生成されたコード内のエラーを特定しやすくするために、マイ コードのみのデバッグを無効にできます。 この機能を有効または無効にする方法の詳細については、「[デバッガーでのコード間の移動](/visualstudio/debugger/navigating-through-code-with-the-debugger)」を参照してください。 また、[`Debug`] メニューを開き、[`Exceptions`] を選択するか、Ctrl + Alt + E キーを押して [`Exceptions`] ダイアログボックスを開くことで、初回例外のキャッチを設定することもできます。 そのダイアログボックスの `Common Language Runtime Exceptions`で、`Thrown` チェックボックスをオンにします。

### <a name="implementation-of-the-type-provider"></a>型プロバイダーの実装

ここでは、型プロバイダーの実装の主なセクションを順に説明します。 まず、カスタム型プロバイダーの型自体を定義します。

```fsharp
[<TypeProvider>]
type SampleTypeProvider(config: TypeProviderConfig) as this =
```

この型はパブリックである必要があります。また、別F#のプロジェクトが型を含むアセンブリを参照しているときにコンパイラが型プロバイダーを認識するように、 [typeprovider](https://msdn.microsoft.com/library/bdf7b036-7490-4ace-b79f-c5f1b1b37947)属性でマークする必要があります。 *Config*パラメーターは省略可能であり、存在する場合は、コンパイラにF#よって作成される型プロバイダーインスタンスのコンテキスト構成情報が含まれています。

次に、 [ITypeProvider](https://msdn.microsoft.com/library/2c2b0571-843d-4a7d-95d4-0a7510ed5e2f)インターフェイスを実装します。 この場合、基本型として `TypeProviderForNamespaces` API の `ProvidedTypes` 型を使用します。 このヘルパー型は、集中的に指定された名前空間の有限のコレクションを指定できます。個々の名前空間には、集中的に指定された固定の型 (有限数) が直接含まれています。 このコンテキストでは、プロバイダー*集中的*は、必要または使用されていない場合でも型を生成します。

```fsharp
inherit TypeProviderForNamespaces(config)
```

次に、指定された型の名前空間を指定するローカル プライベート値を定義し、型プロバイダー アセンブリ自体を見つけます。 このアセンブリは後で、指定された型が消去される場合の論理親の型として使用されます。

```fsharp
let namespaceName = "Samples.HelloWorldTypeProvider"
let thisAssembly = Assembly.GetExecutingAssembly()
```

次に、各型 (Type1…Type100) を指定する関数を作成します。 この関数は、このトピックの後半で詳しく説明します。

```fsharp
let makeOneProvidedType (n:int) = …
```

次に、指定された型 100 個を生成します。

```fsharp
let types = [ for i in 1 .. 100 -> makeOneProvidedType i ]
```

次に、指定された名前空間としてそれらの型を追加します。

```fsharp
do this.AddNamespace(namespaceName, types)
```

最後に、型プロバイダー DLL を作成していることを示すアセンブリ属性を追加します。

```fsharp
[<assembly:TypeProviderAssembly>]
do()
```

### <a name="providing-one-type-and-its-members"></a>1 つの型とそのメンバーの指定

`makeOneProvidedType` 関数は、型の 1 つを指定する実際の処理を行います。

```fsharp
let makeOneProvidedType (n:int) =
…
```

この手順では、この関数の実装を説明します。 最初に、指定された型を作成します (たとえば、n = 1 のときは Type1、n = 57 のときは Type57)。

```fsharp
// This is the provided type. It is an erased provided type and, in compiled code,
// will appear as type 'obj'.
let t = ProvidedTypeDefinition(thisAssembly, namespaceName,
                               "Type" + string n,
                               baseType = Some typeof<obj>)
```

次の点に注意してください。

- この指定された型は消去されます。  基本型が `obj`であることを示すため、インスタンスはコンパイル済みコードで[obj](https://msdn.microsoft.com/library/dcf2430f-702b-40e5-a0a1-97518bf137f7)型の値として表示されます。

- 入れ子になっていない型を指定する場合、アセンブリと名前空間を指定する必要があります。 消去型の場合、アセンブリは型プロバイダー アセンブリ自体である必要があります。

次に、型に XML ドキュメントを追加します。 このドキュメントは遅延されます。つまり、ホスト コンパイラで必要な場合に、要求に応じて計算されます。

```fsharp
t.AddXmlDocDelayed (fun () -> sprintf "This provided type %s" ("Type" + string n))
```

次に、指定された静的プロパティを型に追加します。

```fsharp
let staticProp = ProvidedProperty(propertyName = "StaticProperty",
                                  propertyType = typeof<string>,
                                  isStatic = true,
                                  getterCode = (fun args -> <@@ "Hello!" @@>))
```

このプロパティを取得すると、常に文字列 "Hello!" に評価されます。 プロパティの `GetterCode` は F# クォートを使用しますが、これはホスト コンパイラがプロパティを取得するために生成するコードを表します。 引用符の詳細については、「[コードF#引用符」 ()](https://msdn.microsoft.com/library/6f055397-a1f0-4f9a-927c-f0d7c6951155)を参照してください。

XML ドキュメントをプロパティに追加します。

```fsharp
staticProp.AddXmlDocDelayed(fun () -> "This is a static property")
```

ここで、指定された型に指定されたプロパティを添付します。 指定された各メンバーは 1 つの型にのみ添付します。 これに従わない場合、そのメンバーにアクセスできなくなります。

```fsharp
t.AddMember staticProp
```

ここで、パラメーターを受け取らない指定されたコンストラクターを作成します。

```fsharp
let ctor = ProvidedConstructor(parameters = [ ],
                               invokeCode = (fun args -> <@@ "The object data" :> obj @@>))
```

そのコンストラクターの `InvokeCode` は F# クォートを返しますが、これはコンストラクターが呼び出されたときにホスト コンパイラが生成するコードを表します。 たとえば、次のようなコンストラクターを使用できます。

```fsharp
new Type10()
```

指定された型のインスタンスは、基になるデータである "The object data" を使って作成されます。 引用符で囲まれたコードには、指定された型を宣言したときに指定したように、その型がこの指定された型の消去であるため、 [obj](https://msdn.microsoft.com/library/dcf2430f-702b-40e5-a0a1-97518bf137f7)への変換が含まれています。

XML ドキュメントをコンストラクターに追加し、指定された型に指定されたコンストラクターを追加します。

```fsharp
ctor.AddXmlDocDelayed(fun () -> "This is a constructor")

t.AddMember ctor
```

パラメーターを 1 つ受け取る指定された 2 番目のコンストラクターを作成します。

```fsharp
let ctor2 =
ProvidedConstructor(parameters = [ ProvidedParameter("data",typeof<string>) ],
                    invokeCode = (fun args -> <@@ (%%(args.[0]) : string) :> obj @@>))
```

そのコンストラクターの `InvokeCode` も F# クォートを返しますが、そのクォートはそのメソッドを呼び出すためにホスト コンパイラが生成したコードを表します。 たとえば、次のようなコンストラクターを使用できます。

```fsharp
new Type10("ten")
```

指定された型のインスタンスが、基になるデータである "ten" を使って作成されます。 既に気付いている読者もいるかもしれませんが、`InvokeCode` 関数の戻り値はクォートです。 この関数への入力は式のリストで、コンストラクター パラメーターごとに 1 つのリストです。 この場合、1 つのパラメーター値を表す式は `args.[0]` にあります。 コンストラクターへの呼び出しのコードは、戻り値を消去型 `obj` に強制的に変換します。 指定された 2 番目のコンストラクターをその型に追加した後、指定されたインスタンス プロパティを作成します。

```fsharp
let instanceProp =
    ProvidedProperty(propertyName = "InstanceProperty",
                     propertyType = typeof<int>,
                     getterCode= (fun args ->
                        <@@ ((%%(args.[0]) : obj) :?> string).Length @@>))
instanceProp.AddXmlDocDelayed(fun () -> "This is an instance property")
t.AddMember instanceProp
```

このプロパティを取得すると、その戻り値は表現オブジェクトである文字列の長さです。 `GetterCode` プロパティは、ホスト コンパイラがプロパティを取得するために生成するコードを指定する F# クォートを返します。 `InvokeCode` と同様に、`GetterCode` 関数はクォートを返します。 ホスト コンパイラはこの関数を一連の引数を使って呼び出します。 この場合、引数には、getter が呼び出されるインスタンスを表す単一の式だけが含まれます。これは、`args.[0]`を使用してアクセスできます。 `GetterCode` の実装では、`obj`消去された型の結果の引用符に分割し、キャストを使用して、オブジェクトが文字列であることを確認するためのコンパイラの機構を満たしています。 `makeOneProvidedType` の次の部分は 1 個のパラメーターを持つインスタンス メソッドを指定します。

```fsharp
let instanceMeth =
    ProvidedMethod(methodName = "InstanceMethod",
                   parameters = [ProvidedParameter("x",typeof<int>)],
                   returnType = typeof<char>,
                   invokeCode = (fun args ->
                       <@@ ((%%(args.[0]) : obj) :?> string).Chars(%%(args.[1]) : int) @@>))

instanceMeth.AddXmlDocDelayed(fun () -> "This is an instance method")
// Add the instance method to the type.
t.AddMember instanceMeth
```

最後に、100 個のプロパティを含む入れ子構造の型を作成します。 この入れ子構造の型とそのプロパティの作成は遅延されます。つまり、要求に応じて計算されます。

```fsharp
t.AddMembersDelayed(fun () ->
  let nestedType = ProvidedTypeDefinition("NestedType", Some typeof<obj>)

  nestedType.AddMembersDelayed (fun () ->
    let staticPropsInNestedType =
      [
          for i in 1 .. 100 ->
              let valueOfTheProperty = "I am string "  + string i
    
              let p =
                ProvidedProperty(propertyName = "StaticProperty" + string i,
                  propertyType = typeof<string>,
                  isStatic = true,
                  getterCode= (fun args -> <@@ valueOfTheProperty @@>))
    
              p.AddXmlDocDelayed(fun () ->
                  sprintf "This is StaticProperty%d on NestedType" i)
    
              p
      ]

    staticPropsInNestedType)

  [nestedType])
```

### <a name="details-about-erased-provided-types"></a>指定された型が消去される場合の詳細

このセクションの例では、*指定された型の消去*のみを行います。これは、次のような場合に特に便利です。

- データとメソッドのみを含む情報空間のプロバイダーを作成する場合。

- 情報空間の実用上、正確なランタイム型のセマンティクスが重要ではないプロバイダーを作成する場合。

- 情報空間のプロバイダーを作成する際に、情報空間の規模が大きく相互接続していて、情報空間の .NET 型を実際に生成するのが技術的に不可能な場合。

この例では、指定された型は消去されてそれぞれ型 `obj` に変換され、コンパイル済みコードではすべて型 `obj` で表されます。 実際、次の例の基になるオブジェクトは文字列ですが、.NET コンパイル済みコードでは `System.Object` として表されます。 使用されるすべての型消去に当てはまりますが、明示的なボックス化、ボックス化解除、およびキャストを使用すると消去型を無効にすることができます。 この場合、オブジェクトを使用したときに無効なキャスト例外が発生する可能性があります。 プロバイダー ランタイムは間違った表現から保護するため独自のプライベート表現型を定義できます。 F# では消去型を定義できません。 指定された型が消去される場合があるだけです。 型プロバイダーで消去型を使用する場合、または消去型を指定するプロバイダーで消去型を使用する場合のいずれも、実際およびセマンティクスの両方の影響を理解しておく必要があります。 消去型には実際の .NET 型はありません。 したがって、型に対して正確なリフレクションを実行できず、厳密なランタイム型のセマンティクスに依存するランタイム キャストなどの手法を使用する場合は消去型が無効になる場合があります。 消去型の無効化により、多くの場合、実行時に型キャスト例外が発生します。

### <a name="choosing-representations-for-erased-provided-types"></a>指定された型が消去される場合の表現の選択

指定された型の消去を使用する場合、表現が必須ではない場合があります。 たとえば、指定された型が消去される場合にその型には静的なプロパティとメンバーのみが含まれてコンストラクターは含まれない場合があり、メソッドもプロパティもその型のインスタンスを返しません。 指定された型が消去される場合のインスタンスを取得できる場合は、次の事項を検討する必要があります。

**指定された型の消去は何ですか。**

- 指定された型の消去は、コンパイル済みの .NET コードにおけるその型の表現である。

- 指定された消去クラス型の消去は、常に型の継承チェーンにおける消去型でない最初の基本型である。

- 指定された消去インターフェイス型の消去は、常に `System.Object` である。

**指定された型の表現はどのようなものですか。**

- 指定された型が消去される場合に使用可能なオブジェクトのセットがその表現として呼び出される。 このドキュメントの例では、指定された型 `Type1..Type100` が消去される場合の表現はすべて、常に文字列オブジェクトです。

指定された型の表現はすべて指定された型の消去と互換性がある必要があります (互換性がない場合、F# コンパイラがその型プロバイダーの使用に対してエラーを生成するか、または検証不可能で無効な .NET コードが生成されます。 有効ではない表現を指定するコードを返す型プロバイダーは無効です)。

次のアプローチのどちらかを使って指定されたオブジェクトの表現を選択できます。どちらもよく使用されます。

- 既存の .NET 型に対して厳密に型指定されたラッパーを指定するだけであれば、多くの場合、使用する型を消去してその型に変換し、その型のインスタンスを表現として使用するのが理にかなっています。 このアプローチは、厳密に型指定されたバージョンを使用するとき、その型の既存のメソッドのほとんどをそのまま使用できる場合に適しています。

- 既存の .NET API とは大きく異なる API を作成する場合は、指定された型の型消去および表現となるランタイム型を作成するのが理にかなっています。

このドキュメントの例では、指定されたオブジェクトの表現として文字列を使用します。 しばしば、表現に他のオブジェクトを使用する方が適切な場合もあります。 たとえば、プロパティ バッグとしてディクショナリを使用できます。

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new Dictionary<string,obj>()) :> obj @@>))
```

もう 1 つの方法として、型プロバイダーで、1 つ以上のランタイム操作と共に表現を形成するために実行時に使用される型を定義できます。

```fsharp
type DataObject() =
    let data = Dictionary<string,obj>()
    member x.RuntimeOperation() = data.Count
```

指定されたメンバーはこのオブジェクト型のインスタンスを作成できます。

```fsharp
ProvidedConstructor(parameters = [],
    invokeCode= (fun args -> <@@ (new DataObject()) :> obj @@>))
```

この場合、必要に応じて、`baseType` を作成するときにこの型を `ProvidedTypeDefinition` として指定することによって、この型を型消去として使用することもできます。

```fsharp
ProvidedTypeDefinition(…, baseType = Some typeof<DataObject> )
…
ProvidedConstructor(…, InvokeCode = (fun args -> <@@ new DataObject() @@>), …)
```

### <a name="key-lessons"></a>レッスンの要点

前のセクションでは、一連の型、プロパティ、およびメソッドを指定する、消去の単純な型プロバイダーの作成方法を説明しました。 また、型プロバイダーから消去型を指定することの長所と短所を含む型消去の概念について、および消去型の表現についても説明しました。

## <a name="a-type-provider-that-uses-static-parameters"></a>静的パラメーターを使用する型プロバイダー

静的データによって型プロバイダーをパラメーター化できると、プロバイダーがローカルまたはリモート データにアクセスする必要がないような、多くの興味深いシナリオが可能になります。 このセクションでは、このようなプロバイダーを用意するための基本的な手法について説明します。

### <a name="type-checked-regex-provider"></a>型チェック済み Regex プロバイダー

.NET <xref:System.Text.RegularExpressions.Regex> ライブラリをラップする正規表現の型プロバイダーを、次のコンパイル時保証を提供するインターフェイスに実装するとしたらどうでしょう。

- 正規表現が有効かどうかを確認できる。

- パターンが一致したとき、正規表現内のグループ名に基づいて名前付きプロパティを提供できる。

このセクションでは、これらの利点が実現するように正規表現パターンによってパラメーター化される `RegexTyped` 型を作成するための型プロバイダーを使用する方法を示します。 コンパイラは、提供されたパターンが有効でない場合にエラーを報告しますが、型プロバイダーはパターンからグループを抽出できるので、パターンが一致したとき名前付きプロパティを使用してグループにアクセスできます。 型プロバイダーを設計するとき、公開された API がエンド ユーザーにどのように表示される必要があるか、そのデザインが .NET コードにどのように変換されるかを考慮する必要があります。 次の例は、市外局番のコンポーネントを取得するために、このような API を使用する方法を示しています。

```fsharp
type T = RegexTyped< @"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)">
let reg = T()
let result = T.IsMatch("425-555-2345")
let r = reg.Match("425-555-2345").Group_AreaCode.Value //r equals "425"
```

型プロバイダーがこれらの呼び出しをどのように変換するかを次の例に示します。

```fsharp
let reg = new Regex(@"(?<AreaCode>^\d{3})-(?<PhoneNumber>\d{3}-\d{4}$)")
let result = reg.IsMatch("425-123-2345")
let r = reg.Match("425-123-2345").Groups.["AreaCode"].Value //r equals "425"
```

次の点に注意してください。

- 標準 Regex 型は、パラメーター化された `RegexTyped` 型を表します。

- `RegexTyped` コンストラクターは、Regex コンストラクターを呼び出し、パターンの静的な型引数を渡します。

- `Match` メソッドの結果は、標準の <xref:System.Text.RegularExpressions.Match> 型で表されます。

- 各名前付きグループは指定されたプロパティになり、プロパティにアクセスすると、パターン一致の `Groups` コレクションでインデクサーが使用されます。

次のコードはこのようなプロバイダーの実装におけるコア ロジックです。この例では指定された型へのすべてのメンバーの追加は省略されています。 それぞれの追加されたメンバーについては、このトピックの後半の該当するセクションを参照してください。 完全なコードについては、CodePlex web サイトの[ F# 3.0 サンプルパック](https://archive.codeplex.com/?p=fsharp3sample)からサンプルをダウンロードしてください。

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

          match parameterValues with
          | [| :? string as pattern|] ->

            // Create an instance of the regular expression.
            //
            // This will fail with System.ArgumentException if the regular expression is not valid.
            // The exception will escape the type provider and be reported in client code.
            let r = System.Text.RegularExpressions.Regex(pattern)

            // Declare the typed regex provided type.
            // The type erasure of this type is 'obj', even though the representation will always be a Regex
            // This, combined with hiding the object methods, makes the IntelliSense experience simpler.
            let ty =
              ProvidedTypeDefinition(
                thisAssembly,
                rootNamespace,
                typeName,
                baseType = Some baseTy)

            ...

            ty
          | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

次の点に注意してください。

- 型プロバイダーは、`pattern` (必須) と `options` (既定値が指定されるので省略可能) の 2 つの静的パラメーターを受け取ります。

- 静的引数が提供された後、正規表現のインスタンスを作成します。 Regex が正しくない場合、このインスタンスは例外をスローし、このエラーはユーザーに報告されます。

- `DefineStaticParameters` コールバック内では、引数が提供された後に返される型を定義します。

- IntelliSense による効率化が損なわれないように、このコードは `HideObjectMethods` を true に設定します。 この属性により、`Equals`、`GetHashCode`、`Finalize`、`GetType` の各メンバーが指定されたオブジェクトの IntelliSense リストで抑制されます。

- `obj` をそのメソッドの基本型として使用しますが、この型のランタイム表現には `Regex` オブジェクトを使用します。次にその例を示します。

- 正規表現が有効でない場合、`Regex` コンストラクターを呼び出すと <xref:System.ArgumentException> がスローされます。 コンパイラはこの例外をキャッチし、コンパイル時、または Visual Studio のエディターでエラー メッセージを表示します。 この例外によって、アプリケーションを実行せずに正規表現が有効かどうかを確認できます。

前に定義した型は、まだ意味のあるメソッドまたはプロパティを含んでいないので、有効ではありません。 最初に、静的な `IsMatch` メソッドを追加します。

```fsharp
let isMatch =
    ProvidedMethod(
        methodName = "IsMatch",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = typeof<bool>,
        isStatic = true,
        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string."
ty.AddMember isMatch
```

このコードは、文字列を入力として受け取り `IsMatch` を返す `bool` メソッドを定義します。 唯一わかりにくい部分は、`args` 定義内の `InvokeCode` 引数の使用です。 この例では、`args` はこのメソッドの引数を表すクォートのリストです。 メソッドがインスタンス メソッドの場合、最初の引数が `this` 引数を表します。 ただし、静的メソッドでは、引数はすべて、メソッドの明示的な引数でしかありません。 クォート値の型は指定された戻り値の型 (この場合は `bool`) に一致していることに注意してください。 また、このコードでは、指定されたメソッドも IntelliSense によって提供できる有用なドキュメントを持つように `AddXmlDoc` メソッドが使用されていることに注意してください。

次に、インスタンスの Match メソッドを追加します。 ただし、厳密に型指定された方法でグループにアクセスできるように、このメソッドは指定された型 `Match` の値を返す必要があります。 したがって、最初に `Match` 型を宣言します。 この型は静的引数として提供されたパターンに依存するため、パラメーター化された型定義内に入れ子にする必要があります。

```fsharp
let matchTy =
    ProvidedTypeDefinition(
        "MatchType",
        baseType = Some baseTy,
        hideObjectMethods = true)

ty.AddMember matchTy
```

次に、各グループの Match 型にプロパティを 1 つ追加します。 実行時には、一致は <xref:System.Text.RegularExpressions.Match> 値として表されるので、そのプロパティを定義するクォートはインデックス付きの <xref:System.Text.RegularExpressions.Match.Groups> プロパティを使用して適切なグループを取得する必要があります。

```fsharp
for group in r.GetGroupNames() do
    // Ignore the group named 0, which represents all input.
    if group <> "0" then
    let prop =
      ProvidedProperty(
        propertyName = group,
        propertyType = typeof<Group>,
        getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
        prop.AddXmlDoc(sprintf @"Gets the ""%s"" group from this match" group)
    matchTy.AddMember prop
```

ここでも指定されたプロパティに XML ドキュメントを追加していることに注意してください。 `GetterCode` 関数が指定されている場合はプロパティを読み取ることができ、`SetterCode` 関数が指定されている場合はプロパティを書き出すことができるので、その結果、プロパティは読み取り専用になることにも注意してください。

これで、この `Match` 型の値を返すインスタンス メソッドを作成できます。

```fsharp
let matchMethod =
    ProvidedMethod(
        methodName = "Match",
        parameters = [ProvidedParameter("input", typeof<string>)],
        returnType = matchTy,
        invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)

matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

ty.AddMember matchMeth
```

インスタンス メソッドを作成しているので、`args.[0]` はそのメソッドが呼び出される `RegexTyped` インスタンスを表し、`args.[1]` は入力引数を表します。

最後に、指定された型のインスタンスを作成できるように、コンストラクターを指定します。

```fsharp
let ctor =
    ProvidedConstructor(
        parameters = [],
        invokeCode = fun args -> <@@ Regex(pattern, options) :> obj @@>)

ctor.AddXmlDoc("Initializes a regular expression instance.")

ty.AddMember ctor
```

このコンストラクターは単に消去して標準の .NET Regex インスタンスの作成に変換します。`obj` は指定された型の消去なので、このインスタンスもまたオブジェクトにボックス化されます。 この変更によって、トピックの前半で示した API の使用例は期待どおりに機能します。 次に最終的に完成したコードを示します。

```fsharp
namespace Samples.FSharp.RegexTypeProvider

open System.Reflection
open Microsoft.FSharp.Core.CompilerServices
open Samples.FSharp.ProvidedTypes
open System.Text.RegularExpressions

[<TypeProvider>]
type public CheckedRegexProvider() as this =
    inherit TypeProviderForNamespaces()

    // Get the assembly and namespace used to house the provided types.
    let thisAssembly = Assembly.GetExecutingAssembly()
    let rootNamespace = "Samples.FSharp.RegexTypeProvider"
    let baseTy = typeof<obj>
    let staticParams = [ProvidedStaticParameter("pattern", typeof<string>)]

    let regexTy = ProvidedTypeDefinition(thisAssembly, rootNamespace, "RegexTyped", Some baseTy)

    do regexTy.DefineStaticParameters(
        parameters=staticParams,
        instantiationFunction=(fun typeName parameterValues ->

            match parameterValues with
            | [| :? string as pattern|] ->

                // Create an instance of the regular expression.

                let r = System.Text.RegularExpressions.Regex(pattern)

                // Declare the typed regex provided type.

                let ty =
                    ProvidedTypeDefinition(
                        thisAssembly,
                        rootNamespace,
                        typeName,
                        baseType = Some baseTy)

                ty.AddXmlDoc "A strongly typed interface to the regular expression '%s'"

                // Provide strongly typed version of Regex.IsMatch static method.
                let isMatch =
                    ProvidedMethod(
                        methodName = "IsMatch",
                        parameters = [ProvidedParameter("input", typeof<string>)],
                        returnType = typeof<bool>,
                        isStatic = true,
                        invokeCode = fun args -> <@@ Regex.IsMatch(%%args.[0], pattern) @@>)

                isMatch.AddXmlDoc "Indicates whether the regular expression finds a match in the specified input string"

                ty.AddMember isMatch

                // Provided type for matches
                // Again, erase to obj even though the representation will always be a Match
                let matchTy =
                    ProvidedTypeDefinition(
                        "MatchType",
                        baseType = Some baseTy,
                        hideObjectMethods = true)

                // Nest the match type within parameterized Regex type.
                ty.AddMember matchTy

                // Add group properties to match type
                for group in r.GetGroupNames() do
                    // Ignore the group named 0, which represents all input.
                    if group <> "0" then
                        let prop =
                          ProvidedProperty(
                            propertyName = group,
                            propertyType = typeof<Group>,
                            getterCode = fun args -> <@@ ((%%args.[0]:obj) :?> Match).Groups.[group] @@>)
                        prop.AddXmlDoc(sprintf @"Gets the ""%s"" group from this match" group)
                        matchTy.AddMember(prop)

                // Provide strongly typed version of Regex.Match instance method.
                let matchMeth =
                  ProvidedMethod(
                    methodName = "Match",
                    parameters = [ProvidedParameter("input", typeof<string>)],
                    returnType = matchTy,
                    invokeCode = fun args -> <@@ ((%%args.[0]:obj) :?> Regex).Match(%%args.[1]) :> obj @@>)
                matchMeth.AddXmlDoc "Searches the specified input string for the first occurrence of this regular expression"

                ty.AddMember matchMeth

                // Declare a constructor.
                let ctor =
                  ProvidedConstructor(
                    parameters = [],
                    invokeCode = fun args -> <@@ Regex(pattern) :> obj @@>)

                // Add documentation to the constructor.
                ctor.AddXmlDoc "Initializes a regular expression instance"

                ty.AddMember ctor

                ty
            | _ -> failwith "unexpected parameter values"))

    do this.AddNamespace(rootNamespace, [regexTy])

[<TypeProviderAssembly>]
do ()
```

### <a name="key-lessons"></a>レッスンの要点

このセクションでは静的パラメーターを操作する型プロバイダーを作成する方法を説明しました。 プロバイダーは、静的パラメーターをチェックし、その値に基づいて操作を指定します。

## <a name="a-type-provider-that-is-backed-by-local-data"></a>ローカル データに基づく型プロバイダー

しばしば、静的パラメーターだけでなくローカルまたはリモート システムからの情報にも基づく API を提供する型プロバイダーが便利な場合があります。 このセクションでは、ローカル データ ファイルなどのローカル データに基づく型プロバイダーについて説明します。

### <a name="simple-csv-file-provider"></a>単純な CSV ファイル プロバイダー

単純な例として、コンマ区切り値 (CSV) 形式の指数データにアクセスするための型プロバイダーを考えてみましょう。 ここでは、次の表に示すように、CSV ファイルにはヘッダー行があり、その後に浮動小数点型のデータが続いているとします。

|距離 (メートル)|時間 (秒)|
|----------------|-------------|
|50.0|3.7|
|100.0|5.2|
|150.0|6.4|

ここでは、`Distance` 型の `float<meter>` プロパティと `Time` 型の `float<second>` プロパティの行を取得するために使用できる型を指定する方法を示します。 説明を簡単にするために、次のように仮定します。

- ヘッダー名の形式は、"名前 (単位)" または "名前 (単位)" で、コンマは使用できません。

- 単位は、 [fsharp.core module (F#)](https://msdn.microsoft.com/library/3cb43485-11f5-4aa7-a779-558f19d4013b)モジュールで定義されている、すべてのシステムインターナショナル (si) 単位です。

- 単位はすべて単純な単位 (たとえば、メートル) で、複合単位 (メートル/秒など) ではありません。

- すべての列に浮動小数点データが含まれています。

より詳細なプロバイダーを使用すると、これらの制限は緩和されます。

ここでも、最初の手順は API がどのように表示されるかを検討することです。 前の表の内容を含む `info.csv` ファイル (コンマ区切り形式) の場合、プロバイダーを使用して次の例のようなコードを作成できます。

```fsharp
let info = new MiniCsv<"info.csv">()
for row in info.Data do
let time = row.Time
printfn "%f" (float time)
```

この場合、コンパイラはこれらの呼び出しを次の例のように変換します。

```fsharp
let info = new CsvFile("info.csv")
for row in info.Data do
let (time:float) = row.[1]
printfn "%f" (float time)
```

最適な変換を行うには、型プロバイダーのアセンブリで実際の `CsvFile` 型を定義する型プロバイダーが必要です。 型プロバイダーは、多くの場合、重要なロジックをラップするためにいくつかのヘルパー型とメソッドを使用します。 メジャーは実行時に消去されるので、行の消去型として `float[]` を使用できます。 コンパイラでは、異なる列は異なるメジャー型を持つと見なして処理します。 たとえば、この例の最初の列は `float<meter>` 型、2 番目の列は `float<second>` 型を持ちます。 ただし、消去された表現は非常に単純です。

次のコードはその実装のコアを示します。

```fsharp
// Simple type wrapping CSV data
type CsvFile(filename) =
    // Cache the sequence of all data lines (all lines but the first)
    let data =
        seq {
            for line in File.ReadAllLines(filename) |> Seq.skip 1 ->
                line.Split(',') |> Array.map float
        }
        |> Seq.cache
    member _.Data = data

[<TypeProvider>]
type public MiniCsvProvider(cfg:TypeProviderConfig) as this =
    inherit TypeProviderForNamespaces(cfg)

    // Get the assembly and namespace used to house the provided types.
    let asm = System.Reflection.Assembly.GetExecutingAssembly()
    let ns = "Samples.FSharp.MiniCsvProvider"

    // Create the main provided type.
    let csvTy = ProvidedTypeDefinition(asm, ns, "MiniCsv", Some(typeof<obj>))

    // Parameterize the type by the file to use as a template.
    let filename = ProvidedStaticParameter("filename", typeof<string>)
    do csvTy.DefineStaticParameters([filename], fun tyName [| :? string as filename |] ->

        // Resolve the filename relative to the resolution folder.
        let resolvedFilename = Path.Combine(cfg.ResolutionFolder, filename)

        // Get the first line from the file.
        let headerLine = File.ReadLines(resolvedFilename) |> Seq.head

        // Define a provided type for each row, erasing to a float[].
        let rowTy = ProvidedTypeDefinition("Row", Some(typeof<float[]>))

        // Extract header names from the file, splitting on commas.
        // use Regex matching to get the position in the row at which the field occurs
        let headers = Regex.Matches(headerLine, "[^,]+")

        // Add one property per CSV field.
        for i in 0 .. headers.Count - 1 do
            let headerText = headers.[i].Value

            // Try to decompose this header into a name and unit.
            let fieldName, fieldTy =
                let m = Regex.Match(headerText, @"(?<field>.+) \((?<unit>.+)\)")
                if m.Success then

                    let unitName = m.Groups.["unit"].Value
                    let units = ProvidedMeasureBuilder.Default.SI unitName
                    m.Groups.["field"].Value, ProvidedMeasureBuilder.Default.AnnotateType(typeof<float>,[units])

                else
                    // no units, just treat it as a normal float
                    headerText, typeof<float>

            let prop =
                ProvidedProperty(fieldName, fieldTy,
                    getterCode = fun [row] -> <@@ (%%row:float[]).[i] @@>)

            // Add metadata that defines the property's location in the referenced file.
            prop.AddDefinitionLocation(1, headers.[i].Index + 1, filename)
            rowTy.AddMember(prop)

        // Define the provided type, erasing to CsvFile.
        let ty = ProvidedTypeDefinition(asm, ns, tyName, Some(typeof<CsvFile>))

        // Add a parameterless constructor that loads the file that was used to define the schema.
        let ctor0 =
            ProvidedConstructor([],
                invokeCode = fun [] -> <@@ CsvFile(resolvedFilename) @@>)
        ty.AddMember ctor0

        // Add a constructor that takes the file name to load.
        let ctor1 = ProvidedConstructor([ProvidedParameter("filename", typeof<string>)],
            invokeCode = fun [filename] -> <@@ CsvFile(%%filename) @@>)
        ty.AddMember ctor1

        // Add a more strongly typed Data property, which uses the existing property at runtime.
        let prop =
            ProvidedProperty("Data", typedefof<seq<_>>.MakeGenericType(rowTy),
                getterCode = fun [csvFile] -> <@@ (%%csvFile:CsvFile).Data @@>)
        ty.AddMember prop

        // Add the row type as a nested type.
        ty.AddMember rowTy
        ty)

    // Add the type to the namespace.
    do this.AddNamespace(ns, [csvTy])
```

実装の次の点に注意してください。

- オーバーロードされたコンストラクターでは、元のファイル、またはそれと同一のスキーマを持つファイルを読み取ることができます。 このパターンは、ローカルまたはリモートのデータ ソースの型プロバイダーを作成するときによく使用され、ローカル ファイルをリモート データのテンプレートとして使用できるようにします。

- 型プロバイダーコンストラクターに渡された[Typeproviderconfig](https://msdn.microsoft.com/library/1cda7b9a-3d07-475d-9315-d65e1c97eb44)値を使用して、相対ファイル名を解決できます。

- 指定されたプロパティの場所を定義するには `AddDefinitionLocation` メソッドを使用できます。 したがって、指定されたプロパティで `Go To Definition` を使用すると、CSV ファイルが Visual Studio で開かれます。

- SI 単位を調べて適切な `ProvidedMeasureBuilder` 型を生成するには、`float<_>` 型を使用できます。

### <a name="key-lessons"></a>レッスンの要点

このセクションでは、単純なスキーマがデータ ソース自体に含まれているローカル データ ソースのための型プロバイダーを作成する方法を説明しました。

## <a name="going-further"></a>理解を深める

以降のセクションでは、さらに理解を深めるための参考情報を示します。

### <a name="a-look-at-the-compiled-code-for-erased-types"></a>コンパイル済みコードにおける消去型

型プロバイダーの使用が生成されたコードにどのように対応するかを説明するために、このトピックの前半で使用した `HelloWorldTypeProvider` を使って次の関数を見てみます。

```fsharp
let function1 () =
    let obj1 = Samples.HelloWorldTypeProvider.Type1("some data")
    obj1.InstanceProperty
```

ildasm.exe を使用して逆コンパイルされたコードのイメージを次に示します。

```il
.class public abstract auto ansi sealed Module1
extends [mscorlib]System.Object
{
.custom instance void [FSharp.Core]Microsoft.FSharp.Core.CompilationMappingAtt
ribute::.ctor(valuetype [FSharp.Core]Microsoft.FSharp.Core.SourceConstructFlags)
= ( 01 00 07 00 00 00 00 00 )
.method public static int32  function1() cil managed
{
// Code size       24 (0x18)
.maxstack  3
.locals init ([0] object obj1)
IL_0000:  nop
IL_0001:  ldstr      "some data"
IL_0006:  unbox.any  [mscorlib]System.Object
IL_000b:  stloc.0
IL_000c:  ldloc.0
IL_000d:  call       !!0 [FSharp.Core_2]Microsoft.FSharp.Core.LanguagePrimit
ives/IntrinsicFunctions::UnboxGeneric<string>(object)
IL_0012:  callvirt   instance int32 [mscorlib_3]System.String::get_Length()
IL_0017:  ret
} // end of method Module1::function1

} // end of class Module1
```

例に示すように、`Type1` 型と `InstanceProperty` プロパティについて示す部分はすべて消去されています。残っているのは関係するランタイム型に対する操作のみです。

### <a name="design-and-naming-conventions-for-type-providers"></a>型プロバイダーのデザインと名前付け規則

型プロバイダーを作成するときは、次の規則に従ってください。

**接続プロトコルのプロバイダー**一般に、OData や SQL 接続など、データおよびサービス接続プロトコル用のほとんどのプロバイダー Dll の名前は、`TypeProvider` または `TypeProviders`で終わる必要があります。 たとえば、次の文字列のような DLL 名を使用します。

`Fabrikam.Management.BasicTypeProviders.dll`

指定された型が対応する名前空間のメンバーであり、実装した接続プロトコルを示すようにします。

```fsharp
  Fabrikam.Management.BasicTypeProviders.WmiConnection<…>
  Fabrikam.Management.BasicTypeProviders.DataProtocolConnection<…>
```

**一般的なコーディング用のユーティリティプロバイダー**。  正規表現用などのユーティリティ型プロバイダーでは、次の例に示すように、型プロバイダーが基本ライブラリに含まれる場合があります。

```fsharp
#r "Fabrikam.Core.Text.Utilities.dll"
```

この場合、指定された型は、通常の .NET デザイン規則に従って適切な位置に表示されます。

```fsharp
  open Fabrikam.Core.Text.RegexTyped

  let regex = new RegexTyped<"a+b+a+b+">()
```

**シングルトンデータソース**。 型プロバイダーの中には、単一の専用データ ソースに接続してデータのみを指定するものがあります。 この場合、`TypeProvider` サフィックスを削除して、通常の .NET の名前付け規則を使用する必要があります。

```fsharp
#r "Fabrikam.Data.Freebase.dll"

let data = Fabrikam.Data.Freebase.Astronomy.Asteroids
```

詳細については、このトピックで後述する `GetConnection` デザイン規則を参照してください。

### <a name="design-patterns-for-type-providers"></a>型プロバイダーのパターンのデザイン

以降のセクションでは、型プロバイダーを作成するときに使用できるデザイン パターンを説明します。

#### <a name="the-getconnection-design-pattern"></a>GetConnection デザイン パターン

ほとんどの型プロバイダーは、FSharp.Data.TypeProviders.dll で型プロバイダーに使用される `GetConnection` パターンを使用するように作成されます。次にその例を示します。

```fsharp
#r "Fabrikam.Data.WebDataStore.dll"

type Service = Fabrikam.Data.WebDataStore<…static connection parameters…>

let connection = Service.GetConnection(…dynamic connection parameters…)

let data = connection.Astronomy.Asteroids
```

#### <a name="type-providers-backed-by-remote-data-and-services"></a>リモート データとサービスに基づく型プロバイダー

リモート データとサービスに基づく型プロバイダーを作成する前に、接続型プログラミングに内在するさまざまな問題を検討する必要があります。 これらの問題には、次の検討事項が含まれています。

- スキーマ マッピング

- スキーマ変更がある場合の活性と無効化

- スキーマ キャッシュ

- データ アクセス操作の非同期実装

- LINQ クエリを含むクエリのサポート

- 資格情報と認証

このトピックでは、これらの問題をこれ以上説明しません。

### <a name="additional-authoring-techniques"></a>その他の作成方法

独自の型プロバイダーを作成するとき、次の方法も使用できます。

### <a name="creating-types-and-members-on-demand"></a>型およびメンバーを要求に応じて作成する

ProvidedType API には遅延バージョンの AddMember があります。

```fsharp
  type ProvidedType =
      member AddMemberDelayed  : (unit -> MemberInfo)      -> unit
      member AddMembersDelayed : (unit -> MemberInfo list) -> unit
```

これらのバージョンは、型のオンデマンド空間の作成に使用されます。

### <a name="providing-array-types-and-generic-type-instantiations"></a>配列型とジェネリック型のインスタンス化の提供

指定されたメンバー (シグネチャには、配列型、byref 型、ジェネリック型のインスタンス化など) を使用して、通常の `MakeArrayType`、`MakePointerType`、および `ProvidedTypeDefinitions`を含む <xref:System.Type>の任意のインスタンスの `MakeGenericType` を使用します。

> [!NOTE]
> 場合によっては、`ProvidedTypeBuilder.MakeGenericType`でヘルパーを使用することが必要になることがあります。  詳細については、[型プロバイダー SDK のドキュメント](https://github.com/fsprojects/FSharp.TypeProviders.SDK/blob/master/README.md#explicit-construction-of-code-makegenerictype-makegenericmethod-and-uncheckedquotations)を参照してください。

### <a name="providing-unit-of-measure-annotations"></a>測定単位の注釈を指定する

ProvidedTypes API は、メジャーの注釈を指定するためのヘルパーを指定します。 たとえば、`float<kg>` 型を指定するには次のコードを使用します。

```fsharp
  let measures = ProvidedMeasureBuilder.Default
  let kg = measures.SI "kilogram"
  let m = measures.SI "meter"
  let float_kg = measures.AnnotateType(typeof<float>,[kg])
```

  `Nullable<decimal<kg/m^2>>` 型を指定するには次のコードを使用します。

```fsharp
  let kgpm2 = measures.Ratio(kg, measures.Square m)
  let dkgpm2 = measures.AnnotateType(typeof<decimal>,[kgpm2])
  let nullableDecimal_kgpm2 = typedefof<System.Nullable<_>>.MakeGenericType [|dkgpm2 |]
```

### <a name="accessing-project-local-or-script-local-resources"></a>プロジェクト ローカル リソースまたはスクリプト ローカル リソースにアクセスする

型プロバイダーの各インスタンスには、構築時に `TypeProviderConfig` 値を指定できます。 この値には、そのプロバイダー用の "解決フォルダー"(コンパイル用のプロジェクト フォルダーまたはスクリプトを含んでいるディレクトリ)、参照されるアセンブリのリスト、その他の情報が含まれます。

### <a name="invalidation"></a>無効化

プロバイダーは、無効化シグナルを生成して F# 言語サービスにスキーマの前提が変わった可能性があることを通知できます。 無効化が発生すると、プロバイダーが Visual Studio にホストされている場合は型チェックが再度実行されます。 このシグナルは、プロバイダーが F# Interactive または F# コンパイラ (fsc.exe) でホストされている場合は無視されます。

### <a name="caching-schema-information"></a>スキーマ情報をキャッシュする

多くの場合、プロバイダーはスキーマ情報へのアクセスをキャッシュする必要があります。 キャッシュされたデータは静的パラメーターまたはユーザー データとして指定されたファイル名を使用して格納されます。 スキーマ キャッシュの例には、`LocalSchemaFile` アセンブリ内の型プロバイダーの `FSharp.Data.TypeProviders` パラメーターがあります。 これらのプロバイダーの実装では、この静的パラメーターは、型プロバイダーがデータ ソースにネットワーク経由でアクセスする代わりに、指定されたローカル ファイル内のスキーマ情報を使用するように指定します。 また、キャッシュされたスキーマ情報を使用するには、静的パラメーター `ForceUpdate` を `false` に設定する必要があります。 同様の手法でオンラインとオフラインのデータ アクセスを有効にすることができます。

### <a name="backing-assembly"></a>バッキング アセンブリ

`.dll` または `.exe` ファイルをコンパイルすると、生成された型のバッキング .dll ファイルは、結果のアセンブリに静的にリンクされます。 このリンクは、中間言語 (IL) の型定義とマネージド リソースをバッキング アセンブリから最終アセンブリにコピーして作成されます。 F# Interactive を使用すると、バッキング .dll ファイルはコピーされず、代わりに F# Interactive プロセスに直接読み込まれます。

### <a name="exceptions-and-diagnostics-from-type-providers"></a>型プロバイダーからの例外と診断

指定された型のすべてのメンバーの使用において例外がスローされる可能性があります。 いずれの場合も、型プロバイダーが例外をスローした場合、ホスト コンパイラはそのエラーの原因を特定の型プロバイダーに求めます。

- 型プロバイダーの例外は、内部コンパイル エラーになることはありません。

- 型プロバイダーは警告を発生できません。

- F# コンパイラ、F# 開発環境、または F# Interactive でホストされている型プロバイダーからの例外はすべてキャッチされます。 Message プロパティは常にエラー テキストであり、スタック トレースは表示されません。 例外をスローする場合は、`System.NotSupportedException`、`System.IO.IOException`、`System.Exception`の例をスローすることができます。

#### <a name="providing-generated-types"></a>生成された型の指定

ここまでで、このドキュメントでは、消去された型を指定する方法について説明しました。 F# の型プロバイダー メカニズムを使用して、ユーザー プログラムに実際の .NET 型定義として追加される生成された型を指定することもできます。 生成され指定された型は型定義を使用して参照する必要があります。

```fsharp
open Microsoft.FSharp.TypeProviders

type Service = ODataService<"http://services.odata.org/Northwind/Northwind.svc/">
```

F# 3.0 リリースに含まれている ProvidedTypes-0.2 ヘルパー コードは、生成された型を指定するためのサポートが限られています。 生成された型の定義は次の条件を満たす必要があります。

- `isErased` を `false`に設定する必要があります。

- 生成された型は、生成されたコードフラグメントのコンテナーを表す、新しく構築された `ProvidedAssembly()`に追加する必要があります。

- プロバイダーは、実際のバッキング .NET .dll ファイルを含むアセンブリを持ち、対応する .dll ファイルがディスク上にある。

## <a name="rules-and-limitations"></a>規則と制約

型プロバイダーを作成するときは、次の規則と制約に注意してください。

### <a name="provided-types-must-be-reachable"></a>指定された型は到達可能でなければなりません

すべての指定された型は、入れ子になっていない型から到達可能である必要があります。 入れ子になっていない型は、`TypeProviderForNamespaces` コンストラクターへの呼び出し、または `AddNamespace` への呼び出しで指定されます。 たとえば、プロバイダーが `StaticClass.P : T` 型を指定する場合、T は入れ子になっていない型、または 1 階層だけ入れ子になっている型である必要があります。

たとえば、一部のプロバイダーには、`DataTypes` の各型を含む `T1, T2, T3, ...` などの静的クラスがあります。 それ以外の場合、アセンブリ A 内の型 T に対する参照は見つかるが、型はそのアセンブリ内に見つからないというエラーが表示されます。 このエラーが表示された場合は、すべての下位の型が指定された型から到達可能であることを確認します。 注: これらの `T1, T2, T3...` 型は *、その場で*実行される型と呼ばれます。 必ずこれらの型をアクセス可能な名前空間または親の型に入れてください。

### <a name="limitations-of-the-type-provider-mechanism"></a>型プロバイダー メカニズムの制約

F# の型プロバイダー メカニズムには、次の制約があります。

- F# の型プロバイダーの基になるインフラストラクチャは、指定されたジェネリック型または指定されたジェネリック メソッドをサポートしません。

- このメカニズムは、静的パラメーターを持つ入れ子になった型をサポートしません。

## <a name="development-tips"></a>開発のヒント

開発プロセスでは、次のヒントが役に立つ場合があります。

### <a name="run-two-instances-of-visual-studio"></a>Visual Studio の2つのインスタンスを実行する

型プロバイダーがリビルドされることを防止する .dll ファイルのロックをテスト IDE が取得するので、型プロバイダーを 1 つのインスタンスで作成し、そのプロバイダーを別のインスタンスでテストできます。 したがって、最初のインスタンスでプロバイダーをビルドしている間は Visual Studio の 2 番目のインスタンスを閉じておき、プロバイダーがビルドされた後に、2 番目のインスタンスを再び開く必要があります。

### <a name="debug-type-providers-by-using-invocations-of-fscexe"></a>Fsc.exe の呼び出しを使用して型プロバイダーをデバッグする

次のツールを使用して型プロバイダーを呼び出すことができます。

- fsc.exe (F# コマンド ライン コンパイラ)

- fsi.exe (F# Interactive コンパイラ)

- devenv.exe (Visual Studio)

多くの場合、型プロバイダーは、テスト スクリプト ファイル (script.fsx など) で fsc.exe を使用することによって、最も簡単にデバッグできます。 コマンド プロンプトからデバッガーを起動できます。

```console
devenv /debugexe fsc.exe script.fsx
```

  stdout への出力のログを使用できます。

## <a name="see-also"></a>関連項目

- [型プロバイダー](index.md)
- [型プロバイダー SDK](https://github.com/fsprojects/FSharp.TypeProviders.SDK)
