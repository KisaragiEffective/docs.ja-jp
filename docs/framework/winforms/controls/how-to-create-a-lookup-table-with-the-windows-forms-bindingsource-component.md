---
title: BindingSource コンポーネントを使用したルックアップテーブルの作成
ms.date: 03/30/2017
helpviewer_keywords:
- lookup tables
- tables [Windows Forms], creating lookup tables
- BindingSource component [Windows Forms], creating a lookup table
- BindingSource component [Windows Forms], examples
ms.assetid: 622fce80-879d-44be-abbf-8350ec22ca2b
ms.openlocfilehash: ccf2bfa6cf3f56a38b55f8c87004c42a46172891
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76736814"
---
# <a name="how-to-create-a-lookup-table-with-the-windows-forms-bindingsource-component"></a>方法 : Windows フォーム BindingSource コンポーネントを使用してルックアップ テーブルを作成する
ルックアップ テーブルは、関連するテーブル内のレコードのデータを表示する列を持つ、データ テーブルです。 以下の手順では、<xref:System.Windows.Forms.ComboBox> コントロールを使用して、親テーブルから子テーブルへの外部キー リレーションシップを持つフィールドを表示します。  
  
 これらの 2 つのテーブルとこの関係をわかりやすく視覚化するために、親テーブルと子テーブルの例を次に示します。  
  
 CustomersTable (親テーブル)  
  
|CustomerID|CustomerName|  
|----------------|------------------|  
|712|Paul Koch|  
|713|Tamara Johnston|  
  
 OrdersTable (子テーブル)  
  
|OrderID|OrderDate|CustomerID|  
|-------------|---------------|----------------|  
|903|February 12, 2004|712|  
|904|February 13, 2004|713|  
  
 このシナリオでは、一方のテーブル (CustomersTable) に、表示および保存する実際の情報を格納します。 ただし領域を節約するために、このテーブルには情報を明確化するデータは含まれていません。 もう一方のテーブル (OrdersTable) には、どの顧客 ID 番号がどの発注日および発注 ID に相当するか、について表示関連の情報のみが格納されます。 顧客名の記述は含まれません。  
  
 ルックアップ テーブルを作成するには、[ComboBox コントロール](combobox-control-windows-forms.md)で次の 4 つの重要なプロパティを設定します。  
  
- <xref:System.Windows.Forms.ComboBox.DataSource%2A> プロパティには、テーブルの名前が格納されます。  
  
- <xref:System.Windows.Forms.ListControl.DisplayMember%2A> プロパティには、コントロール テキスト (顧客名) に対して表示する、テーブルのデータ列が格納されます。  
  
- <xref:System.Windows.Forms.ListControl.ValueMember%2A> プロパティには、格納された情報 (親テーブルの ID 番号) を持つ、テーブルのデータ列が格納されます。  
  
- <xref:System.Windows.Forms.ListControl.SelectedValue%2A> プロパティは、<xref:System.Windows.Forms.ListControl.ValueMember%2A> に基づいて子テーブルのルックアップ値を提供します。  
  
 以下の手順では、フォームをルックアップ テーブルとしてレイアウトし、フォームのコントロールにデータをバインドする方法を示します。 この手順を完了するには、データ ソース、および前述の外部キー リレーションシップを持つ親テーブルと子テーブルから構成されるデータ ソースが必要です。  
  
### <a name="to-create-the-user-interface"></a>ユーザー インターフェイスを作成するには  
  
1. **[ツールボックス]** から <xref:System.Windows.Forms.ComboBox> コントロールをフォームにドラッグします。  
  
     このコントロールは、親テーブルの列を表示します。  
  
2. 子テーブルの詳細を表示する他のコントロールをドラッグします。 テーブル内のデータの形式によって、選択するコントロールが決まります。 詳細については、「[Windows フォーム コントロールの機能別一覧](windows-forms-controls-by-function.md)」を参照してください。  
  
3. <xref:System.Windows.Forms.BindingNavigator> コントロールをフォームにドラッグします。これにより、子テーブル内のデータを移動できるようになります。  
  
### <a name="to-connect-to-the-data-and-bind-it-to-controls"></a>データに接続し、コントロールにバインドするには  
  
1. <xref:System.Windows.Forms.ComboBox> を選択し、スマート タスク グリフをクリックして [スマート タスク] ダイアログ ボックスを表示します。  
  
2. **[データ バインド項目を使用する]** を選択します。  
  
3. **[データ ソース]** ドロップダウン ボックスの横の矢印をクリックします。 プロジェクトまたはフォームに対してデータ ソースがすでに構成されている場合はデータ ソースが表示されますが、設定されていない場合は、次の手順を実行します (この例では、Northwind サンプル データベースの Customers テーブルと Orders テーブルを使用します。これらのテーブルについては括弧の中で参照します)。  
  
    1. **[プロジェクト データ ソースの追加]** をクリックしてデータに接続し、データ ソースを作成します。  
  
    2. **データ ソース構成ウィザード**の開始ページで **[次へ]** をクリックします。  
  
    3. **[データソースの種類を選択]** ページで、 **[データベース]** をクリックします。  
  
    4. **[データ接続の選択]** ページの利用可能な接続の一覧から、データ接続を選択します。 目的のデータ接続を選択できない場合は、 **[新しい接続]** を選択して新しいデータ接続を作成します。  
  
    5. **[次の名前で接続を保存する]** をオンにして、接続文字列をアプリケーション構成ファイルに保存します。  
  
    6. アプリケーションで使用するデータベース オブジェクトを選択します。 この場合、外部キー リレーションシップを持つ親テーブルと子テーブル (Customers と Orders など) を選択します。  
  
    7. 必要な場合は、既定のデータセット名を変更します。  
  
    8. **[完了]** をクリックします。  
  
4. **[表示メンバー]** ドロップダウン ボックスで、コンボ ボックスに表示する列名 (ContactName など) を選択します。  
  
5. **[値メンバー]** ドロップダウン ボックスで、子テーブルでルックアップ操作を実行する列 (CustomerID など) を選択します。  
  
6. **[選択された値]** ドロップダウン ボックスで **[プロジェクト データ ソース]** に移動し、親テーブルと子テーブルを含む、作成したばかりのデータセットに移動します。 親テーブルの Value Member に対応する、子テーブルのプロパティ (Orders.CustomerID など) を選択します。 適切な <xref:System.Windows.Forms.BindingSource>、データ セット、およびテーブル アダプタ コンポーネントが作成され、フォームに追加されます。  
  
7. <xref:System.Windows.Forms.BindingNavigator> コントロールを子テーブルの <xref:System.Windows.Forms.BindingSource> (`OrdersBindingSource` など) にバインドします。  
  
8. <xref:System.Windows.Forms.ComboBox> および <xref:System.Windows.Forms.BindingNavigator> コントロール以外のコントロールを、表示する子テーブルの <xref:System.Windows.Forms.BindingSource> (`OrdersBindingSource` など) の詳細フィールドにバインドします。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.BindingSource>
- [BindingSource コンポーネント](bindingsource-component.md)
- [ComboBox コントロール](combobox-control-windows-forms.md)
- [Visual Studio でのデータへのコントロールのバインド](/visualstudio/data-tools/bind-controls-to-data-in-visual-studio)
