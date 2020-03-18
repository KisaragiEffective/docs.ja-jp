---
title: コントロールのプロパティ
ms.date: 03/30/2017
helpviewer_keywords:
- custom controls [Windows Forms], properties overview (using code)
- controls [Windows Forms], properties
- properties [Windows Forms]
ms.assetid: 2785279b-fb57-4937-8f6b-2050e475db6f
ms.openlocfilehash: 82bfab15cef4946661a37d2d88fbe1b797f3d816
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741182"
---
# <a name="properties-in-windows-forms-controls"></a>Windows フォーム コントロールのプロパティ
Windows フォームコントロールは、基本クラス <xref:System.Windows.Forms.Control?displayProperty=nameWithType>を形成する多くのプロパティを継承します。 これには、<xref:System.Windows.Forms.Control.Font%2A>、<xref:System.Windows.Forms.Control.ForeColor%2A>、<xref:System.Windows.Forms.Control.BackColor%2A>、<xref:System.Windows.Forms.Control.Bounds%2A>、<xref:System.Windows.Forms.Control.ClientRectangle%2A>、<xref:System.Windows.Forms.Control.DisplayRectangle%2A>、<xref:System.Windows.Forms.Control.Enabled%2A>、<xref:System.Windows.Forms.Control.Focused%2A>、<xref:System.Windows.Forms.Control.Height%2A>、<xref:System.Windows.Forms.Control.Width%2A>、<xref:System.Windows.Forms.Control.Visible%2A>、<xref:System.Windows.Forms.Control.AutoSize%2A>などのプロパティが含まれます。 継承されたプロパティの詳細については、「<xref:System.Windows.Forms.Control?displayProperty=nameWithType>」を参照してください。  
  
 コントロールで継承されたプロパティをオーバーライドしたり、新しいプロパティを定義したりできます。  
  
## <a name="in-this-section"></a>このセクションの内容  
 [プロパティの定義](defining-a-property-in-windows-forms-controls.md)  
 カスタム コントロールまたはカスタム コンポーネントのプロパティを実装する方法と、そのプロパティをデザイン環境に統合する方法について説明します。  
  
 [ShouldSerialize メソッドと Reset メソッドによる既定値の定義](defining-default-values-with-the-shouldserialize-and-reset-methods.md)  
 カスタム コントロールまたはカスタム コンポーネントの既定のプロパティ値を定義する方法について説明します。  
  
 [プロパティ変更イベント](property-changed-events.md)  
 プロパティ値が変更されたときに、プロパティ変更通知を有効にする方法について説明します。  
  
 [方法 : 内在コントロールのプロパティを公開する](how-to-expose-properties-of-constituent-controls.md)  
 カスタム複合コントロール内の内在コントロールのプロパティを公開する方法について説明します。  
  
 [カスタム コントロールへのメソッドの実装](method-implementation-in-custom-controls.md)  
 カスタム コントロールおよびカスタム コンポーネントにメソッドを実装する方法について説明します。  
  
## <a name="reference"></a>リファレンス  
 <xref:System.Windows.Forms.UserControl>  
 複合コントロールを実装するための基本クラスについて説明します。  
  
 <xref:System.ComponentModel.TypeConverterAttribute>  
 カスタムプロパティ型に使用する <xref:System.ComponentModel.TypeConverter> を指定する属性を文書にします。  
  
 <xref:System.ComponentModel.EditorAttribute>  
 カスタムプロパティに使用する <xref:System.Drawing.Design.UITypeEditor> を指定する属性を文書にします。  
  
## <a name="related-sections"></a>関連項目  
 [Windows フォーム コントロールの属性](attributes-in-windows-forms-controls.md)  
 カスタム コントロールとコンポーネントのプロパティや他のメンバーに適用できる属性について説明します。  
  
 [コンポーネントのデザイン時属性](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/tk67c2t8(v=vs.120))  
 ビジュアル デザイナーでデザインするときに正しく表示されるようにコンポーネントとコントロールに適用するメタデータ属性の一覧を表示します。  
  
 [デザイン時サポートの拡張](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/37899azc(v=vs.120))  
 デザイン時サポートを提供するエディターやデザイナーなどのクラスを実装する方法について説明します。
