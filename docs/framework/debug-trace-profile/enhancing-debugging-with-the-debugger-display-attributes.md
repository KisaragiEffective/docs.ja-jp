---
title: デバッガー表示属性によるデバッグ機能の拡張
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- debugger, display attributes
- DebuggerTypeProxyAttribute attribute
- debugging [.NET Framework], debugger display attributes
- DebuggerDisplayAttribute attribute
- display attributes for debugger
- DebuggerBrowsableAttribute attribute
ms.assetid: 72bb7aa9-459b-42c4-9163-9312fab4c410
ms.openlocfilehash: ca118bffb045a0e7e3a5084916a0ff8020ebda90
ms.sourcegitcommit: 9c54866bcbdc49dbb981dd55be9bbd0443837aa2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/14/2020
ms.locfileid: "77216492"
---
# <a name="enhancing-debugging-with-the-debugger-display-attributes"></a>デバッガー表示属性によるデバッグ機能の拡張

デバッガー表示属性は、型を指定し、その型の実行時の動作を最もよく理解している型の開発者が、デバッガーで表示されたときに、その型がどのように見えるかも指定できるようにします。 さらに、`Target` プロパティを提供するデバッガー表示属性は、ソース コードの知識がなくても、ユーザーがアセンブリ レベルで適用することができます。 <xref:System.Diagnostics.DebuggerDisplayAttribute> 属性は、デバッガーの変数ウィンドウで型やメンバーを表示する方法を制御します。 <xref:System.Diagnostics.DebuggerBrowsableAttribute> 属性は、デバッガーの変数ウィンドウにフィールドまたはプロパティを表示するかどうか、および表示方法を決定します。 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性は、型に対して代理の型 (プロキシ) を指定し、その型をデバッガー ウィンドウで表示する方法を変更します。 プロキシまたは代替の種類を持つ変数を表示すると、プロキシは、デバッガーの表示ウィンドウで元の型を表します。 デバッガーの変数ウィンドウには、プロキシ型のパブリック メンバーのみが表示されます。 プライベート メンバーは表示されません。  
  
## <a name="using-the-debuggerdisplayattribute"></a>DebuggerDisplayAttribute の使用  

<xref:System.Diagnostics.DebuggerDisplayAttribute.%23ctor%2A> コンストラクターには引数が 1 つあり、それは、型のインスタンスの値列に表示される文字列です。 この文字列には、中かっこ ({ と }) を含めることができます。 かっこ内のテキストは、式として評価されます。 たとえば、次の C# コードでは、`MyHashtable` のインスタンスのデバッガー表示を展開するプラス記号 (+) が選択された場合に、"Count = 4" が表示されます。  

```csharp
[DebuggerDisplay("Count = {count}")]
class MyHashtable
{
    public int count = 4;
}
```

式で参照されるプロパティに適用される属性は処理されません。 C# コンパイラでは、対象の型の現在のインスタンスへのこの参照に対し、暗黙的なアクセスのみを持つ一般的な式が許可されています。 この式は制限されており、エイリアス、ローカル、またはポインターに対するアクセスはありません。 C# コードでは、対象となる型の現在のインスタンスのみの `this` ポインターに暗黙的にアクセスする一般的な式をかっこ内で使用できます。

たとえば、C# オブジェクトにオーバーライドされた `ToString()` がある場合、デバッガーはそのオーバーライドを呼び出し、標準の `{<typeName>}.` ではなく、その結果を表示します。したがって、オーバーライドされた `ToString()` がある場合、<xref:System.Diagnostics.DebuggerDisplayAttribute> を使用する必要はありません。 両方を使用すると、<xref:System.Diagnostics.DebuggerDisplayAttribute> 属性が `ToString()` オーバーライドよりも優先されます。

## <a name="using-the-debuggerbrowsableattribute"></a>DebuggerBrowsableAttribute の使用
 <xref:System.Diagnostics.DebuggerBrowsableAttribute> をフィールドまたはプロパティに適用して、デバッガー ウィンドウにフィールドまたはプロパティを表示する方法を指定します。 この属性のコンストラクターは、次の状態のいずれかを指定する <xref:System.Diagnostics.DebuggerBrowsableState> 列挙値を 1 つ取得します。

- <xref:System.Diagnostics.DebuggerBrowsableState.Never> は、データ ウィンドウにメンバーが表示されないことを示します。  たとえば、この値をフィールドで <xref:System.Diagnostics.DebuggerBrowsableAttribute> に使用すると、そのフィールドが階層から削除され、型のインスタンスのプラス記号 (+) をクリックして囲む型を展開したときに、そのフィールドが表示されなくなります。

- <xref:System.Diagnostics.DebuggerBrowsableState.Collapsed> は、メンバーが表示されますが、既定で展開されていないことを示します。  これは既定の動作です。

- <xref:System.Diagnostics.DebuggerBrowsableState.RootHidden> は、メンバー自体は表示されませんが、メンバーが配列またはコレクションである場合は、その構成オブジェクトが表示されることを示します。

> [!NOTE]
> .NET Framework version 2.0 では、<xref:System.Diagnostics.DebuggerBrowsableAttribute> は Visual Basic でサポートされません。

次のコード例は、<xref:System.Diagnostics.DebuggerBrowsableAttribute> を使用して、それに続くプロパティがクラスのデバッグ ウィンドウに表示されないようにします。

```csharp
[DebuggerBrowsable(DebuggerBrowsableState.Never)]
public static string y = "Test String";
```

## <a name="using-the-debuggertypeproxy"></a>DebuggerTypeProxy の使用
 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性は、型のデバッグ ビューを大幅かつ根本的に変更する必要があるものの、型そのものは変えない場合に使用します。 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性は、開発者が型の表示を調整できるように、型の表示プロキシを指定するために使用します。  <xref:System.Diagnostics.DebuggerDisplayAttribute> と同様に、この属性は、アセンブリ レベルで使用できます。この場合、<xref:System.Diagnostics.DebuggerTypeProxyAttribute.Target%2A> プロパティは、プロキシが使用される型を指定します。 推奨される使用法は、この属性が、属性が適用される型の中で発生するプライベートの入れ子にされた型を指定することです。  型ビューアーをサポートする式エバリュエーターが、型が表示されるときに、この属性をチェックします。 属性が見つかると、式エバリュエーターは、属性が適用される型の代わりに表示プロキシ型を使用します。

 <xref:System.Diagnostics.DebuggerTypeProxyAttribute> が存在する場合は、デバッガーの変数ウィンドウには、プロキシ型のパブリック メンバーのみが表示されます。 プライベート メンバーは表示されません。 データ ウィンドウの動作は、属性拡張ビューでは変更されません。

 不要なパフォーマンスの低下を避けるため、データ ウィンドウでユーザーが型の横にあるプラス記号 (+) をクリックするか、<xref:System.Diagnostics.DebuggerBrowsableAttribute> 属性の適用により、オブジェクトが展開されるまで、表示プロキシの属性は処理されません。 そのため、表示型に属性を適用しないことをお勧めします。 属性は、表示型の本体内で適用でき、また適用する必要があります。

 次のコード例は、<xref:System.Diagnostics.DebuggerTypeProxyAttribute> を使用してデバッガーの表示プロキシとして使用する型を指定します。

```csharp
[DebuggerTypeProxy(typeof(HashtableDebugView))]
class MyHashtable : Hashtable
{
    private const string TestString =
        "This should not appear in the debug window.";

    internal class HashtableDebugView
    {
        private Hashtable hashtable;
        public const string TestStringProxy =
            "This should appear in the debug window.";

        // The constructor for the type proxy class must have a
        // constructor that takes the target type as a parameter.
        public HashtableDebugView(Hashtable hashtable)
        {
            this.hashtable = hashtable;
        }
    }
}
```

## <a name="example"></a>例

### <a name="description"></a>説明

次のコード例を Visual Studio で表示すると、<xref:System.Diagnostics.DebuggerDisplayAttribute>、<xref:System.Diagnostics.DebuggerBrowsableAttribute>、および <xref:System.Diagnostics.DebuggerTypeProxyAttribute> 属性を適用した結果を確認できます。

### <a name="code"></a>コード

[!code-cpp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/cpp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/cpp/program.cpp#1)]
[!code-csharp[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/CS/program.cs#1)]
[!code-vb[System.Diagnostics.DebuggerBrowsableAttribute#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.Diagnostics.DebuggerBrowsableAttribute/VB/module1.vb#1)]

## <a name="see-also"></a>参照

- <xref:System.Diagnostics.DebuggerDisplayAttribute>
- <xref:System.Diagnostics.DebuggerBrowsableAttribute>
- <xref:System.Diagnostics.DebuggerTypeProxyAttribute>
