---
title: コントロールによって表示されるテキストを設定する
ms.date: 08/20/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms, captions
- Button control [Windows Forms], button text
- StdFont object and CommandButton caption
- captions [Windows Forms], Windows Forms controls
- Text property [Windows Forms], Windows Forms control
- Button control [Windows Forms], text display
- labels [Windows Forms], adding to CommandButton control
- buttons [Windows Forms], text
- captions [Windows Forms], setting
- text
- examples [Windows Forms], controls
- text [Windows Forms], Windows Forms controls
- controls [Windows Forms], captions
- forms [Windows Forms], captions
ms.assetid: 36b95bff-8780-479d-b86a-f1a0673653aa
ms.openlocfilehash: eb02cbc3b335b0d5856f786b21d1d202cf444211
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738421"
---
# <a name="how-to-set-the-text-displayed-by-a-windows-forms-control"></a>方法: Windows フォームコントロールによって表示されるテキストを設定する

Windows フォームコントロールには、通常、コントロールの主な機能に関連するテキストが表示されます。 たとえば、<xref:System.Windows.Forms.Button> コントロールには、通常、ボタンがクリックされた場合に実行されるアクションを示すキャプションが表示されます。 すべてのコントロールに対して、<xref:System.Windows.Forms.Control.Text%2A> プロパティを使用してテキストを設定または返すことができます。 <xref:System.Windows.Forms.Control.Font%2A> プロパティを使用して、フォントを変更することができます。

[デザイナー](#designer)を使用してテキストを設定することもできます。

## <a name="programmatic"></a>プログラムによる

1. <xref:System.Windows.Forms.Control.Text%2A> プロパティを文字列に設定します。

   下線付きのアクセスキーを作成するには、アクセスキーにする文字の前にアンパサンド (&) を含めます。

2. <xref:System.Windows.Forms.Control.Font%2A> プロパティを型 <xref:System.Drawing.Font> のオブジェクトに設定します。

    ```vb
    Button1.Text = "Click here to save changes"
    Button1.Font = New Font("Arial", 10, FontStyle.Bold, GraphicsUnit.Point)
    ```

    ```csharp
    button1.Text = "Click here to save changes";
    button1.Font = new Font("Arial", 10, FontStyle.Bold, GraphicsUnit.Point);
    ```

    ```cpp
    button1->Text = "Click here to save changes";
    button1->Font = new System::Drawing::Font("Arial", 10, FontStyle::Bold, GraphicsUnit::Point);
    ```

    > [!NOTE]
    > エスケープ文字を使用すると、メニュー項目など、通常は別の解釈がなされるユーザー インターフェイス要素の特殊文字を表示できます。 たとえば、次のコード行では、メニュー項目のテキストが "& Now For all" というテキストに設定されています。

    ```vb
    MPMenuItem.Text = "&& Now For Something Completely Different"
    ```

    ```csharp
    mpMenuItem.Text = "&& Now For Something Completely Different";
    ```

    ```cpp
    mpMenuItem->Text = "&& Now For Something Completely Different";
    ```

## <a name="designer"></a>デザイナー

1. Visual Studio の **[プロパティ]** ウィンドウで、コントロールの**Text**プロパティを適切な文字列に設定します。

   下線付きのショートカットキーを作成するには、ショートカットキーにする文字の前にアンパサンド (&) を含めます。

2. **[プロパティ]** ウィンドウで、 **[フォント]** プロパティの横にある省略記号ボタン (![参照ボタン ([...]) を選択します (Visual Studio のプロパティウィンドウの](./media/visual-studio-ellipsis-button.png))。

   [標準フォント] ダイアログボックスで、フォント、フォントスタイル、サイズ、効果 (取り消し線、下線など)、および必要なスクリプトを選択します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Control.Text%2A?displayProperty=nameWithType>
- [方法: Windows フォーム コントロールのアクセス キーを作成する](how-to-create-access-keys-for-windows-forms-controls.md)
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
