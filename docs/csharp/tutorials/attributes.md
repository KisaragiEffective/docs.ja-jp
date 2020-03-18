---
title: 'チュートリアル: 属性を使用する - C#'
description: C# での属性の機能について説明します。
author: mgroves
ms.technology: csharp-fundamentals
ms.date: 03/06/2017
ms.assetid: b152cf36-76e4-43a5-b805-1a1952e53b79
ms.openlocfilehash: 24cb7d35a89fda78511dc4ba725b69c5d601a008
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75937470"
---
# <a name="use-attributes-in-c"></a>C\# で属性を使用する

属性は、情報をコードに宣言的に関連付けるための手段を提供します。 また、さまざまなターゲットに適用できる再利用可能な要素も提供します。

たとえば `[Obsolete]` 属性について考えてみましょう。 この属性はクラス、構造体、メソッド、コンストラクトなどに適用できます。 これは、その要素が古いことを "_宣言_" します。 この属性を検索して、対応する何らかのアクションを実行するのは、C# コンパイラの役目です。

このチュートリアルでは、コードに属性を追加する方法、独自の属性を作成して使用する方法、.NET Core に組み込まれているいくつかの属性を使用する方法について説明します。

## <a name="prerequisites"></a>必須コンポーネント
お使いのコンピューターを、.NET Core が実行されるように設定する必要があります。 インストールの手順については、[.NET Core のダウンロード](https://dotnet.microsoft.com/download) ページを参照してください。
このアプリケーションは、Windows、Ubuntu Linux、macOS または Docker コンテナーで実行できます。
お好みのコード エディターをインストールしてください。 次の説明では、オープン ソースのクロス プラットフォーム エディターである [Visual Studio Code](https://code.visualstudio.com/) を使用しています。 しかし、他の使い慣れたツールを使用しても構いません。

## <a name="create-the-application"></a>アプリケーションを作成する

すべてのツールをインストールしたら、新しい .NET Core アプリケーションを作成します。 コマンド ライン ジェネレーターを使用するには、お使いのシェルで次のコマンドを実行します。

`dotnet new console`

このコマンドにより、必要最小限の .NET Core プロジェクト ファイルが作成されます。 `dotnet restore` を実行して、このプロジェクトのコンパイルに必要な依存関係を復元する必要があります。

[!INCLUDE[DotNet Restore Note](~/includes/dotnet-restore-note.md)]

プログラムを実行するには `dotnet run` を使用します。 コンソールに "Hello, World" という出力が表示されます。

## <a name="how-to-add-attributes-to-code"></a>コードに属性を追加する方法

C# では、属性は `Attribute` 基底クラスを継承するクラスです。 `Attribute` クラスから継承したクラスは、コードの他の部分で一種の "タグ" として使用できます。
たとえば `ObsoleteAttribute` という名前の属性があります。 これは、そのコードが古いので現在は使用できないことを警告するために使用されます。 この属性を、角かっこを使用して、たとえばクラスに適用することができます。

[!code-csharp[Obsolete attribute example](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ObsoleteExample1)]

この属性の名前は `ObsoleteAttribute` ですが、コードでは `[Obsolete]` と記述するだけで使用できます。 これは C# が準拠している規則によります。
完全な名前 `[ObsoleteAttribute]` も使用できます。

クラスを現在使用されていないとマークするときは、その "*理由*" と、代わりに "*何を*" 使用べきかについての情報を提供することをお勧めします。 これは、Obsolete 属性に文字列パラメーターを渡すことで行います。

[!code-csharp[Obsolete attribute example with parameters](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ObsoleteExample2)]

この文字列は、`var attr = new ObsoleteAttribute("some string")` と記述した場合と同様に、`ObsoleteAttribute` コンストラクターに引数として渡されます。

属性コンストラクターに渡すパラメーターは、単純な型/リテラル (`bool, int, double, string, Type, enums, etc`) とそれらの配列のみに限られます。
式または変数は使用できません。 位置指定パラメーターや名前付きパラメーターは自由に使用できます。

## <a name="how-to-create-your-own-attribute"></a>独自の属性を作成する方法

属性の作成は、`Attribute` 基底クラスからの継承と同じくらいに簡単です。

[!code-csharp[Create your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CreateAttributeExample1)]

これで、`[MySpecial]` (または `[MySpecialAttribute]`) をコード ベースの他の場所で属性として使用できます。

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CreateAttributeExample2)]

.NET の基本クラス ライブラリに含まれる `ObsoleteAttribute` のような属性は、コンパイラ内で特定の動作をトリガーします。 しかし、作成した属性はメタデータとしてのみ機能するため、属性クラス内のコードは実行されません。 そのメタデータを、コードの他の場所で操作する必要があります (詳細については、このチュートリアルの公判で説明します)。

ここに注意すべき "罠" があります。 前述のように、属性を使用するときは、特定の型のみを引数として渡すことができます。 しかし、属性の型を作成するときに、C# コンパイラによってパラメーターの作成が阻止されることはありません。 下の例では、正常にコンパイルされるコンストラクターを使用して属性を作成しています。

[!code-csharp[Valid constructor used in an attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeGothca1)]

しかし、このコンストラクターは属性構文では使用できません。

[!code-csharp[Invalid attempt to use the attribute constructor](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeGotcha2)]

上のコードでは、次のようなエラーが発生します。`Attribute constructor parameter 'myClass' has type 'Foo', which is not a valid attribute parameter type`

## <a name="how-to-restrict-attribute-usage"></a>属性の用途を制限する方法

属性はさまざまな "ターゲット" に対して使用できます。 上の例ではクラスに使用しましたが、次のターゲットに対しても使用できます。

* Assembly
* クラス
* コンストラクター
* Delegate
* Enum
* event
* フィールド
* GenericParameter
* Interface
* メソッド
* Module
* パラメーター
* プロパティ
* ReturnValue
* 構造体

C# の既定では、属性クラスを作成した場合、その属性は可能なすべての属性ターゲットに使用できます。 属性を特定のターゲットにのみ使用できるように制限するには、属性クラスに対して`AttributeUsageAttribute` を使用します。 つまり、属性に属性を設定します。

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeUsageExample1)]

クラスまたは構造体以外のターゲットに上の属性を設定しようとすると、次のようなコンパイラ エラーが発生します。`Attribute 'MyAttributeForClassAndStructOnly' is not valid on this declaration type. It is only valid on 'class, struct' declarations`

[!code-csharp[Using your own attribute](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#AttributeUsageExample2)]

## <a name="how-to-use-attributes-attached-to-a-code-element"></a>コード要素にアタッチされた属性を使用する方法

属性はメタデータとして機能します。 外からの力が働かないかぎり、実際には何の処理も実行しません。

属性を見つけて操作するには、通常、[Reflection](../programming-guide/concepts/reflection.md) が必要になります。 このチュートリアルでは Reflection について詳しく説明しませんが、基本的な考えとしては、Reflection を使用すると C# で他のコードを調べるコードを記述できます。

たとえば、Reflection を使用して次のクラスに関する情報を取得できます (コードの先頭に `using System.Reflection;` を追加する)。

[!code-csharp[Getting type information with Reflection](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ReflectionExample1)]

出力は次のようになります。`The assembly qualified name of MyClass is ConsoleApplication.MyClass, attributes, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`

`TypeInfo` オブジェクト (または `MemberInfo`、`FieldInfo` など) を取得したら、`GetCustomAttributes` メソッドを使用できます。 このメソッドは `Attribute` オブジェクトのコレクションを返します。
また、`GetCustomAttribute` を使用して Attribute 型を指定することもできます。

`MyClass` クラス (前の例で `[Obsolete]` 属性を適用したクラス)の `MemberInfo` インスタンスに対して `GetCustomAttributes` を使用する例を以下に示します。

[!code-csharp[Getting type information with Reflection](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#ReflectionExample2)]

コンソールには次のように出力されます。`Attribute on MyClass: ObsoleteAttribute` `MyClass` に他の属性を追加してみてください。

`Attribute` オブジェクトは限定的にインスタンス化されることに注意してください。 つまり、`GetCustomAttribute` または `GetCustomAttributes` を使用するまでインスタンス化されません。
また、インスタンス化は使用のたびに行われます。 行内で `GetCustomAttributes` を 2 回呼び出すと、`ObsoleteAttribute` の異なる 2 つのインスタンスが返されます。

## <a name="common-attributes-in-the-base-class-library-bcl"></a>基本クラス ライブラリ (BCL) のよく使用される属性

属性は、さまざまなツールやフレームワークで使用されます。 NUnit は、`[Test]` や `[TestFixture]` などの属性を NUnit テスト ランナーで使用します。 ASP.NET MVC は、`[Authorize]` などの属性を使用して、MVC アクションに対する横断的な処理を実行するためのアクション フィルター フレームワークを提供します。 [PostSharp](https://www.postsharp.net) は、属性構文を使用して C# でアスペクト指向プログラミングを行えるようにします。

.NET Core の基本クラス ライブラリに組み込まれている、よく使用される属性のいくつかを以下に示します。

* `[Obsolete]`。 これは上の例で使用した属性で、`System` 名前空間に格納されています。 この属性は、コード ベースの変更に関する宣言的なドキュメントを提供するのに便利です。 メッセージは文字列の形式で指定でき、別のブール型パラメーターを使用すると、コンパイラの警告をコンパイラのエラーにエスカレートすることができます。

* `[Conditional]`。 この属性は `System.Diagnostics` 名前空間に格納されています。 この属性はメソッド (または属性クラス) に適用できます。 コンス トラクターに文字列を渡す必要があります。
その文字列が `#define` ディレクティブと一致しない場合、そのメソッドの呼び出し (メソッド自体ではありません) が C# コンパイラによって除外されます。 通常、この属性はデバッグ (診断) 目的で使用されます。

* `[CallerMemberName]`。 この属性はパラメーターに使用でき、`System.Runtime.CompilerServices` 名前空間に格納されています。 この属性は、別のメソッドを呼び出しているメソッドの名前を挿入するために使用します。 これは通常、さまざまな UI フレームワークで INotifyPropertyChanged を実装する際に "マジック文字列" を排除するための方法として使用されます。 以下に例を示します。

[!code-csharp[Using CallerMemberName when implementing INotifyPropertyChanged](../../../samples/snippets/csharp/tutorials/attributes/Program.cs#CallerMemberName1)]

上のコードでは、リテラルの `"Name"` 文字列を使用する必要はありません。 これは入力ミス関連のバグを防ぎ、リファクタリングや名前変更をスムーズにするのに役立ちます。

## <a name="summary"></a>まとめ

属性によって C# に宣言機能が追加されますが、それらはメタデータ形式のコードであり、単独では機能しません。
