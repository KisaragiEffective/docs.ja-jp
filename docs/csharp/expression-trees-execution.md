---
title: 式ツリーの実行
description: 式ツリーの実行について説明します。式ツリーを実行可能な中間言語 (IL) 命令に変換します。
ms.date: 06/20/2016
ms.technology: csharp-advanced-concepts
ms.assetid: 109e0ac5-2a9c-48b4-ac68-9b6219cdbccf
ms.openlocfilehash: 802a83f52f9c05a99c3f49f8f6511eff81ef3eaa
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79146022"
---
# <a name="executing-expression-trees"></a>式ツリーの実行

[前へ -- 式ツリーをサポートするフレームワークの型](expression-classes.md)

*式ツリー*はコードを表すデータ構造です。
式ツリーはコンパイル済みの実行可能なコードではありません。 式ツリーで表される .NET コードを実行する場合は、実行可能な IL 命令に変換する必要があります。

## <a name="lambda-expressions-to-functions"></a>ラムダ式から関数への変換

すべての LambdaExpression、または LambdaExpression の派生型は、実行可能な IL に変換できます。 その他の式の型は直接コードに変換できません。 実際には、この制限はほとんど影響がありません。 ラムダ式は、実行可能な中間言語 (IL) に変換して実行する唯一の式の型です。 (直接 `ConstantExpression` を実行することにどのような意味があるでしょうか。 何かに役立つでしょうか。)`LambdaExpression` である式ツリー、または `LambdaExpression` の派生型はすべて IL に変換できます。
式の型 `Expression<TDelegate>` は .NET Core ライブラリで唯一の具体的な例です。 この型は任意のデリゲート型にマップされる式を表すのに使用されます。 この型はデリゲート型にマップされるため、.NET で式を検証し、ラムダ式のシグネチャと一致する適切なデリゲートの IL を生成することができます。

通常、これによって式とそれに対応するデリゲートの間で単純なマッピングが作成されます。 たとえば、`Expression<Func<int>>` によって表される式ツリーは、`Func<int>` 型のデリゲートに変換されます。 任意の戻り値の型と引数リストをもつラムダ式の場合、そのラムダ式によって表される実行可能コードのターゲット型となるデリゲート型が存在します。

`LambdaExpression` 型には、式ツリーを実行可能コードに変換するために使用する `Compile` と `CompileToMethod` メンバーが含まれます。 `Compile` メソッドはデリゲートを作成します。 `CompileToMethod` メソッドは、式ツリーのコンパイル済み出力を表す IL によって `MethodBuilder` オブジェクトを更新します。 なお、`CompileToMethod` は完全なデスクトップ フレームワークでのみ利用可能で、.NET Core では利用できません。

必要に応じて、生成されたデリゲート オブジェクトのシンボル デバッグ情報を受け取る `DebugInfoGenerator` を用意することもできます。 これにより、式ツリーをデリゲート オブジェクトに変換し、生成されたデリゲートの完全なデバッグ情報を入手できます。

次のコードを使用して、式をデリゲートに変換します。

```csharp
Expression<Func<int>> add = () => 1 + 2;
var func = add.Compile(); // Create Delegate
var answer = func(); // Invoke Delegate
Console.WriteLine(answer);
```

デリゲート型は式の型が基になっています。 厳密に型指定された方法でデリゲート オブジェクトを使用する場合は、戻り値の型および引数リストを把握する必要があります。 `LambdaExpression.Compile()` メソッドは `Delegate` 型を返します。 コンパイル時ツールを使用して、引数リストまたは戻り値の型を確認するには、適切なデリゲート型にキャストする必要があります。

## <a name="execution-and-lifetimes"></a>実行と有効期間

`LambdaExpression.Compile()` を呼び出したときに作成されたデリゲートを呼び出して、コードを実行します。 上の例の `add.Compile()` がデリゲートを返す部分に、これが表示されています。 `func()` を呼び出して、デリゲートを呼び出すことにより、コードを実行します。

そのデリゲートは式ツリーのコードを表します。 そのデリゲートのハンドルを保持して、後で呼び出すことができます。 デリゲートが表すコードを実行するたびに、式ツリーをコンパイルする必要はありません。 (式ツリーは不変であるため、あとで同じ式ツリーをコンパイルしても、同じコードを実行するデリゲートが作成されます。)

より高度なキャッシュ機構を作成して、不要なコンパイルの呼び出しを回避することで、パフォーマンスを向上させようとしないように気を付けてください。 また、2 つの任意の式ツリーを比較して、同じアルゴリズムを表しているかどうかを確認することも、実行に時間がかかります。 `LambdaExpression.Compile()` への余分な呼び出しを回避して、コンピューティング時間を削減しても、2 つの異なる式ツリーから同じ実行可能コードが得られることを確認するコードを実行するのにもっと多くの時間がかかってしまいます。

## <a name="caveats"></a>注意事項

ラムダ式をデリゲートにコンパイルして、そのデリゲートを呼び出すのは、式ツリーで実行する最も単純な操作の 1 つです。 ただし、この簡単な操作にも気をつけるべき点があります。

ラムダ式は、式で参照される任意のローカル変数に対するクロージャを作成します。 デリゲートの一部となる任意の変数は、`Compile` を呼び出す場所で使用でき、得られたデリゲートを実行するときに使用できるように保証する必要があります。

通常は、このことが true であることをコンパイラが確認します。 しかし、式が `IDisposable` を実装する変数にアクセスする場合、式ツリーでオブジェクトが保持されている一方で、コードがオブジェクトを破棄する可能性があります。

たとえば、次のコードは `int` が `IDisposable` を実装しないため、正常に機能します。

```csharp
private static Func<int, int> CreateBoundFunc()
{
    var constant = 5; // constant is captured by the expression tree
    Expression<Func<int, int>> expression = (b) => constant + b;
    var rVal = expression.Compile();
    return rVal;
}
```

デリゲートはローカル変数 `constant` への参照を取得します。
`CreateBoundFunc` によって返される関数をあとで実行するとき、その変数はいつでもアクセスできます。

しかし、次の `IDisposable` を実装するクラス (かなり不自然です) はどうでしょうか。

```csharp
public class Resource : IDisposable
{
    private bool isDisposed = false;
    public int Argument
    {
        get
        {
            if (!isDisposed)
                return 5;
            else throw new ObjectDisposedException("Resource");
        }
    }

    public void Dispose()
    {
        isDisposed = true;
    }
}
```

下記に示す式で使用する場合、`Resource.Argument`プロパティによって参照されるコードを実行すると、`ObjectDisposedException` が得られます。

```csharp
private static Func<int, int> CreateBoundResource()
{
    using (var constant = new Resource()) // constant is captured by the expression tree
    {
        Expression<Func<int, int>> expression = (b) => constant.Argument + b;
        var rVal = expression.Compile();
        return rVal;
    }
}
```

このメソッドから返されたデリゲートは、`constant` オブジェクトを捕捉しますが、このオブジェクトはすでに破棄されています。 (オブジェクトは `using` ステートメントで宣言されたため、破棄されています。)

このため、このメソッドから返されたデリゲートを実行すると、実行時に `ObjectDisposedException` がスローされます。

コンパイル時の構成要素を表す実行時エラーが発生するのは変だと思うかもしれませんが、式ツリーを扱う場合はそういうものです。

この問題はさまざまなバリエーションがあるため、これを回避する一般的なガイダンスを提供することは困難です。 式を定義するときにローカル変数へのアクセスに注意するとともに、パブリック API によって返される可能性がある式ツリーを作成する場合、(`this` によって表される) 現在のオブジェクトでのアクセス状態に注意してください。

式のコードが他のアセンブリのメソッドまたはプロパティを参照する場合があります。 そのアセンブリには、式の定義時、式のコンパイル時、得られたデリゲートの呼び出し時にアクセスが必要になります。 アセンブリが存在しない場合、`ReferencedAssemblyNotFoundException` が発生します。

## <a name="summary"></a>まとめ

ラムダ式を表す式ツリーをコンパイルすると、実行可能なデリゲートを作成できます。 この方法は、式ツリーが表すコードを実行するメカニズムの 1 つです。

式ツリーは、作成した任意の構成要素を実行するコードを表わしています。 コードをコンパイルして実行する環境が、式を作成する環境と一致している限り、すべてが期待どおりに動作します。 環境が一致していない場合、エラーが確実に予想されます。このエラーは、式ツリーを使用するコードを最初にテストする段階で発見されるはずです。

[次へ -- 式の解釈](expression-trees-interpreting.md)
