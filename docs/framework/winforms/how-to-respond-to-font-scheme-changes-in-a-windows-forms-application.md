---
title: Windows フォームアプリでのフォントスキームの変更に応答する
titleSuffix: ''
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, font scheme changes
ms.assetid: 4db27702-22e7-43bf-a07d-9a004549853c
ms.openlocfilehash: e3b96139a7cfd4b268d81b1da58229527e2beb87
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76739228"
---
# <a name="how-to-respond-to-font-scheme-changes-in-a-windows-forms-application"></a>方法 : Windows フォーム アプリケーションでのフォント パターンの変更に応答する
Windows オペレーティングシステムでは、ユーザーはシステム全体のフォント設定を変更して、既定のフォントのサイズを大きくまたは小さくすることができます。 これらのフォント設定を変更することは、視覚に障碍のあるユーザーや、画面上のテキストを読み込むために大きな種類を必要とするユーザーにとって非常に重要です。 これらの変更に対応するように Windows フォームアプリケーションを調整するには、フォントスキームが変更されたときに、フォームとすべての含まれるテキストのサイズを増減します。 フォントサイズの変化に合わせてフォームを動的に調整するには、フォームにコードを追加します。  
  
 通常、Windows フォームで使用される既定のフォントは、`GetStockObject(DEFAULT_GUI_FONT)`への <xref:Microsoft.Win32> 名前空間呼び出しによって返されるフォントです。 この呼び出しによって返されるフォントは、画面の解像度が変更された場合にのみ変更されます。 次の手順に示すように、コードでは既定のフォントを <xref:System.Drawing.SystemFonts.IconTitleFont%2A> に変更して、フォントサイズの変更に応答する必要があります。  
  
### <a name="to-use-the-desktop-font-and-respond-to-font-scheme-changes"></a>デスクトップフォントを使用してフォント設定の変更に応答するには  
  
1. フォームを作成し、必要なコントロールを追加します。 詳細については、「[方法: コマンドラインから Windows フォームアプリケーションを作成](how-to-create-a-windows-forms-application-from-the-command-line.md)する」および「 [Windows フォームで使用するコントロール](./controls/controls-to-use-on-windows-forms.md)」を参照してください。  
  
2. <xref:Microsoft.Win32> 名前空間への参照をコードに追加します。  
  
     [!code-csharp[WinFormsAutoScaling#2](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#2)]
     [!code-vb[WinFormsAutoScaling#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#2)]  
  
3. フォームのコンストラクターに次のコードを追加して、必要なイベントハンドラーをフックし、フォームで使用されている既定のフォントを変更します。  
  
     [!code-csharp[WinFormsAutoScaling#3](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#3)]
     [!code-vb[WinFormsAutoScaling#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#3)]  
  
4. <xref:Microsoft.Win32.UserPreferenceCategory.Window> カテゴリが変更されたときにフォームが自動的に拡大縮小されるようにする <xref:Microsoft.Win32.SystemEvents.UserPreferenceChanged> イベントのハンドラーを実装します。  
  
     [!code-csharp[WinFormsAutoScaling#4](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#4)]
     [!code-vb[WinFormsAutoScaling#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#4)]  
  
5. 最後に、<xref:Microsoft.Win32.SystemEvents.UserPreferenceChanged> イベントハンドラーをデタッチする <xref:System.Windows.Forms.Form.FormClosing> イベントのハンドラーを実装します。  
  
     > [!IMPORTANT]
     > このコードを含めないと、アプリケーションでメモリリークが発生します。  
  
     [!code-csharp[WinFormsAutoScaling#5](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#5)]
     [!code-vb[WinFormsAutoScaling#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#5)]  
  
6. コードをコンパイルして実行します。  
  
### <a name="to-manually-change-the-font-scheme-in-windows-xp"></a>Windows XP でフォントスキームを手動で変更するには  
  
1. Windows フォームアプリケーションの実行中に、Windows デスクトップを右クリックし、ショートカットメニューの **[プロパティ]** をクリックします。  
  
2. **[表示プロパティ]** ダイアログボックスで、 **[外観]** タブをクリックします。  
  
3. **[フォントサイズ]** ボックスの一覧で、新しいフォントサイズを選択します。  
  
     このフォームは、デスクトップフォントスキームの実行時の変更に反応するようになります。 ユーザーが**通常**のフォント、**大きなフォント**、および**特大のフォント**を変更すると、フォントが変更され、適切に拡大縮小されます。  
  
## <a name="example"></a>例  
 [!code-csharp[WinFormsAutoScaling#1](~/samples/snippets/csharp/VS_Snippets_Winforms/WinFormsAutoScaling/CS/Form1.cs#1)]
 [!code-vb[WinFormsAutoScaling#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/WinFormsAutoScaling/VB/Form1.vb#1)]  
  
 このコード例のコンストラクターには `InitializeComponent`の呼び出しが含まれています。これは、Visual Studio で新しい Windows フォームプロジェクトを作成するときに定義されます。 コマンドラインでアプリケーションをビルドする場合は、このコード行を削除します。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ContainerControl.PerformAutoScale%2A>
- [Windows フォームにおける自動スケーリング](automatic-scaling-in-windows-forms.md)
