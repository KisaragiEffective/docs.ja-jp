---
title: '方法 : コントロールにツールボックス ビットマップを指定する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Toolbox [Windows Forms], adding bitmaps for custom controls
- custom controls [Windows Forms], Toolbox bitmaps
- bitmaps [Windows Forms], custom controls
ms.assetid: 0ed0840a-616d-41ba-a27d-3573241932ad
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 61f60aaeab904dff80408a1dc46c2882fb5e22b9
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458314"
---
# <a name="how-to-provide-a-toolbox-bitmap-for-a-control"></a>方法 : コントロールにツールボックス ビットマップを指定する

コントロールの特殊なアイコンを Visual Studio の**ツールボックス**に表示する場合は、<xref:System.Drawing.ToolboxBitmapAttribute>を使用して特定のイメージを指定できます。 このクラスは "*属性*" であり、他のクラスに追加できる特殊なクラスです。 属性の詳細につい[てはC#、「」の「](../../../csharp/programming-guide/concepts/attributes/index.md)属性 Visual Basic の[概要 (Visual Basic)](../../../visual-basic/programming-guide/concepts/attributes/index.md) 」を参照C#してください。

<xref:System.Drawing.ToolboxBitmapAttribute>を使用すると、16 x 16 ピクセルのビットマップのパスとファイル名を示す文字列を指定できます。 コントロールを**ツールボックス**に追加すると、このビットマップがコントロールの横に表示されます。 また、<xref:System.Type>を指定することもできます。この場合、その型に関連付けられているビットマップが読み込まれます。 <xref:System.Type> と文字列の両方を指定した場合、コントロールは、<xref:System.Type> パラメーターで指定された型を含むアセンブリの文字列パラメーターで指定された名前を持つイメージリソースを検索します。

## <a name="to-specify-a-toolbox-bitmap-for-your-control"></a>コントロールのツールボックス ビットマップを指定するには

1. Visual Basic の `Class` キーワードの前、およびビジュアルC#のクラス宣言の上にある、コントロールのクラス宣言に <xref:System.Drawing.ToolboxBitmapAttribute> を追加します。

    ```vb
    ' Specifies the bitmap associated with the Button type.
    <ToolboxBitmap(GetType(Button))> Class MyControl1
    ' Specifies a bitmap file.
    End Class
    <ToolboxBitmap("C:\Documents and Settings\Joe\MyPics\myImage.bmp")> _
       Class MyControl2
    End Class
    ' Specifies a type that indicates the assembly to search, and the name
    ' of an image resource to look for.
    <ToolboxBitmap(GetType(MyControl), "MyControlBitmap")> Class MyControl
    End Class
    ```

    ```csharp
    // Specifies the bitmap associated with the Button type.
    [ToolboxBitmap(typeof(Button))]
    class MyControl1 : UserControl
    {
    }
    // Specifies a bitmap file.
    [ToolboxBitmap(@"C:\Documents and Settings\Joe\MyPics\myImage.bmp")]
    class MyControl2 : UserControl
    {
    }
    // Specifies a type that indicates the assembly to search, and the name
    // of an image resource to look for.
    [ToolboxBitmap(typeof(MyControl), "MyControlBitmap")]
    class MyControl : UserControl
    {
    }
    ```

2. プロジェクトをリビルドします。

    > [!NOTE]
    > ビットマップは、自動生成されたコントロールとコンポーネントのツールボックスには表示されません。 ビットマップを表示するには、 **[ツールボックス アイテムの選択]** ダイアログ ボックスを使用してコントロールを再読み込みします。 詳細については、「[チュートリアル: ツールボックスへのカスタム コンポーネントの自動設定](walkthrough-automatically-populating-the-toolbox-with-custom-components.md)」を参照してください。

## <a name="see-also"></a>関連項目

- <xref:System.Drawing.ToolboxBitmapAttribute>
- [チュートリアル: ツールボックスへのカスタム コンポーネントの自動設定](walkthrough-automatically-populating-the-toolbox-with-custom-components.md)
- [デザイン時の Windows フォーム コントロールの開発](developing-windows-forms-controls-at-design-time.md)
- [属性の概要 (Visual Basic)](../../../visual-basic/programming-guide/concepts/attributes/index.md)
- [属性 (C#)](../../../csharp/programming-guide/concepts/attributes/index.md)
