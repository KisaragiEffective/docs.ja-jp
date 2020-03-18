---
title: コントロールのプロパティを定義する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- properties [Windows Forms], defining in code
- custom controls [Windows Forms], defining properties in code
ms.assetid: c2eb8277-a842-4d99-89a9-647b901a0434
ms.openlocfilehash: 0fec817226a7da4b44ec992f9e384a2ad5449001
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746111"
---
# <a name="defining-a-property-in-windows-forms-controls"></a>Windows フォーム コントロールのプロパティの定義
プロパティの概要については、「[プロパティの概要](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/65zdfbdt(v=vs.120))」を参照してください。 プロパティを定義するときには、いくつかの重要な考慮事項があります。  
  
- 定義するプロパティに属性を適用する必要があります。 属性によって、デザイナーでプロパティがどのように表示されるかが指定されます。 詳細については、「[コンポーネントのデザイン時属性](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/tk67c2t8(v=vs.120))」を参照してください。  
  
- プロパティを変更してコントロールのビジュアル表示に影響を与える場合は、`set` アクセサーから (コントロールが <xref:System.Windows.Forms.Control>から継承する) <xref:System.Windows.Forms.Control.Invalidate%2A> メソッドを呼び出します。 <xref:System.Windows.Forms.Control.Invalidate%2A> は、コントロールを再描画する <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドを呼び出します。 <xref:System.Windows.Forms.Control.Invalidate%2A> を複数回呼び出すと、効率を上げるために <xref:System.Windows.Forms.Control.OnPaint%2A> が1回だけ呼び出されます。  
  
- .NET Framework クラス ライブラリでは、整数、10 進数、ブール値など、一般的なデータ型に対応する型コンバーターを使用できます。 型コンバーターは、一般に文字列から値への変換を行うために使用されます (文字列データから他のデータ型に変換)。 一般的なデータ型は、値を文字列に変換し、文字列を適切なデータ型に変換する既定の型コンバーターに関連付けられています。 カスタム (つまり、非標準的な) データ型であるプロパティを定義する場合、そのプロパティに関連付けられる型コンバーターを指定する属性を適用する必要があります。 また、属性を使用してカスタム UI 型エディターとプロパティを関連付けることもできます。 UI 型エディターには、プロパティやデータ型を編集するためのユーザー インターフェイスが備わっています。 たとえば、カラー ピッカーなどの UI 型エディターがあります。 属性の例は、このトピックの最後に記載されています。  
  
    > [!NOTE]
    > 型コンバーターまたは UI 型エディターをカスタム プロパティに使用できない場合、「[デザイン時サポートの拡張](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/37899azc(v=vs.120))」に示されているものを実装できます。  
  
 次のコード フラグメントは、カスタム コントロール `EndColor` に対して `FlashTrackBar` という名前のカスタム プロパティを定義します。  
  
```vb  
Public Class FlashTrackBar  
   Inherits Control  
   ...  
   ' Private data member that backs the EndColor property.  
   Private _endColor As Color = Color.LimeGreen  
  
   ' The Category attribute tells the designer to display  
   ' it in the Flash grouping.   
   ' The Description attribute provides a description of  
   ' the property.   
   <Category("Flash"), _  
   Description("The ending color of the bar.")>  _  
   Public Property EndColor() As Color  
      ' The public property EndColor accesses _endColor.  
      Get  
         Return _endColor  
      End Get  
      Set  
         _endColor = value  
         If Not (baseBackground Is Nothing) And showGradient Then  
            baseBackground.Dispose()  
            baseBackground = Nothing  
         End If  
         ' The Invalidate method calls the OnPaint method, which redraws    
         ' the control.  
         Invalidate()  
      End Set  
   End Property  
   ...  
End Class  
```  
  
```csharp  
public class FlashTrackBar : Control {  
   ...  
   // Private data member that backs the EndColor property.  
   private Color endColor = Color.LimeGreen;  
   // The Category attribute tells the designer to display  
   // it in the Flash grouping.   
   // The Description attribute provides a description of  
   // the property.   
   [  
   Category("Flash"),  
   Description("The ending color of the bar.")  
   ]  
   // The public property EndColor accesses endColor.  
   public Color EndColor {  
      get {  
         return endColor;  
      }  
      set {  
         endColor = value;  
         if (baseBackground != null && showGradient) {  
            baseBackground.Dispose();  
            baseBackground = null;  
         }  
         // The Invalidate method calls the OnPaint method, which redraws   
         // the control.  
         Invalidate();  
      }  
   }  
   ...  
}  
```  
  
 次のコード フラグメントは、型コンバーターと UI 型エディターをプロパティ `Value` に関連付けます。 この場合 `Value` は整数であり、既定の型コンバーターを持ちますが、<xref:System.ComponentModel.TypeConverterAttribute> 属性は、デザイナーがパーセンテージとして表示できるようにするカスタム型コンバーター (`FlashTrackBarValueConverter`) を適用します。 UI 型エディター `FlashTrackBarValueEditor` により、そのパーセントを視覚的に表示できます。 また、この例では、<xref:System.ComponentModel.TypeConverterAttribute> または <xref:System.ComponentModel.EditorAttribute> 属性によって指定された型コンバーターまたはエディターが既定のコンバーターをオーバーライドしていることも示しています。  
  
```vb  
<Category("Flash"), _  
TypeConverter(GetType(FlashTrackBarValueConverter)), _  
Editor(GetType(FlashTrackBarValueEditor), _  
GetType(UITypeEditor)), _  
Description("The current value of the track bar.  You can enter an actual value or a percentage.")>  _  
Public ReadOnly Property Value() As Integer  
...  
End Property  
```  
  
```csharp  
[  
Category("Flash"),   
TypeConverter(typeof(FlashTrackBarValueConverter)),  
Editor(typeof(FlashTrackBarValueEditor), typeof(UITypeEditor)),  
Description("The current value of the track bar.  You can enter an actual value or a percentage.")  
]  
public int Value {  
...  
}  
```  
  
## <a name="see-also"></a>参照

- [Windows フォーム コントロールのプロパティ](properties-in-windows-forms-controls.md)
- [ShouldSerialize メソッドと Reset メソッドによる既定値の定義](defining-default-values-with-the-shouldserialize-and-reset-methods.md)
- [プロパティ変更イベント](property-changed-events.md)
- [Windows フォーム コントロールの属性](attributes-in-windows-forms-controls.md)
