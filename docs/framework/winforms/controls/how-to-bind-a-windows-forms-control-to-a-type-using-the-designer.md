---
title: デザイナーを使用してコントロールを型にバインドする
ms.date: 03/30/2017
helpviewer_keywords:
- controls [Windows Forms], binding to a type
- BindingSource component [Windows Forms], binding to a type
- types [Windows Forms], binding controls to
ms.assetid: 5ab984b5-c2d0-4638-a572-1c84013e8746
ms.openlocfilehash: 2257489e123ceeea819ad3538952db51b726c7e5
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742019"
---
# <a name="how-to-bind-a-windows-forms-control-to-a-type-using-the-designer"></a>方法 : デザイナーを使って Windows フォーム コントロールを型にバインドする

データをやり取りするコントロールを作成する際、オブジェクトではなく型にコントロールをバインドすることが必要な場合があります。 データを使用できないが、データ バインド コントロールで型のパブリック インターフェイスからデータを表示する必要がある場合、通常はデザイン時にコントロールを型にバインドする必要があります。 次の手順では、型にバインドされている新しい <xref:System.Windows.Forms.BindingSource> を作成し、その型のプロパティの1つを <xref:System.Windows.Forms.TextBox>の <xref:System.Windows.Forms.TextBox.Text%2A> プロパティにバインドする方法について説明します。

### <a name="to-bind-the-bindingsource-to-a-type"></a>BindingSource を型にバインドするには

1. Windows フォームプロジェクトを作成します (**ファイル** > **新しい** > **プロジェクト** >  **C# Visual**または**Visual Basic** > **クラシックデスクトップ** > **Windows フォームアプリケーション**)。

2. **デザイン**ビューで、<xref:System.Windows.Forms.BindingSource> コンポーネントをフォームにドラッグします。

3. **[プロパティ]** ウィンドウで、[<xref:System.Windows.Forms.BindingSource.DataSource%2A>] プロパティの矢印をクリックします。

4. **DataSource UI 型エディター**で、 **[プロジェクト データ ソースの追加]** をクリックします。

5. **[データ ソースの種類を選択]** ページで、 **[オブジェクト]** を選択して **[次へ]** をクリックします。

6. バインド先となる型を選択します。

    - バインド先となる型が現在のプロジェクトにある場合、または、型を含むアセンブリが参照として既に追加されている場合は、ノードを展開し、目的の型を探して選択します。

      \- または -

    - バインド先の型が、現在参照の一覧にない別のアセンブリにある場合は、 **[参照の追加]** をクリックし、 **[プロジェクト]** タブをクリックします。必要なビジネスオブジェクトが含まれているプロジェクトを選択し、 **[OK]** をクリックします。 このプロジェクトは、アセンブリの一覧に表示されるため、ノードを展開し、目的の型を探して選択します。

      > [!NOTE]
      > フレームワークまたは Microsoft アセンブリの型にバインドする場合は、 **[Microsoft または System で始まるアセンブリを表示しない]** チェックボックスをオフにします。

7. **[次へ]** をクリックし、 **[完了]** をクリックします。

### <a name="to-bind-the-control-to-the-bindingsource"></a>コントロールを BindingSource にバインドするには

1. フォームに <xref:System.Windows.Forms.TextBox> を追加します。

2. **[プロパティ]** ウィンドウで **(DataBindings)** ノードを展開します。

3. <xref:System.Windows.Forms.TextBox.Text%2A> プロパティの横にある矢印をクリックします。

4. **DATASOURCE UI 型エディター**で、前に追加した <xref:System.Windows.Forms.BindingSource> のノードを展開し、<xref:System.Windows.Forms.TextBox>の <xref:System.Windows.Forms.TextBox.Text%2A> プロパティにバインドするバインドされた型のプロパティを選択します。

## <a name="see-also"></a>参照

- [BindingSource コンポーネント](bindingsource-component.md)
- [方法: Windows フォーム コントロールを型にバインドする](how-to-bind-a-windows-forms-control-to-a-type.md)
- [Visual Studio でのデータへのコントロールのバインド](/visualstudio/data-tools/bind-controls-to-data-in-visual-studio)
