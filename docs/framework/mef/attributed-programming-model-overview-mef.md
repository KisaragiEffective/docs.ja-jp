---
title: 属性付きプログラミング モデルの概要 (MEF)
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- MEF, attributed programming model
- attributed programming model [MEF]
ms.assetid: 49b787ff-2741-4836-ad51-c3017dc592d4
ms.openlocfilehash: c6b1093d2e821a55cc5513b077a270748a780b71
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75347624"
---
# <a name="attributed-programming-model-overview-mef"></a>属性付きプログラミング モデルの概要 (MEF)

MEF (Managed Extensibility Framework) における *プログラミング モデル* とは、MEF で操作する一連の概念オブジェクトを定義するための特定の方法です。 これらの概念オブジェクトには、パート、インポート、およびエクスポートが含まれます。 MEF では、これらのオブジェクトが使用されますが、その表現方法は指定されていません。 そのため、カスタマイズしたプログラミング モデルを含むさまざまなプログラミング モデルを使用できます。

MEF で使用される既定のプログラミング モデルは、 *属性付きプログラミング モデル*です。 属性付きプログラミング モデルでは、パート、インポート、エクスポート、およびその他のオブジェクトを、通常の .NET Framework クラスを装飾する属性を使用して定義します。 ここでは、属性付きプログラミング モデルの属性を使用して MEF アプリケーションを作成する方法を説明します。

<a name="import_and_export_basics"></a>

## <a name="import-and-export-basics"></a>インポートとエクスポートの基本

*エクスポート* とは、パートがコンテナー内の他のパートに提供する値です。 *インポート* とは、パートがコンテナーに対して示す要件で、使用可能なエクスポートによって満たされます。 属性付きプログラミング モデルでインポートとエクスポートを宣言するには、クラスやメンバーを `Import` 属性と `Export` 属性で装飾します。 `Export` 属性で装飾できるのは、クラス、フィールド、プロパティ、およびメソッドです。 `Import` 属性で装飾できるのは、フィールド、プロパティ、およびコンストラクター パラメーターです。

インポートとエクスポートが一致するには、インポートとエクスポートの *コントラクト*が同じである必要があります。 コントラクトは、 *コントラクト名*と呼ばれる文字列と、 *コントラクト型*と呼ばれる型 (エクスポートまたはインポートされるオブジェクトの型) で構成されます。 エクスポートが特定のインポートを満たすと見なされるのは、コントラクト名とコントラクト型の両方が一致する場合だけです。

これらのコントラクト パラメーターは、どちらも明示的でも暗黙的でもかまいません。 次のコードは、基本的なインポートを宣言するクラスを示しています。

```vb
Public Class MyClass1
    <Import()>
    Public Property MyAddin As IMyAddin
End Class
```

```csharp
public class MyClass
{
    [Import]
    public IMyAddin MyAddin { get; set; }
}
```

このインポートでは、 `Import` 属性にコントラクト型のパラメーターもコントラクト名のパラメーターもアタッチされていません。 したがって、これらは両方とも、装飾されたプロパティから推論されます。 この例では、コントラクト型は `IMyAddin`になり、コントラクト名は、コントラクト型から作成される一意の文字列になります (したがって、このコントラクト名と一致するのは、同じように `IMyAddin`型から推論された名前を持つエクスポートだけです)。

上のインポートと一致するエクスポートの例を次に示します。

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

このエクスポートのコントラクト型は `IMyAddin` です。これは、 `Export` 属性のパラメーターとして指定されています。 エクスポートされる型は、コントラクト型と同じ型、コントラクト型から派生する型、またはコントラクト型を実装する型 (コントラクト型がインターフェイスの場合) のいずれかである必要があります。 このエクスポートの実際の型は `MyLogger` で、 `IMyAddin`インターフェイスを実装しています。 このエクスポートのコントラクト名はコントラクト型から推論されるため、このエクスポートは前のインポートと一致します。

> [!NOTE]
> エクスポートとインポートは、パブリック クラスかパブリック メンバーで宣言するのが一般的です。 その他の宣言もサポートされていますが、プライベート、プロテクト、または内部のメンバーをエクスポートしたりインポートしたりすると、パートの分離モデルが壊れるため、推奨されません。

エクスポートとインポートが一致と見なされるには、コントラクト型が厳密に一致している必要があります。 次に例を示します。

```vb
<Export()> 'WILL NOT match the previous import!
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export] //WILL NOT match the previous import!
public class MyLogger : IMyAddin { }
```

このエクスポートのコントラクト型は、 `MyLogger` ではなく `IMyAddin`です。 `MyLogger` は `IMyAddin`を実装するため、 `IMyAddin` オブジェクトにキャストできますが、コントラクト型が同じではないため、このエクスポートは前のインポートと一致しません。

一般に、ほとんどのコントラクトはコントラクト型とメタデータで定義する必要があり、コントラクト名を指定する必要はありません。 ただし、コントラクト名を直接指定することが重要である場合もあります。 最も一般的なのは、プリミティブなどの一般的な型を共有する複数の値をクラスでエクスポートする場合です。 コントラクト名を指定するには、 `Import` 属性または `Export` 属性の最初のパラメーターを使用します。 次のコードでは、インポートとエクスポートに `MajorRevision`というコントラクト名が指定されています。

```vb
Public Class MyExportClass

    'This one will match
    <Export("MajorRevision")>
    Public ReadOnly Property MajorRevision As Integer
        Get
            Return 4
        End Get
    End Property

    <Export("MinorRevision")>
    Public ReadOnly Property MinorRevision As Integer
        Get
            Return 16
        End Get
    End Property
End Class
```

```csharp
public class MyClass
{
    [Import("MajorRevision")]
    public int MajorRevision { get; set; }
}

public class MyExportClass
{
    [Export("MajorRevision")] //This one will match.
    public int MajorRevision = 4;

    [Export("MinorRevision")]
    public int MinorRevision = 16;
}
```

コントラクト型は、指定しなくても、インポートやエクスポートの型から推論されます。 ただし、インポートとエクスポートが一致と見なされるには、コントラクト名が明示的に指定されていても、コントラクト型も一致する必要があります。 たとえば、上の `MajorRevision` フィールドが文字列だった場合は、推論されるコントラクト型が一致しなくなるため、コントラクト名が同じであっても、エクスポートとインポートが一致しなくなります。

### <a name="importing-and-exporting-a-method"></a>メソッドのインポートとエクスポート

`Export` 属性は、クラス、プロパティ、または関数と同じようにメソッドを装飾することもできます。 型は推論できないため、メソッドのエクスポートではコントラクト型またはコントラクト名を指定する必要があります。 型には、カスタム デリゲートまたはジェネリック型 ( `Func`など) を指定できます。 次のクラスは、 `DoSomething`というメソッドをエクスポートしています。

```vb
Public Class MyAddin

    'Explicitly specifying a generic type
    <Export(GetType(Func(Of Integer, String)))>
    Public Function DoSomething(ByVal TheParam As Integer) As String
        Return Nothing 'Function body goes here
    End Function

End Class
```

```csharp
public class MyAddin
{
    //Explicitly specifying a generic type.
    [Export(typeof(Func<int, string>))]
    public string DoSomething(int TheParam);
}
```

このクラスの `DoSomething` メソッドは、1 つの `int` パラメーターを受け取って `string`を返します。 このエクスポートに一致するには、インポート側のパートで適切なメンバーが宣言されている必要があります。 `DoSomething` メソッドをインポートするクラスを次に示します。

```vb
Public Class MyClass1

    'Contract name must match!
    <Import()>
    Public Property MajorRevision As Func(Of Integer, String)
End Class
```

```csharp
public class MyClass
{
    [Import] //Contract name must match!
    public Func<int, string> DoSomething { get; set; }
}
```

`Func<T, T>` オブジェクトの使用方法の詳細については、「 <xref:System.Func%602>」を参照してください。

<a name="types_of_imports"></a>

## <a name="types-of-imports"></a>インポートの種類

MEF では、動的インポート、遅延インポート、前提条件インポート、省略可能インポートなど、複数の種類のインポートがサポートされています。

### <a name="dynamic-imports"></a>動的インポート

インポート側のクラスが、特定のコントラクト名を持つ任意の型のエクスポートと一致するようにするには、 クラスで *動的インポート*を宣言します。 次のインポートは、コントラクト名が "TheString" の任意のエクスポートと一致します。

```vb
Public Class MyClass1

    <Import("TheString")>
    Public Property MyAddin

End Class
```

```csharp
public class MyClass
{
    [Import("TheString")]
    public dynamic MyAddin { get; set; }
}
```

`dynamic` キーワードから推論されるコントラクト型は、任意のコントラクト型と一致します。 この場合、インポートで コントラクト名を指定する必要があります (コントラクト名が指定されていない場合、インポートはエクスポートと一致しないと見なされます)。次のエクスポートはどちらも、前のインポートと一致します。

```vb
<Export("TheString", GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class

<Export("TheString")>
Public Class MyToolbar

End Class
```

```csharp
[Export("TheString", typeof(IMyAddin))]
public class MyLogger : IMyAddin { }

[Export("TheString")]
public class MyToolbar { }
```

当然ながら、インポート側のクラスが任意の型のオブジェクトに対応できるようになっている必要があります。

### <a name="lazy-imports"></a>遅延インポート

インポート側のクラスで、インポートされるオブジェクトへの間接参照を使用して、そのオブジェクトがすぐにインスタンス化されないようにするには、 クラスで *のコントラクト型を使用して、* 遅延インポート `Lazy<T>`を宣言します。 次のインポート側プロパティは遅延インポートを宣言しています。

```vb
Public Class MyClass1

    <Import()>
    Public Property MyAddin As Lazy(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [Import]
    public Lazy<IMyAddin> MyAddin { get; set; }
}
```

`Lazy<T>` のコントラクト型は、合成エンジンでは `T`のコントラクト型と同じと見なされます。 したがって、上のインポートは次のエクスポートと一致します。

```vb
<Export(GetType(IMyAddin))>
Public Class MyLogger
    Implements IMyAddin

End Class
```

```csharp
[Export(typeof(IMyAddin))]
public class MyLogger : IMyAddin { }
```

`Import` 属性でコントラクト名とコントラクト型を指定する方法については、前の「インポートとエクスポートの基本」を参照してください。

### <a name="prerequisite-imports"></a>前提条件インポート

エクスポートされる MEF パートは、通常、直接要求された場合や、一致したインポートを満たす必要がある場合に、合成エンジンによって作成されます。 合成エンジンがパートを作成するとき、既定ではパラメーターなしのコンストラクターが使用されます。 別のコンストラクターが使用されるようにするには、そのコンストラクターを `ImportingConstructor` 属性でマークします。

合成エンジンで使用するコンストラクターは、各パートで 1 つだけ指定できます。 パラメーターなしのコンストラクターも `ImportingConstructor` 属性も指定されていない場合、または複数の `ImportingConstructor` 属性が指定されている場合は、エラーが発生します。

`ImportingConstructor` 属性でマークされたコンストラクターでは、パラメーターを設定するために、すべてのパラメーターが自動的にインポートとして宣言されます。 これは、パートの初期化の際に使用されるインポートを宣言するのに便利です。 次のクラスは、 `ImportingConstructor` を使用してインポートを宣言しています。

```vb
Public Class MyClass1

    Private _theAddin As IMyAddin

    'Parameterless constructor will NOT be used
    'because the ImportingConstructor
    'attribute is present.
    Public Sub New()

    End Sub

    'This constructor will be used.
    'An import with contract type IMyAddin
    'is declared automatically.
    <ImportingConstructor()>
    Public Sub New(ByVal MyAddin As IMyAddin)
        _theAddin = MyAddin
    End Sub

End Class
```

```csharp
public class MyClass
{
    private IMyAddin _theAddin;

    //Parameterless constructor will NOT be
    //used because the ImportingConstructor
    //attribute is present.
    public MyClass() { }

    //This constructor will be used.
    //An import with contract type IMyAddin is
    //declared automatically.
    [ImportingConstructor]
    public MyClass(IMyAddin MyAddin)
    {
        _theAddin = MyAddin;
    }
}
```

`ImportingConstructor` 属性では、すべてのパラメーター インポートに対して、推論されるコントラクト型とコントラクト名が既定で使用されます。 これをオーバーライドするには、パラメーターを `Import` 属性で装飾して、コントラクト型とコントラクト名を明示的に定義します。 次のコードは、この構文を使用して親クラスの代わりに派生クラスをインポートするコンストラクターを示しています。

```vb
<ImportingConstructor()>
Public Sub New(<Import(GetType(IMySubAddin))> ByVal MyAddin As IMyAddin)

End Sub
```

```csharp
[ImportingConstructor]
public MyClass([Import(typeof(IMySubAddin))]IMyAddin MyAddin)
{
    _theAddin = MyAddin;
}
```

コレクション パラメーターには特別な注意が必要です。 たとえば、 `ImportingConstructor` 型のパラメーターを持つコンストラクターに対して `IEnumerable<int>`を指定すると、そのインポートは、 `IEnumerable<int>`型の一連のエクスポートではなく、 `int`型の 1 つのエクスポートと一致します。 `int`型の一連のエクスポートと一致するようにするには、そのパラメーターを `ImportMany` 属性で装飾する必要があります。

`ImportingConstructor` 属性でインポートとして宣言したパラメーターは、 *前提条件インポート*としてもマークされます。 MEF では、通常はエクスポートとインポートが *循環参照*を形成することが許可されます。 循環参照とは、たとえば、オブジェクト A がオブジェクト B をインポートし、そのオブジェクト B がオブジェクト A をインポートするような場合です。通常は、循環参照が問題になることはなく、合成コンテナーで両方のオブジェクトが正常に構築されます。

パートのコンストラクターで、インポートされる値が必要な場合、そのオブジェクトは循環参照に参加できません。 オブジェクト A を構築するには先にオブジェクト B が構築されている必要がある場合に、オブジェクト B がオブジェクト A をインポートしていると、循環参照を解決できなくなるため、合成エラーが発生します。 したがって、コンストラクターのパラメーターで宣言されているインポートは前提条件インポートになります。前提条件インポートとは、それらがすべて満たされないと、それらを必要とするオブジェクトのエクスポートを使用できるようにならないインポートです。

### <a name="optional-imports"></a>省略可能インポート

`Import` 属性は、パートが機能するための要件を指定します。 インポートが満たされないと、パートの合成が失敗し、そのパートを使用できません。

*プロパティを使用すると、インポートを* 省略可能 `AllowDefault` として指定できます。 この場合、インポートが使用可能などのエクスポートとも一致しない場合でも、合成は成功します。また、プロパティの型 (オブジェクトプロパティの場合は`null`、ブール値の場合は `false`、数値プロパティの場合は 0) に対して、インポートプロパティが既定値に設定されます。次のクラスは、オプションのインポートを使用します。

```vb
Public Class MyClass1

    <Import(AllowDefault:=True)>
    Public Property thePlugin As Plugin

    'If no matching export is available,
    'thePlugin will be set to null.
End Class
```

```csharp
public class MyClass
{
    [Import(AllowDefault = true)]
    public Plugin thePlugin { get; set; }

    //If no matching export is available,
    //thePlugin will be set to null.
}
```

### <a name="importing-multiple-objects"></a>複数のオブジェクトのインポート

`Import` 属性が正しく合成されるのは、1 つのエクスポートのみと一致する場合だけです。 それ以外の場合は合成エラーが生成されます。 同じコントラクトに一致する複数のエクスポートをインポートするには、 `ImportMany` 属性を使用します。 この属性でマークされているインポートは常に省略可能です。 たとえば、一致するエクスポートが存在しなくても合成は失敗しません。 次のクラスは、 `IMyAddin`型の任意の数のエクスポートをインポートします。

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of IMyAddin)

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<IMyAddin> MyAddin { get; set; }
}
```

インポートされた配列にアクセスするには、通常の `IEnumerable<T>` の構文とメソッドを使用します。 通常の配列 (`IMyAddin[]`) を使用することもできます。

このパターンは、 `Lazy<T>` 構文と組み合わせて使用する場合に非常に重要になる場合があります。 たとえば、 `ImportMany`、 `IEnumerable<T>`、および `Lazy<T>`を使用すると、任意の数のオブジェクトへの間接参照をインポートして、必要になったものだけをインスタンス化できます。 次のクラスはこのパターンを示しています。

```vb
Public Class MyClass1

    <ImportMany()>
    Public Property MyAddin As IEnumerable(Of Lazy(Of IMyAddin))

End Class
```

```csharp
public class MyClass
{
    [ImportMany]
    public IEnumerable<Lazy<IMyAddin>> MyAddin { get; set; }
}
```

<a name="avoiding_discovery"></a>

## <a name="avoiding-discovery"></a>検出の回避

パートがカタログの一部として検出されないようにしたい場合があります (パートが、使用ではなく継承を目的とする基底クラスである場合など)。 これを実現するには 2 つの方法があります。 最初の方法は、パート クラスで `abstract` キーワードを使用する方法です。 抽象クラスではエクスポートは提供されませんが、その抽象クラスから派生するクラスに、継承されたエクスポートが提供されます。

クラスを抽象クラスにできない場合は、 `PartNotDiscoverable` 属性で装飾できます。 この属性で装飾されたパートは、カタログに含まれません。 次の例は、これらのパターンを示しています。 `DataOne` は、カタログによって検出されます。 `DataTwo` は、抽象クラスであるため検出されません。 `DataThree` は、 `PartNotDiscoverable` 属性を使用しているため検出されません。

```vb
<Export()>
Public Class DataOne
    'This part will be discovered
    'as normal by the catalog.
End Class

<Export()>
Public MustInherit Class DataTwo
    'This part will not be discovered
    'by the catalog.
End Class

<PartNotDiscoverable()>
<Export()>
Public Class DataThree
    'This part will also not be discovered
    'by the catalog.
End Class
```

```csharp
[Export]
public class DataOne
{
    //This part will be discovered
    //as normal by the catalog.
}

[Export]
public abstract class DataTwo
{
    //This part will not be discovered
    //by the catalog.
}

[PartNotDiscoverable]
[Export]
public class DataThree
{
    //This part will also not be discovered
    //by the catalog.
}
```

<a name="metadata_and_metadata_views"></a>

## <a name="metadata-and-metadata-views"></a>メタデータとメタデータ ビュー

エクスポートでは、 *メタデータ*と呼ばれる自身に関する追加情報を提供できます。 メタデータを使用すると、エクスポートされるオブジェクトのプロパティをインポート側のパートに伝えることができます。 インポート側のパートでは、そのデータを使用して、使用するエクスポートを決定したり、エクスポートを構築せずにその情報を収集したりできます。 そのため、メタデータを使用するインポートは、遅延インポートである必要があります。

メタデータを使用するには、 *メタデータ ビュー*と呼ばれるインターフェイスを宣言して、使用できるようにするメタデータを宣言するのが一般的です。 メタデータ ビュー インターフェイスには、それぞれが `get` アクセサーを持つプロパティのみを含める必要があります。 次のインターフェイスはメタデータ ビューの例です。

```vb
Public Interface IPluginMetadata

    ReadOnly Property Name As String

    <DefaultValue(1)>
    ReadOnly Property Version As Integer

End Interface
```

```csharp
public interface IPluginMetadata
{
    string Name { get; }

    [DefaultValue(1)]
    int Version { get; }
}
```

ジェネリック コレクションの `IDictionary<string, object>`をメタデータ ビューとして使用することもできますが、型チェックの利点がなくなるため、使用しないことをお勧めします。

通常は、メタデータ ビューに含まれているすべてのプロパティが必要になり、それらを提供していないエクスポートは一致と見なされませんが、 `DefaultValue` 属性を指定すると、プロパティを省略可能にすることができます。 この場合、そのプロパティが含まれていないと、 `DefaultValue`のパラメーターとして指定した既定値が割り当てられます。 次の例は、メタデータで装飾された 2 つの異なるクラスです。 これらのクラスは、どちらも前のメタデータ ビューと一致します。

```vb
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class MyLogger
    Implements IPlugin

End Class

'Version is not required because of the DefaultValue
<Export(GetType(IPlugin))>
<ExportMetadata("Name", "Disk Writer")>
Public Class DWriter
    Implements IPlugin

End Class
```

```csharp
[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
}

[Export(typeof(IPlugin)),
    ExportMetadata("Name", "Disk Writer")]
    //Version is not required because of the DefaultValue
public class DWriter : IPlugin
{
}
```

メタデータは、 `Export` 属性の後に `ExportMetadata` 属性を使用して指定します。 各メタデータは名前/値のペアで構成され、 メタデータの名前の部分が、メタデータ ビューの対応するプロパティの名前と一致する必要があります。一致した場合は、メタデータの値がそのプロパティに割り当てられます。

使用するメタデータ ビューを指定するのはインポーターです。 メタデータを含むインポートは遅延インポートとして宣言され、 `Lazy<T,T>`の 2 番目の型パラメーターとしてメタデータ インターフェイスが指定されます。 次のクラスは、前のパートをメタデータと共にインポートします。

```vb
Public Class Addin

    <Import()>
    Public Property plugin As Lazy(Of IPlugin, IPluginMetadata)
End Class
```

```csharp
public class Addin
{
    [Import]
    public Lazy<IPlugin, IPluginMetadata> plugin;
}
```

メタデータは、多くの場合、 `ImportMany` 属性と組み合わせて使用されます。これにより、使用可能なインポートを解析して 1 つのインポートだけを選択してインスタンス化したり、コレクションをフィルター処理して特定の条件に一致するものを抽出したりできます。 次のクラスでは、 `IPlugin` の値が "Logger" である `Name` オブジェクトのみをインスタンス化しています。

```vb
Public Class User

    <ImportMany()>
    Public Property plugins As IEnumerable(Of Lazy(Of IPlugin, IPluginMetadata))

    Public Function InstantiateLogger() As IPlugin

        Dim logger As IPlugin
        logger = Nothing

        For Each Plugin As Lazy(Of IPlugin, IPluginMetadata) In plugins
            If Plugin.Metadata.Name = "Logger" Then
                logger = Plugin.Value
            End If
        Next
        Return logger
    End Function

End Class
```

```csharp
public class User
{
    [ImportMany]
    public IEnumerable<Lazy<IPlugin, IPluginMetadata>> plugins;

    public IPlugin InstantiateLogger()
    {
        IPlugin logger = null;

        foreach (Lazy<IPlugin, IPluginMetadata> plugin in plugins)
        {
            if (plugin.Metadata.Name == "Logger")
                logger = plugin.Value;
        }
        return logger;
    }
}
```

<a name="import_and_export_inheritance"></a>

## <a name="import-and-export-inheritance"></a>インポートとエクスポートの継承

クラスがパートを継承する場合、そのクラスもパートになることがあります。 インポートは常にサブクラスに継承されるため、 パートのサブクラスは常に、同じインポートを親クラスとするパートになります。

`Export` 属性を使用して宣言したエクスポートは、サブクラスに継承されません。 ただし、 `InheritedExport` 属性を使用すると、パート自体をエクスポートできます。 この場合、そのパートのサブクラスは、コントラクト名およびコントラクト型を含め、同じエクスポートを継承して提供します。 `Export` は、 `InheritedExport` 属性とは違って、適用できるのはクラス レベルだけであり、メンバー レベルでは適用できません。 したがって、メンバー レベルのエクスポートを継承することはできません。

次の 4 つのクラスは、インポートとエクスポートの継承の基本原則を示しています。 `NumTwo` は `NumOne`を継承するため、 `NumTwo` は `IMyData`をインポートします。 通常のエクスポートは継承されないため、 `NumTwo` は何もエクスポートしません。 `NumFour` は `NumThree`を継承します。 `NumThree` では `InheritedExport`が使用されているため、 `NumFour` には、コントラクト型が `NumThree`の 1 つのエクスポートがあります。 メンバー レベルのエクスポートは継承されないため、 `IMyData` はエクスポートされません。

```vb
<Export()>
Public Class NumOne
    <Import()>
    Public Property MyData As IMyData
End Class

Public Class NumTwo
    Inherits NumOne

    'Imports are always inherited, so NumTwo will
    'Import IMyData

    'Ordinary exports are not inherited, so
    'NumTwo will NOT export anything.  As a result it
    'will not be discovered by the catalog!

End Class

<InheritedExport()>
Public Class NumThree

    <Export()>
    Public Property MyData As IMyData

    'This part provides two exports, one of
    'contract type NumThree, and one of
    'contract type IMyData.

End Class

Public Class NumFour
    Inherits NumThree

    'Because NumThree used InheritedExport,
    'this part has one export with contract
    'type NumThree.

    'Member-level exports are never inherited,
    'so IMyData is not exported.

End Class
```

```csharp
[Export]
public class NumOne
{
    [Import]
    public IMyData MyData { get; set; }
}

public class NumTwo : NumOne
{
    //Imports are always inherited, so NumTwo will
    //import IMyData.

    //Ordinary exports are not inherited, so
    //NumTwo will NOT export anything. As a result it
    //will not be discovered by the catalog!
}

[InheritedExport]
public class NumThree
{
    [Export]
    Public IMyData MyData { get; set; }

    //This part provides two exports, one of
    //contract type NumThree, and one of
    //contract type IMyData.
}

public class NumFour : NumThree
{
    //Because NumThree used InheritedExport,
    //this part has one export with contract
    //type NumThree.

    //Member-level exports are never inherited,
    //so IMyData is not exported.
}
```

`InheritedExport` 属性にメタデータが関連付けられている場合は、そのメタデータも継承されます (詳細については、前の「メタデータとメタデータビュー」セクションを参照してください)。継承されたメタデータは、サブクラスでは変更できません。 ただし、同じコントラクト名およびコントラクト型と新しいメタデータを使用して `InheritedExport` 属性を再宣言すると、継承されたメタデータをサブクラスで新しいメタデータに置き換えることができます。 次のクラスはこの基本原則を示しています。 ここでは、 `MegaLogger` を継承する `Logger` パートに `InheritedExport` 属性が含まれています。 `MegaLogger` で Status という新しいメタデータが再宣言されているため、 `Logger`のメタデータの Name と Version は継承されません。

```vb
<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Name", "Logger")>
<ExportMetadata("Version", 4)>
Public Class Logger
    Implements IPlugin

    'Exports with contract type IPlugin
    'and metadata "Name" and "Version".
End Class

Public Class SuperLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Name" and "Version", exactly the same
    'as the Logger class.

End Class

<InheritedExport(GetType(IPlugin))>
<ExportMetadata("Status", "Green")>
Public Class MegaLogger
    Inherits Logger

    'Exports with contract type IPlugin and
    'metadata "Status" only. Re-declaring
    'the attribute replaces all metadata.

End Class
```

```csharp
[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Name", "Logger"),
    ExportMetadata("Version", 4)]
public class Logger : IPlugin
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version".
}

public class SuperLogger : Logger
{
    //Exports with contract type IPlugin and
    //metadata "Name" and "Version", exactly the same
    //as the Logger class.
}

[InheritedExport(typeof(IPlugin)),
    ExportMetadata("Status", "Green")]
public class MegaLogger : Logger        {
    //Exports with contract type IPlugin and
    //metadata "Status" only. Re-declaring
    //the attribute replaces all metadata.
}
```

`InheritedExport` 属性を再宣言してメタデータをオーバーライドする際は、コントラクト型が同じであることを確認してください (前の例では、`IPlugin` はコントラクト型です)。異なる場合は、をオーバーライドするのではなく、2番目の属性がパートから独立した2番目のエクスポートを作成します。 したがって、 `InheritedExport` 属性をオーバーライドする際は、一般に、上の例のようにコントラクト型を明示的に指定する必要があります。

インターフェイスは直接インスタンス化できないため、通常は `Export` 属性や `Import` 属性で装飾できませんが、 インターフェイス レベルで `InheritedExport` 属性を使用すると、インターフェイスを装飾できます。この場合、そのエクスポートは、関連付けられているメタデータと共に、そのインターフェイスを実装するすべてのクラスに継承されます。 ただし、そのインターフェイス自体をパートとして使用できるようにはなりません。

<a name="custom_export_attributes"></a>

## <a name="custom-export-attributes"></a>カスタム エクスポート属性

基本エクスポート属性の `Export` および `InheritedExport`を拡張して、メタデータを属性プロパティとして含めることができます。 この手法は、多数のパートに同じようなメタデータを適用したり、メタデータ属性の継承ツリーを作成したりするのに便利です。

カスタム属性では、コントラクト型、コントラクト名、またはその他のメタデータを指定できます。 カスタム属性を定義するには、 `ExportAttribute` (または `InheritedExportAttribute`) を継承するクラスを `MetadataAttribute` 属性で装飾する必要があります。 次のクラスはカスタム属性を定義しています。

```vb
<MetadataAttribute()>
<AttributeUsage(AttributeTargets.Class, AllowMultiple:=false)>
Public Class MyAttribute
    Inherits ExportAttribute

    Public Property MyMetadata As String

    Public Sub New(ByVal myMetadata As String)
        MyBase.New(GetType(IMyAddin))

        myMetadata = myMetadata
    End Sub

End Class
```

```csharp
[MetadataAttribute]
[AttributeUsage(AttributeTargets.Class, AllowMultiple=false)]
public class MyAttribute : ExportAttribute
{
    public MyAttribute(string myMetadata)
        : base(typeof(IMyAddin))
    {
        MyMetadata = myMetadata;
    }

    public string MyMetadata { get; private set; }
}
```

このクラスは、 `MyAttribute` というコントラクト型と `IMyAddin` というメタデータを含む `MyMetadata`というカスタム属性を定義しています。 `MetadataAttribute` 属性でマークされているクラスのすべてのプロパティは、カスタム属性で定義されているメタデータと見なされます 次の 2 つの宣言は等価です。

```vb
<Export(GetType(IMyAddin))>
<ExportMetadata("MyMetadata", "theData")>
Public Property myAddin As MyAddin
```

```vb
<MyAttribute("theData")>
Public Property myAddin As MyAddin
```

```csharp
[Export(typeof(IMyAddin)),
    ExportMetadata("MyMetadata", "theData")]
public MyAddin myAddin { get; set; }
```

```csharp
[MyAttribute("theData")]
public MyAddin myAddin { get; set; }
```

1 つ目の宣言では、コントラクト型とメタデータが明示的に定義されています。 2 つ目の宣言では、コントラクト型とメタデータがカスタム属性で暗黙的に定義されています。 カスタム属性を使用すると、特に、多数のパートに同じ大量のメタデータを適用する必要がある場合 (著者情報や著作権情報の場合など) には、多くの時間を節約し、作業の繰り返しを削減できます。 さらに、カスタム属性の継承ツリーを作成して、バリエーションに対応できるようにすることもできます。

カスタム属性で省略可能なメタデータを作成するには、 `DefaultValue` 属性を使用します。 この属性をカスタム属性クラスのプロパティに適用すると、そのプロパティが省略可能に指定され、エクスポーターによって指定される必要がなくなります。 値が指定されていない場合は、そのプロパティ型の既定値 (通常は、 `null`、 `false`、または 0) が割り当てられます。

<a name="creation_policies"></a>

## <a name="creation-policies"></a>作成ポリシー

パートでインポートが指定され、合成が実行される場合、合成コンテナーは、一致するエクスポートを検索します。 インポートと一致するエクスポートが見つかった場合は、インポート側のメンバーが、エクスポートされるオブジェクトのインスタンスに設定されます。 このインスタンスがどこから取得されるかは、エクスポート側のパートの *作成ポリシー*によって制御されます。

作成ポリシーには、 *共有* と *非共有*の 2 種類があります。 共有の作成ポリシーを持つパートは、そのコントラクトを持つパートのコンテナー内のすべてのインポートの間で共有されます。 合成エンジンで一致が検出され、インポート側のプロパティが設定される場合は、そのパートがまだ存在しないときにのみ新しいコピーがインスタンス化され、それ以外のときは既存のコピーが使用されます。 したがって、多数のオブジェクトが同じパートを参照する場合もあります。 このポリシーは、場所によって変化する可能性がある内部状態に依存するパートに対しては使用できません。 このポリシーは、静的なパート、サービスを提供するパート、メモリやその他のリソースを大量に消費するパートに適しています。

非共有の作成ポリシーを持つパートは、そのいずれかのエクスポートと一致するインポートが検出されるたびに作成されます。 したがって、パートのエクスポートされたコントラクトのいずれかに一致するコンテナー内のすべてのインポートに対して新しいコピーがインスタンス化されます。 それらのコピーの内部状態は共有されません。 このポリシーは、各インポートに固有の内部状態が必要なパートに適しています。

パートの作成ポリシーは、インポートとエクスポートの両方で指定できます。値は、 `Shared`、 `NonShared`、または `Any`の中から選択できます。 既定値は、インポートもエクスポートも `Any` です。 `Shared` または `NonShared` が指定されているエクスポートは、同じ値か `Any`が指定されているインポートとのみ一致します。 同様に、 `Shared` または `NonShared` が指定されているインポートは、同じ値か `Any`が指定されているエクスポートとのみ一致します。 互換性のない作成ポリシーを持つインポートとエクスポートは、コントラクト名やコントラクト型が一致しない場合と同様に、一致とは見なされません。 インポートとエクスポートの両方で `Any`が指定されている場合、および作成ポリシーが指定されていないために既定値の `Any`が使用される場合は、既定で共有の作成ポリシーが使用されます。

次の例では、インポートとエクスポートの両方で作成ポリシーが指定されています。 `PartOne` では作成ポリシーが指定されていないため、既定値の `Any`が使用されます。 `PartTwo` では作成ポリシーが指定されていないため、既定値の `Any`が使用されます。 インポートとエクスポートの両方で既定値の `Any`が使用されるため、 `PartOne` は共有されます。 `PartThree` では `Shared` の作成ポリシーが指定されているため、 `PartTwo` と `PartThree` は `PartOne`の同じコピーを共有します。 `PartFour` では `NonShared` の作成ポリシーが指定されているため、 `PartFour` は `PartFive`で共有されません。 `PartSix` では `NonShared` の作成ポリシーが指定されているため、 `PartFive` と `PartSix` は、どちらも `PartFour`とは別のコピーを受け取ります。 `PartSeven` では `Shared` の作成ポリシーが指定されているため、 `PartFour` の作成ポリシーを持つ `Shared`はエクスポートされていないため、 `PartSeven` のインポートは一致が検出されず、満たされません。

```vb
<Export()>
Public Class PartOne
    'The default creation policy for an export is Any.
End Class

Public Class PartTwo

    <Import()>
    Public Property partOne As PartOne

    'The default creation policy for an import is Any.
    'If both policies are Any, the part will be shared.

End Class

Public Class PartThree

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partOne As PartOne

    'The Shared creation policy is explicitly specified.
    'PartTwo and PartThree will receive references to the
    'SAME copy of PartOne.

End Class

<Export()>
<PartCreationPolicy(CreationPolicy.NonShared)>
Public Class PartFour
    'The NonShared creation policy is explicitly specified.
End Class

Public Class PartFive

    <Import()>
    Public Property partFour As PartFour

    'The default creation policy for an import is Any.
    'Since the export's creation policy was explicitly
    'defined, the creation policy for this property will
    'be non-shared.

End Class

Public Class PartSix

    <Import(RequiredCreationPolicy:=CreationPolicy.NonShared)>
    Public Property partFour As PartFour

    'Both import and export specify matching creation
    'policies.  PartFive and PartSix will each receive
    'SEPARATE copies of PartFour, each with its own
    'internal state.

End Class

Public Class PartSeven

    <Import(RequiredCreationPolicy:=CreationPolicy.Shared)>
    Public Property partFour As PartFour

    'A creation policy mismatch.  Because there is no
    'exported PartFour with a creation policy of Shared,
    'this import does not match anything and will not be
    'filled.

End Class
```

```csharp
[Export]
public class PartOne
{
    //The default creation policy for an export is Any.
}

public class PartTwo
{
    [Import]
    public PartOne partOne { get; set; }

    //The default creation policy for an import is Any.
    //If both policies are Any, the part will be shared.
}

public class PartThree
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartOne partOne { get; set; }

    //The Shared creation policy is explicitly specified.
    //PartTwo and PartThree will receive references to the
    //SAME copy of PartOne.
}

[Export]
[PartCreationPolicy(CreationPolicy.NonShared)]
public class PartFour
{
    //The NonShared creation policy is explicitly specified.
}

public class PartFive
{
    [Import]
    public PartFour partFour { get; set; }

    //The default creation policy for an import is Any.
    //Since the export's creation policy was explicitly
    //defined, the creation policy for this property will
    //be non-shared.
}

public class PartSix
{
    [Import(RequiredCreationPolicy = CreationPolicy.NonShared)]
    public PartFour partFour { get; set; }

    //Both import and export specify matching creation
    //policies.  PartFive and PartSix will each receive
    //SEPARATE copies of PartFour, each with its own
    //internal state.
}

public class PartSeven
{
    [Import(RequiredCreationPolicy = CreationPolicy.Shared)]
    public PartFour partFour { get; set; }

    //A creation policy mismatch.  Because there is no
    //exported PartFour with a creation policy of Shared,
    //this import does not match anything and will not be
    //filled.
}
```

<a name="life_cycle_and_disposing"></a>

## <a name="life-cycle-and-disposing"></a>ライフ サイクルと破棄

パートは合成コンテナーでホストされるため、ライフ サイクルが通常のオブジェクトより複雑になることがあります。 パートには、実装できるライフ サイクル関連の重要なインターフェイスが 2 つあります。1 つは `IDisposable` 、もう 1 つは `IPartImportsSatisfiedNotification`です。

終了時に作業を実行したりリソースを解放したりする必要があるパートは、通常の .NET Framework オブジェクトのように `IDisposable`を実装する必要があります。 ただし、パートの作成や参照の管理はコンテナーで行われるため、パートの `Dispose` メソッドを呼び出すのは、そのパートを所有しているコンテナーだけにする必要があります。 コンテナー自体にも `IDisposable`が実装されており、その `Dispose` のクリーンアップの一部として、所有しているすべてのパートの `Dispose` が呼び出されます。 そのため、合成コンテナーと、その合成コンテナーが所有しているパートが不要になった場合は、合成コンテナーを破棄する必要があります。

有効期間が長い合成コンテナーでは、非共有の作成ポリシーを持つパートによるメモリの消費が問題になることがあります。 これらのパートは共有されないため、何度も作成される可能性がありますが、コンテナー自体が破棄されるまで破棄されません。 この問題に対処するため、コンテナーには `ReleaseExport` メソッドが用意されています。 共有されないエクスポートに対してこのメソッドを呼び出すと、そのエクスポートが合成コンテナーから削除されて破棄されます。 削除されるエクスポートによってのみ使用されるパートも、ツリーの下位にあるものも含め、一緒に削除されて破棄されます。 これにより、合成コンテナー自体を破棄せずにリソースを再利用できます。

`IPartImportsSatisfiedNotification` には、 `OnImportsSatisfied`という 1 つのメソッドが含まれています。 このメソッドは、合成コンテナーにより、このインターフェイスを実装するパートに対して、合成が完了してそのパートのインポートを使用できるようになったときに呼び出されます。 パートは、他のパートのインポートを満たすために合成エンジンによって作成されます。 パートのインポートが設定されるまでは、パートのコンストラクターで、インポートされる値に依存する初期化や、インポートされる値を操作する初期化を実行できません。初期化を実行できるようにするには、 `ImportingConstructor` 属性を使用して、それらの値を必須として指定します。 通常はこの方法が推奨されますが、コンストラクター インジェクションを使用できない場合もあります。 そのような場合は、 `OnImportsSatisfied`で初期化を実行できます。この場合は、パートに `IPartImportsSatisfiedNotification`を実装する必要があります。

## <a name="see-also"></a>関連項目

- [Channel 9 ビデオ: Managed Extensibility Framework を使用してアプリケーションを開く](https://channel9.msdn.com/events/TechEd/NorthAmerica/2009/DTL328)
- [Channel 9 ビデオ: Managed Extensibility Framework (MEF) 2.0](https://channel9.msdn.com/posts/NET-45-Oleg-Lvovitch-and-Kevin-Ransom-Managed-Extensibility-Framework-MEF-20)
