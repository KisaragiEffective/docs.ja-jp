---
title: MEF (Managed Extensibility Framework)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Managed Extensibility Framework, overview
- MEF, overview
ms.assetid: 6c61b4ec-c6df-4651-80f1-4854f8b14dde
ms.openlocfilehash: 9a601ac860ac3bf81dd01980b020470d3323772f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79181286"
---
# <a name="managed-extensibility-framework-mef"></a>MEF (Managed Extensibility Framework)

ここでは、.NET Framework 4 で導入された Managed Extensibility Framework の概要を説明します。

## <a name="what-is-mef"></a>MEF とは

Managed Extensibility Framework (MEF) は、軽量で拡張可能なアプリケーションを作成するためのライブラリです。 これにより、アプリケーション開発者は、拡張機能を見つけたら、それをそのまま使用できます。構成は必要ありません。 拡張機能の開発者は、コードを簡単にカプセル化できるため、ハードコーディングによる脆弱な依存関係を回避できます。 MEF により、アプリケーション内だけでなく、アプリケーション間でも拡張機能を再利用できます。

## <a name="the-problem-of-extensibility"></a>機能拡張の問題

機能拡張のサポートを提供する必要のある大規模アプリケーションの設計担当者である場合を想像してください。 アプリケーションには数多くの小規模コンポーネントを含める必要がある可能性があり、アプリケーションはこれらを作成して実行する必要があります。

このような問題を解決する最も簡単な方法は、アプリケーションにコンポーネントをソース コードとして組み込み、これらのコードをコードから直接呼び出すことです。 この方法には、数多くの明らかな欠点があります。 最も重要な点は、ソース コードを変更することなく新しいコンポーネントを追加できないという点です。これは、Web アプリケーションなどでは許容されますが、クライアント アプリケーションでは許容されません。 同様に重要な問題点として、開発元がサードパーティであるためにコンポーネントのソース コードにアクセスできないことがあります。また、同じ理由から、サードパーティ側からの (自分が開発したコンポーネントのソース コードへの) アクセスを許可することもできません。

より高度な方法として、拡張ポイントまたはインターフェイスを提供して、アプリケーションとそのコンポーネントを切り離すことを許可する方法があります。 このモデルでは、必要に応じて、コンポーネントによって実装可能なインターフェイスと、そのインターフェイスがアプリケーションと対話するための API を提供できます。 これにより、ソース コードへのアクセスが必要になるという問題は解決されますが、この方法そのものにまだ問題があります。

アプリケーションには独自にコンポーネントを検出する機能がないため、アプリケーションに対して使用可能なコンポーネントと読み込む必要のあるコンポーネントを明示的に指定する必要があります。 これは、通常、構成ファイルに使用可能なコンポーネントを明示的に登録することで指定します。 これは、登録されたコンポーネントが正しいことが想定されることにより、特に開発者ではなくエンド ユーザーが更新を行う場合は、保守の問題が生じることを意味します。

さらに、アプリケーション自体の厳密に定義されたチャネルを介する場合を除き、コンポーネントは相互に通信することができません。 アプリケーションの設計担当者が特定の通信の必要性が生じる状況を想定しなかった場合、通常、通信は不可能です。

最後に、コンポーネントの開発者は、どのアセンブリが実装するインターフェイスを含んでいるかに対するハードコーディングによる依存関係を許容する必要があります。 これにより、1 つのコンポーネントを複数のアプリケーションで使用することが困難となり、コンポーネントのテスト フレームワークを作成するときに問題が生じる可能性もあります。

## <a name="what-mef-provides"></a>MEF が提供する機能

使用可能なコンポーネントを明示的に登録する代わりに、MEF では、そのようなコンポーネントを "*合成*" によって暗黙的に検出できます。 MEF コンポーネント ("*パート*") は、その依存関係 ("*インポート*") とそれが使用可能にする機能 ("*エクスポート*) の両方を宣言的に指定します。 パートが作成されると、MEF 合成エンジンは、他のパートから使用可能なパートでそのインポートを満たします。

この方法により、前のセクションで説明した問題が解決されます。 MEF パートは、その機能を宣言的に指定するため、実行時に検出できます。これは、アプリケーションがハードコーディングされた参照または脆弱な構成ファイルを使用せずにパートを使用できることを意味します。 MEF を使用すると、アプリケーションは、メタデータを使用してパートを検出し、検証することができます。これらのパートをインスタンス化したり、そのアセンブリを読み込んだりする必要はありません。 結果として、拡張機能がいつどのように読み込まれる必要があるかを慎重に指定する必要がありません。

パートは、それが提供するエクスポートに加えて、他のパートで満たされるインポートを指定できます。 これにより、パート間の通信が可能になるだけでなく、簡単になり、コードのファクタリングが可能になります。 たとえば、多くのコンポーネントに共通のサービスを個々のパートにファクタリングして、簡単に変更したり置換したりすることができます。

MEF モデルでは、特定のアプリケーション アセンブリでハードコーディングによる依存関係を指定する必要がないため、アプリケーション間で拡張機能を再利用できます。 これによってテスト ハーネスの開発も容易になり、アプリケーションに依存せずに拡張コンポーネントをテストできます。

MEF を使用して記述された拡張アプリケーションは、拡張コンポーネントで満たすことのできるインポートを宣言できます。また、拡張機能に対してアプリケーション サービスを公開するためにエクスポートを宣言できる場合もあります。 各拡張コンポーネントは、エクスポートを宣言するだけではなく、インポートを宣言することもあります。 こうすると、拡張コンポーネント自体が自動的に拡張可能になります。

## <a name="where-mef-is-available"></a>MEF を使用できる場所

MEF は、.NET Framework 4 の不可欠な構成要素であり、.NET Framework が使用されている環境であればどのような環境でも使用できます。 MEF は、Windows フォームや WPF などのテクノロジを使用するクライアント アプリケーション、または ASP.NET を使用するサーバー アプリケーションで使用できます。

## <a name="mef-and-maf"></a>MEF と MAF

アプリケーションが拡張機能を特定および管理できるように設計された MAF (Managed Add-in Framework) は、以前のバージョンの .NET Framework で導入されました。 MAF では MEF と比較してやや高度なレベルに重点が置かれており、MEF では発見可能性、拡張性、および移植性に重点が置かれているのに対し、MAF では拡張機能の特定とアセンブリの読み込みおよびアンロードに重点が置かれています。 この 2 つのフレームワークは円滑に相互運用でき、1 つのアプリケーションで両方を活用することができます。

## <a name="simplecalculator-an-example-application"></a>SimpleCalculator:アプリケーションの例

MEF が実行できる処理を理解する最も簡単な方法は、単純な MEF アプリケーションを作成することです。 この例では、SimpleCalculator という名前の単純な電卓を作成します。 SimpleCalculator の目的は、"5+3" や "6-2" などの形式で基本的な算術命令を受け取り、正しい答えを返すコンソール アプリケーションを作成することです。 MEF を使用すると、アプリケーション コードを変更せずに、新しい演算子を追加できます。

この例の完成したコードをダウンロードするには、[SimpleCalculator のサンプル (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/simple-calculator-vb/) をご覧ください。

> [!NOTE]
> SimpleCalculator を作成する目的は、これを使用する実際のシナリオを必ずしも提供することではなく、MEF の概念と構文を示すことにあります。 MEF の機能を最大限に活用できるアプリケーションの多くは、SimpleCalculator よりも複雑です。 その他のサンプルについては、GitHub の「[Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef)」をご覧ください。

- 開始するには、Visual Studio で新しいコンソール アプリケーション プロジェクトを作成し、`SimpleCalculator` という名前を付けます。

- MEF がある `System.ComponentModel.Composition` アセンブリに参照を追加します。

- *Module1.vb* または *Program.cs* を開き、`System.ComponentModel.Composition` および `System.ComponentModel.Composition.Hosting` に `Imports` または `using` のステートメントを追加します。 これらの 2 つの名前空間には、拡張可能なアプリケーションの開発に必要な MEF 型が含まれています。

- Visual Basic を使用している場合は、`Public` モジュールを宣言する行に `Module1` キーワードを追加します。

## <a name="composition-container-and-catalogs"></a>合成コンテナーとカタログ

MEF 合成モデルのコアは、すべての使用可能なパートが含まれ、合成を実行する "*合成コンテナー*" です 合成はインポートとエクスポートの組み合わせです。 最も一般的な種類の合成コンテナーは <xref:System.ComponentModel.Composition.Hosting.CompositionContainer> であり、これは SimpleCalculator で使用します。

Visual Basic を使用している場合は、*Module1.vb* に `Program` という名前のパブリック クラスを追加します。

*Module1.vb* または *Program.cs* の `Program` クラスに次の行を追加します。

```vb
Dim _container As CompositionContainer
```

```csharp
private CompositionContainer _container;
```

合成コンテナーは、*カタログ*を使用して使用可能なパートを検出します。 カタログは、いくつかのソースから使用できるパートを検出するためのオブジェクトです。 MEF では、指定された型、アセンブリ、またはディレクトリからパートを検出するカタログを提供します。 アプリケーション開発者は、Web サービスなどの他のソースからパートを検出する新しいカタログを簡単に作成できます。

`Program` クラスに次のコンストラクターを追加します。

```vb
Public Sub New()
    ' An aggregate catalog that combines multiple catalogs.
     Dim catalog = New AggregateCatalog()

    ' Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(New AssemblyCatalog(GetType(Program).Assembly))

    ' Create the CompositionContainer with the parts in the catalog.
    _container = New CompositionContainer(catalog)

    ' Fill the imports of this object.
    Try
        _container.ComposeParts(Me)
    Catch ex As CompositionException
        Console.WriteLine(ex.ToString)
    End Try
End Sub
```

```csharp
private Program()
{
    // An aggregate catalog that combines multiple catalogs.
    var catalog = new AggregateCatalog();
    // Adds all the parts found in the same assembly as the Program class.
    catalog.Catalogs.Add(new AssemblyCatalog(typeof(Program).Assembly));

    // Create the CompositionContainer with the parts in the catalog.
    _container = new CompositionContainer(catalog);

    // Fill the imports of this object.
    try
    {
        this._container.ComposeParts(this);
    }
    catch (CompositionException compositionException)
    {
        Console.WriteLine(compositionException.ToString());
   }
}
```

<xref:System.ComponentModel.Composition.AttributedModelServices.ComposeParts%2A> を呼び出すことで、合成コンテナーに対して特定のパート セット (この例では `Program` の現在のインスタンス) を合成するように指示します。 ただし、この時点では、`Program` に満たす必要のあるインポートがないため、何も実行されません。

## <a name="imports-and-exports-with-attributes"></a>属性を使用したインポートとエクスポート

まず、`Program` で電卓をインポートします。 これにより、`Program` に渡されるコンソール入力やコンソール出力などのユーザー インターフェイスに関連する要素が電卓のロジックから切り離されます。

`Program` クラスに次のコードを追加します。

```vb
<Import(GetType(ICalculator))>
Public Property calculator As ICalculator
```

```csharp
[Import(typeof(ICalculator))]
public ICalculator calculator;
```

`calculator` オブジェクトの宣言に特に変わった点はありませんが、<xref:System.ComponentModel.Composition.ImportAttribute> 属性で装飾されていることに注意してください。 この属性は、インポートとなるもの、つまり、オブジェクトが合成されると合成エンジンによって満たされるものを宣言します。

各インポートには "*コントラクト*" があり、これにより、対応するエクスポートが決定されます。 コントラクトは、明示的に指定された文字列であったり、指定された型に基づいて MEF によって自動的に生成されたりします (この例では、`ICalculator` というインターフェイス)。 対応するコントラクトで宣言されたエクスポートが、このインポートを満たします。 `calculator` オブジェクトの型は実際には `ICalculator` ですが、これは必須ではありません。 コントラクトは、インポートするオブジェクトの型に依存しません。 この例では、`typeof(ICalculator)` は省略できます。 MEF では、明示的に指定しない限り、インポートの型に基づいて自動的にコントラクトが想定されます。

次のように、この単純なインターフェイスをモジュールまたは `SimpleCalculator` 名前空間に追加します。

```vb
Public Interface ICalculator
    Function Calculate(input As String) As String
End Interface
```

```csharp
public interface ICalculator
{
    String Calculate(String input);
}
```

`ICalculator` を定義したので、これを実装するクラスが必要です。 モジュールまたは `SimpleCalculator` 名前空間に次のクラスを追加します。

```vb
<Export(GetType(ICalculator))>
Public Class MySimpleCalculator
   Implements ICalculator

End Class
```

```csharp
[Export(typeof(ICalculator))]
class MySimpleCalculator : ICalculator
{

}
```

これが、`Program` でインポートに対応するエクスポートになります。 エクスポートがインポートと一致するには、エクスポートで同じコントラクトを使用する必要があります。 `typeof(MySimpleCalculator)` に基づいてコントラクトでエクスポートすると、不一致が発生し、インポートは満たされません。コントラクトは正確に対応している必要があります。

合成コンテナーはこのアセンブリで使用可能なすべてのパートを使用して設定されるため、`MySimpleCalculator` パートが使用可能になります。 `Program` のコンストラクターが `Program` オブジェクトで合成を実行するとき、そのインポートは、それを満たすために作成される `MySimpleCalculator` オブジェクトで満たされます。

ユーザー インターフェイス レイヤー (`Program`) がそれ以外のことを認識する必要はありません。 したがって、`Main` メソッド内の残りのユーザー インターフェイス ロジックを記述できます。

`Main` メソッドに次のコードを追加します。

```vb
Sub Main()
    ' Composition is performed in the constructor.
    Dim p As New Program()
    Dim s As String
    Console.WriteLine("Enter Command:")
    While (True)
        s = Console.ReadLine()
        Console.WriteLine(p.calculator.Calculate(s))
    End While
End Sub
```

```csharp
static void Main(string[] args)
{
    // Composition is performed in the constructor.
    var p = new Program();
    string s;
    Console.WriteLine("Enter Command:");
    while (true)
    {
        s = Console.ReadLine();
        Console.WriteLine(p.calculator.Calculate(s));
    }
}
```

 このコードは、単純に入力行を読み取り、結果に対して `Calculate` の `ICalculator` 関数を呼び出して、結果をコンソールに出力します。 これが、`Program` で必要なすべてのコードです。 その他の処理は、パート内で行われます。

## <a name="further-imports-and-importmany"></a>その他のインポートと ImportMany

SimpleCalculator に拡張性を持たせるためには、演算の一覧をインポートする必要があります。 通常の <xref:System.ComponentModel.Composition.ImportAttribute> 属性は、1 つの <xref:System.ComponentModel.Composition.ExportAttribute> だけで満たされます。 使用可能なエクスポートが複数あれば、合成エンジンでエラーが発生します。 任意の数のエクスポートで満たすことのできるインポートを作成するには、<xref:System.ComponentModel.Composition.ImportManyAttribute> 属性を使用します。

`MySimpleCalculator` クラスに次の演算プロパティを追加します。

```vb
<ImportMany()>
Public Property operations As IEnumerable(Of Lazy(Of IOperation, IOperationData))
```

```csharp
[ImportMany]
IEnumerable<Lazy<IOperation, IOperationData>> operations;
```

<xref:System.Lazy%602> は、エクスポートの間接参照を格納するために MEF に用意されている型です。 これにより、エクスポートされるオブジェクト自体に加えて、"*エクスポート メタデータ*" (エクスポートされるオブジェクトについて説明する情報) も取得できます。 各 <xref:System.Lazy%602> には、実際の操作を表す `IOperation` オブジェクト、およびそのメタデータを表す `IOperationData` オブジェクトが含まれます。

次の単純なインターフェイスをモジュールまたは `SimpleCalculator` 名前空間に追加します。

```vb
Public Interface IOperation
    Function Operate(left As Integer, right As Integer) As Integer
End Interface

Public Interface IOperationData
    ReadOnly Property Symbol As Char
End Interface
```

```csharp
public interface IOperation
{
     int Operate(int left, int right);
}

public interface IOperationData
{
    Char Symbol { get; }
}
```

 ここでは、各演算のメタデータは、演算を表す +、-、\* などの記号です。 加算の演算を使用可能にするには、次のクラスをモジュールまたは `SimpleCalculator` 名前空間に追加します。

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "+"c)>
Public Class Add
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left + right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '+')]
class Add: IOperation
{
    public int Operate(int left, int right)
    {
        return left + right;
    }
}
```

<xref:System.ComponentModel.Composition.ExportAttribute> 属性は、以前と同じように機能します。 <xref:System.ComponentModel.Composition.ExportMetadataAttribute> 属性は、そのエクスポートに対し、名前と値のペアの形式でメタデータをアタッチします。 `Add` クラスは `IOperation` を実装していますが、`IOperationData` を実装するクラスは明示的に定義されていません。 代わりに、提供されるメタデータの名前に基づくプロパティを使用して、MEF によってクラスが暗黙的に作成されます。 (MEF でメタデータにアクセスする方法にはいくつかありますが、これはその 1 つです)。

MEF での合成は "*再帰的*" です。 先ほど、`Program` オブジェクトを明示的に合成しました。これは `ICalculator` をインポートし、その型は `MySimpleCalculator` であることが判明しました。 この `MySimpleCalculator` は `IOperation` オブジェクトのコレクションをインポートし、このインポートは `MySimpleCalculator` が作成されるときに `Program` のインポートと同時に満たされます。 `Add` クラスがさらに別のインポートを宣言している場合は、そのインポートも満たされる必要があり、宣言されているインポートごとにそれが繰り返されます。 満たされないインポートが残ると、合成エラーが発生します (省略可能なインポートを宣言する、またはそれらに既定値を割り当てることはできます)。

## <a name="calculator-logic"></a>電卓のロジック

3 つのパートを作成したので、残っている作業は電卓ロジックを作成することです。 `MySimpleCalculator` クラスに次のコードを追加して、`Calculate` メソッドを実装します。

```vb
Public Function Calculate(input As String) As String Implements ICalculator.Calculate
    Dim left, right As Integer
    Dim operation As Char
    ' Finds the operator.
    Dim fn = FindFirstNonDigit(input)
    If fn < 0 Then
        Return "Could not parse command."
    End If
    operation = input(fn)
    Try
        ' Separate out the operands.
        left = Integer.Parse(input.Substring(0, fn))
        right = Integer.Parse(input.Substring(fn + 1))
    Catch ex As Exception
        Return "Could not parse command."
    End Try
    For Each i As Lazy(Of IOperation, IOperationData) In operations
        If i.Metadata.symbol = operation Then
            Return i.Value.Operate(left, right).ToString()
        End If
    Next
    Return "Operation not found!"
End Function
```

```csharp
public String Calculate(string input)
{
    int left;
    int right;
    char operation;
    // Finds the operator.
    int fn = FindFirstNonDigit(input);
    if (fn < 0) return "Could not parse command.";

    try
    {
        // Separate out the operands.
        left = int.Parse(input.Substring(0, fn));
        right = int.Parse(input.Substring(fn + 1));
    }
    catch
    {
        return "Could not parse command.";
    }

    operation = input[fn];

    foreach (Lazy<IOperation, IOperationData> i in operations)
    {
        if (i.Metadata.Symbol.Equals(operation)) return i.Value.Operate(left, right).ToString();
    }
    return "Operation Not Found!";
}
```

1 つ目のステップでは、入力文字列が解析され、左オペランド、右オペランド、および演算子文字が特定されます。 `foreach` ループでは、`operations` コレクションのすべてのメンバーが検証されます。 これらのオブジェクトの型は <xref:System.Lazy%602> です。またそのメタデータ値とエクスポートされるオブジェクトにはそれぞれ <xref:System.Lazy%602.Metadata%2A> プロパティと <xref:System.Lazy%601.Value%2A> プロパティでアクセスできます。 この場合、`Symbol` オブジェクトの `IOperationData` プロパティが一致することが検出されると、電卓は `Operate` オブジェクトの `IOperation` メソッドを呼び出し、結果を返します。

電卓を完成させるには、文字列内で最初に出現する数字以外の文字の位置を返すヘルパー メソッドも必要です。 `MySimpleCalculator` クラスに次のヘルパー メソッドを追加します。

```vb
Private Function FindFirstNonDigit(s As String) As Integer
    For i = 0 To s.Length - 1
        If Not Char.IsDigit(s(i)) Then Return i
    Next
    Return -1
End Function
```

```csharp
private int FindFirstNonDigit(string s)
{
    for (int i = 0; i < s.Length; i++)
    {
        if (!Char.IsDigit(s[i])) return i;
    }
    return -1;
}
```

これで、プロジェクトをコンパイルして実行できるようになりました。 Visual Basic の場合、`Public` に `Module1` キーワードを追加していることを確認してください。 コンソール ウィンドウで "5+3" などの加算演算を入力すると、電卓が結果を返します。 それ以外の演算子を入力すると、"Operation Not Found!" という メッセージが表示されます。

## <a name="extending-simplecalculator-using-a-new-class"></a>新しいクラスを使用した SimpleCalculator の拡張

これで、電卓が機能するようになりました。新しい演算の追加は簡単です。 モジュールまたは `SimpleCalculator` 名前空間に次のクラスを追加します。

```vb
<Export(GetType(IOperation))>
<ExportMetadata("Symbol", "-"c)>
Public Class Subtract
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left - right
    End Function
End Class
```

```csharp
[Export(typeof(IOperation))]
[ExportMetadata("Symbol", '-')]
class Subtract : IOperation
{
    public int Operate(int left, int right)
    {
        return left - right;
    }
}
```

プロジェクトをコンパイルして実行します。 "5-3" などの減算演算を入力します。 電卓は、加算だけでなく減算もサポートするようになりました。

## <a name="extending-simplecalculator-using-a-new-assembly"></a>新しいアセンブリを使用した SimpleCalculator の拡張

ソース コードにクラスを追加するのは簡単ですが、MEF には、アプリケーション独自のソースの外部でパートを検索する機能が用意されています。 この例を示すためには、<xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog> を追加することで、独自のアセンブリだけではなくディレクトリでもパートを検索するように SimpleCalculator を変更する必要があります。

SimpleCalculator プロジェクトに `Extensions` という名前の新しいディレクトリを追加します。 これは、アプリケーション レベルではなく、プロジェクト レベルで追加してください。 次に、`ExtendedOperations` という名前の新しいクラス ライブラリ プロジェクトをソリューションに追加します。 新しいプロジェクトをコンパイルし、個別のアセンブリを作成します。

ExtendedOperations プロジェクトのプロジェクト プロパティ デザイナーを開き、 **[コンパイル]** タブまたは **[ビルド]** タブをクリックします。SimpleCalculator プロジェクト ディレクトリの Extensions ディレクトリ ( *..\SimpleCalculator\Extensions\\* ) を指すように、 **[ビルド出力パス]** または **[出力パス]** を変更します。

 *Module1.vb* または *Program.cs* で、`Program` コンストラクターに次の行を追加します。

```vb
catalog.Catalogs.Add(New DirectoryCatalog("C:\SimpleCalculator\SimpleCalculator\Extensions"))
```

```csharp
catalog.Catalogs.Add(new DirectoryCatalog("C:\\SimpleCalculator\\SimpleCalculator\\Extensions"));
```

例に示されているパスを、Extensions ディレクトリのパスに置き換えます。 (この絶対パスはデバッグにのみ使用します。 運用アプリケーションでは、相対パスを使用します。)これで、<xref:System.ComponentModel.Composition.Hosting.DirectoryCatalog> により、Extensions ディレクトリ内のアセンブリで見つかったすべてのパーツが合成コンテナーに追加されます。

ExtendedOperations プロジェクトで、SimpleCalculator と System.ComponentModel.Composition への参照を追加します。 ExtendedOperations クラス ファイルで、System.ComponentModel.Composition の `Imports` ステートメントまたは `using` ステートメントを追加します。 Visual Basic では、SimpleCalculator の `Imports` ステートメントも追加します。 次に、ExtendedOperations クラス ファイルに次のクラスを追加します。

```vb
<Export(GetType(SimpleCalculator.IOperation))>
<ExportMetadata("Symbol", "%"c)>
Public Class Modulo
    Implements IOperation

    Public Function Operate(left As Integer, right As Integer) As Integer Implements IOperation.Operate
        Return left Mod right
    End Function
End Class
```

```csharp
[Export(typeof(SimpleCalculator.IOperation))]
[ExportMetadata("Symbol", '%')]
public class Mod : SimpleCalculator.IOperation
{
    public int Operate(int left, int right)
    {
        return left % right;
    }
}
```

コントラクトを対応させるには、<xref:System.ComponentModel.Composition.ExportAttribute> 属性が <xref:System.ComponentModel.Composition.ImportAttribute> と同じ型である必要があります。

プロジェクトをコンパイルして実行します。 新しい Mod (%) 演算子をテストします。

## <a name="conclusion"></a>まとめ

ここでは、MEF の基本的な概念について説明しました。

- パート、カタログ、および合成コンテナー

     パートと合成コンテナーは、MEF アプリケーションの基本のビルド ブロックです。 パートは、それ自体までの (自身を含む) 値をインポートまたはエクスポートするオブジェクトです。 カタログは、特定のソースからのパートのコレクションを提供します。 合成コンテナーは、カタログによって提供されるパートを使用して合成 (インポートのエクスポートへの結合) を実行します。

- インポートとエクスポート

     インポートとエクスポートは、コンポーネントが通信を行う手段です。 インポートを使用して、コンポーネントは特定の値またはオブジェクトが必要であることを指定します。エクスポートを使用して、コンポーネントは値が使用可能であることを指定します。 各インポートは、コントラクトを通じて、エクスポートの一覧と対応付けされます。

## <a name="next-steps"></a>次の手順

この例の完成したコードをダウンロードするには、[SimpleCalculator のサンプル (Visual Basic)](https://docs.microsoft.com/samples/dotnet/samples/simple-calculator-vb/) をご覧ください。

 詳しい情報とコード例については、「[Managed Extensibility Framework](https://github.com/MicrosoftArchive/mef)」をご覧ください。 MEF 型の一覧については、<xref:System.ComponentModel.Composition?displayProperty=nameWithType> 名前空間を参照してください。
