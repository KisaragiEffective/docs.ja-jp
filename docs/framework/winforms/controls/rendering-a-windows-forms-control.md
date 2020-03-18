---
title: コントロールをレンダリングする
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- custom controls [Windows Forms], rendering
- OnPaintBackground method [Windows Forms], invoking in Windows Forms custom controls
- custom controls [Windows Forms], graphics resources
- custom controls [Windows Forms], invalidation and painting
ms.assetid: aae8e1e6-4786-432b-a15e-f4c44760d302
ms.openlocfilehash: 0e1a7ca5dc0414d518c081b1db2dca5477d4f743
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742385"
---
# <a name="rendering-a-windows-forms-control"></a>Windows フォーム コントロールのレンダリング
レンダリングとは、ユーザーの画面で視覚的表現を作成するプロセスを指します。 Windows フォームは、GDI (新しい Windows グラフィックスライブラリ) を使用してレンダリングを行います。 GDI へのアクセスを提供するマネージクラスは、<xref:System.Drawing?displayProperty=nameWithType> 名前空間とその副名前空間にあります。  
  
 コントロールのレンダリングには、次の要素が含まれます。  
  
- 基本クラス <xref:System.Windows.Forms.Control?displayProperty=nameWithType>によって提供される描画機能。  
  
- GDI グラフィックスライブラリの重要な要素。  
  
- 描画領域のジオメトリ。  
  
- グラフィックスリソースを解放する手順。  
  
## <a name="drawing-functionality-provided-by-control"></a>コントロールによって提供される描画機能  
 基本クラス <xref:System.Windows.Forms.Control> は、<xref:System.Windows.Forms.Control.Paint> イベントを通じて描画機能を提供します。 コントロールは、表示を更新する必要があるたびに、<xref:System.Windows.Forms.Control.Paint> イベントを発生させます。 .NET Framework のイベントの詳細については、「[イベントの処理と発生](../../../standard/events/index.md)」を参照してください。  
  
 <xref:System.Windows.Forms.Control.Paint> イベント <xref:System.Windows.Forms.PaintEventArgs>のイベントデータクラスは、コントロールを描画するために必要なデータ (グラフィックスオブジェクトへのハンドル、および描画する領域を表す四角形オブジェクト) を保持します。 これらのオブジェクトは、次のコードフラグメントに太字で示されています。  
  
```vb  
Public Class PaintEventArgs  
   Inherits EventArgs  
   Implements IDisposable  
  
   Public ReadOnly Property ClipRectangle() As System.Drawing.Rectangle  
      ...  
   End Property  
  
   Public ReadOnly Property Graphics() As System.Drawing.Graphics  
      ...  
   End Property  
   ' Other properties and methods.  
   ...  
End Class  
```  
  
```csharp  
public class PaintEventArgs : EventArgs, IDisposable {  
public System.Drawing.Rectangle ClipRectangle {get;}  
public System.Drawing.Graphics Graphics {get;}  
// Other properties and methods.  
...  
}  
```  
  
 <xref:System.Drawing.Graphics> は、このトピックの後半の「GDI の説明」で説明されているように、描画機能をカプセル化するマネージクラスです。 <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> は <xref:System.Drawing.Rectangle> 構造体のインスタンスであり、コントロールが描画できる領域を定義します。 コントロール開発者は、このトピックで後述するジオメトリについて説明しているように、コントロールの <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> プロパティを使用して、<xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> を計算できます。  
  
 コントロールは、<xref:System.Windows.Forms.Control>から継承する <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドをオーバーライドすることにより、レンダリングロジックを提供する必要があります。 <xref:System.Windows.Forms.Control.OnPaint%2A> は、グラフィックスオブジェクトへのアクセス、および <xref:System.Drawing.Design.PaintValueEventArgs.Graphics%2A> を通じて描画される四角形と、渡された <xref:System.Windows.Forms.PaintEventArgs> インスタンスの <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> プロパティを取得します。  
  
```vb  
Protected Overridable Sub OnPaint(pe As PaintEventArgs)  
```  
  
```csharp  
protected virtual void OnPaint(PaintEventArgs pe);  
```  
  
 基本 <xref:System.Windows.Forms.Control> クラスの <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドは描画機能を実装しませんが、<xref:System.Windows.Forms.Control.Paint> イベントに登録されているイベントデリゲートを呼び出すだけです。 <xref:System.Windows.Forms.Control.OnPaint%2A>をオーバーライドする場合は、通常、基本クラスの <xref:System.Windows.Forms.Control.OnPaint%2A> メソッドを呼び出して、登録されているデリゲートが <xref:System.Windows.Forms.Control.Paint> イベントを受け取るようにする必要があります。 ただし、画面全体を描画するコントロールは、基本クラスの <xref:System.Windows.Forms.Control.OnPaint%2A>を呼び出すことはできません。これにより、ちらつきが生じます。 <xref:System.Windows.Forms.Control.OnPaint%2A> イベントをオーバーライドする例については、「[方法: 進行状況を示す Windows フォームコントロールを作成](how-to-create-a-windows-forms-control-that-shows-progress.md)する」を参照してください。  
  
> [!NOTE]
> コントロールから直接 <xref:System.Windows.Forms.Control.OnPaint%2A> を呼び出さないでください。代わりに、<xref:System.Windows.Forms.Control.Invalidate%2A> メソッド (<xref:System.Windows.Forms.Control>から継承)、または <xref:System.Windows.Forms.Control.Invalidate%2A>を呼び出すその他のメソッドを呼び出します。 <xref:System.Windows.Forms.Control.Invalidate%2A> メソッドは、<xref:System.Windows.Forms.Control.OnPaint%2A>を呼び出します。 <xref:System.Windows.Forms.Control.Invalidate%2A> メソッドはオーバーロードされ、<xref:System.Windows.Forms.Control.Invalidate%2A> `e`に指定された引数によっては、コントロールが画面領域の一部またはすべてを再描画します。  
  
 基本 <xref:System.Windows.Forms.Control> クラスは、描画に便利な別のメソッドである <xref:System.Windows.Forms.Control.OnPaintBackground%2A> メソッドを定義します。  
  
```vb  
Protected Overridable Sub OnPaintBackground(pevent As PaintEventArgs)  
```  
  
```csharp  
protected virtual void OnPaintBackground(PaintEventArgs pevent);  
```  
  
 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> は、ウィンドウの背景 (およびその図形) を描画し、高速であることが保証されています。また、個々の描画要求は、再描画する必要があるすべての領域を対象とする1つの <xref:System.Windows.Forms.Control.Paint> イベントに結合されるため、<xref:System.Windows.Forms.Control.OnPaint%2A> になります。 たとえば、コントロールのグラデーションカラーの背景を描画する必要がある場合は、<xref:System.Windows.Forms.Control.OnPaintBackground%2A> を呼び出すことができます。  
  
 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> にはイベントのような用語があり、`OnPaint` メソッドと同じ引数を取りますが、<xref:System.Windows.Forms.Control.OnPaintBackground%2A> は true のイベントメソッドではありません。 `PaintBackground` イベントはなく、<xref:System.Windows.Forms.Control.OnPaintBackground%2A> はイベントデリゲートを呼び出しません。 <xref:System.Windows.Forms.Control.OnPaintBackground%2A> メソッドをオーバーライドする場合、派生クラスは基底クラスの <xref:System.Windows.Forms.Control.OnPaintBackground%2A> メソッドを呼び出す必要がありません。  
  
## <a name="gdi-basics"></a>GDI + の基礎  
 <xref:System.Drawing.Graphics> クラスには、円、三角形、円弧、楕円などのさまざまな図形を描画するためのメソッドと、テキストを表示するためのメソッドが用意されています。 <xref:System.Drawing?displayProperty=nameWithType> 名前空間とその副名前空間には、図形 (円、四角形、円弧など)、色、フォント、ブラシなどのグラフィックス要素をカプセル化するクラスが含まれています。 GDI の詳細については、「[マネージグラフィックスクラスの使用](../advanced/using-managed-graphics-classes.md)」を参照してください。 GDI の基本事項については、「[方法: 進行状況を示す Windows フォームコントロールを作成](how-to-create-a-windows-forms-control-that-shows-progress.md)する」でも説明しています。  
  
## <a name="geometry-of-the-drawing-region"></a>描画領域のジオメトリ  
 コントロールの <xref:System.Windows.Forms.Control.ClientRectangle%2A> プロパティは、ユーザーの画面上のコントロールで使用できる四角形の領域を指定します。一方、<xref:System.Windows.Forms.PaintEventArgs> の <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> プロパティは、実際に描画される領域を指定します。 (描画は、引数として <xref:System.Windows.Forms.PaintEventArgs> インスタンスを受け取る <xref:System.Windows.Forms.Control.Paint> イベントメソッドで行われることに注意してください)。 コントロールは、コントロールの表示の小さいセクションが変更された場合のように、使用可能な領域の一部だけを描画する必要がある場合があります。 このような状況では、コントロール開発者は、描画する実際の四角形を計算し、それを <xref:System.Windows.Forms.Control.Invalidate%2A>に渡す必要があります。 引数として <xref:System.Drawing.Rectangle> または <xref:System.Drawing.Region> を受け取る <xref:System.Windows.Forms.Control.Invalidate%2A> のオーバーロードされたバージョンでは、その引数を使用して <xref:System.Windows.Forms.PaintEventArgs>の <xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> プロパティを生成します。  
  
 次のコードフラグメントは、`FlashTrackBar` カスタムコントロールが、描画する四角形の領域を計算する方法を示しています。 `client` 変数は、<xref:System.Windows.Forms.PaintEventArgs.ClipRectangle%2A> プロパティを表します。 完全なサンプルについては、「[方法: 進行状況を示す Windows フォームコントロールを作成](how-to-create-a-windows-forms-control-that-shows-progress.md)する」を参照してください。  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#6](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#6)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#6](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#6)]  
  
## <a name="freeing-graphics-resources"></a>グラフィックスリソースの解放  
 グラフィックスオブジェクトは、システムリソースを使用するため、コストが高くなります。 このようなオブジェクトには、<xref:System.Drawing.Graphics?displayProperty=nameWithType> クラスのインスタンスだけでなく、<xref:System.Drawing.Brush?displayProperty=nameWithType>、<xref:System.Drawing.Pen?displayProperty=nameWithType>、およびその他のグラフィックスクラスのインスタンスが含まれます。 グラフィックスリソースは、必要なときにのみ作成し、使用が終了したらすぐにリリースすることが重要です。 <xref:System.IDisposable> インターフェイスを実装する型を作成する場合は、リソースを解放するために、終了時にその <xref:System.IDisposable.Dispose%2A> メソッドを呼び出します。  
  
 次のコードフラグメントは、`FlashTrackBar` カスタムコントロールが <xref:System.Drawing.Brush> リソースを作成および解放する方法を示しています。 完全なソースコードについては、「[方法: 進行状況を示す Windows フォームコントロールを作成](how-to-create-a-windows-forms-control-that-shows-progress.md)する」を参照してください。  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#5)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#5)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#4](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#4)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#4)]  
  
 [!code-csharp[System.Windows.Forms.FlashTrackBar#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/CS/FlashTrackBar.cs#3)]
 [!code-vb[System.Windows.Forms.FlashTrackBar#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.FlashTrackBar/VB/FlashTrackBar.vb#3)]  
  
## <a name="see-also"></a>参照

- [方法: 進行状況を示す Windows フォーム コントロールを作成する](how-to-create-a-windows-forms-control-that-shows-progress.md)
