---
title: .NET Framework のアクセシビリティの新機能
ms.custom: updateeachrelease
ms.date: 04/18/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- what's new [.NET Framework]
ms.openlocfilehash: 4dbc2024aa2e956b23030ae6eab987e65e006d12
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "79398677"
---
# <a name="whats-new-in-accessibility-in-the-net-framework"></a>.NET Framework のアクセシビリティの新機能

.NET Framework では、ユーザーのためにアプリケーションのアクセシビリティ向上を目指しています。 ユーザー補助機能により、支援機能の利用者にとってアプリケーションがさらに使いやすくなります。 .NET Framework 4.7.1 以降、.NET Framework にはさまざまなアクセシビリティ機能改善が含まれるようになりました。それを利用することで開発者はアクセシビリティを備えたアプリケーションを開発できます。

## <a name="accessibility-switches"></a>アクセシビリティのスイッチ

アプリのターゲットが .NET Framework 4.7 である場合、またはそれより前のバージョンがターゲットであっても実行環境が .NET Framework 4.7.1 以降である場合は、ユーザー補助機能を使用するようにアプリを構成できます。 また、アプリのターゲットが 4.7.1 .NET Framework 以降である場合は、従来の機能を使用するように (そして、ユーザー補助機能を利用しないように) アプリを構成することもできます。 ユーザー補助機能が含まれる .NET Framework の各バージョンには、バージョン固有のアクセシビリティ スイッチがあります。これを、アプリケーションの構成ファイルの [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) セクションの [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 要素に追加します。 サポートされているスイッチは次のとおりです。

|バージョン|Switch|
|---|---|
|.NET Framework 4.7.1|"Switch.UseLegacyAccessibilityFeatures"|
|.NET Framework 4.7.2|"Switch.UseLegacyAccessibilityFeatures.2"|
|.NET Framework 4.8|"Switch.UseLegacyAccessibilityFeatures.3"|

### <a name="taking-advantage-of-accessibility-enhancements"></a>アクセシビリティ拡張機能の利用

.NET Framework 4.7.1 以降をターゲット対象とするアプリケーションでは、この新しいユーザー補助機能が既定で有効になっています。 また、それより前のバージョンの .NET Framework がターゲットであっても、実行環境が .NET Framework 4.7.1 以降であるアプリケーションの場合は、アプリケーションの構成ファイルの [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) セクションの [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 要素にスイッチを追加し、値を `false` に設定することで、古いアクセシビリティ動作を利用しないことを (そして、それにより向上したアクセシビリティ機能改善を利用することを) 選択できます。 .NET Framework 4.7.1 で導入されたアクセシビリティの機能強化を有効にする方法を次に示します。

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false" />
</runtime>
```

新しいバージョンの .NET Framework のユーザー補助機能を選択する場合は、古いバージョンの .NET Framework の機能も明示的に選択する必要があります。 .NET Framework 4.7.1 と 4.7.2 両方のアクセシビリティ機能改善を利用するようにアプリを構成するには、次の [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 要素が必要です。

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false" />
</runtime>
```

.NET Framework 4.7.1、4.7.2、4.8 のアクセシビリティ機能改善を利用するようにアプリを構成するには、次の [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) 要素が必要です。

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
</runtime>
```

### <a name="restoring-legacy-behavior"></a>従来の動作に戻す

アプリケーションがバージョン 4.7.1 以降の .NET Framework をターゲットにしている場合、アプリケーションの構成ファイルの [`<AppContextSwitchOverrides>`](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) セクションの [`<runtime>`](../configure-apps/file-schema/runtime/index.md) 要素にスイッチを追加し、値を `true` に設定することで、アクセシビリティ機能を無効にできます。 たとえば、次の構成は、.NET Framework 4.7.2 で導入されたユーザー補助機能を無効にします。

```xml
<runtime>
    <!-- AppContextSwitchOverrides value attribute is in the form of 'key1=true|false;key2=true|false  -->
    <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures.2=true" />
</runtime>
```

## <a name="whats-new-in-accessibility-in-net-framework-48"></a>.NET Framework 4.8 のアクセシビリティの新機能

.NET Framework 4.8 には、次の領域の新しいユーザー補助機能が含まれています。

- [Windows フォーム](#winforms48)

- [Windows Presentation Foundation (WPF)](#wpf48)

- [Windows Workflow Foundation (WF) ワークフロー デザイナー](#wf48)

<a name="winforms48" />

### <a name="windows-forms"></a>Windows フォーム

.NET Framework 4.8 では、多くの一般的に使用されているコントロールに対する LiveRegion と通知イベントのサポートが Windows フォームで追加されました。 ユーザーがキーボードを使用してコントロールに移動するときのツールヒントのサポートも追加されました。

**Label と StatusStrip の UIA LiveRegion サポート**

アプリケーション開発者は UIA LiveRegion を使用することで、ユーザーが作業しているところからは離れた場所にあるコントロールのテキスト変更をスクリーン リーダーに通知することができます。 これは、たとえば接続状態を示す <xref:System.Windows.Forms.StatusStrip> コントロールなどで便利です。 接続が切断されて状態が変わった場合に、開発者はスクリーン リーダーに通知することができます。

.NET Framework 4.8 以降、Windows フォームでは <xref:System.Windows.Forms.Label> と <xref:System.Windows.Forms.StatusStrip> 両方のコントロールに対して UIA LiveRegion が実装されています。 たとえば、次のコードでは LiveRegion が <xref:System.Windows.Forms.Label>という名前の `label1` コントロールで使用されています。

```csharp
public Form1()
{
   InitializeComponent();
   label1.AutomationLiveSetting = AutomationLiveSetting.Polite;
}

…
Label1.Text = “Ready!”;
```

ユーザーがアプリケーションの操作中であるかどうかに関わらず、ナレーターにより "Ready" と読み上げられます。

<xref:System.Windows.Forms.UserControl> を LiveRegion として実装することもできます。

```csharp
using System;
using System.Windows.Forms;
using System.Windows.Forms.Automation;

namespace WindowsFormsApplication
{
   public partial class UserControl1 : UserControl, IAutomationLiveRegion
   {
      public UserControl1()
      {
         InitializeComponent();
      }

      public AutomationLiveSetting AutomationLiveSetting { get; set; }
      private AutomationLiveSetting IAutomationLiveRegion.GetLiveSetting()
      {
         return this.AutomationLiveSetting;
      }

      protected override void OnTextChanged(EventArgs e)
      {
         base.OnTextChanged(e);
         AutomationNotifications.UiaRaiseLiveRegionChangedEvent(this.AccessibilityObject);
      }
   }
}
```

**UIA 通知イベント**

Windows 10 Fall Creators Update で導入された UIA 通知イベントをアプリで使用すると、UIA イベントを発生させ、対応するコントロールを UI に準備することなく、イベント用に指定したテキストに基づいてナレーターに単純に読み上げさせることができます。 一部のシナリオでは、これはアプリのアクセシビリティを飛躍的に向上させる最も簡単な方法です。 長時間かかる可能性のあるいくつかのプロセスの進行状況を通知するのにも便利です。 UIA 通知イベントの詳細については、[デスクトップ アプリで新しい UI 通知イベントを使用する方法](https://docs.microsoft.com/archive/blogs/winuiautomation/can-your-desktop-app-leverage-the-new-uia-notification-event-in-order-to-have-narrator-say-exactly-what-your-customers-need)に関する記事を参照してください。

次の例では、[通知イベント](xref:System.Windows.Forms.AccessibleObject.RaiseAutomationNotification%2A)を発生させています。

```csharp
MethodInfo raiseMethod = typeof(AccessibleObject).GetMethod("RaiseAutomationNotification");
if (raiseMethod != null) {
   raiseMethod.Invoke(progressBar1.AccessibilityObject, new object[3] {/*Other*/ 4, /*All*/ 2, "The progress is 50%." });
}
```

**キーボード アクセスのツールヒント**

.NET Framework 4.7.2 以前のバージョンをターゲットとしたアプリケーションでは、[tooltip](xref:System.Windows.Forms.ToolTip) コントロールがポップ アップするようトリガーする唯一の方法は、マウス ポインターをそのコントロールに移動することでした。 .NET Framework 4.8 以降、キーボードを使用するユーザーは、Tab キーまたは方向キーを修飾キーと共に、または修飾キーなしで使用してコントロールにフォーカスすることで、コントロールのツールヒントをトリガーできます。 この特別なアクセシビリティの機能強化には、追加の [AppContext スイッチ](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)が必要となります。

```xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.6.1"/>
   </startup>
   <runtime>
      <!-- AppContextSwitchOverrides values are in the form of 'key1=true|false;key2=true|false  -->
      <!-- Please note that disabling Switch.UseLegacyAccessibilityFeatures, Switch.UseLegacyAccessibilityFeatures.2 and Switch.UseLegacyAccessibilityFeatures.3 is required to disable Switch.System.Windows.Forms.UseLegacyToolTipDisplay -->
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.System.Windows.Forms.UseLegacyToolTipDisplay=false"/>
   </runtime>
</configuration>
```

次の図は、ユーザーがキーボードを使用してボタンを選択したときのツールヒントを示しています。

![ユーザーがキーボードを使用してボタンに移動したときのツールヒントのスクリーンショット。](./media/whats-new-in-accessibility/select-tooltip-with-keyboard.png)

<a name="wpf48" />

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

.NET Framework 4.8 以降、WPF に数多くのアクセシビリティ機能改善が導入されました。

**可視性が Collapsed または Hidden の要素はスクリーン ナレーターで読み上げられない**

可視性が Collapsed または Hidden の要素は、スクリーン リーダーによって読み上げられなくなりました。 可視性が <xref:System.Windows.Visibility.Collapsed?displayProperty=nameWithType> または <xref:System.Windows.Visibility.Hidden?displayProperty=nameWithType> の要素を含むユーザー インターフェイスは、スクリーン リーダーによってユーザーに読み上げるときに誤って伝わる場合があります。 .NET Framework 4.8 以降、WPF では UIAutomation ツリーのコントロール ビューに Collapsed または Hidden の要素は含まれないため、スクリーン リーダーによってこれらの要素が読み上げられることは今後ありません。

**非装飾ベースのテキスト選択での SelectionTextBrush プロパティの使用**

.NET Framework 4.7.2 では、装飾層を使用せずに <xref:System.Windows.Controls.TextBox> と <xref:System.Windows.Controls.PasswordBox> テキスト選択を描画する機能が WPF に追加されました。 このシナリオでは、選択したテキストの前景色は <xref:System.Windows.SystemColors.HighlightTextBrush?displayProperty=nameWithType> により指定されています。

.NET Framework 4.8 で追加された新しいプロパティ `SelectionTextBrush` を使用すると、開発者は非装飾ベースのテキスト選択を使用する場合に、選択したテキストに対して特定のブラシを選ぶことができます。 このプロパティは、非装飾ベースのテキスト選択が有効な WPF アプリケーション内の <xref:System.Windows.Controls.Primitives.TextBoxBase> から派生したコントロールと <xref:System.Windows.Controls.PasswordBox> コントロールでのみ機能します。 <xref:System.Windows.Controls.RichTextBox> コントロールでは機能しません。 非装飾ベースのテキスト選択が有効ではない場合、このプロパティは無視されます。

このプロパティを使用するには、単に XAML コードに追加して、適切なブラシまたはバインドを使用します。 結果のテキスト選択は次のようになります。

![実行中のアプリのスクリーンショット。Hello World という言葉が選択されています。](./media/whats-new-in-accessibility/selectiontextbrush-property.png)

`SelectionBrush` と `SelectionTextBrush` プロパティを組み合わせて使用することで、背景色と前景色の適切な組み合わせを生成することができます。

**UIAutomation ControllerFor プロパティのサポート**

UIAutomation の `ControllerFor` プロパティは、このプロパティをサポートするオートメーション要素によって操作されるオートメーション要素の配列を返します。 このプロパティは通常、自動提案アクセシビリティで使用されます。 `ControllerFor`は、オートメーション要素がアプリケーション UI またはデスクトップの 1 つ以上のセグメントに影響する場合に使用されます。 それ以外の場合、コントロール操作の影響を UI 要素と関連付けることは困難です。 この機能により、コントロールで `ControllerFor`プロパティの値を指定できるようになります。

.NET Framework 4.8 では、新しい仮想メソッド <xref:System.Windows.Automation.Peers.AutomationPeer.GetControlledPeersCore?displayProperty=nameWithType?displayProperty=nameWithType> が追加されました。 `ControllerFor` プロパティの値を指定するには、単にこのメソッドをオーバーライドして、この `List<AutomationPeer>` で操作されているコントロールの <xref:System.Windows.Automation.Peers.AutomationPeer> を返します。

```csharp
public class AutoSuggestTextBox: TextBox
{
   protected override AutomationPeer OnCreateAutomationPeer()
   {
      return new AutoSuggestTextBoxAutomationPeer(this);
   }

   public ListBox SuggestionListBox;
}

internal class AutoSuggestTextBoxAutomationPeer : TextBoxAutomationPeer
{
   public AutoSuggestTextBoxAutomationPeer(AutoSuggestTextBox owner) : base(owner)
   {
   }

   protected override List<AutomationPeer> GetControlledPeersCore()
   {
      List<AutomationPeer> controlledPeers = new List<AutomationPeer>();
      AutoSuggestTextBox owner = Owner as AutoSuggestTextBox;
      controlledPeers.Add(UIElementAutomationPeer.CreatePeerForElement(owner.SuggestionListBox));
      return controlledPeers;
   }
}
```

**キーボード アクセスのツールヒント**

.NET Framework 4.7.2 以前のバージョンでは、ツールヒントはユーザーがマウス カーソルをコントロールの上に置いたときにのみ表示されます。 .NET Framework 4.8 では、ツールヒントはキーボード フォーカス上に、そしてキーボード ショートカットによっても表示されます。

この機能を使用するには、アプリケーションで .NET Framework 4.8 をターゲットとするか、または `Switch.UseLegacyAccessibilityFeatures.3` と `Switch.UseLegacyToolTipDisplay` [AppContext](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) スイッチを使用してオプトインする必要があります。 サンプルのアプリケーション構成ファイルを以下に示します。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false;Switch.UseLegacyToolTipDisplay=false" />
   </runtime>
</configuration>
```

一度有効にすると、ツールヒントを含むすべてのコントロールで、キーボード フォーカスをコントロールが受けるとツールヒントが表示されます。 ツールヒントは時間が経過したとき、またはキーボード フォーカスが変わったときに、消すことができます。 ユーザーは新しいキーボード ショートカット Ctrl + Shift + F10 を使用してツールヒントを手動で消すこともできます。 ツールヒントが消えたら、同じキーボード ショートカットを使用して再度表示することができます。

> [!NOTE]
> [ コントロール上の ](xref:System.Windows.Controls.Ribbon.RibbonToolTip)リボンのツールヒント<xref:System.Windows.Controls.Ribbon.Ribbon>は、キーボード フォーカスに表示されません。キーボード ショートカットでのみ表示されます。

**UIAutomation プロパティの SizeOfSet および PositionInSet のサポートを追加**

Windows 10 では 2 つの新しい UIAutomation プロパティである `SizeOfSet` と `PositionInSet` が導入されました。アプリケーションで使用してセット内の項目数を示すことができます。 スクリーン リーダーなどの UIAutomation クライアント アプリケーションは、これらのプロパティに関してアプリケーションに照会して、アプリケーションの UI の正確な表記を読み上げることができます。

.NET Framework 4.8 以降、WPF ではこれら 2 つのプロパティが WPF アプリケーションの UIAutomation に公開されています。 その方法は 2 つあります。

- 依存関係プロパティを使用。

  WPF では 2 つの新しい依存関係プロパティである <xref:System.Windows.Automation.AutomationProperties.SizeOfSet?displayProperty=nameWithType> と <xref:System.Windows.Automation.AutomationProperties.PositionInSet?displayProperty=nameWithType> が追加されました。 開発者は XAML を使用してその値を設定できます。

  ```xaml
  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="1">Button 1</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="2">Button 2</Button>

  <Button AutomationProperties.SizeOfSet="3"
    AutomationProperties.PositionInSet="3">Button 3</Button>
  ```

- AutomationPeer 仮想メソッドをオーバーライド。

  <xref:System.Windows.Automation.Peers.AutomationPeer.GetSizeOfSetCore> と <xref:System.Windows.Automation.Peers.AutomationPeer.GetPositionInSetCore> 仮想メソッドが、AutomationPeer クラスに追加されました。 次の例に示すように、開発者はこれらのメソッドをオーバーライドすることで、`SizeOfSet` と `PositionInSet` の値を指定できます。

  ```csharp
  public class MyButtonAutomationPeer : ButtonAutomationPeer
  {
    protected override int GetSizeOfSetCore()
    {
        // Call into your own logic to provide a value for SizeOfSet
        return CalculateSizeOfSet();
    }

    protected override int GetPositionInSetCore()
    {
        // Call into your own logic to provide a value for PositionInSet
        return CalculatePositionInSet();
    }
  }
  ```

さらに、<xref:System.Windows.Controls.ItemsControl> インスタンス内の項目により、これらのプロパティの値を自動的に、開発者による追加のアクションなしで指定することができます。 <xref:System.Windows.Controls.ItemsControl> がグループ化されている場合、グループのコレクションはセットとして表されます。各グループは別々のセットとしてカウントされ、グループ内の各項目ではグループ内の自身の位置とグループのサイズが示されます。 自動値は仮想化によって影響を受けません。 項目は実現されていない場合でも、セットの合計サイズに含まれ、その兄弟項目のセット内の位置に影響を与えます。

自動値は、アプリケーションが .NET Framework 4.8 をターゲットとしている場合のみ提供されます。 以前のバージョンの .NET Framework をターゲットとするアプリケーションの場合は、次の App.config ファイルに示すように、`Switch.UseLegacyAccessibilityFeatures.3` [AppContext スイッチ](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)を設定することができます。

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
   <startup>
      <supportedRuntime version="v4.0" sku=".NETFramework,Version=v4.5" />
   </startup>
   <runtime>
      <AppContextSwitchOverrides value="Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;Switch.UseLegacyAccessibilityFeatures.3=false" />
   </runtime>
</configuration>
```

<a name="wf48" />

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Windows Workflow Foundation (WF) ワークフロー デザイナー

.NET Framework 4.8 ではワークフロー デザイナーに次の変更が行われました。

- ナレーターを使用するユーザー向けの、FlowSwitch case ラベルの改善。

- ナレーターを使用するユーザー向けの、ボタン説明の改善。

- ハイ コントラスト テーマを選ぶユーザー向けの、要素間のコントラスト比の向上や、フォーカス要素に使われる選択ボックスの認識性の向上など、ワークフロー デザイナーとそのコントロールの表示に関する改善。

アプリケーションが .NET Framework 4.7.2 以前のバージョンをターゲットとしている場合、アプリケーション構成ファイルで `Switch.UseLegacyAccessibilityFeatures.3` [AppContext スイッチ](../configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md)を `false` に設定することで、これらの変更にオプトインすることができます。 詳細については、この記事の「[アクセシビリティ拡張機能の利用](#taking-advantage-of-accessibility-enhancements)」セクションを参照してください。

## <a name="whats-new-in-accessibility-in-net-framework-472"></a>.NET Framework 4.7.2 のアクセシビリティの新機能

.NET Framework 4.7.2 には、次の領域の新しいユーザー補助機能が含まれています。

- [Windows フォーム](#winforms472)

- [Windows Presentation Foundation (WPF)](#wpf472)

<a name="winforms472"></a>

### <a name="windows-forms"></a>Windows フォーム

**ハイ コントラスト テーマでの OS で定義された色**

.NET Framework 4.7.2 以降、Windows フォームは、オペレーティング システムで定義されている色をハイ コントラスト テーマで使用します。 これは、次のコントロールに影響します。

- <xref:System.Windows.Forms.ToolStripDropDownButton> コントロールのドロップダウン矢印。

- <xref:System.Windows.Forms.Button> が <xref:System.Windows.Forms.RadioButton> または <xref:System.Windows.Forms.CheckBox> に設定された、<xref:System.Windows.Forms.ButtonBase.FlatStyle>、<xref:System.Windows.Forms.FlatStyle.Flat?displayProperty=nameWithType>、および <xref:System.Windows.Forms.FlatStyle.Popup?displayProperty=nameWithType> コントロール。 以前は、選択されたテキストと背景の色のコントラストが同じで、見分けるのが困難でした。

- <xref:System.Windows.Forms.GroupBox> プロパティが <xref:System.Windows.Forms.Control.Enabled> に設定されている `false` に含まれるコントロール。

- <xref:System.Windows.Forms.ToolStripButton>、<xref:System.Windows.Forms.ToolStripComboBox>、<xref:System.Windows.Forms.ToolStripDropDownButton> コントロール。これらは、ハイ コントラスト モードでの明度コントラストの比率が高くなりました。

- <xref:System.Windows.Forms.DataGridViewLinkCell.LinkColor> の <xref:System.Windows.Forms.DataGridViewLinkCell> プロパティ。

**ナレーターの機能強化**

.NET Framework 4.7.2 以降では、ナレーターのサポートが次のように強化されました。

- <xref:System.Windows.Forms.ToolStripMenuItem.ShortcutKeys?displayProperty=nameWithType> のテキストを読み上げるときに、<xref:System.Windows.Forms.ToolStripMenuItem> プロパティの値を読み上げます。

- <xref:System.Windows.Forms.ToolStripMenuItem> の <xref:System.Windows.Forms.Control.Enabled> プロパティが `false` に設定されている場合、それを示します。

- <xref:System.Windows.Forms.ListView.CheckBoxes?displayProperty=nameWithType> プロパティが `true` に設定されているとき、チェック ボックスの状態に関するフィードバックを提供します。

- ナレーター スキャン モードのフォーカス順序が、ClickOnce ダウンロード ダイアログ ウィンドウでのコントロールの視覚的な順序と一致します。

**DataGridView の機能強化**

.NET Framework 4.7.2 以降、<xref:System.Windows.Forms.DataGridView> コントロールには次のアクセシビリティ機能改善が導入されました。

- キーボードを使って行を並べ替えることができます。 ユーザーは、F3 キーを使って現在の列で並べ替えることができます。

- <xref:System.Windows.Forms.DataGridView.SelectionMode?displayProperty=nameWithType> が <xref:System.Windows.Forms.DataGridViewSelectionMode.FullRowSelect?displayProperty=nameWithType> に設定されていると、ユーザーが現在の行のセルを Tab で移動するとき、列ヘッダーの色が変わって現在の列を示します。

- <xref:System.Windows.Forms.AccessibleObject.Parent?displayProperty=nameWithType> の <xref:System.Windows.Forms.DataGridViewLinkCell.DataGridViewLinkCellAccessibleObject?displayProperty=nameWithType> プロパティは、正しい親コントロールを返します。

**視覚的な手掛かりの向上**

- <xref:System.Windows.Forms.RadioButton> プロパティが空の <xref:System.Windows.Forms.CheckBox> および <xref:System.Windows.Forms.ButtonBase.Text> コントロールは、フォーカスを受け取るとフォーカス インジケーターを表示します。

**強化されたプロパティ グリッドのサポート**

- <xref:System.Windows.Forms.PropertyGrid> コントロールの子要素は、PropertyGrid 要素が有効になっている場合にのみ、`true` プロパティに対して <xref:System.Windows.Automation.ValuePattern.IsReadOnlyProperty> を返すようになりました。

- <xref:System.Windows.Forms.PropertyGrid> コントロールの子要素は、PropertyGrid 要素をユーザーが変更できる場合にのみ、`false` プロパティに対して <xref:System.Windows.Automation.AutomationElement.IsEnabledProperty> を返します。

**キーボード ナビゲーションの向上**

- <xref:System.Windows.Forms.ToolStripButton> コントロールは、<xref:System.Windows.Forms.ToolStripPanel> プロパティが <xref:System.Windows.Forms.ToolStripPanel.TabStop> に設定された `true` 内に含まれているときにフォーカスできます

<a name="wpf472"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**CheckBox および RadioButton コントロールに対する変更**

.NET Framework 4.7.1 以前のバージョンでは、WPF の <xref:System.Windows.Controls.CheckBox?displayProperty=nameWIthType> および <xref:System.Windows.Controls.RadioButton?displayProperty=nameWIthType> コントロールのフォーカス ビジュアルに整合性がなく、クラシック テーマとハイ コントラスト テーマでは正しくありません。  これらの問題は、コントロールに内容が何も設定されていない場合に発生します。  これにより、テーマ間の遷移が混乱し、フォーカス ビジュアルが見にくくなることがあります。

.NET Framework 4.7.2 では、これらのビジュアルのテーマ間の整合性が向上し、クラシック モードとハイ コントラスト テーマでの視認性がよくなっています。

**WPF アプリケーションでホストされる WinForms コントロール**

.NET Framework 4.7.1 以前のバージョンの WPF アプリケーションでホストされている WinForms コントロールでは、そのレイヤーの最初または最後のコントロールが WPF <xref:System.Windows.Forms.Integration.ElementHost> コントロールの場合、ユーザーは Tab キーで WinForms レイヤーから外に移動できませんでした。 .NET Framework 4.7.2 では、ユーザーは Tab キーで WinForms レイヤーから外に移動できます。

ただし、WinForms レイヤーをエスケープしないフォーカスに依存する自動化アプリケーションは、意図したように機能しなくなる可能性があります。

## <a name="whats-new-in-accessibility-in-net-framework-471"></a>.NET Framework 4.7.1 のアクセシビリティの新機能

.NET Framework 4.7.1 には、次の領域の新しいユーザー補助機能が含まれています。

- [Windows Presentation Foundation (WPF)](#wpf471)

- [Windows フォーム](#winforms471)

- [ASP.NET Web コントロール](#aspnet471)

- [.NET SDK Tools](#tools471)

- [Windows Workflow Foundation (WF) ワークフロー デザイナー](#wf471)

<a name="wpf471"></a>

### <a name="windows-presentation-foundation-wpf"></a>Windows Presentation Foundation (WPF)

**スクリーン リーダーの機能強化**

アクセシビリティ機能改善を有効にすると、.NET Framework 4.7.1 は次の面でスクリーン リーダーの機能を強化します。

- .NET Framework 4.7 以前のバージョンでは、<xref:System.Windows.Controls.Expander> コントロールはスクリーン リーダーによってボタンとして読み上げられました。 .NET Framework 4.7.1 以降では、展開と折りたたみが可能なグループとして正しく読み上げられます。

- .NET Framework 4.7 以前のバージョンでは、<xref:System.Windows.Controls.DataGridCell> コントロールはスクリーン リーダーによって "カスタム" として読み上げられました。 .NET Framework 4.7.1 以降では、データ グリッド セル (ローカライズされます) として正しく読み上げられます。

- .NET Framework 4.7.1 以降、スクリーン リーダーによって編集可能な <xref:System.Windows.Controls.ComboBox> の名前が読み上げられます。

- .NET Framework 4.7 以前のバージョンの場合、<xref:System.Windows.Controls.PasswordBox> コントロールが "ビューに項目がありません" として読み上げられたか、他の誤動作が発生しました。 この問題は .NET Framework 4.7.1 以降で修正されています。

**UIAutomation LiveRegion サポート**

ナレーターなどのスクリーン リーダーはアプリケーションの UI コンテンツをユーザーのために読み上げます。通常、フォーカスを合わせた UI コンテンツのテキストが読み上げられます。 ただし、UI 要素が変わり、フォーカスがなくなったとき、ユーザーに通知されず、重要な情報を逃すことがあります。 ライブ領域は、この問題の解決を目指しています。 開発者はこれを利用し、UI 要素が大幅に変更されたことをスクリーン リーダーやその他の UIAutomation クライアントに指示できます。 指示後、スクリーン リーダーでは、その変更をユーザーに通知する方法とタイミングが決定されます。

ライブ領域をサポートするために、次の API が WPF に追加されました。

- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveSettingProperty?displayProperty=nameWithType> フィールドと <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveRegionChangedEvent?displayProperty=nameWithType> フィールド。**LiveSetting** プロパティと **LiveRegionChanged** イベントを識別します。 XAML を利用して設定できます。

- **AutomationProperties.LiveSetting** 添付プロパティ。UI の変化がユーザーにとって重要であることをスクリーン リーダーに伝えます。

- <xref:System.Windows.Automation.AutomationProperties.LiveSettingProperty?displayProperty=nameWithType> プロパティ。**AutomationProperties.LiveSetting** 添付プロパティを識別します。

- <xref:System.Windows.Automation.Peers.AutomationPeer.GetLiveSettingCore%2A?displayProperty=nameWithType> メソッド。**LiveSetting** 値を提供するようにオーバーライドできます。

- <xref:System.Windows.Automation.AutomationProperties.GetLiveSetting%2A?displayProperty=nameWithType> メソッドと <xref:System.Windows.Automation.AutomationProperties.SetLiveSetting%2A?displayProperty=nameWithType> メソッド。**LiveSetting** 値を取得し、設定します。

- <xref:System.Windows.Automation.AutomationLiveSetting?displayProperty=nameWithType> 列挙。次の指定可能 **LiveSetting** 値を定義します。

  - [https://login.microsoftonline.com/consumers/](<xref:System.Windows.Automation.AutomationLiveSetting.Off?displayProperty=nameWithType>) ライブ領域の内容が変更された場合でも、要素が通知を送信することはありません。
  - [https://login.microsoftonline.com/consumers/](<xref:System.Windows.Automation.AutomationLiveSetting.Polite?displayProperty=nameWithType>) ライブ領域の内容が変更された場合、要素は非割り込み型の通知を送信します。

  - [https://login.microsoftonline.com/consumers/](<xref:System.Windows.Automation.AutomationLiveSetting.Assertive?displayProperty=nameWithType>) ライブ領域の内容が変更された場合、要素により割り込み通知が送信されます。

関心のある要素で **AutomationProperties.LiveSetting** プロパティを作成できます。次の例をご覧ください。

```xaml
<TextBlock Name="myTextBlock" AutomationProperties.LiveSetting="Assertive">announcement</TextBlock>
```

ライブ領域のデータが変化し、スクリーン リーダーに通知する必要があれば、次のサンプルのように、明示的にイベントを発生させます。

```csharp
var peer = FrameworkElementAutomationPeer.FromElement(myTextBlock);

peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged);
```

```vb
Dim peer = FrameworkElementAutomationPeer.FromElement(myTextBlock)
peer.RaiseAutomationEvent(AutomationEvents.LiveRegionChanged)

```

**ハイ コントラスト**

.NET Framework 4.7.1 以降、さまざまな WPF コントロールでハイ コントラストが強化されました。 <xref:System.Windows.SystemParameters.HighContrast%2A> テーマが設定されているときにこれを確認できます。 チェックの内容は次のとおりです

- <xref:System.Windows.Controls.Expander> コントロール

  フォーカスを合わせたとき、<xref:System.Windows.Controls.Expander> コントロールが見やすくなりました。 <xref:System.Windows.Controls.ComboBox>、<xref:System.Windows.Controls.ListBox>、<xref:System.Windows.Controls.RadioButton> コントロールのキーボードも見やすくなりました。 次に例を示します。

  次の処理の前

  ![展開コントロールのスクリーンショット。フォーカスありとフォーカスなしのビジュアル。](./media/whats-new-in-accessibility/expander-control-before.png)

  後:

  ![展開コントロールのスクリーンショット。コントロールのテキストが点線で囲まれています。](./media/whats-new-in-accessibility/expander-control-after.png)

- <xref:System.Windows.Controls.CheckBox> コントロールと <xref:System.Windows.Controls.RadioButton> コントロール

  <xref:System.Windows.Controls.CheckBox> コントロールと <xref:System.Windows.Controls.RadioButton> コントロールのテキストは、ハイ コントラスト テーマで選択されているとき、見やすくなりました。 次に例を示します。

  次の処理の前

  ![ラジオ ボタンとチェック ボタンのスクリーンショット。ハイ コントラストのテーマでテキストが読みにくくなっています。](./media/whats-new-in-accessibility/high-contrast-radio-button-before.png)

  後:

  ![ラジオ ボタンとチェック ボタンのスクリーンショット。ハイ コントラストのテーマでテキストが読みやすくなっています。](./media/whats-new-in-accessibility/high-contrast-radio-button-after.png)

- <xref:System.Windows.Controls.ComboBox> コントロール

  .NET Framework 4.7.1 以降、無効にした <xref:System.Windows.Controls.ComboBox> コントロールの枠線が無効にしたテキストと同じ色になります。 次に例を示します。

  次の処理の前

  ![無効になっている ComboBox のスクリーンショット。さまざまな色の罫線とコントロール テキスト。](./media/whats-new-in-accessibility/combo-disabled-before.png)

  後:

  ![無効になっている ComboBox のスクリーンショット。罫線とコントロール テキストが同じ色です。](./media/whats-new-in-accessibility/combo-disabled-after.png)

  また、無効にしているボタンにフォーカスを合わせたとき、正しいテーマ色が使用されます。

  次の処理の前

  ![黒色ボタンのスクリーンショット。"Focus Me" というテキストが灰色です。](./media/whats-new-in-accessibility/button-theme-colors-before.png)

  後:

  ![青色ボタンのスクリーンショット。"Focus Me" というテキストが黒色です。](./media/whats-new-in-accessibility/button-theme-colors-after.png)

  最後になりますが、.NET Framework 4.7 以前のバージョンでは、<xref:System.Windows.Controls.ComboBox> コントロールのスタイルを `Toolbar.ComboBoxStyleKey` に設定すると、ドロップダウンの矢印が見えなくなりました。 この問題は .NET Framework 4.7.1 以降で修正されています。 次に例を示します。

  次の処理の前

  ![ComboBox コントロールのスクリーンショット。ドロップダウンの矢印が見えません。](./media/whats-new-in-accessibility/combo-box-style-key-before.png)

  後:

  ![ComboBox コントロールのスクリーンショット。ドロップダウンの矢印が見えます。](./media/whats-new-in-accessibility/combo-box-style-key-after.png)

- <xref:System.Windows.Controls.DataGrid> コントロール

  .NET Framework 4.7.1 以降、<xref:System.Windows.Controls.DataGrid> コントロールの並べ替えインジケーターで正しいテーマ色が使われるようになりました。 次に例を示します。

  次の処理の前

  ![並べ替えインジケーター矢印のスクリーンショット (機能改善前)](./media/whats-new-in-accessibility/sort-indicator-before.png)

  後:

  ![並べ替えインジケーター矢印のスクリーンショット (機能改善後)](./media/whats-new-in-accessibility/sort-indicator-after.png)

  また、.NET Framework 4.7 以前のバージョンでは、ハイ コントラスト モードでカーソルを合わせたとき、既定のリンク スタイルが正しくない色に変化しました。 これは .NET Framework 4.7.1 以降で修正されています。 同様に、.NET Framework 4.7.1 以降、<xref:System.Windows.Controls.DataGrid> チェックボックス列でキーボード フォーカス フィードバックに既定の色が使用されます。

  次の処理の前

  ![リンクのスクリーンショット。"Click Me!" という文字を 赤色で確認できます。](./media/whats-new-in-accessibility/default-link-style-before.png)

  後:

  ![リンクのスクリーンショット。"Click Me!" という文字を 黄色で確認できます。](./media/whats-new-in-accessibility/default-link-style-after.png)

.NET Framework 4.7.1 での WPF アクセシビリティ機能改善の詳細については、「[WPF でのアクセシビリティの向上](../migration-guide/retargeting/4.7-4.7.1.md#accessibility-improvements-in-wpf)」を参照してください。

<a name="winforms471"></a>

### <a name="windows-forms-accessibility-improvements"></a>Windows フォームのアクセシビリティ機能改善

.NET Framework 4.7.1 では、Windows フォーム (WinForms) のアクセシビリティが次の領域で変更されました。

**ハイ コントラスト モードの表示が改善されました**

.NET Framework 4.7.1 以降では、オペレーティング システムで利用できるハイ コントラスト モードを選択したとき、さまざまな WinForms コントロールの表示が改善されました。 Windows 10 では、ハイ コントラストのシステム色の一部で値が変わりました。Windows フォームは Windows 10 Win32 フレームワークに基づきます。 最も快適に利用するためには、最新版の Windows で実行し、テスト アプリケーションに app.manifest ファイルを追加して最新の OS 変更を選択し、次の画像のように Windows 10 対応 OS 行のコメントを解除します。

```xml
<!-- Windows 10 -->
<supportedOS Id=”{8e0f7a12-bfb3-4fe8-b9a5-48fd50a15a9a}” />
```

ハイ コントラストの変更例:

- <xref:System.Windows.Forms.MenuStrip> 項目のチェックマークが見やすくなりました。

- 選択したとき、無効になっている <xref:System.Windows.Forms.MenuStrip> 項目が見やすくなりました。

- 選択した <xref:System.Windows.Forms.Button> コントロールのテキストが背景色に対して際立ち、見やすくなりました。

- 無効になっているテキストが読みやすくなりました。 次に例を示します。

  次の処理の前

  ![ハイ コントラスト モードでさまざまなコントロールを使用するアプリのスクリーンショット (アクセシビリティ前)。](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-before.png)

  後:

  ![ハイ コントラスト モードでさまざまなコントロールを使用するアプリのスクリーンショット (アクセシビリティ後)。](./media/whats-new-in-accessibility/high-contrast-mode-menu-items-after.png)

- スレッド例外ダイアログのハイ コントラストが改善されました。

**ナレーター サポートが改善されました**

.NET Framework 4.7.1 の Windows フォームでは、ナレーターのアクセシビリティが次の面で改善されました。

- <xref:System.Windows.Forms.MonthCalendar> コントロールに、他の UI 自動化ツールと同様に、ナレーターからアクセスできます。

- <xref:System.Windows.Forms.CheckedListBox> コントロールは、項目のチェック状態が変わったとき、ナレーターに通知します。その結果、リスト アイテムの値を変えたことがユーザーに通知されます。

- <xref:System.Windows.Forms.DataGridViewCell> コントロールは正しい読み取り専用状態をナレーターに報告します。

- ナレーターが無効になっている <xref:System.Windows.Forms.ToolStripMenuItem> テキストを読み上げるようになりました。以前は、無効になっているメニュー項目はスキップされました。

**UIAutomation アクセシビリティ パターンのサポートが強化されました**

.NET Framework 4.7.1 以降、アクセシビリティ技術ツールの開発者は、一部の WinForms コントロールについて、API アクセシビリティの共通のパターンとプロパティを活用できます。 アクセシビリティ機能改善の例:

- <xref:System.Windows.Forms.ComboBox> と <xref:System.Windows.Forms.ToolStripSplitButton> が[展開/折りたたみパターン](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)対応になりました。

- <xref:System.Windows.Forms.DataGridViewCheckBoxCell> が[切り替えパターン](../ui-automation/implementing-the-ui-automation-toggle-control-pattern.md)対応になりました。

- <xref:System.Windows.Forms.ToolStripItem> コントロールが <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name> プロパティと[展開/折りたたみパターン](../ui-automation/implementing-the-ui-automation-expandcollapse-control-pattern.md)対応になりました。

- <xref:System.Windows.Forms.NumericUpDown> コントロールと <xref:System.Windows.Forms.DomainUpDown> コントロールが <xref:System.Windows.Automation.AutomationElement.AutomationElementInformation.Name> プロパティ対応になりました。

**プロパティ ブラウザーが使いやすくなりました**

.NET Framework 4.7.1 以降、Windows フォームは次の点で改善されました。

- さまざまなドロップダウン選択ウィンドウを利用することでキーボード操作が便利になりました。
- 不要なタブ ストップが減りました。
- コントロールの種類の報告機能が改善されました。
- ナレーターの動作が改善されました。

<a name="aspnet471"></a>

### <a name="aspnet-web-controls"></a>ASP.NET Web コントロール

.NET Framework 4.7.1 および Visual Studio 2017 バージョン 15.3 以降では、ASP.NET によって、ASP.NET Web コントロールによる Visual Studio のアクセシビリティ テクノロジの処理方法が向上しています。 主な変更点は以下のとおりです。

- **[詳細の表示]** ウィザードの **[フィールドの追加]** ダイアログや **ListView** ウィザードの **[ListView の構成]** ダイアログなど、コントロールで不足していた UI アクセシビリティ パターンを実装するための変更。

- **データ ページャー フィールド エディター**など、ハイ コントラスト モードでの表示を改善するための変更。

- DataPager コントロールの **[ページャーのフィールドを編集]** ウィザードの **[フィールド]** ダイアログ、 **[ObjectContext の構成]** ダイアログ、 **[データ ソースの構成]** ウィザードの **[データの選択の構成]** ダイアログなど、キーボードの操作性を改善するための変更。

<a name="tools471"></a>

### <a name="net-sdk-tools"></a>.NET SDK Tools

[構成エディター ツール (SvcConfigEditor.exe)](../wcf/configuration-editor-tool-svcconfigeditor-exe.md) および[サービス トレース ビューアー ツール (SvcTraceViewer.exe)](../wcf/service-trace-viewer-tool-svctraceviewer-exe.md) が、さまざまなアクセシビリティの問題を修正して改善されています。 その問題のほとんどは、名前の未定義や、特定の UI の自動化パターンが正しく実装されていないといった軽微なものです。 このような問題は、多くのユーザーが認識しないものですが、スクリーン リーダーのような支援技術を使用するお客様はこれらの SDK ツールのアクセシビリティの強化を実感されるでしょう。

これらの機能強化により、キーボード フォーカスの順序など、以前のいくつかの動作が変更されます。

<a name="wf471"></a>

### <a name="windows-workflow-foundation-wf-workflow-designer"></a>Windows Workflow Foundation (WF) ワークフロー デザイナー

ワークフロー デザイナーでのアクセシビリティに関する変更は次のとおりです。

- 一部のコントロールで、タブ オーダーが左から右、上から下に変更されます。

  - <xref:System.ServiceModel.Activities.InitializeCorrelation> アクティビティの関連付けデータを設定するための関連付けの初期化ウィンドウ。

  - <xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply>、および <xref:System.ServiceModel.Activities.ReceiveReply> アクティビティのコンテンツ定義ウィンドウ。

- キーボードで利用できる機能が増えます:

  - アクティビティのプロパティを編集する際、プロパティ グループに初めてフォーカスを設定したときに、プロパティ グループをキーボードで折りたたむことができます。

  - 警告アイコンにキーボードでアクセスできます。

  - **[プロパティ]** ウィンドウの **[その他のプロパティ]** ボタンに、キーボードでアクセスできます。

  - キーボードを使って、ワークフロー デザイナーの **[引数]** および **[変数]** ウィンドウのヘッダー項目にアクセスできます。

- 次のような場合に、フォーカスのある項目の可視性が向上しました:

  - ワークフロー デザイナーおよびアクティビティ デザイナーで使われているデータ グリッドへの行の追加。

  - <xref:System.ServiceModel.Activities.ReceiveReply> および <xref:System.ServiceModel.Activities.SendReply> アクティビティ内のフィールド間の Tab 移動。

  - 変数または引数の既定値の設定

- スクリーン リーダーが正しく認識できるようになりました:

  - ワークフロー デザイナーで設定されたブレークポイント。

  - <xref:System.Activities.Statements.FlowSwitch%601>、<xref:System.Activities.Statements.FlowDecision>、<xref:System.ServiceModel.Activities.CorrelationScope> アクティビティ。
  - <xref:System.ServiceModel.Activities.Receive> アクティビティの内容。

  - <xref:System.Activities.Statements.InvokeMethod> アクティビティのターゲットの種類。

  - <xref:System.Activities.Statements.TryCatch> アクティビティの [例外] コンボ ボックスと [Finally] セクション。

  - メッセージング アクティビティの [メッセージの種類] コンボ ボックス、[関連付け初期化子の追加] ウィンドウのスプリッター、[コンテンツ定義] ウィンドウで、および [CorrelatesOn の定義] ウィンドウ (<xref:System.ServiceModel.Activities.Receive>、<xref:System.ServiceModel.Activities.Send>、<xref:System.ServiceModel.Activities.SendReply>、および <xref:System.ServiceModel.Activities.ReceiveReply>)。

  - ステート マシンの遷移と遷移先。

  - <xref:System.Activities.Statements.FlowDecision> アクティビティの注釈とコネクタ。

  - アクティビティのコンテキスト (右クリック) メニュー。

  - プロパティ グリッドの、プロパティ値エディター、[検索のクリア] ボタン、[カテゴリ別] および [アルファベット順] の並べ替えボタン、[式エディター] ダイアログ。

  - ワークフロー デザイナーのズーム パーセンテージ。

  - <xref:System.Activities.Statements.Parallel> および <xref:System.Activities.Statements.Pick> アクティビティの区切り記号。

  - <xref:System.Activities.Statements.InvokeDelegate> アクティビティ。

  - ディクショナリ アクティビティの [型の選択] ウィンドウ (`Microsoft.Activities.AddToDictionary<TKey,TValue>`、`Microsoft.Activities.RemoveFromDictionary<TKey,TValue>` など)。

  - [.NET 型の参照と選択] ウィンドウ。

  - ワークフロー デザイナーでの階層リンク。

- ハイ コントラスト テーマを選ぶと、要素間のコントラスト比の向上や、フォーカス要素に使われる選択ボックスの認識性の向上など、ワークフロー デザイナーとそのコントロールの表示について多くの点が向上していることがわかります。

## <a name="see-also"></a>参照

- [.NET Framework の新機能](index.md)
