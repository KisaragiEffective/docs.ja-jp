---
title: COM 呼び出し可能ラッパー
ms.date: 10/23/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- CCW
- COM interop, COM wrappers
- COM wrappers
- callable wrappers
- interoperation with unmanaged code, COM wrappers
- COM callable wrappers
ms.assetid: d04be3b5-27b9-4f5b-8469-a44149fabf78
ms.openlocfilehash: 6f2f4055a95dbcea8d7872b5c5fa3ccede8c2c8c
ms.sourcegitcommit: 559fcfbe4871636494870a8b716bf7325df34ac5
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/30/2019
ms.locfileid: "73120727"
---
# <a name="com-callable-wrapper"></a>COM 呼び出し可能ラッパー

COM クライアントが .NET オブジェクトを呼び出すと、共通言語ランタイムがマネージド オブジェクトとそのオブジェクトのための COM 呼び出し可能ラッパー (CCW: COM Callable Wrapper) を作成します。 COM クライアントは .NET オブジェクトを直接参照できないため、CCW をマネージド オブジェクトのプロキシとして使用します。

ランタイムは、サービスを要求している COM クライアントの数に関係なく、1 つのマネージド オブジェクトに対して 1 つの CCW を作成します。 次の図に示すように、複数の COM クライアントが、INew インターフェイスを公開する CCW への参照を保持できます。 CCW は、INew インターフェイスを実装するマネージド オブジェクトへの 1 つの参照を保持し、ガベージ コレクションされます。 COM クライアントと .NET クライアントは、同一のマネージド オブジェジェクトに対して同時に要求できます。

![INew を公開する CCW への参照を保持している複数の COM クライアント。](./media/com-callable-wrapper/com-callable-wrapper-clients.gif)

COM 呼び出し可能ラッパーは、.NET ランタイム内で実行されている他のクラスからは見えません。 CCW の主な目的は、マネージド コードとアンマネージド コードの間の呼び出しをマーシャリングすることですが、CCW は、CCW にラップされているマネージド オブジェクトのオブジェクト ID やオブジェクトの有効期間の管理も行います。

## <a name="object-identity"></a>オブジェクト ID

ランタイムは、ランタイムのガベージ コレクション ヒープから .NET オブジェクトにメモリを割り当てます。これにより、ランタイムは、メモリ内で必要に応じてオブジェクトを移動させることができます。 一方、CCW に対して、ランタイムは、コレクション ヒープ以外からメモリを割り当てます。これにより、COM クライアントはラッパーを直接参照できます。

## <a name="object-lifetime"></a>オブジェクトの有効期間

CCW にラップされている .NET クライアントとは異なり、CCW は、従来の COM 方式で参照カウントを使用します。 CCW の参照カウントが 0 (ゼロ) に達すると、ラッパーはマネージド オブジェクトの参照を解放します。 参照が残っていないマネージド オブジェクトは、次のガベージ コレクション サイクルで収集されます。

## <a name="simulating-com-interfaces"></a>COM インターフェイスのシミュレート

CCW は、パブリックで COM から参照できるすべてのインターフェイス、データ型、および戻り値を、COM によるインターフェイス ベースの対話の適用と整合性のある方法で COM クライアントに公開します。 COM クライアントの場合、.NET オブジェクトのメソッドを呼び出すことは、COM オブジェクトのメソッドを呼び出すことと同じです。

このシームレスなアプローチを実現するために、CCW は **IUnknown** や **IDispatch** などの従来の COM インターフェイスを製造します。 次の図が示すように、CCW は、ラップしている .NET オブジェクトの 1 つの参照を保持します。 COM クライアントと .NET オブジェクトの両方は、CCW のプロキシとスタブ構築を介して相互に対話します。

![CCW が COM インターフェイスを製造する方法を示す図。](./media/com-callable-wrapper/com-callable-wrapper-interfaces.gif)

マネージド環境でクラスによって明示的に実装されるインターフェイスを公開するだけでなく、.NET ランタイムは、オブジェクトの代わりに、次の表に一覧表示されている COM インターフェイスの実装を提供します。 .NET クラスは、これらのインターフェイスの独自の実装を提供することで、既定の動作をオーバーライドできます。 ただし、ランタイムは **IUnknown** と **IDispatch** インターフェイス実装を常に提供します。

|Interface|説明|
|---------------|-----------------|
|**IDispatch**|型への遅延バインディングのメカニズムを提供します。|
|**IErrorInfo**|エラー、そのソース、ヘルプ ファイル、ヘルプ コンテキスト、およびエラーを定義したインターフェイスの GUID (.NET クラスでは常に **GUID_NULL**) に関する説明文を示します。|
|**IProvideClassInfo**|マネージド クラスによって実装される **ITypeInfo** インターフェイスに COM クライアントがアクセスできるようにします。 COM からインポートされていない型については、.NET Core 上で `COR_E_NOTSUPPORTED` を返します。 |
|**ISupportErrorInfo**|マネージド オブジェクトが **IErrorInfo** インターフェイスをサポートするかどうかを COM クライアントが判別できるようにします。 その場合は、クライアントが最新の例外オブジェクトへのポインターを取得できるようにします。 すべてのマネージド型は、**IErrorInfo** インターフェイスをサポートします。|
|**ITypeInfo** (.NET Framework のみ)|Tlbexp.exe によって生成された型情報と完全に等しい、クラスの型情報を提供します。|
|**IUnknown**|COM クライアントが CCW の有効期間を管理し、強制型変換を提供するための、**IUnknown** インターフェイスの標準的な実装を提供します。|

 マネージド クラスは、次の表で説明されている COM インターフェイスを提供することもできます。

|Interface|説明|
|---------------|-----------------|
|(\_*classname*) クラス インターフェイス|マネージド オブジェクトで明示的に公開されている、すべてのパブリック インターフェイス、メソッド、プロパティ、およびフィールドを公開する、ランタイムによって公開され、明示的に定義されていない、インターフェイス、|
|**IConnectionPoint** と **IConnectionPointContainer**|デリゲート ベースのソース イベント (イベント サブスクライバーを登録するためのインターフェイス) を供給するオブジェクトのインターフェイス。|
|**IDispatchEx** (.NET Framework のみ)|クラスが **IExpando** を実装する場合、ランタイムによって提供されているインターフェイス。 **IDispatchEx** インターフェイスは、**IDispatch** インターフェイスの拡張版で、**IDispatch** とは異なり、列挙、追加、削除、および大文字小文字を区別したメンバーの呼び出しが可能になります。|
|**IEnumVARIANT**|クラスが **IEnumerable** を実装する場合、コレクション内のオブジェクトを列挙するコレクション型クラスのインターフェイス。|

## <a name="introducing-the-class-interface"></a>クラス インターフェイスの概要

マネージド コードで明示的に定義されていないクラス インターフェイスは、.NET オブジェクトで明示的に公開されるすべてのパブリック メソッド、プロパティ、フィールド、およびイベントを公開するインターフェイスです。 このインターフェイスは、デュアルまたはディスパッチ専用インターフェイスです。 クラス インターフェイスは、前にアンダー スコアの付いた、.NET クラス自体の名前を受け取ります。 たとえば、クラス Mammal の場合、クラス インターフェイスは \_Mammal です。

派生クラスの場合、クラス インターフェイスは、基本クラスのすべてのパブリック メソッド、プロパティ、およびフィールドも公開します。 派生クラスは、各基本クラスのクラス インターフェイスも公開します。 たとえば、クラス Mammal がクラス MammalSuperclass を拡張し、そのクラスがさらに System.Object を拡張する場合、.NET オブジェクトは COM クライアントに \_Mammal、\_MammalSuperclass、および \_Object という名前の 3 つのクラス インターフェイスを公開します。

たとえば、次の .NET クラスを考えます。

```vb
' Applies the ClassInterfaceAttribute to set the interface to dual.
<ClassInterface(ClassInterfaceType.AutoDual)> _
' Implicitly extends System.Object.
Public Class Mammal
    Sub Eat()
    Sub Breathe()
    Sub Sleep()
End Class
```

```csharp
// Applies the ClassInterfaceAttribute to set the interface to dual.
[ClassInterface(ClassInterfaceType.AutoDual)]
// Implicitly extends System.Object.
public class Mammal
{
    public void Eat() {}
    public void Breathe() {}
    public void Sleep() {}
}
```

COM クライアントは、`_Mammal` という名前のクラス インターフェイスへのポインターを取得できます。 .NET Framework では、[タイプ ライブラリ エクスポーター (Tlbexp.exe)](../../framework/tools/tlbexp-exe-type-library-exporter.md) ツールを使用して、`_Mammal`インターフェイス定義を含むタイプ ライブラリを生成できます。 タイプ ライブラリ エクスポーターは、.NET Core ではサポートされません。 `Mammal` クラスが 1 つ以上のインターフェイスを実装した場合、それらのインターフェイスはコクラスの下に表示されます。

```console
[odl, uuid(…), hidden, dual, nonextensible, oleautomation]
interface _Mammal : IDispatch
{
    [id(0x00000000), propget] HRESULT ToString([out, retval] BSTR*
        pRetVal);
    [id(0x60020001)] HRESULT Equals([in] VARIANT obj, [out, retval]
        VARIANT_BOOL* pRetVal);
    [id(0x60020002)] HRESULT GetHashCode([out, retval] short* pRetVal);
    [id(0x60020003)] HRESULT GetType([out, retval] _Type** pRetVal);
    [id(0x6002000d)] HRESULT Eat();
    [id(0x6002000e)] HRESULT Breathe();
    [id(0x6002000f)] HRESULT Sleep();
}
[uuid(…)]
coclass Mammal
{
    [default] interface _Mammal;
}
```

クラス インターフェイスの生成はオプションです。 既定では、COM 相互運用が、タイプ ライブラリにエクスポートするクラスごとにディスパッチ専用インターフェイスを生成します。 <xref:System.Runtime.InteropServices.ClassInterfaceAttribute> をクラスに適用することによって、このインターフェイスの自動作成を防止または変更することができます。 クラス インターフェイスにより、マネージド クラスを COM に公開するタスクを軽減できますが、その使用は制限されています。

> [!CAUTION]
> 独自のものを明示的に定義する代わりにクラス インターフェイスを使用すると、マネージド クラスの将来のバージョン管理が複雑になることがあります。 クラス インターフェイスを使用する前に、次のガイドラインを参照してください。

### <a name="define-an-explicit-interface-for-com-clients-to-use-rather-than-generating-the-class-interface"></a>クラス インターフェイスを生成するのではなく、COM クライアントが使用する明示的なインターフェイスを定義します。

COM 相互運用はクラス インターフェイスを自動的に生成するため、クラスにバージョン後の変更が生じると、共通言語ランタイムによって公開されているクラス インターフェイスのレイアウトが変更されることがあります。 COM クライアントは通常、インターフェイスのレイアウトの変更を処理するように準備されていないため、クラスのメンバー レイアウトを変更した場合に、クライアントが破損します。

このガイドラインは、COM クライアントに公開されるインターフェイスは変更不可にしておく必要があるという概念を促進しています。 間違えてインターフェイスのレイアウトを並べ替えることで COM クライアントが破損するリスクを抑えるため、インターフェイスを明示的に定義して、クラスに対するすべての変更をインターフェイス レイアウトから分離します。

**ClassInterfaceAttribute** を使用してクラス インターフェイスの自動生成を中止し、次のコード フラグメントに示すように、クラスの明示的なインターフェイスを実装します。

```vb
<ClassInterface(ClassInterfaceType.None)>Public Class LoanApp
    Implements IExplicit
    Sub M() Implements IExplicit.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.None)]
public class LoanApp : IExplicit
{
    int IExplicit.M() { return 0; }
}
```

**ClassInterfaceType.None** 値により、クラス メタデータをタイプ ライブラリにエクスポートするときに、クラス インターフェイスが生成されなくなります。 前の例では、COM クライアントは `IExplicit` インターフェイスを通してのみ `LoanApp` クラスにアクセスできます。

### <a name="avoid-caching-dispatch-identifiers-dispids"></a>ディスパッチ識別子 (DISPID) をキャッシュしないようにします。

クラス インターフェイスの使用は、インターフェイス メンバーの DISPID をキャッシュしていないスクリプト化されたクライアント、Microsoft Visual Basic 6.0 クライアント、または遅延バインディング クライアントのために許容されるオプションです。 DISPID は遅延バインディングを有効にするインターフェイス メンバーを特定します。

クラス インターフェイスでは、インターフェイス内のメンバーの位置に基づいて DISPID が生成されます。 メンバーの順序を変更してクラスをタイプ ライブラリにエクスポートすると、クラス インターフェイスで生成される DISPID が変更されます。

クラス インターフェイスを使用するとき、遅延バインドされた COM クライアントの中断を回避するには、**ClassInterfaceAttribute** に **ClassInterfaceType.AutoDispatch** 値を指定して適用します。 この値は、ディスパッチ専用のクラスのインターフェイスを実装しますが、タイプ ライブラリからインターフェイスの説明を省略します。 インターフェイスの説明がないと、クライアントはコンパイル時に DISPID をキャッシュできません。 これはクラス インターフェイスの既定のインターフェイス型ですが、属性値を明示的に適用することもできます。

```vb
<ClassInterface(ClassInterfaceType.AutoDispatch)> Public Class LoanApp
    Implements IAnother
    Sub M() Implements IAnother.M
…
End Class
```

```csharp
[ClassInterface(ClassInterfaceType.AutoDispatch)]
public class LoanApp
{
    public int M() { return 0; }
}
```

実行時にインターフェイス メンバーの DISPID を取得するために、COM クライアントは **IDispatch.GetIdsOfNames** を呼び出すことができます。 インターフェイスでメソッドを呼び出すには、返された DISPID を **IDispatch.Invoke** への引数として渡します。

### <a name="restrict-using-the-dual-interface-option-for-the-class-interface"></a>クラス インターフェイス用のデュアル インターフェイスのオプションの使用を制限します。

デュアル インターフェイスは、COM クライアントによるインターフェイス メンバーへの事前バインディングと遅延バインディングを有効にします。 デザイン時およびテスト中は、クラス インターフェイスをデュアルに設定すると役に立つ場合があります。 変更されることのないマネージド クラス (およびその基本クラス) の場合、このオプションも使用できます。 それ以外の場合はすべて、クラス インターフェイスをデュアルに設定しないでください。

自動的に生成されたデュアル インターフェイスが適切な場合もまれにありますが、より多くの場合、それはバージョンに関連する複雑さを生じさせます。 たとえば、派生クラスのクラス インターフェイスを使用する COM クライアントは、基本クラスが変更されると簡単に中断します。 サード パーティが基本クラスを提供するとき、クラス インターフェイスのレイアウトを自分で制御することはできません。 さらに、ディスパッチ専用インターフェイスとは異なり、デュアル インターフェイス (**ClassInterfaceType.AutoDual**) は、エクスポートされたタイプ ライブラリ内にクラス インターフェイスの説明を提供します。 そのような説明により、遅延バインディングのクライアントではコンパイル時に DISPID をキャッシュすることが促進されます。

### <a name="ensure-that-all-com-event-notifications-are-late-bound"></a>すべての COM イベント通知が遅延バインドされていることを確認します。

既定では、COM の型情報はマネージド アセンブリに直接埋め込まれているため、プライマリ相互運用アセンブリ (PIA) の必要はありません。 ただし、埋め込まれた型情報の制限の 1 つとして、早期にバインドされた vtable 呼び出しによる COM イベント通知の配信はサポートされず、遅延バインドされた `IDispatch::Invoke` 呼び出しのみがサポートされるということがあります。

アプリケーションから COM イベント インターフェイス メソッドに対して早期にバインドされた呼び出しが必要な場合は、Visual Studio の **[相互運用型の埋め込み]** プロパティを `true` に設定するか、プロジェクト ファイルに次の要素を含めることができます。

```xml
<EmbedInteropTypes>True</EmbedInteropTypes>
```

## <a name="see-also"></a>関連項目

- <xref:System.Runtime.InteropServices.ClassInterfaceAttribute>
- [COM ラッパー](com-wrappers.md)
- [COM への .NET Framework コンポーネントの公開](../../framework/interop/exposing-dotnet-components-to-com.md)
- [COM への .NET Core コンポーネントの公開](../../core/native-interop/expose-components-to-com.md)
- [要件 (相互運用のための .NET 型の)](qualify-net-types-for-interoperation.md)
- [ランタイム呼び出し可能ラッパー](runtime-callable-wrapper.md)
