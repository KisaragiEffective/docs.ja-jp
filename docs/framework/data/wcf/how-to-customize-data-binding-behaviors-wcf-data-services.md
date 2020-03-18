---
title: '方法: データ バインドの動作をカスタマイズする (WCF Data Services)'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF Data Services, customizing
- WCF Data Services, data binding
ms.assetid: 40476b89-8941-4771-8d21-2fe430c85a9d
ms.openlocfilehash: 453562dd1b86756bf9fc1684dc649dba1c24c578
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/28/2019
ms.locfileid: "74569174"
---
# <a name="how-to-customize-data-binding-behaviors-wcf-data-services"></a>方法: データ バインドの動作をカスタマイズする (WCF Data Services)
WCF Data Services では、バインディングコレクションに対してオブジェクトが追加または削除されたとき、またはプロパティの変更が検出されたときに、<xref:System.Data.Services.Client.DataServiceCollection%601> によって呼び出されるカスタムロジックを指定できます。 このカスタムロジックは、<xref:System.Func%602> デリゲートとして参照されるメソッドとして提供されます。このメソッドは、カスタムメソッドの完了時に既定の動作を実行して、イベントの後続の処理を停止する必要がある場合に `true` するときに、既定の動作を実行する必要がある場合に `false` を返します。  
  
 このトピックの例は、`entityChanged` の `entityCollectionChanged` パラメーターと <xref:System.Data.Services.Client.DataServiceCollection%601> パラメーターの両方のカスタム メソッドを示します。 このトピックの例では、Northwind サンプル データ サービスおよび自動生成されたクライアント データ サービス クラスを使用します。 このサービスとクライアントデータクラスは、 [WCF Data Services のクイックスタート](quickstart-wcf-data-services.md)を完了したときに作成されます。  
  
## <a name="example"></a>使用例  
 次の XAML ファイルの分離コード ページは、バインディング コレクションにバインドされたデータへの変更が発生すると呼び出される <xref:System.Data.Services.Client.DataServiceCollection%601> とカスタム メソッドを作成します。 <xref:System.Collections.ObjectModel.ObservableCollection%601.CollectionChanged> イベントが発生すると、指定したメソッドは、バインディング コレクションから削除された項目がデータ サービスから削除されることを防止します。 <xref:System.Collections.ObjectModel.ObservableCollection%601.PropertyChanged> イベントが発生すると、出荷済みの注文に対する変更がないことを確認するために、`ShipDate` の値が検証されます。  
  
 [!code-csharp[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/csharp/VS_Snippets_Misc/astoria_northwind_client/cs/customerorderscustom.xaml.cs#wpfdatabindingcustom)]
 [!code-vb[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom.xaml.vb#wpfdatabindingcustom)]
 [!code-vb[Astoria Northwind Client#WpfDataBindingCustom](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom2.xaml.vb#wpfdatabindingcustom)]  
  
## <a name="example"></a>使用例  
 次の XAML は、前の例のウィンドウを定義します。  
  
 [!code-xaml[Astoria Northwind Client#WpfDataBindingCustomXaml](../../../../samples/snippets/visualbasic/VS_Snippets_Misc/astoria_northwind_client/vb/customerorderscustom.xaml#wpfdatabindingcustomxaml)]  
  
## <a name="see-also"></a>参照

- [WCF Data Services クライアント ライブラリ](wcf-data-services-client-library.md)
