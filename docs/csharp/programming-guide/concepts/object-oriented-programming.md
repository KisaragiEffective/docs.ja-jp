---
title: オブジェクト指向プログラミング (C#)
ms.date: 02/08/2020
ms.assetid: 89574786-65ef-4335-88bc-fbacd094f183
ms.openlocfilehash: 01d6f55bf0752f902f351675c4596abbb8ac85c2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "77627891"
---
# <a name="object-oriented-programming-c"></a>オブジェクト指向プログラミング (C#)

C# は、カプセル化、継承、ポリモーフィズムなど、オブジェクト指向プログラミングを完全にサポートします。

- "*カプセル化*" とは、関連するプロパティ、メソッド、およびその他のメンバーのグループが 1 つの単位またはオブジェクトとして扱われることを意味します。
- "*継承*" は、既存のクラスに基づいて新しいクラスを作成する機能です。
- "*ポリモーフィズム*" とは、同じプロパティまたはメソッドを異なる方法で実装している複数のクラスを、交換して使用できることです。

## <a name="classes-and-objects"></a>クラスとオブジェクト

"*クラス*" と "*オブジェクト*" という用語は、オブジェクトの "*型*" とクラスの "*インスタンス*" をそれぞれ意味します。 そのため、オブジェクトを作成する操作は "*インスタンス化*" と呼ばれます。 設計図との対比を使って説明すると、クラスは設計図であり、オブジェクトはその設計図を基にした建築物です。

クラスを定義するコード例を次に示します。

```csharp
class SampleClass
{
}
```

C# には "*構造体*" と呼ばれている型もありますが、継承とポリモーフィズムに対応する必要がないときに便利です。

構造体を定義するコード例を次に示します。

```csharp
struct SampleStruct
{
}
```

詳細については、キーワードの [class](../../language-reference/keywords/class.md) と [struct](../../language-reference/builtin-types/struct.md) に関する記事を参照してください。

### <a name="class-members"></a>クラス メンバー

各クラスには、さまざまな "*クラス メンバー*" を含めることができます。クラス メンバーには、クラスのデータを記述するプロパティ、クラスの動作を定義するメソッド、異なるクラスやオブジェクト間で通信するためのイベントが含まれます。

#### <a name="properties-and-fields"></a>プロパティとフィールド

フィールドとプロパティは、オブジェクトに格納されている情報を表します。 フィールドは、適用されるアクセス修飾子に基づき、直接読み取ったり、設定したりできるという理由から変数と似ています。

クラスのインスタンス内からアクセスできるフィールドを定義するには:

```csharp
public class SampleClass
{
    string sampleField;
}
```

プロパティには `get` と `set` というアクセサーがあります。これによって、値を設定する方法と値を返す方法を細かく制御できます。

C# では、プロパティ値を格納するプライベート フィールドを作成するか、自動実装プロパティを使用できます。自動実装プロパティでは、値を格納するフィールドが背後で自動的に作成され、プロパティ プロシージャの基本的なロジックが提供されます。

自動実装プロパティを定義するコード例を次に示します。

```csharp
class SampleClass
{
    public int SampleProperty { get; set; }
}
```

プロパティ値の読み取りと書き込みのために追加の操作を実行する必要がある場合は、プロパティ値を格納するフィールドを定義し、値を格納および取得する基本的なロジックを実装します。

```csharp
class SampleClass
{
    private int _sample;
    public int Sample
    {
        // Return the value stored in a field.
        get => _sample;
        // Store the value in the field.
        set =>  _sample = value;
    }
}
```

ほとんどのプロパティには、プロパティ値の設定と取得を行うための両方のメソッドまたはプロシージャがあります。 ただし、読み取り専用または書き込み専用のプロパティを作成して、プロパティの変更や読み取りを制限することもできます。 C# では、`get` プロパティ メソッドまたは `set` プロパティ メソッドを省略します。 ただし、自動実装プロパティを書き込み専用にすることはできません。 読み取り専用の自動実装プロパティは、それが含まれるクラスのコンストラクターで設定できます。

詳細については次を参照してください:

- [get](../../language-reference/keywords/get.md)

- [set](../../language-reference/keywords/set.md)

#### <a name="methods"></a>メソッド

"*メソッド*" は、オブジェクトが実行できる処理です。

クラスのメソッドを定義するコード例を次に示します。

```csharp
class SampleClass
{
    public int sampleMethod(string sampleParam)
    {
        // Insert code here
    }
}
```

クラスには、同じメソッドに対して、パラメーターの数やパラメーターの型が異なる複数の実装 ("*オーバーロード*") を含めることができます。

メソッドをオーバーロードするコード例を次に示します。

```csharp
public int sampleMethod(string sampleParam) {}
public int sampleMethod(int sampleParam) {}
```

ほとんどの場合、メソッドはクラス定義内で宣言します。 ただし、C# では、既存のクラスの実際の定義の外部にメソッドを追加できる "*拡張メソッド*" がサポートされています。

詳細については次を参照してください:

- [メソッド](../classes-and-structs/methods.md)
- [拡張メソッド](../classes-and-structs/extension-methods.md)

#### <a name="constructors"></a>コンストラクター

コンストラクターは、特定の型のオブジェクトを作成するときに自動的に実行されるクラス メソッドです。 コンストラクターは、通常、新しいオブジェクトのデータ メンバーを初期化します。 コンストラクターは、クラスの作成時に 1 回だけ実行できます。 また、コンストラクター内のコードは常に、クラス内の他のすべてのコードより先に実行されます。 他のメソッドと同じように、コンストラクターにも複数のオーバーロードを作成できます。

クラスのコンストラクターを定義するコード例を次に示します。

```csharp
public class SampleClass
{
    public SampleClass()
    {
        // Add code here
    }
}
```

詳細については、「[コンストラクター](../classes-and-structs/constructors.md)」を参照してください。

#### <a name="finalizers"></a>ファイナライザー

ファイナライザーは、クラスのインスタンスを破棄するために使います。 .NET Framework では、アプリケーション内のマネージド オブジェクトのメモリの割り当てと解放は、ガベージ コレクターによって自動的に管理されます。 ただし、アプリケーションで作成されるアンマネージ リソースを適切にクリーンアップするために、ファイナライザーも必要になることがあります。 1 つのクラスに定義できるファイナライザーは 1 つだけです。

.NET Framework のファイナライザーおよびガベージ コレクションについて詳しくは、「[ガベージ コレクション](../../../standard/garbage-collection/index.md)」をご覧ください。

#### <a name="events"></a>イベント

クラスやオブジェクトは、何か重要なことが起こった場合に、イベントを使用して他のクラスまたはオブジェクトに通知を送ります。 イベントを送信する (発生させる) クラスは "*パブリッシャー*" と呼ばれ、イベントを受信する (処理する) クラスは "*サブスクライバー*" と呼ばれます。 イベント、およびイベントの発生と処理の詳細については、「[イベント](../../../standard/events/index.md)」をご覧ください。

- クラスでイベントを宣言するには、[event](../../language-reference/keywords/event.md) キーワードを使います。

- イベントを発生させるには、イベント デリゲートを呼び出します。

- イベントをサブスクライブするには、`+=` 演算子を使用します。イベント サブスクリプションを解除するには、`-=` 演算子を使用します。

#### <a name="nested-classes"></a>入れ子になったクラス

別のクラス内で定義されているクラスを "*入れ子になったクラス*" と呼びます。 既定では、入れ子になったクラスはプライベートです。

```csharp
class Container
{
    class Nested
    {
        // Add code here.
    }
}
```

入れ子になったクラスのインスタンスを作成するには、コンテナー クラスの名前に続けて、ドットと入れ子になったクラスの名前を指定します。

```csharp
Container.Nested nestedInstance = new Container.Nested()
```

### <a name="access-modifiers-and-access-levels"></a>アクセス修飾子とアクセス レベル

すべてのクラスおよびクラス メンバーでは、"*アクセス修飾子*" を使って、他のクラスに提供するアクセス レベルを指定できます。

次のアクセス修飾子を使用できます。

|C# の修飾子|定義|
|------------------|----------------|
|[public](../../language-reference/keywords/public.md)|この型またはメンバーには、同じアセンブリ内の他のコードや、そのアセンブリを参照する別のアセンブリ内の任意のコードからアクセスできます。|
|[private](../../language-reference/keywords/private.md)|この型またはメンバーには、同じクラスのコードのみがアクセスできます。|
|[protected](../../language-reference/keywords/protected.md)|この型またはメンバーには、同じクラスまたは派生クラスのコードのみがアクセスできます。|
|[internal](../../language-reference/keywords/internal.md)|この型またはメンバーには、同じアセンブリ内の任意のコードからアクセスできますが、別のアセンブリからはアクセスできません。|
|[protected internal](../../language-reference/keywords/protected-internal.md)|この型またはメンバーには、同じアセンブリ内の任意のコード、または別のアセンブリ内の任意の派生クラスからアクセスできます。|
|[private protected](../../language-reference/keywords/private-protected.md)|この型またはメンバーには、基底クラス アセンブリ内の同じクラスまたは派生クラスのコードがアクセスできます。|

詳細については、「[アクセス修飾子](../classes-and-structs/access-modifiers.md)」を参照してください。

### <a name="instantiating-classes"></a>クラスのインスタンス化

オブジェクトを作成するには、クラスをインスタンス化する (クラスのインスタンスを作成する) 必要があります。

```csharp
SampleClass sampleObject = new SampleClass();
```

クラスをインスタンス化した後は、インスタンスのプロパティやフィールドに値を割り当てたり、クラスのメソッドを呼び出したりできます。

```csharp
// Set a property value.
sampleObject.sampleProperty = "Sample String";
// Call a method.
sampleObject.sampleMethod();
```

クラスのインスタンス化のプロセスでプロパティに値を割り当てるには、オブジェクト初期化子を使用します。

```csharp
// Set a property value.
SampleClass sampleObject = new SampleClass
    { FirstProperty = "A", SecondProperty = "B" };
```

詳細については次を参照してください:

- [new 演算子](../../language-reference/operators/new-operator.md)
- [オブジェクト初期化子とコレクション初期化子](../classes-and-structs/object-and-collection-initializers.md)

### <a name="static-classes-and-members"></a>静的クラスとメンバー

クラスの静的メンバーは、クラスのすべてのインスタンスで共有されるプロパティ、プロシージャ、またはフィールドです。

静的メンバーを定義するコード例を次に示します。

```csharp
static class SampleClass
{
    public static string SampleString = "Sample String";
}
```

静的メンバーにアクセスするには、クラスのオブジェクトを作成せずにクラスの名前を使います。

```csharp
Console.WriteLine(SampleClass.SampleString);
```

C# の静的クラスには静的メンバーだけがあり、インスタンス化することはできません。 また、静的メンバーから、非静的のプロパティ、フィールド、またはメソッドにアクセスすることもできません。

詳しくは、「[static](../../language-reference/keywords/static.md)」をご覧ください。

### <a name="anonymous-types"></a>匿名型

匿名型を使用すると、データ型のクラス定義を記述せずにオブジェクトを作成できます。 クラスは、コンパイラによって生成されます。 このクラスには使用可能な名前がなく、オブジェクトの宣言時に指定したプロパティが格納されます。

匿名型のインスタンスを作成するコード例を次に示します。

```csharp
// sampleObject is an instance of a simple anonymous type.
var sampleObject =
    new { FirstProperty = "A", SecondProperty = "B" };
```

詳細については次を参照してください:[匿名型](../classes-and-structs/anonymous-types.md)。

## <a name="inheritance"></a>継承

継承を使用すると、他のクラスで定義されている動作を再利用、拡張、および変更する新しいクラスを作成できます。 メンバーが継承される側のクラスを "*基底クラス*" と呼び、メンバーを継承する側のクラスを "*派生クラス*" と呼びます。 ただし、C# のすべてのクラスは、.NET のクラス階層構造をサポートしてすべてのクラスに下位レベルのサービスを提供する <xref:System.Object> クラスを暗黙的に継承します。

> [!NOTE]
> C# は多重継承をサポートしていません。 つまり、派生クラスに対して指定できる基底クラスは 1 つだけです。

基底クラスを継承するコード例を次に示します。

```csharp
class DerivedClass:BaseClass {}
```

既定では、すべてのクラスが継承可能になります。 ただし、クラスを基底クラスとして使用できないように指定したり、基底クラスとしてのみ使用できるクラスを作成したりできます。

クラスを基底クラスとして使用できないように指定する方法を次に示します。

```csharp
public sealed class A { }
```

クラスが基底クラスとしてのみ使用され、インスタンス化できないように指定する方法を次に示します。

```csharp
public abstract class B { }
```

詳細については次を参照してください:

- [sealed](../../language-reference/keywords/sealed.md)

- [abstract](../../language-reference/keywords/abstract.md)

### <a name="overriding-members"></a>メンバーのオーバーライド

既定では、派生クラスは基底クラスのすべてのメンバーを継承します。 継承したメンバーの動作を変更する場合は、そのメンバーをオーバーライドする必要があります。 つまり、派生クラスに、メソッド、プロパティ、またはイベントの新しい実装を定義できます。

プロパティやメソッドのオーバーライド方法を制御するには、次の修飾子を使用します。

|C# の修飾子|定義|
|------------------|----------------|
|[virtual](../../language-reference/keywords/virtual.md)|派生クラスでのクラス メンバーのオーバーライドを許可します。|
|[override](../../language-reference/keywords/override.md)|基底クラスで定義されている仮想メンバー (オーバーライドできるメンバー) をオーバーライドします。|
|[abstract](../../language-reference/keywords/abstract.md)|派生クラスでのクラス メンバーのオーバーライドを必須にします。|
|[new 修飾子](../../language-reference/keywords/new-modifier.md)|基底クラスから継承されたメンバーを隠ぺいします。|

## <a name="interfaces"></a>インターフェイス

インターフェイスは、クラスと同様にプロパティ、メソッド、およびイベントのセットを定義します。 ただし、クラスとは異なり、インターフェイスは実装を提供しません。 インターフェイスはクラスによって実装され、クラスとは別のエンティティとして定義されます。 インターフェイスを実装するクラスは、そのインターフェイスのあらゆる機能を定義に従って厳密に実装する必要があります。この点で、インターフェイスはコントラクトを表しています。

インターフェイスを定義するコード例を次に示します。

```csharp
interface ISampleInterface
{
    void doSomething();
}
```

クラスにインターフェイスを実装するコード例を次に示します。

```csharp
class SampleClass : ISampleInterface
{
    void ISampleInterface.doSomething()
    {
        // Method implementation.
    }
}
```

詳細については、プログラミング ガイド記事の[インターフェイス](../interfaces/index.md)に関する箇所と言語参照記事の [interface](../../language-reference/keywords/interface.md) というキーワードに関する箇所を参照してください。

## <a name="generics"></a>ジェネリック

.NET Framework のクラス、構造体、インターフェイス、およびメソッドは、格納または使用できるオブジェクトの型を定義する "*型パラメーター*" を含むことができます。 ジェネリックの最も一般的な例として、コレクションがあります。コレクションには、その中に格納されるオブジェクトの型を指定できます。

ジェネリック クラスを定義するコード例を次に示します。

```csharp
public class SampleGeneric<T>
{
    public T Field;
}
```

ジェネリック クラスのインスタンスを作成するコード例を次に示します。

```csharp
SampleGeneric<string> sampleObject = new SampleGeneric<string>();
sampleObject.Field = "Sample string";
```

詳細については次を参照してください:

- [ジェネリック](../../../standard/generics/index.md)

- [ジェネリック](../generics/index.md)

## <a name="delegates"></a>デリゲート

"*デリゲート*" は、メソッド シグネチャを定義する型であり、互換性のあるシグネチャを持つ任意のメソッドへの参照を提供できます。 メソッドは、デリゲートを使用して起動する (呼び出す) ことができます。 デリゲートは、他のメソッドへの引数としてメソッドを渡すために使用されます。

> [!NOTE]
> イベント ハンドラーは、デリゲートを介して呼び出されるメソッドにすぎません。 デリゲートを使用したイベント処理について詳しくは、「[イベント](../../../standard/events/index.md)」をご覧ください。

デリゲートを作成するコード例を次に示します。

```csharp
public delegate void SampleDelegate(string str);
```

デリゲートで指定されたシグネチャに一致するメソッドへの参照を作成するコード例を次に示します。

```csharp
class SampleClass
{
    // Method that matches the SampleDelegate signature.
    public static void sampleMethod(string message)
    {
        // Add code here.
    }
    // Method that instantiates the delegate.
    void SampleDelegate()
    {
        SampleDelegate sd = sampleMethod;
        sd("Sample string");
    }
}
```

詳細については、プログラミング ガイド記事の[デリゲート](../delegates/index.md)に関する箇所と言語参照記事の [delegate](../../language-reference/builtin-types/reference-types.md) というキーワードに関する箇所を参照してください。

## <a name="see-also"></a>関連項目

- [C# プログラミング ガイド](../index.md)
