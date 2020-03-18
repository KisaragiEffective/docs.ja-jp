---
title: デザイン時にカスタムコントロールをデバッグする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- debugging [Visual Studio], walkthroughs
- custom controls [Windows Forms], walkthroughs
- visual editors
- debugging [Visual Studio], Windows Forms
- custom controls [Windows Forms], debugging
- designers
- controls [Windows Forms], debugging
- walkthroughs [Windows Forms], debugging
- design-time debugging
ms.assetid: 1fd83ccd-3798-42fc-85a3-6cba99467387
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: d9e292a1219c24571bcb35db2fe357b0197c8812
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740184"
---
# <a name="walkthrough-debug-custom-windows-forms-controls-at-design-time"></a>チュートリアル: デザイン時にカスタム Windows フォームコントロールをデバッグする

カスタムコントロールを作成すると、多くの場合、デザイン時の動作をデバッグする必要があることがわかります。 これは、カスタムコントロールのカスタムデザイナーを作成する場合に特に当てはまります。 詳細については、「[チュートリアル: Visual Studio のデザイン時機能を利用する Windows フォームコントロールの作成](creating-a-wf-control-design-time-features.md)」を参照してください。

他の .NET Framework クラスをデバッグする場合と同様に、Visual Studio を使用してカスタムコントロールをデバッグできます。 違いは、カスタムコントロールのコードを実行している Visual Studio の別のインスタンスをデバッグすることです。

## <a name="create-the-project"></a>プロジェクトを作成する

最初にアプリケーションのプロジェクトを作成します。 このプロジェクトは、カスタムコントロールをホストするアプリケーションをビルドするために使用します。

Visual Studio で、Windows アプリケーションプロジェクトを作成し、デバッグの**例**として名前を指定します。

## <a name="create-the-control-library-project"></a>コントロールライブラリプロジェクトを作成する

1. ソリューションに**Windows コントロールライブラリ**プロジェクトを追加します。

2. DebugControlLibrary プロジェクトに新しい**UserControl**項目を追加します。 **Debugcontrol**という名前を指定します。

3. **ソリューションエクスプローラー**で、ベース名が UserControl1 のコードファイルを削除して、プロジェクトの既定のコントロールを削除します。

4. ソリューションをビルドします。

## <a name="checkpoint"></a>Checkpoint

この時点で、**ツールボックス**にカスタムコントロールが表示されます。

進行状況を確認するには、 **Debugcontrollibrary Components**という名前の新しいタブを探し、クリックして選択します。 開いたときに、コントロールが**Debugcontrol**として表示され、その横に既定のアイコンが表示されます。

## <a name="add-a-property-to-your-custom-control"></a>カスタムコントロールにプロパティを追加する

カスタムコントロールのコードがデザイン時に実行されていることを示すために、プロパティを追加し、プロパティを実装するコードにブレークポイントを設定します。

1. **コードエディター**で**debugcontrol**を開きます。 クラス定義に次のコードを追加します。

    ```vb
    Private demoStringValue As String = Nothing
    <BrowsableAttribute(true)>
    Public Property DemoString() As String

        Get
            Return Me.demoStringValue
        End Get

        Set(ByVal value As String)
            Me.demoStringValue = value
        End Set

    End Property
    ```

    ```csharp
    private string demoStringValue = null;
    [Browsable(true)]
    public string DemoString
    {
        get
        {
            return this.demoStringValue;
        }
        set
        {
            demoStringValue = value;
        }
    }
    ```

2. ソリューションをビルドします。

## <a name="add-your-custom-control-to-the-host-form"></a>カスタムコントロールをホストフォームに追加する

カスタムコントロールのデザイン時動作をデバッグするには、カスタムコントロールクラスのインスタンスをホストフォームに配置します。

1. "デバッグの例" プロジェクトで、 **Windows フォームデザイナー**で Form1 を開きます。

2. **[ツールボックス]** で **[Debugcontrollibrary コンポーネント]** タブを開き、 **debugcontrol**インスタンスをフォームにドラッグします。

3. **[プロパティ]** ウィンドウで `DemoString` カスタムプロパティを見つけます。 他のプロパティと同様に、値を変更することができます。 また、`DemoString` プロパティが選択されている場合は、プロパティの説明文字列が **[プロパティ]** ウィンドウの下部に表示されることにも注意してください。

## <a name="set-up-the-project-for-design-time-debugging"></a>デザイン時デバッグ用にプロジェクトを設定する

カスタムコントロールのデザイン時動作をデバッグするには、カスタムコントロールのコードを実行している Visual Studio の別のインスタンスをデバッグします。

1. **ソリューションエクスプローラー**で**debugcontrollibrary**プロジェクトを右クリックし、 **[プロパティ]** を選択します。

2. **Debugcontrollibrary**プロパティシートで、 **[デバッグ]** タブを選択します。

     **[開始アクション]** セクションで、 **[外部プログラムの開始]** を選択します。 Visual Studio の別のインスタンスをデバッグするには、省略記号![([Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))] ボタンをクリックして、Visual Studio IDE を参照します。 実行可能ファイルの名前は**devenv.exe**で、を既定の場所にインストールした場合、そのパスは *% ProgramFiles (x86)% \ Microsoft Visual Studio\2019\\\<edition > \Common7\IDE*になります。

3. **[OK]** を選択してダイアログ ボックスを閉じます。

4. **Debugcontrollibrary**プロジェクトを右クリックし、 **[スタートアッププロジェクトに設定]** を選択して、このデバッグ構成を有効にします。

## <a name="debug-your-custom-control-at-design-time"></a>デザイン時にカスタムコントロールをデバッグする

これで、デザインモードで実行されるカスタムコントロールをデバッグする準備ができました。 デバッグセッションを開始すると、Visual Studio の新しいインスタンスが作成され、それを使用して "デバッグの例" ソリューションが読み込まれます。 **フォームデザイナー**で Form1 を開くと、カスタムコントロールのインスタンスが作成され、実行が開始されます。

1. **コードエディター**で**debugcontrol**ソースファイルを開き、`DemoString` プロパティの `Set` アクセサーにブレークポイントを設定します。

2. **F5**キーを押してデバッグセッションを開始します。 Visual Studio の新しいインスタンスが作成されます。 インスタンスは、次の2つの方法で区別できます。

    - デバッグインスタンスのタイトルバーで、という単語が**実行さ**れています。

    - デバッグインスタンスの**デバッグ**ツールバーに **[開始]** ボタンが無効になっている

   ブレークポイントは、デバッグインスタンスで設定されます。

3. Visual Studio の新しいインスタンスで、"デバッグの例" ソリューションを開きます。 **[ファイル]** メニューから **[最近使ったプロジェクト]** を選択すると、ソリューションを簡単に見つけることができます。 "デバッグ例 .sln" ソリューションファイルは、最近使用したファイルとして一覧表示されます。

4. **フォームデザイナー**で Form1 を開き、 **debugcontrol**コントロールを選択します。

5. `DemoString` プロパティの値を変更します。 変更をコミットすると、Visual Studio のデバッグインスタンスがフォーカスを取得し、ブレークポイントで実行が停止します。 他のコードと同じように、プロパティアクセサーを1ステップずつ実行できます。

6. デバッグを停止するには、Visual Studio のホストされたインスタンスを終了するか、デバッグインスタンスで **[デバッグの停止]** ボタンを選択します。

## <a name="next-steps"></a>次のステップ

デザイン時にカスタムコントロールをデバッグできるようになったので、Visual Studio IDE とのコントロールの相互作用を拡張する多くの可能性があります。

- <xref:System.ComponentModel.Component> クラスの <xref:System.ComponentModel.Component.DesignMode%2A> プロパティを使用して、デザイン時にのみ実行されるコードを記述できます。 詳細については、<xref:System.ComponentModel.Component.DesignMode%2A> を参照してください。

- コントロールのプロパティには、デザイナーとのカスタムコントロールの対話を操作するために適用できる属性がいくつかあります。 これらの属性は、<xref:System.ComponentModel?displayProperty=nameWithType> 名前空間にあります。

- カスタムコントロールのカスタムデザイナーを作成できます。 これにより、Visual Studio によって公開される拡張可能なデザイナーインフラストラクチャを使用して、デザインエクスペリエンスを完全に制御できます。 詳細については、「[チュートリアル: Visual Studio のデザイン時機能を利用する Windows フォームコントロールの作成](creating-a-wf-control-design-time-features.md)」を参照してください。

## <a name="see-also"></a>参照

- [チュートリアル: Visual Studio のデザイン時機能を活用した Windows フォーム コントロールの作成](creating-a-wf-control-design-time-features.md)
