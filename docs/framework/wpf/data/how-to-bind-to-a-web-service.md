---
title: '方法 : Web サービスにバインドする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- binding data [WPF], Web service
- Web service binding [WPF]
- data binding [WPF], Web service
ms.assetid: 77e2d373-69ba-4cbd-b6f5-2c83c38fc98b
ms.openlocfilehash: 3a3f6edc974448ddab9fe30e97bdc1130d3b97dc
ms.sourcegitcommit: 700ea803fb06c5ce98de017c7f76463ba33ff4a9
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/19/2020
ms.locfileid: "77449973"
---
# <a name="how-to-bind-to-a-web-service"></a>方法 : Web サービスにバインドする
この例では、Web サービスメソッド呼び出しによって返されるオブジェクトにバインドする方法を示します。  
  
## <a name="example"></a>例  
 この例では、MSDN/TechNet Publishing System (MTPS) コンテンツサービスを使用して、指定されたドキュメントでサポートされている言語の一覧を取得します。  
  
 Web サービスを呼び出す前に、その Web サービスへの参照を作成する必要があります。 Visual Studio を使用して MTPS サービスへの Web 参照を作成するには、次の手順に従います。  
  
1. Visual Studio でプロジェクトを開きます。  
  
2. **[プロジェクト]** メニューの **[Web 参照の追加]** をクリックします。  
  
3. ダイアログボックスで、 **URL**を[http://services.msdn.microsoft.com/contentservices/contentservice.asmx?wsdl](https://services.msdn.microsoft.com/contentservices/contentservice.asmx?wsdl)に設定します。  
  
4. [実行 **] をクリックし、[** 参照の**追加**] をクリックします。  
  
 次に、Web サービスメソッドを呼び出し、適切なコントロールまたはウィンドウの <xref:System.Windows.FrameworkElement.DataContext%2A> を、返されたオブジェクトに設定します。 MTPS サービスの `GetContent` メソッドは、`getContentRequest` オブジェクトへの参照を受け取ります。 したがって、次の例では、最初に要求オブジェクトを設定します。  
  
 [!code-csharp[BindToWebService#Namespace](~/samples/snippets/csharp/VS_Snippets_Wpf/BindToWebService/CSharp/Window1.xaml.cs#namespace)]
 [!code-vb[BindToWebService#Namespace](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BindToWebService/VisualBasic/Window1.xaml.vb#namespace)]  
[!code-csharp[BindToWebService#WebServiceCall](~/samples/snippets/csharp/VS_Snippets_Wpf/BindToWebService/CSharp/Window1.xaml.cs#webservicecall)]
[!code-vb[BindToWebService#WebServiceCall](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BindToWebService/VisualBasic/Window1.xaml.vb#webservicecall)]  
  
 <xref:System.Windows.FrameworkElement.DataContext%2A> が設定されたら、<xref:System.Windows.FrameworkElement.DataContext%2A> が設定されているオブジェクトのプロパティへのバインドを作成できます。 この例では、<xref:System.Windows.FrameworkElement.DataContext%2A> は `GetContent` メソッドによって返される `getContentResponse` オブジェクトに設定されます。 次の例では、<xref:System.Windows.Controls.ItemsControl> をにバインドし、`getContentResponse`の `availableVersionsAndLocales` の `locale` 値を表示します。  
  
 [!code-xaml[BindToWebService#Binding](~/samples/snippets/csharp/VS_Snippets_Wpf/BindToWebService/CSharp/Window1.xaml#binding)]  
  
 `getContentResponse`の構造の詳細については、[コンテンツサービスのドキュメント](https://services.msdn.microsoft.com/ContentServices/ContentService.asmx)を参照してください。  
  
## <a name="see-also"></a>参照

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [バインディング ソースの概要](binding-sources-overview.md)
- [XAML でデータをバインディング可能にする](how-to-make-data-available-for-binding-in-xaml.md)
