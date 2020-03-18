---
title: BindingNavigator コントロールを使用してデータ間を移動する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BindingNavigator control [Windows Forms], navigating data
- data [Windows Forms], navigating
- data navigation
- examples [Windows Forms], BindingNavigator control
ms.assetid: 0e5d4f34-bc9b-47cf-9b8d-93acbb1f1dbb
ms.openlocfilehash: 10508cb447e70cc387f9d98effa3bc4b5ccbbaa9
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736001"
---
# <a name="how-to-navigate-data-with-the-windows-forms-bindingnavigator-control"></a>方法 : Windows フォーム BindingNavigator コントロールを使用してデータ間を移動する
Windows フォームに <xref:System.Windows.Forms.BindingNavigator> コントロールが登場したことで、開発者はエンドユーザーに、作成したフォームにおける単純なデータ移動および操作のためのユーザー インターフェイスを提供できるようになります。  
  
 <xref:System.Windows.Forms.BindingNavigator> コントロールは、データセットの最初、最後、次、前のレコードへの移動が構成済みのボタンや、レコードを追加および削除するためのボタンを持つ <xref:System.Windows.Forms.ToolStrip> コントロールです。 <xref:System.Windows.Forms.BindingNavigator> コントロールへのボタンの追加は、<xref:System.Windows.Forms.ToolStrip> コントロールなので簡単です。 例については、「[方法: Windows フォーム BindingNavigator コントロールに [Load]、[Save]、[Cancel] の各ボタンを追加する](load-save-and-cancel-bindingnavigator.md)」を参照してください。  
  
 <xref:System.Windows.Forms.BindingNavigator> コントロールの各ボタンについて、同じ機能をプログラムで許可する <xref:System.Windows.Forms.BindingSource> コンポーネントの対応するメンバーが存在します。 たとえば、<xref:System.Windows.Forms.BindingNavigator.MoveFirstItem%2A> ボタンは <xref:System.Windows.Forms.BindingSource.MoveFirst%2A> コンポーネントの <xref:System.Windows.Forms.BindingSource> メソッドに対応し、<xref:System.Windows.Forms.BindingNavigator.DeleteItem%2A> ボタンは <xref:System.Windows.Forms.BindingSource.RemoveCurrent%2A> メソッドに対応します。 その結果、データ レコード間を移動する <xref:System.Windows.Forms.BindingNavigator> コントロールを有効にするために必要なのは、<xref:System.Windows.Forms.BindingNavigator.BindingSource%2A> プロパティをフォームの適切な <xref:System.Windows.Forms.BindingSource> コンポーネントに設定するだけです。  
  
### <a name="to-set-up-the-bindingnavigator-control"></a>BindingNavigator コントロールを設定するには  
  
1. <xref:System.Windows.Forms.BindingSource> という名前の `bindingSource1` コンポーネントを 1 つ、<xref:System.Windows.Forms.TextBox> と `textBox1` という名前の `textBox2` コントロールを 2 つ追加します。  
  
2. `bindingSource1` をデータにバインドし、テキスト ボックス コントロールを `bindingSource1` にバインドします。 そのためには、以下のコードをフォームに貼り付け、フォームのコンストラクターまたは `LoadData` イベント処理メソッドから <xref:System.Windows.Forms.Form.Load> を呼び出します。  
  
     [!code-csharp[System.Windows.Forms.BindingNavigatorNavigate#2](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/CS/Form1.cs#2)]
     [!code-vb[System.Windows.Forms.BindingNavigatorNavigate#2](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/VB/Form1.vb#2)]  
  
3. <xref:System.Windows.Forms.BindingNavigator> という名前の `bindingNavigator1` コントロールをフォームに追加します。  
  
4. <xref:System.Windows.Forms.BindingNavigator.BindingSource%2A> の `bindingNavigator1` プロパティを `bindingSource1` に設定します。 これは、デザイナーを使用して、またはコード内で実行できます。  
  
     [!code-csharp[System.Windows.Forms.BindingNavigatorNavigate#3](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/CS/Form1.cs#3)]
     [!code-vb[System.Windows.Forms.BindingNavigatorNavigate#3](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/VB/Form1.vb#3)]  
  
## <a name="example"></a>例  
 次のコード例は、これまで説明した手順の完全な例です。  
  
 [!code-csharp[System.Windows.Forms.BindingNavigatorNavigate#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/CS/Form1.cs#1)]
 [!code-vb[System.Windows.Forms.BindingNavigatorNavigate#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.BindingNavigatorNavigate/VB/Form1.vb#1)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
 この例で必要な要素は次のとおりです。  
  
- System、System.Data、System.Drawing、System.Windows.Forms、および System.Xml アセンブリへの参照。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingNavigator>
- [BindingNavigator コントロール](bindingnavigator-control-windows-forms.md)
- [ToolStrip コントロール](toolstrip-control-windows-forms.md)
