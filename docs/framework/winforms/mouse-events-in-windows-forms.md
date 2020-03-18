---
title: マウス イベント
ms.date: 03/30/2017
helpviewer_keywords:
- MouseLeave event [Windows Forms]
- events [Windows Forms], mouse
- Click event [Windows Forms], raising order
- Windows Forms controls, click events
- MouseDoubleClick event [Windows Forms]
- MouseEnter event [Windows Forms]
- MouseHover event [Windows Forms]
- MouseDown event [Windows Forms]
- MouseClick event [Windows Forms]
- MouseMove event [Windows Forms]
- mouse [Windows Forms], events
- MouseUp event
ms.assetid: 8cf0070d-793b-4876-b09e-d20d28280fab
ms.openlocfilehash: 4909f56fc3935848fd18bc35c1cb56b5407a24c8
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76740967"
---
# <a name="mouse-events-in-windows-forms"></a>Windows フォームにおけるマウス イベント

マウス入力を処理する場合、通常はマウスのポインターの位置とマウス ボタンの状態を確認しようとします。 このトピックでは、マウスのイベントからこの情報を取得する方法について詳しく説明し、Windows フォーム コントロールでマウス クリック イベントが発生する順序について説明します。 すべてのマウスイベントの一覧と説明については、「 [Windows フォームでのマウス入力のしくみ](how-mouse-input-works-in-windows-forms.md)」を参照してください。  「[イベントハンドラーの概要」 (Windows フォーム)](event-handlers-overview-windows-forms.md)と「イベントの[概要」 (Windows フォーム)](events-overview-windows-forms.md)も参照してください。

## <a name="mouse-information"></a>マウスの情報

<xref:System.Windows.Forms.MouseEventArgs> は、マウス ボタンのクリック、およびマウスの動きの追跡に関連するマウス イベントのハンドラーに送信します。 <xref:System.Windows.Forms.MouseEventArgs> は、マウスのボタンが押された、およびマウスのホイールがスクロールされたといった、クライアント座標のマウス ポインターの場所を含む、マウスの現在の状態に関する情報を提供します。 マウス ポインターがコントロールの境界内に入った、または境界から出たときの通知など、いくつかのマウスイベントは、それ以上の情報はなしで <xref:System.EventArgs> をイベント ハンドラーに送信します。

マウス ボタン、または、マウス ポインターの位置の現在の状態を確認して、マウス イベントの処理を回避する場合は、<xref:System.Windows.Forms.Control.MouseButtons%2A> クラスの <xref:System.Windows.Forms.Control.MousePosition%2A> プロパティと <xref:System.Windows.Forms.Control> プロパティも使用できます。 <xref:System.Windows.Forms.Control.MouseButtons%2A> は現在押されているマウス ボタンに関する情報を返します。 <xref:System.Windows.Forms.Control.MousePosition%2A> はマウス ポインターの画面の座標を返しますが、<xref:System.Windows.Forms.Cursor.Position%2A> によって返される値と同じです。

## <a name="converting-between-screen-and-client-coordinates"></a>画面の座標とクライアント座標の間の変換

マウスの位置情報は、クライアント座標の場合と画面の座標の場合があるため、ポインターの座標システムの変換が必要になることがあります。 これは、<xref:System.Windows.Forms.Control.PointToClient%2A> クラスで利用できる <xref:System.Windows.Forms.Control.PointToScreen%2A> メソッドと <xref:System.Windows.Forms.Control> メソッドを使用して簡単に実行できます。

## <a name="standard-click-event-behavior"></a>標準のクリック イベントの動作

マウスのクリック イベントを適切な順序で処理する場合は、Windows フォーム コントロールでクリック イベントが発生した順序を知る必要があります。 Windows フォームのコントロールは、すべて、(どちらのマウス ボタンかは関係なく) マウス ボタンを押したときと離したときに同じ順序でクリック イベントを発生させますが、各コントロールについて、次のリストに記載する例外があります。 マウス ボタンのシングル クリックに対して発生したイベントの順序を次に示します。

1. <xref:System.Windows.Forms.Control.MouseDown> イベント。

2. <xref:System.Windows.Forms.Control.Click> イベント。

3. <xref:System.Windows.Forms.Control.MouseClick> イベント。

4. <xref:System.Windows.Forms.Control.MouseUp> イベント。

マウスボタンをダブルクリックしたときに発生するイベントの順序は次のとおりです。

1. <xref:System.Windows.Forms.Control.MouseDown> イベント。

2. <xref:System.Windows.Forms.Control.Click> イベント。

3. <xref:System.Windows.Forms.Control.MouseClick> イベント。

4. <xref:System.Windows.Forms.Control.MouseUp> イベント。

5. <xref:System.Windows.Forms.Control.MouseDown> イベント。

6. <xref:System.Windows.Forms.Control.DoubleClick> イベント。 (これは、問題のコントロールで <xref:System.Windows.Forms.ControlStyles.StandardDoubleClick> スタイルのビットが `true` に設定されているかどうかに応じて異なる可能性があります。 <xref:System.Windows.Forms.ControlStyles> のビットの設定方法の詳細については、<xref:System.Windows.Forms.Control.SetStyle%2A> メソッドを参照してください。)

7. <xref:System.Windows.Forms.Control.MouseDoubleClick> イベント。

8. <xref:System.Windows.Forms.Control.MouseUp> イベント。

マウスクリックイベントの順序を示すコード例については、「[方法: Windows フォームコントロールでユーザー入力イベントを処理する](how-to-handle-user-input-events-in-windows-forms-controls.md)」を参照してください。

### <a name="individual-controls"></a>個別のコントロール

次のコントロールは、標準のマウス クリック イベントの動作に準拠していません。

- <xref:System.Windows.Forms.Button>
- <xref:System.Windows.Forms.CheckBox>
- <xref:System.Windows.Forms.ComboBox>
- <xref:System.Windows.Forms.RadioButton>

  > [!NOTE]
  > <xref:System.Windows.Forms.ComboBox> コントロールについて、ユーザーが編集フィールド、ボタン、またはリスト内の項目をクリックすると、後述するイベントの動作が発生します。

  - 左クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 右クリック : クリック イベントは発生しません

  - 左ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 右ダブルクリック : クリック イベントは発生しません

- <xref:System.Windows.Forms.TextBox>、<xref:System.Windows.Forms.RichTextBox>、<xref:System.Windows.Forms.ListBox>、<xref:System.Windows.Forms.MaskedTextBox> および <xref:System.Windows.Forms.CheckedListBox> の各コントロール

  > [!NOTE]
  > ユーザーがこれらのコントロール内で任意の場所をクリックすると、後述するイベントの動作が発生します。

  - 左クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 右クリック : クリック イベントは発生しません

  - 左ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.DoubleClick>、<xref:System.Windows.Forms.Control.MouseDoubleClick>

  - 右ダブルクリック : クリック イベントは発生しません

- <xref:System.Windows.Forms.ListView> コントロール

  > [!NOTE]
  > ユーザーが <xref:System.Windows.Forms.ListView> コントロールの項目をクリックした場合のみ、後述するイベントの動作が発生します。 コントロールのその他の場所をクリックした場合、イベントは発生しません。 後述するイベントに加えて、<xref:System.Windows.Forms.ListView.BeforeLabelEdit> と <xref:System.Windows.Forms.ListView.AfterLabelEdit> のイベントがあり、<xref:System.Windows.Forms.ListView> コントロールによる検証を使用する場合のためにまとめておきます。

  - 左クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 右クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 左ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.DoubleClick>、<xref:System.Windows.Forms.Control.MouseDoubleClick>

  - 右ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.DoubleClick>、<xref:System.Windows.Forms.Control.MouseDoubleClick>

- <xref:System.Windows.Forms.TreeView> コントロール

  > [!NOTE]
  > ユーザーが項目自体をクリックするか、<xref:System.Windows.Forms.TreeView> コントロールの項目の右側をクリックした場合にのみ、後述するイベントの動作が発生します。 コントロールのその他の場所をクリックした場合、イベントは発生しません。 後述するイベントに加えて、<xref:System.Windows.Forms.TreeView.BeforeCheck>、<xref:System.Windows.Forms.TreeView.BeforeSelect>、<xref:System.Windows.Forms.TreeView.BeforeLabelEdit>、<xref:System.Windows.Forms.TreeView.AfterSelect>、<xref:System.Windows.Forms.TreeView.AfterCheck>、および <xref:System.Windows.Forms.TreeView.AfterLabelEdit> の各イベントがあり、<xref:System.Windows.Forms.TreeView> コントロールを持つ検証を使用する場合のためにまとめておきます。

  - 左クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 右クリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>

  - 左ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.DoubleClick>、<xref:System.Windows.Forms.Control.MouseDoubleClick>

  - 右ダブルクリック : <xref:System.Windows.Forms.Control.Click>、<xref:System.Windows.Forms.Control.MouseClick>、<xref:System.Windows.Forms.Control.DoubleClick>、<xref:System.Windows.Forms.Control.MouseDoubleClick>

### <a name="painting-behavior-of-toggle-controls"></a>トグルコントロールの描画動作

<xref:System.Windows.Forms.ButtonBase> クラスから派生するコントロールなど、切り替えコントロールには、次のようなマウス クリック イベントと組み合わせた独自の描画の動作があります。

1. ユーザーがマウス ボタンを押します。

2. 押された状態で、コントロールが描画します。

3. <xref:System.Windows.Forms.Control.MouseDown> イベントが発生します。

4. ユーザーがマウス ボタンを離します。

5. 離した状態でコントロールが描画します。

6. <xref:System.Windows.Forms.Control.Click> イベントが発生します。

7. <xref:System.Windows.Forms.Control.MouseClick> イベントが発生します。

8. <xref:System.Windows.Forms.Control.MouseUp> イベントが発生します。

    > [!NOTE]
    > マウス ボタンが押されているときにユーザーがポインターを切り替えコントロールの外に移動した (例 : <xref:System.Windows.Forms.Button> コントロールを押しているときにマウスを移動した) 場合、離された状態で切り替えコントロールが描画し、<xref:System.Windows.Forms.Control.MouseUp> イベントのみが発生します。 <xref:System.Windows.Forms.Control.Click> イベントまたは <xref:System.Windows.Forms.Control.MouseClick> イベントは、このような状況では発生しません。

## <a name="see-also"></a>参照

- [Windows フォーム アプリケーションにおけるマウス入力](mouse-input-in-a-windows-forms-application.md)
