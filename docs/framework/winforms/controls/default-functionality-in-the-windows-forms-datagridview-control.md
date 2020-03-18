---
title: DataGridView コントロールの既定の機能
ms.date: 03/30/2017
helpviewer_keywords:
- data grids [Windows Forms], default functionality in DataGridView control
- DataGridView control [Windows Forms], default functionality
ms.assetid: 4405f697-cad1-4839-9bcd-8ddb09d9f00e
ms.openlocfilehash: b695883ac7ec3fb0c459adb66214b0eceab3a128
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76746133"
---
# <a name="default-functionality-in-the-windows-forms-datagridview-control"></a>Windows フォーム DataGridView コントロールの既定の機能
Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールにより、ユーザーは大量の既定機能を使用できます。  
  
## <a name="default-functionality"></a>既定の機能  
 既定では、<xref:System.Windows.Forms.DataGridView> コントロールです。  
  
- テーブルを垂直方向にスクロールしても列ヘッダーおよび行ヘッダーが見える状態のまま自動的に表示します。  
  
- 現在の行を示す選択を含む行ヘッダーがあります。  
  
- 最初のセルに選択を示す四角形があります。  
  
- ユーザーが列の区切り線をダブルクリックすると、自動的にサイズを変更できる列があります。  
  
- では、アプリケーションの `Main` メソッドから <xref:System.Windows.Forms.Application.EnableVisualStyles%2A> メソッドが呼び出されたときに、Windows XP と Windows Server 2003 ファミリの visual スタイルが自動的にサポートされます。  
  
 また、既定では、<xref:System.Windows.Forms.DataGridView> コントロールの内容を編集できます。  
  
- ユーザーがダブルクリックするか、セル中で F2 キーを押した場合、コントロールは自動的にセルを編集モードにし、ユーザーが入力したセルの内容を更新します。  
  
- ユーザーがグリッドの最後までスクロールすると、新しいレコードを追加するための行が存在することがわかります。 ユーザーがこの行をクリックすると、<xref:System.Windows.Forms.DataGridView> コントロールに新しい行が追加され、既定値が使用されます。 ユーザーが ESC キーを押すと、この新しい行は消えます。  
  
- ユーザーが行ヘッダーをクリックすると、行全体が選択されます。  
  
 <xref:System.Windows.Forms.DataGridView.DataSource%2A> プロパティを設定して、<xref:System.Windows.Forms.DataGridView> コントロールをデータソースにバインドする場合、コントロールは次のようになります。  
  
- では、データソースの列の名前が列ヘッダーのテキストとして自動的に使用されます。  
  
- には、データソースの内容が設定されます。 <xref:System.Windows.Forms.DataGridView> 列は、データソースの各列に対して自動的に作成されます。  
  
- テーブル内の表示されている各行に対して行を作成します。  
  
- は、ユーザーが列ヘッダーをクリックしたときに、基になるデータに基づいて行を自動的に並べ替えます。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.DataGridView>
- [DataGridView コントロール](datagridview-control-windows-forms.md)
