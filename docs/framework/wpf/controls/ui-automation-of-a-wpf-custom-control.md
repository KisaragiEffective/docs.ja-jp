---
title: カスタムコントロールの UI オートメーション
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom controls [WPF], applying UI automation
- accessibility [WPF], applying to custom controls
- custom controls [WPF], improving accessibility
- UI Automation [WPF], using with custom controls
ms.assetid: 47b310fc-fbd5-4ce2-a606-22d04c6d4911
ms.openlocfilehash: a370ed2c6b1f3273eca93b4865a032e8299f1cfb
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738202"
---
# <a name="ui-automation-of-a-wpf-custom-control"></a>WPF カスタム コントロールの UI オートメーション
[!INCLUDE[TLA#tla_uiautomation](../../../../includes/tlasharptla-uiautomation-md.md)] は、オートメーション クライアントが使用してさまざまなプラットフォームやフレームワークのユーザー インターフェイスを調査または操作できる、単一の汎用的なインターフェイスを提供します。 [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] では、品質保証 (テスト) のコードとスクリーン リーダーなどのユーザー補助アプリケーションの両方を使用して、ユーザー インターフェイス要素を調べて、他のコードからその要素を使用してユーザーの操作をシュミレーションできます。 すべてのプラットフォームにまたがる [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] について詳しくは、「ユーザー補助機能」を参照してください。  
  
 このトピックでは、WPF アプリケーションで動作するカスタム コントロールのサーバー側 UI オートメーション プロバイダーを実装する方法について説明します。 WPF は、ユーザー インターフェイス要素のツリーに対応するピア オートメーション オブジェクトのツリーを使用して [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] をサポートします。 テスト コードおよびユーザー補助機能を提供するアプリケーションは、オートメーション ピア オブジェクトを直接 (プロセス内コードに対して)、または [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] が提供する汎用のインターフェイスを介して使用できます。  

<a name="AutomationPeerClasses"></a>   
## <a name="automation-peer-classes"></a>オートメーション ピア クラス  
 WPF コントロールは、<xref:System.Windows.Automation.Peers.AutomationPeer>から派生したピアクラスのツリーを通じて [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] をサポートします。 規則上、ピア クラス名は、コントロールのクラス名で始まり、"AutomationPeer" で終わります。 たとえば、<xref:System.Windows.Automation.Peers.ButtonAutomationPeer> は、<xref:System.Windows.Controls.Button> コントロールクラスのピアクラスです。 ピアクラスは [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] コントロール型とほぼ同じですが、WPF 要素に固有です。 [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] インターフェイスを介して WPF アプリケーションにアクセスするオートメーション コードは、オートメーション ピアを直接使用しませんが、同じプロセス空間のオートメーション コードは、オートメーション ピアを直接使用できます。  
  
<a name="BuiltInAutomationPeerClasses"></a>   
## <a name="built-in-automation-peer-classes"></a>組み込みのオートメーション ピア クラス  
 要素は、ユーザーからのインターフェイス アクティビティを受け入れる場合、またはスクリーン リーダー アプリケーションのユーザーが必要とする情報が含まれている場合、オートメーション ピア クラスを実装します。 すべての WPF のビジュアル要素にオートメーション ピアがあるわけではありません。 オートメーションピアを実装するクラスの例として、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Controls.TextBox>、および <xref:System.Windows.Controls.Label>があります。 オートメーションピアを実装しないクラスの例としては、<xref:System.Windows.Controls.Border>などの <xref:System.Windows.Controls.Decorator>から派生したクラスや、<xref:System.Windows.Controls.Panel>に基づくクラス (<xref:System.Windows.Controls.Grid> や <xref:System.Windows.Controls.Canvas>など) があります。  
  
 基本 <xref:System.Windows.Controls.Control> クラスに対応するピアクラスがありません。 <xref:System.Windows.Controls.Control>から派生したカスタムコントロールに対応するピアクラスが必要な場合は、<xref:System.Windows.Automation.Peers.FrameworkElementAutomationPeer>からカスタムピアクラスを派生させる必要があります。  
  
<a name="SecurityConsiderations"></a>   
## <a name="security-considerations-for-derived-peers"></a>派生ピアのセキュリティに関する考慮事項  
 オートメーション ピアは、部分信頼環境で実行する必要があります。 UIAutomationClient アセンブリ内のコードは部分信頼環境で動作するように構成されていないため、オートメーション ピア コードでそのアセンブリを参照しないでください。 代わりに、UIAutomationTypes アセンブリのクラスを使用する必要があります。 たとえば、Uiautomationtypes.dll アセンブリの <xref:System.Windows.Automation.AutomationElementIdentifiers> クラスを使用する必要があります。これは、Uiautomationclient.dll アセンブリの <xref:System.Windows.Automation.AutomationElement> クラスに対応しています。 オートメーション ピア コードで UIAutomationTypes アセンブリを参照すると安全です。  
  
<a name="PeerNavigation"></a>   
## <a name="peer-navigation"></a>ピア ナビゲーション  
 オートメーションピアを見つけた後、インプロセスコードは、オブジェクトの <xref:System.Windows.Automation.Peers.AutomationPeer.GetChildren%2A> および <xref:System.Windows.Automation.Peers.AutomationPeer.GetParent%2A> メソッドを呼び出すことによって、ピアツリー内を移動できます。 コントロール内の WPF 要素間のナビゲーションは、ピアの <xref:System.Windows.Automation.Peers.AutomationPeer.GetChildrenCore%2A> メソッドの実装によってサポートされています。 UI オートメーション システムは、このメソッドを呼び出して、コントロール内に含まれるサブ要素のツリーを構築します。たとえば、リスト ボックス内の項目を一覧表示します。 既定の <xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetChildrenCore%2A?displayProperty=nameWithType> メソッドは、要素のビジュアルツリーを走査して、オートメーションピアのツリーを構築します。 カスタム コントロールは、このメソッドをオーバーライドして子要素をオートメーション クライアントに公開し、情報を伝達するまたはユーザーによる操作を許可する要素のオートメーション ピアを返します。  
  
<a name="Customizations"></a>   
## <a name="customizations-in-a-derived-peer"></a>派生ピアのカスタマイズ  
 <xref:System.Windows.UIElement> および <xref:System.Windows.ContentElement> から派生したすべてのクラスには、保護された仮想メソッド <xref:System.Windows.UIElement.OnCreateAutomationPeer%2A>が含まれています。 WPF は <xref:System.Windows.UIElement.OnCreateAutomationPeer%2A> を呼び出して、各コントロールのオートメーションピアオブジェクトを取得します。 オートメーション コードは、ピアを使用して、コントロールの特性と機能に関する情報を取得し、対話型の使用をシミュレートします。 オートメーションをサポートするカスタムコントロールは、<xref:System.Windows.UIElement.OnCreateAutomationPeer%2A> をオーバーライドし、<xref:System.Windows.Automation.Peers.AutomationPeer>から派生したクラスのインスタンスを返す必要があります。 たとえば、カスタムコントロールが <xref:System.Windows.Controls.Primitives.ButtonBase> クラスから派生した場合、<xref:System.Windows.UIElement.OnCreateAutomationPeer%2A> によって返されるオブジェクトは <xref:System.Windows.Automation.Peers.ButtonBaseAutomationPeer>から派生する必要があります。  
  
 カスタム コントロールを実装する場合、カスタム コントロールに固有の動作を説明する基本オートメーション ピア クラスからの「コア」メソッドをオーバーライドする必要があります。  
  
### <a name="override-oncreateautomationpeer"></a>OnCreateAutomationPeer のオーバーライド  
 カスタムコントロールの <xref:System.Windows.UIElement.OnCreateAutomationPeer%2A> メソッドをオーバーライドして、<xref:System.Windows.Automation.Peers.AutomationPeer>から直接または間接的に派生する必要があるプロバイダーオブジェクトを返すようにします。  
  
### <a name="override-getpattern"></a>GetPattern のオーバーライド  
 オートメーション ピアは、サーバー側の [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] プロバイダーの実装の一部を簡略化しますが、カスタム コントロール オートメーション ピアでもパターン インターフェイスを処理する必要があります。 非 WPF プロバイダーと同様に、ピアは、<xref:System.Windows.Automation.Provider.IInvokeProvider>などの <xref:System.Windows.Automation.Provider?displayProperty=nameWithType> 名前空間にインターフェイスの実装を提供することによって、コントロールパターンをサポートします。 コントロール パターンのインターフェイスは、ピア自体または別のオブジェクトによって実装できます。 ピアの <xref:System.Windows.Automation.Peers.AutomationPeer.GetPattern%2A> の実装は、指定されたパターンをサポートするオブジェクトを返します。 [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] コードは、<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetPattern%2A> メソッドを呼び出し、<xref:System.Windows.Automation.Peers.PatternInterface> 列挙値を指定します。 <xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetPattern%2A> のオーバーライドは、指定されたパターンを実装するオブジェクトを返す必要があります。 コントロールにパターンのカスタム実装がない場合は、その実装を取得するために <xref:System.Windows.Automation.Peers.AutomationPeer.GetPattern%2A> の基本型の実装を呼び出すことができます。また、このコントロール型でパターンがサポートされていない場合は null を取得できます。 たとえば、カスタム NumericUpDown コントロールを範囲内の値に設定して、その [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] ピアが <xref:System.Windows.Automation.Provider.IRangeValueProvider> インターフェイスを実装するようにすることができます。 次の例は、<xref:System.Windows.Automation.Peers.PatternInterface.RangeValue?displayProperty=nameWithType> 値に応答するために、ピアの <xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetPattern%2A> メソッドをオーバーライドする方法を示しています。  
  
 [!code-csharp[CustomControlNumericUpDown#GetPattern](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp/CustomControlLibrary/NumericUpDown.cs#getpattern)]
 [!code-vb[CustomControlNumericUpDown#GetPattern](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic/customcontrollibrary/numericupdown.vb#getpattern)]  
  
 <xref:System.Windows.Automation.Peers.UIElementAutomationPeer.GetPattern%2A> メソッドでは、サブ要素をパターンプロバイダーとして指定することもできます。 次のコードは、<xref:System.Windows.Controls.ItemsControl> がスクロールパターンの処理を内部 <xref:System.Windows.Controls.ScrollViewer> コントロールのピアに転送する方法を示しています。  
  
```csharp  
public override object GetPattern(PatternInterface patternInterface)  
{  
    if (patternInterface == PatternInterface.Scroll)  
    {  
        ItemsControl owner = (ItemsControl) base.Owner;  
  
        // ScrollHost is internal to the ItemsControl class  
        if (owner.ScrollHost != null)  
        {  
            AutomationPeer peer = UIElementAutomationPeer.CreatePeerForElement(owner.ScrollHost);  
            if ((peer != null) && (peer is IScrollProvider))  
            {  
                peer.EventsSource = this;  
                return (IScrollProvider) peer;  
            }  
        }  
    }  
    return base.GetPattern(patternInterface);  
}  
```  
  
```vb  
Public Class Class1  
    Public Overrides Function GetPattern(ByVal patternInterface__1 As PatternInterface) As Object  
        If patternInterface1 = PatternInterface.Scroll Then  
            Dim owner As ItemsControl = DirectCast(MyBase.Owner, ItemsControl)  
  
            ' ScrollHost is internal to the ItemsControl class  
            If owner.ScrollHost IsNot Nothing Then  
                Dim peer As AutomationPeer = UIElementAutomationPeer.CreatePeerForElement(owner.ScrollHost)  
                If (peer IsNot Nothing) AndAlso (TypeOf peer Is IScrollProvider) Then  
                    peer.EventsSource = Me  
                    Return DirectCast(peer, IScrollProvider)  
                End If  
            End If  
        End If  
        Return MyBase.GetPattern(patternInterface1)  
    End Function  
End Class  
```  
  
 パターン処理のサブ要素を指定するために、このコードはサブ要素オブジェクトを取得し、<xref:System.Windows.Automation.Peers.UIElementAutomationPeer.CreatePeerForElement%2A> メソッドを使用してピアを作成します。次に、新しいピアの <xref:System.Windows.Automation.Peers.AutomationPeer.EventsSource%2A> プロパティを現在のピアに設定し、新しいピアを返します。 サブ要素に <xref:System.Windows.Automation.Peers.AutomationPeer.EventsSource%2A> を設定すると、そのサブ要素がオートメーションピアツリーに表示されなくなり、サブ要素によって発生したすべてのイベントが <xref:System.Windows.Automation.Peers.AutomationPeer.EventsSource%2A>で指定されたコントロールからの発信元として指定されます。 <xref:System.Windows.Controls.ScrollViewer> コントロールはオートメーションツリーに表示されません。また、生成されたスクロールイベントは、<xref:System.Windows.Controls.ItemsControl> オブジェクトから発生したものとして表示されます。  
  
### <a name="override-core-methods"></a>「コア」メソッドのオーバーライド  
 オートメーション コードは、ピア クラスのパブリック メソッドを呼び出して、コントロールに関する情報を取得します。 コントロールに関する情報を提供するには、コントロールの実装が基本オートメーション ピア クラスで提供されているものとは異なる場合に、"Core" で終わる名前を持つ各メソッドをオーバーライドします。 少なくとも、次の例に示すように、コントロールには <xref:System.Windows.Automation.Peers.AutomationPeer.GetClassNameCore%2A> および <xref:System.Windows.Automation.Peers.AutomationPeer.GetAutomationControlTypeCore%2A> メソッドを実装する必要があります。  
  
 [!code-csharp[CustomControlNumericUpDown#CoreOverrides](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp/CustomControlLibrary/NumericUpDown.cs#coreoverrides)]
 [!code-vb[CustomControlNumericUpDown#CoreOverrides](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic/customcontrollibrary/numericupdown.vb#coreoverrides)]  
  
 <xref:System.Windows.Automation.Peers.AutomationPeer.GetAutomationControlTypeCore%2A> の実装では、<xref:System.Windows.Automation.ControlType> 値を返すことによってコントロールを記述します。 <xref:System.Windows.Automation.ControlType.Custom?displayProperty=nameWithType>は返すことができますが、コントロールが正確に記述されている場合は、より具体的なコントロール型の1つを返す必要があります。 <xref:System.Windows.Automation.ControlType.Custom?displayProperty=nameWithType> の戻り値には、プロバイダーが [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)]を実装するための追加の作業が必要であり、[!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] クライアント製品では、コントロールの構造、キーボードの相互作用、および考えられるコントロールパターンを予測できません。  
  
 <xref:System.Windows.Automation.Peers.AutomationPeer.IsContentElementCore%2A> および <xref:System.Windows.Automation.Peers.AutomationPeer.IsControlElementCore%2A> メソッドを実装して、コントロールにデータコンテンツが含まれているか、ユーザーインターフェイス (またはその両方) で対話型ロールを満たしているかどうかを示します。 既定では、両方のメソッドが `true` を返します。 これらの設定は、オートメーション ツリーをフィルター処理するこれらのメソッドを使用できるスクリーン リーダーなどのオートメーション ツールの使いやすさを向上します。 <xref:System.Windows.Automation.Peers.AutomationPeer.GetPattern%2A> メソッドがサブ要素ピアにパターン処理を転送する場合、サブ要素ピアの <xref:System.Windows.Automation.Peers.AutomationPeer.IsControlElementCore%2A> メソッドは false を返して、オートメーションツリーからサブ要素ピアを非表示にすることができます。 たとえば、<xref:System.Windows.Controls.ListBox> でのスクロールは <xref:System.Windows.Controls.ScrollViewer>によって処理され、<xref:System.Windows.Automation.Peers.PatternInterface.Scroll?displayProperty=nameWithType> のオートメーションピアは、<xref:System.Windows.Automation.Peers.ScrollViewerAutomationPeer> に関連付けられている <xref:System.Windows.Automation.Peers.ListBoxAutomationPeer>の <xref:System.Windows.Automation.Peers.AutomationPeer.GetPattern%2A> メソッドによって返されます。したがって、<xref:System.Windows.Automation.Peers.ScrollViewerAutomationPeer> の <xref:System.Windows.Automation.Peers.AutomationPeer.IsControlElementCore%2A> メソッドは `false`を返し、<xref:System.Windows.Automation.Peers.ScrollViewerAutomationPeer> がオートメーションツリーに表示されないようにします。  
  
 オートメーション ピアは、コントロールに適切な既定値を提供する必要があります。 コントロールを参照する XAML は、<xref:System.Windows.Automation.AutomationProperties> 属性を含めることによって、コアメソッドのピア実装をオーバーライドできることに注意してください。 たとえば、次の XAML は、カスタマイズされた [!INCLUDE[TLA2#tla_uiautomation](../../../../includes/tla2sharptla-uiautomation-md.md)] プロパティが 2 つあるボタンを作成します。  
  
```xaml  
<Button AutomationProperties.Name="Special"   
    AutomationProperties.HelpText="This is a special button."/>  
```  
  
### <a name="implement-pattern-providers"></a>パターン プロバイダーの実装  
 所有要素が <xref:System.Windows.Controls.Control>から直接派生している場合は、カスタムプロバイダーによって実装されるインターフェイスが明示的に宣言されます。 たとえば、次のコードでは、範囲値を実装する <xref:System.Windows.Controls.Control> のピアを宣言しています。  
  
```csharp  
public class RangePeer1 : FrameworkElementAutomationPeer, IRangeValueProvider { }  
```  
  
```vb  
Public Class RangePeer1  
    Inherits FrameworkElementAutomationPeer  
    Implements IRangeValueProvider  
End Class  
```  
  
 所有しているコントロールが、<xref:System.Windows.Controls.Primitives.RangeBase>などの特定の種類のコントロールから派生している場合、そのピアを同等の派生ピアクラスから派生させることができます。 この場合、ピアは、<xref:System.Windows.Automation.Provider.IRangeValueProvider>の基本実装を提供する <xref:System.Windows.Automation.Peers.RangeBaseAutomationPeer>から派生します。 次のコードは、そのようなピアの宣言を示します。  
  
```csharp  
public class RangePeer2 : RangeBaseAutomationPeer { }  
```  
  
```vb  
Public Class RangePeer2  
    Inherits RangeBaseAutomationPeer  
End Class  
```  
  
実装の例について[C#](https://github.com/dotnet/samples/tree/master/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp)は、「」または[Visual Basic](https://github.com/dotnet/samples/tree/master/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic) NumericUpDown カスタムコントロールを実装して使用するソースコードを参照してください。  
  
### <a name="raise-events"></a>イベントを発生させる  
 オートメーション クライアントは、オートメーション イベントをサブスクライブできます。 カスタムコントロールは、<xref:System.Windows.Automation.Peers.AutomationPeer.RaiseAutomationEvent%2A> メソッドを呼び出すことによって、コントロールの状態の変更を報告する必要があります。 同様に、プロパティ値が変更された場合は、<xref:System.Windows.Automation.Peers.AutomationPeer.RaisePropertyChangedEvent%2A> メソッドを呼び出します。 次のコードは、コントロール コード内からピア オブジェクトを取得し、イベントを発生させるメソッドを呼び出す方法を示しています。 最適化として、このコードは、このイベント型のリスナーがいるかどうかを判断します。 リスナーがいる場合にのみイベントを発生させると、不要なオーバーヘッドを回避し、コントロールの応答性を維持できます。  
  
 [!code-csharp[CustomControlNumericUpDown#RaiseEventFromControl](~/samples/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp/CustomControlLibrary/NumericUpDown.cs#raiseeventfromcontrol)]
 [!code-vb[CustomControlNumericUpDown#RaiseEventFromControl](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic/customcontrollibrary/numericupdown.vb#raiseeventfromcontrol)]  
  
## <a name="see-also"></a>参照

- [UI オートメーションの概要](../../ui-automation/ui-automation-overview.md)
- [サーバー側 UI オートメーション プロバイダーの実装](../../ui-automation/server-side-ui-automation-provider-implementation.md)
- [GitHub の NumericUpDown カスタムC#コントロール ()](https://github.com/dotnet/samples/tree/master/snippets/csharp/VS_Snippets_Wpf/CustomControlNumericUpDown/CSharp)  
- [GitHub の NumericUpDown カスタムコントロール (Visual Basic)](https://github.com/dotnet/samples/tree/master/snippets/visualbasic/VS_Snippets_Wpf/CustomControlNumericUpDown/visualbasic)
