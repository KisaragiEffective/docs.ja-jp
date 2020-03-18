---
title: '方法: Windows Presentation Foundation 要素にデータをバインドする (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data binding, WCF Data Services
- WCF Data Services, data binding
ms.assetid: d6538ab0-0abe-426a-b9d9-e6f3a5ca2016
ms.openlocfilehash: b623dc10bfe1b723c62b2861b4142a138904e64a
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569339"
---
# <a name="how-to-bind-data-to-windows-presentation-foundation-elements-wcf-data-services"></a>方法: Windows Presentation Foundation 要素にデータをバインドする (WCF Data Services)
WCF Data Services を使用すると、<xref:System.Windows.Controls.ListBox> や <xref:System.Windows.Controls.ComboBox> などの Windows Presentation Foundation (WPF) 要素を <xref:System.Data.Services.Client.DataServiceCollection%601>のインスタンスにバインドできます。これは、コントロールによって発生したイベントを処理して、コントロールのデータに加えられた変更と <xref:System.Data.Services.Client.DataServiceContext> 同期を維持します。 詳細については、「[コントロールへのデータのバインド](binding-data-to-controls-wcf-data-services.md)」を参照してください。  
  
 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の例は、WPF で `SalesOrders` ウィンドウを定義する XAML (Extensible Application Markup Language) の分離コード ページからの抜粋です。 ウィンドウが読み込まれると、国/地域でフィルター処理された顧客を返すクエリの結果に基づいて <xref:System.Data.Services.Client.DataServiceCollection%601> が作成されます。 このページングされた結果のすべてのページが関連する注文と一緒に読み込まれ、WPF ウィンドウのルート レイアウト コントロールである <xref:System.Windows.FrameworkElement.DataContext%2A> の <xref:System.Windows.Controls.StackPanel> プロパティにバインドされます。 詳細については、「[遅延コンテンツの読み込み](loading-deferred-content-wcf-data-services.md)」を参照してください。  
  
 [!code-csharp[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderswpf3.xaml.cs#bindpageddata)]
 [!code-vb[Astoria Northwind Client#BindPagedData](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf3.xaml.vb#bindpageddata)]  
  
## <a name="example"></a>使用例  
 次の XAML は、前の例の WPF の `SalesOrders` ウィンドウを定義します。  
  
 [!code-xaml[Astoria Northwind Client#BindPagedDataXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf3.xaml#bindpageddataxaml)]  
  
## <a name="example"></a>使用例  
 次の例は、WPF で `SalesOrders` ウィンドウを定義する XAML (Extensible Application Markup Language) の分離コード ページからの抜粋です。 ウィンドウが読み込まれると、国/地域でフィルター処理された関連オブジェクトを持つ顧客を返すクエリの結果に基づいて <xref:System.Data.Services.Client.DataServiceCollection%601> が作成されます。 この結果は、WPF ウィンドウのルート レイアウト コントロールである <xref:System.Windows.FrameworkElement.DataContext%2A> の <xref:System.Windows.Controls.StackPanel> プロパティにバインドされます。  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderswpf.xaml.cs#wpfdatabinding)]
 [!code-vb[Astoria Northwind Client#WpfDataBinding](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf.xaml.vb#wpfdatabinding)]  
  
## <a name="example"></a>使用例  
 次の XAML は、前の例の WPF の `SalesOrders` ウィンドウを定義します。  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderswpf.xaml#wpfdatabindingxaml)]
