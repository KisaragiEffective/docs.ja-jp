---
title: コントロールのアクセスキーを作成する
ms.date: 08/20/2019
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- controls [Windows Forms], access keys
- Button control [Windows Forms], access keys
- dialog box controls [Windows Forms], mnemonics
- access keys [Windows Forms], creating for controls
- mnemonics [Windows Forms], adding to dialog box controls
- mnemonics
- ampersand character in shortcut key
- Windows Forms controls, access keys
- examples [Windows Forms], controls
- Text property [Windows Forms], specifying access keys for controls
- keyboard shortcuts [Windows Forms], creating for controls
- access keys [Windows Forms], Windows Forms
- ALT key
ms.assetid: 4faa0991-28ec-4eca-91db-51dc2cd6a7ac
ms.openlocfilehash: 7f6b0a5838cacfc1189fba819a54b3423d567ea0
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76731173"
---
# <a name="how-to-create-access-keys-for-windows-forms-controls"></a>方法: Windows フォームコントロールのアクセスキーを作成する

*アクセスキー*は、メニュー、メニュー項目、またはボタンなどのコントロールのラベルのテキスト内の下線付きの文字です。 ユーザーは、アクセスキーを使用して、Alt キーを定義済みのアクセスキーと組み合わせて押すことで、ボタンを "クリック" できます。 たとえば、ボタンがフォームを印刷するためのプロシージャを実行していて、その `Text` プロパティが "Print" に設定されている場合、文字 "P" の前にアンパサンドを追加すると、実行時にボタンのテキストに文字 "P" が下線付きで表示されます。 ユーザーは、Alt + P キーを押して、ボタンに関連付けられているコマンドを実行できます。

フォーカスを受け取ることができないコントロールは、アクセスキーを持つことができません。

## <a name="programmatic"></a>プログラムによる

`Text` プロパティを、ショートカットにする文字の前にアンパサンド (&) を含む文字列に設定します。

```vb
' Set the letter "P" as an access key.
Button1.Text = "&Print"
```

```csharp
// Set the letter "P" as an access key.
button1.Text = "&Print";
```

```cpp
// Set the letter "P" as an access key.
button1->Text = "&Print";
```

> [!NOTE]
> アクセスキーを作成せずにキャプションにアンパサンドを使用するには、2つのアンパサンド (& &) を含めます。 キャプションにはアンパサンドが1つ表示され、下線付きの文字はありません。

## <a name="designer"></a>デザイナー

Visual Studio の **[プロパティ]** ウィンドウで、 **[Text]** プロパティを、アクセスキーにする文字の前にアンパサンド (' & ') を含む文字列に設定します。 たとえば、文字 "P" をアクセスキーとして設定するには、「 **& Print**」と入力します。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Button>
- [方法: Windows フォームのボタンのクリックに応答する](how-to-respond-to-windows-forms-button-clicks.md)
- [方法: Windows フォーム コントロールによって表示されるテキストを設定する](how-to-set-the-text-displayed-by-a-windows-forms-control.md)
- [各 Windows フォーム コントロールのラベル設定とショートカットの作成](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
