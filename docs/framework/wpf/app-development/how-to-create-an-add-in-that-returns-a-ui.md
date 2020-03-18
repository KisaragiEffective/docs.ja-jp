---
title: '方法 : UI を返すアドインを作成する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- creating an add-in that returns a UI [WPF]
- implementing add-in pipeline segments [WPF]
- add-in [WPF], returns a UI
ms.assetid: 57f274b7-4c66-4b72-92eb-81939a393776
ms.openlocfilehash: d799c91b9abdf7882a0fcd3f0b656eac553b188c
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73460147"
---
# <a name="how-to-create-an-add-in-that-returns-a-ui"></a>方法 : UI を返すアドインを作成する
この例では、Windows Presentation Foundation (WPF) をホスト WPF スタンドアロンアプリケーションに返すアドインを作成する方法を示します。  
  
 このアドインは、WPF ユーザーコントロールの UI を返します。 ユーザー コントロールの中身は 1 つのボタンであり、クリックすると、メッセージ ボックスを表示します。 WPF スタンドアロンアプリケーションは、アドインをホストし、アドインによって返されたユーザーコントロールをメインアプリケーションウィンドウの内容として表示します。  
  
 **必須コンポーネント**  
  
 この例では、このシナリオを有効にする .NET Framework アドインモデルの WPF 拡張機能に焦点を当て、次のことを前提としています。  
  
- パイプライン、アドイン、およびホスト開発を含む .NET Framework アドインモデルに関する知識。 これらの概念についてよく知らない場合は、「[アドインと拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))」を参照してください。 パイプライン、アドイン、およびホストアプリケーションの実装方法を示すチュートリアルについては、「[チュートリアル: 拡張可能なアプリケーションの作成](/previous-versions/dotnet/netframework-4.0/bb788290(v%3dvs.100))」を参照してください。  
  
- .NET Framework アドインモデルに対する WPF 拡張機能について説明します。 [Wpf アドインの概要](wpf-add-ins-overview.md)については、こちらを参照してください。  
  
## <a name="example"></a>例  
 WPF UI を返すアドインを作成するには、各パイプラインセグメント、アドイン、およびホストアプリケーションに特定のコードが必要です。  

<a name="Contract"></a>   
## <a name="implementing-the-contract-pipeline-segment"></a>コントラクト パイプライン セグメントの実装  
 UI を返すには、コントラクトによってメソッドが定義されている必要があります。また、戻り値の型は <xref:System.AddIn.Contract.INativeHandleContract>である必要があります。 これは、次のコードの `IWPFAddInContract` コントラクトの `GetAddInUI` メソッドによって示されます。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#ContractCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/Contracts/IWPFAddInContract.cs#contractcode)]
 [!code-vb[SimpleAddInReturnsAUISample#ContractCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/Contracts/IWPFAddInContract.vb#contractcode)]  
  
<a name="AddInView"></a>   
## <a name="implementing-the-add-in-view-pipeline-segment"></a>アドイン ビュー パイプライン セグメントの実装  
 アドインは、<xref:System.Windows.FrameworkElement>のサブクラスとして提供する Ui を実装するため、`IWPFAddInView.GetAddInUI` に関連付けられたアドインビューのメソッドは <xref:System.Windows.FrameworkElement>型の値を返す必要があります。 次のコードは、コントラクトのアドイン ビューがインターフェイスとして実装されることを示しています。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#AddInViewCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/AddInViews/IWPFAddInView.cs#addinviewcode)]
 [!code-vb[SimpleAddInReturnsAUISample#AddInViewCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/AddInViews/IWPFAddInView.vb#addinviewcode)]  
  
<a name="AddInSideAdapter"></a>   
## <a name="implementing-the-add-in-side-adapter-pipeline-segment"></a>アドイン側アダプター パイプライン セグメントの実装  
 コントラクトメソッドは <xref:System.AddIn.Contract.INativeHandleContract>を返しますが、アドインは、(アドインビューで指定されているように) <xref:System.Windows.FrameworkElement> を返します。 したがって、分離境界を越える前に、<xref:System.Windows.FrameworkElement> を <xref:System.AddIn.Contract.INativeHandleContract> に変換する必要があります。 この作業は、次のコードに示すように、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ViewToContractAdapter%2A>を呼び出すことによってアドイン側のアダプターによって実行されます。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#AddInSideAdapterCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/AddInSideAdapters/WPFAddIn_ViewToContractAddInSideAdapter.cs#addinsideadaptercode)]
 [!code-vb[SimpleAddInReturnsAUISample#AddInSideAdapterCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/AddInSideAdapters/WPFAddIn_ViewToContractAddInSideAdapter.vb#addinsideadaptercode)]  
  
<a name="HostView"></a>   
## <a name="implementing-the-host-view-pipeline-segment"></a>ホスト ビュー パイプライン セグメントの実装  
 ホストアプリケーションには <xref:System.Windows.FrameworkElement>が表示されるため、`IWPFAddInHostView.GetAddInUI` に関連付けられているホストビューのメソッドは <xref:System.Windows.FrameworkElement>型の値を返す必要があります。 次のコードは、コントラクトのホスト ビューがインターフェイスとして実装されることを示しています。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#HostViewCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/HostViews/IWPFAddInHostView.cs#hostviewcode)]
 [!code-vb[SimpleAddInReturnsAUISample#HostViewCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/HostViews/IWPFAddInHostView.vb#hostviewcode)]  
  
<a name="HostSideAdapter"></a>   
## <a name="implementing-the-host-side-adapter-pipeline-segment"></a>ホスト側アダプター パイプライン セグメントの実装  
 コントラクトメソッドは <xref:System.AddIn.Contract.INativeHandleContract>を返しますが、ホストアプリケーションは、(ホストビューで指定されているように) <xref:System.Windows.FrameworkElement> を必要とします。 したがって、分離境界を越えた後、<xref:System.AddIn.Contract.INativeHandleContract> を <xref:System.Windows.FrameworkElement> に変換する必要があります。 この作業は、次のコードに示すように、<xref:System.AddIn.Pipeline.FrameworkElementAdapters.ContractToViewAdapter%2A>を呼び出すことによってホスト側アダプターによって実行されます。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#HostSideAdapterCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/HostSideAdapters/WPFAddIn_ContractToViewHostSideAdapter.cs#hostsideadaptercode)]
 [!code-vb[SimpleAddInReturnsAUISample#HostSideAdapterCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/HostSideAdapters/WPFAddIn_ContractToViewHostSideAdapter.vb#hostsideadaptercode)]  
  
<a name="AddIn"></a>   
## <a name="implementing-the-add-in"></a>アドインの実装  
 アドイン側のアダプターとアドインビューを作成した場合、アドイン (`WPFAddIn1.AddIn`) は、<xref:System.Windows.FrameworkElement> オブジェクト (この例では <xref:System.Windows.Controls.UserControl>) を返すために `IWPFAddInView.GetAddInUI` メソッドを実装する必要があります。 <xref:System.Windows.Controls.UserControl>の実装 (`AddInUI`) を次のコードに示します。  
  
 [!code-xaml[SimpleAddInReturnsAUISample#AddInUIMarkup](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/WPFAddIn1/AddInUI.xaml#addinuimarkup)]  
  
 [!code-csharp[SimpleAddInReturnsAUISample#AddInUICodeBehind](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/WPFAddIn1/AddInUI.xaml.cs#addinuicodebehind)]
 [!code-vb[SimpleAddInReturnsAUISample#AddInUICodeBehind](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/WPFAddIn1/AddInUI.xaml.vb#addinuicodebehind)]  
  
 アドインによる `IWPFAddInView.GetAddInUI` の実装では、次のコードに示すように、単に `AddInUI`の新しいインスタンスを返す必要があります。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#AddInCode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/WPFAddIn1/AddIn.cs#addincode)]
 [!code-vb[SimpleAddInReturnsAUISample#AddInCode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/WPFAddIn1/AddIn.vb#addincode)]  
  
<a name="App"></a>   
## <a name="implementing-the-host-application"></a>ホスト アプリケーションの実装  
 ホスト側のアダプターとホストビューを作成すると、ホストアプリケーションは .NET Framework アドインモデルを使用してパイプラインを開き、アドインのホストビューを取得して、`IWPFAddInHostView.GetAddInUI` メソッドを呼び出すことができます。 これらの手順を次のコードに示します。  
  
 [!code-csharp[SimpleAddInReturnsAUISample#GetUICode](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/CSharp/Host/MainWindow.xaml.cs#getuicode)]
 [!code-vb[SimpleAddInReturnsAUISample#GetUICode](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleAddInReturnsAUISample/VisualBasic/Host/MainWindow.xaml.vb#getuicode)]  
  
## <a name="see-also"></a>関連項目

- [アドインおよび拡張機能](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/bb384200(v%3dvs.100))
- [WPF のアドインの概要](wpf-add-ins-overview.md)
