---
title: データ バインディング
ms.date: 03/30/2017
helpviewer_keywords:
- master-details lists
- data [Windows Forms], data binding
- reports [Windows Forms], Windows Forms
- lookup tables [Windows Forms], data binding
- data [Windows Forms], complex data binding
- data binding [Windows Forms], Windows Forms
- data [Windows Forms], simple data binding
- Windows Forms controls, data binding
- data-bound controls [Windows Forms], Windows Forms
ms.assetid: 419aac5e-819b-4aad-88b0-73a2f8c0bd27
ms.openlocfilehash: c5cb16d57dde35ca3b243191f27ea118cf19a680
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742320"
---
# <a name="data-binding-and-windows-forms"></a>データ連結と Windows フォーム
Windows フォームでは、従来のデータ ソースだけでなく、データを含むほぼすべての構造にバインドできます。 実行時に計算する値、ファイルから読み取る値、または他のコントロールの値から派生する値の配列にバインドできます。  
  
 さらに、任意のコントロールのプロパティをデータ ソースにバインドできます。 従来のデータ バインディングでは、通常は <xref:System.Windows.Forms.Control.Text%2A> コントロールの <xref:System.Windows.Forms.TextBox> プロパティなどの表示プロパティをデータ ソースにバインドします。 .NET Framework では、バインディングを使用して他のプロパティを設定することもできます。 バインディングを使用して、次のタスクを実行できます。  
  
- イメージ コントロールのグラフィックの設定。  
  
- 1 つ以上のコントロールの背景色の設定。  
  
- コントロール サイズの設定。  
  
 基本的には、データ バインディングは、フォーム上の任意のコントロールの実行時にアクセス可能なプロパティを自動的に設定する方法です。  
  
## <a name="types-of-data-binding"></a>データ バインディングの種類  
 Windows フォームでは、単純バインディングと複合バインディングの 2 種類のデータ バインディングを利用できます。 それぞれに異なる利点があります。  
  
|データ バインディングの種類|[説明]|  
|--------------------------|-----------------|  
|単純データ バインディング|データセット テーブル内の列の値など、1 つのデータ要素にバインドするコントロールの機能。 これは、<xref:System.Windows.Forms.TextBox> コントロールや <xref:System.Windows.Forms.Label> コントロールなどのコントロールで一般的に使用される種類のバインディングです。これらのコントロールでは通常、1 つの値のみが表示されます。 実際には、コントロールのどのプロパティもデータベース内のフィールドにバインドできます。 この機能は Visual Studio で幅広くサポートされています。<br /><br /> 詳細については、次を参照してください。<br /><br /> [データバインディングに関連する -   インターフェイス](interfaces-related-to-data-binding.md)<br />-   [方法: Windows フォーム内のデータを移動する](how-to-navigate-data-in-windows-forms.md)<br />-   [方法: Windows フォームに単純バインドコントロールを作成](how-to-create-a-simple-bound-control-on-a-windows-form.md)する|  
|複合データ バインディング|複数のデータ要素、一般的にはデータベース内の複数のレコードにバインドするコントロールの機能。 複合バインディングは、リストベース バインディングとも呼ばれます。 複合バインディングをサポートするコントロールの例には、<xref:System.Windows.Forms.DataGridView>、<xref:System.Windows.Forms.ListBox>、および <xref:System.Windows.Forms.ComboBox> の各コントロールがあります。 複合データバインディングの例については、「[方法: Windows フォーム ComboBox または ListBox コントロールをデータにバインド](./controls/how-to-bind-a-windows-forms-combobox-or-listbox-control-to-data.md)する」を参照してください。|  
  
## <a name="bindingsource-component"></a>BindingSource コンポーネント  
 データ バインディングを単純化するために、Windows フォームでは、データ ソースを <xref:System.Windows.Forms.BindingSource> コンポーネントにバインドしてから、コントロールを <xref:System.Windows.Forms.BindingSource> にバインドできます。 <xref:System.Windows.Forms.BindingSource> は、単純バインディングまたは複合バインディングのシナリオで使用できます。 いずれの場合も、<xref:System.Windows.Forms.BindingSource> はデータ ソースとバインドされたコントロールの間の媒介として機能し、変更通知、通貨管理などのサービスを提供します。  
  
## <a name="common-scenarios-that-employ-data-binding"></a>データ バインディングを使用する一般的なシナリオ  
 ほぼすべての商用アプリケーションで、通常はデータ バインディングによってある種類のデータ ソースから別の種類のデータ ソースに読み取られた情報を使用します。 次のリストに、データの表示および操作の方法としてデータ バインディングを使用する最も一般的なシナリオをいくつか示します。  
  
|シナリオ|[説明]|  
|--------------|-----------------|  
|レポーティング|レポートは、印刷された文書でデータを表示および集計する柔軟な方法を提供します。 データ ソースの選択したコンテンツを画面またはプリンターに出力するレポートを作成することは、非常に一般的に行われます。 一般的なレポートには、リスト、請求書、および概要などがあります。 通常、項目はリストの列として並べられ、サブ項目が各リスト項目に分類されますが、データに最も適したレイアウトをユーザーが選択する必要があります。|  
|データ エントリ|大量の関連データを入力したり、ユーザーに情報の入力を求めたりする場合は、データ エントリ フォームを使用する方法が一般的です。 ユーザーは、テキスト ボックス、オプション ボタン、ドロップダウン リスト、およびチェック ボックスを使用して、情報を入力したり、オプションを選択したりできます。 情報は送信され、入力された情報に基づく構造を持つデータベースに格納されます。|  
|マスター/詳細リレーションシップ|マスター/詳細アプリケーションは、関連データを表現するための形式の 1 つです。 具体的には、2 つのデータ テーブルと、それらを結合する関係があります。典型的なビジネスの例では、"顧客" テーブルと "注文" テーブルがあり、それらの間には、顧客とそれに対応する注文を結合する関係があります。 2つの Windows フォーム <xref:System.Windows.Forms.DataGridView> コントロールを使用してマスター/詳細アプリケーションを作成する方法の詳細については、「[方法: 2 つの Windows フォーム DataGridView コントロールを使用してマスター/詳細フォームを作成](./controls/create-a-master-detail-form-using-two-datagridviews.md)する」を参照してください。|  
|参照テーブル|別の一般的なデータ表示/操作シナリオに、テーブル ルックアップがあります。 多くの場合、より大きなデータ表示の一部として、<xref:System.Windows.Forms.ComboBox> コントロールを使用してデータを表示および操作します。 重要な点は、<xref:System.Windows.Forms.ComboBox> コントロールで表示されるデータは、データベースに書き込まれるデータとは異なることです。 たとえば、食料品店から入手できる項目を表示する <xref:System.Windows.Forms.ComboBox> コントロールがあり、製品の名前 (パン、牛乳、卵) を表示するとします。 しかし、データベース内での情報の取得を容易にしたり、データベースを正規化したりするには、ある注文の特定の項目に関する情報を項目番号 (#501、#603 など) で格納する必要がある場合があります。 そのため、フォームの <xref:System.Windows.Forms.ComboBox> コントロールにある食料品項目の "わかりやすい名前" と、注文に表示される該当する項目番号との間には暗黙的なつながりがあります。 これがテーブル ルックアップの本質です。 詳細については、「[方法: Windows フォーム BindingSource コンポーネントを使用してルックアップテーブルを作成](./controls/how-to-create-a-lookup-table-with-the-windows-forms-bindingsource-component.md)する」を参照してください。|  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.Binding>
- [Windows フォームでのデータ バインディング](windows-forms-data-binding.md)
- [方法: データ ソースに Windows フォーム DataGrid コントロールをバインドする](./controls/how-to-bind-the-windows-forms-datagrid-control-to-a-data-source.md)
- [BindingSource コンポーネント](./controls/bindingsource-component.md)
