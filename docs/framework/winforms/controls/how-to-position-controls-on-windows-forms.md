---
title: コントロールを配置する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
f1_keywords:
- Location
- Location.Y
- Location.X
helpviewer_keywords:
- controls [Windows Forms]
- controls [Windows Forms], moving
- snaplines
- controls [Windows Forms], positioning
ms.assetid: 4693977e-34a4-4f19-8221-68c3120c2b2b
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 144a0021c74f0fb5afec1d511315168fb28528e9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76735913"
---
# <a name="how-to-position-controls-on-windows-forms"></a>方法: Windows フォームにコントロールを配置する

コントロールを配置するには、Visual Studio で Windows フォームデザイナーを使用するか、<xref:System.Windows.Forms.Control.Location%2A> プロパティを指定します。

## <a name="position-a-control-on-the-design-surface-of-the-windows-forms-designer"></a>Windows フォームデザイナーのデザインサーフェイスにコントロールを配置する

Visual Studio で、マウスを使用して適切な位置にコントロールをドラッグします。

> [!NOTE]
> コントロールを選択し、方向キーを使用して移動し、より正確に配置します。 また、*スナップ線*を使用すると、フォームにコントロールを正確に配置できます。 詳細については、「[チュートリアル: スナップ線を使用した Windows フォームでのコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)」を参照してください。

## <a name="position-a-control-using-the-properties-window"></a>プロパティウィンドウを使用してコントロールを配置する

1. Visual Studio で、配置するコントロールを選択します。

2. **[プロパティ]** ウィンドウで、<xref:System.Windows.Forms.Control.Location%2A> プロパティの値をコンマで区切って入力し、そのコンテナー内にコントロールを配置します。

   最初の数値 (X) は、コンテナーの左境界線からの距離です。2番目の数値 (Y) は、コンテナー領域の上境界からの距離をピクセル単位で表したものです。

   > [!NOTE]
   > <xref:System.Windows.Forms.Control.Location%2A> プロパティを展開して、 **X**と**Y**の値を個別に入力できます。

## <a name="position-a-control-programmatically"></a>プログラムによるコントロールの配置

1. コントロールの <xref:System.Windows.Forms.Control.Location%2A> プロパティを <xref:System.Drawing.Point>に設定します。

    ```vb
    Button1.Location = New Point(100, 100)
    ```

    ```csharp
    button1.Location = new Point(100, 100);
    ```

    ```cpp
    button1->Location = Point(100, 100);
    ```

2. <xref:System.Windows.Forms.Control.Left%2A> サブプロパティを使用して、コントロールの位置の X 座標を変更します。

    ```vb
    Button1.Left = 300
    ```

    ```csharp
    button1.Left = 300;
    ```

    ```cpp
    button1->Left = 300;
    ```

## <a name="increment-a-controls-location-programmatically"></a>プログラムによってコントロールの位置をインクリメントする

<xref:System.Windows.Forms.Control.Left%2A> サブプロパティを設定して、コントロールの X 座標をインクリメントします。

```vb
Button1.Left += 200
```

```csharp
button1.Left += 200;
```

```cpp
button1->Left += 200;
```

> [!NOTE]
> コントロールの X 位置と Y 位置を同時に設定するには、<xref:System.Windows.Forms.Control.Location%2A> プロパティを使用します。 位置を個別に設定するには、コントロールの <xref:System.Windows.Forms.Control.Left%2A> (**X**) または <xref:System.Windows.Forms.Control.Top%2A> (**Y**) サブプロパティを使用します。 この構造体にはボタンの座標のコピーが含まれているため、ボタンの位置を表す <xref:System.Drawing.Point> 構造体の X 座標と Y 座標を暗黙的に設定することは避けてください。

## <a name="see-also"></a>参照

- [Windows フォーム コントロール](index.md)
- [チュートリアル: スナップ線を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-snaplines.md)
- [チュートリアル: TableLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-tablelayoutpanel.md)
- [チュートリアル: FlowLayoutPanel を使用した Windows フォーム上のコントロールの配置](walkthrough-arranging-controls-on-windows-forms-using-a-flowlayoutpanel.md)
- [各 Windows フォーム コントロールのラベル設定とショートカットの作成](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
- [Windows フォームで使用するコントロール](controls-to-use-on-windows-forms.md)
- [Windows フォーム コントロールの機能別一覧](windows-forms-controls-by-function.md)
- [方法: Windows フォームの画面位置を設定する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2010/52aha046(v=vs.100))
