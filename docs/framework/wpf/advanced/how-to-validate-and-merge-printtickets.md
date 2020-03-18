---
title: '方法 : PrintTickets を検証およびマージする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- merging PrintTickets [WPF]
- PrintTicket [WPF], merging
- validation of PrintTickets [WPF]
- PrintTicket [WPF], validation
ms.assetid: 4fe2d501-d0b0-4fef-86af-6ffe6c162532
ms.openlocfilehash: bd7f399555b343a52ec6f36aa3b8c706747d8b06
ms.sourcegitcommit: 011314e0c8eb4cf4a11d92078f58176c8c3efd2d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/09/2020
ms.locfileid: "77094528"
---
# <a name="how-to-validate-and-merge-printtickets"></a>方法 : PrintTickets を検証およびマージする
Microsoft Windows[印刷スキーマ](/windows/win32/printdocs/printschema)には、柔軟で拡張可能な <xref:System.Printing.PrintCapabilities> と <xref:System.Printing.PrintTicket> の要素が含まれています。 前者は印刷デバイスの機能を示しています。後者では、デバイスが特定の順序のドキュメント、個々のドキュメント、または個々のページに対してこれらの機能をどのように使用するかを指定します。  
  
 印刷をサポートするアプリケーションの一般的なタスクシーケンスは、次のとおりです。  
  
1. プリンターの機能を確認します。  
  
2. これらの機能を使用するように <xref:System.Printing.PrintTicket> を構成します。  
  
3. <xref:System.Printing.PrintTicket>を検証します。  
  
 この記事では、これを行う方法について説明します。  
  
## <a name="example"></a>例  
 次の簡単な例では、プリンターが両面印刷をサポートしているかどうかのみを知りたいと考えています。 主な手順は次のとおりです。  
  
1. <xref:System.Printing.PrintQueue.GetPrintCapabilities%2A> メソッドを使用して <xref:System.Printing.PrintCapabilities> オブジェクトを取得します。  
  
2. 必要な機能があるかどうかをテストします。 次の例では、<xref:System.Printing.PrintCapabilities> オブジェクトの <xref:System.Printing.PrintCapabilities.DuplexingCapability%2A> プロパティをテストして、シートの横に "ページめくり" がある用紙の両面に印刷する機能があるかどうかを確認します。 <xref:System.Printing.PrintCapabilities.DuplexingCapability%2A> はコレクションであるため、<xref:System.Collections.ObjectModel.ReadOnlyCollection%601>の `Contains` メソッドを使用します。  
  
    > [!NOTE]
    > この手順は、厳密には必要ありません。 以下で使用する <xref:System.Printing.PrintQueue.MergeAndValidatePrintTicket%2A> 方法では、<xref:System.Printing.PrintTicket> 内の各要求をプリンターの機能に照らしてチェックします。 要求された機能がプリンターでサポートされていない場合、プリンタードライバーは、メソッドによって返された <xref:System.Printing.PrintTicket> の代替要求を置き換えます。  
  
3. プリンターが両面印刷をサポートしている場合は、このサンプルコードによって、二重化を要求する <xref:System.Printing.PrintTicket> が作成されます。 ただし、アプリケーションでは、<xref:System.Printing.PrintTicket> 要素で使用可能なすべてのプリンター設定が指定されていません。 これは、プログラマとプログラム時間の両方で無駄になります。 代わりに、このコードでは二重の要求のみを設定し、この <xref:System.Printing.PrintTicket> を、完全に構成および検証された既存の、<xref:System.Printing.PrintTicket>(この場合はユーザーの既定の <xref:System.Printing.PrintTicket>) とマージします。  
  
4. このサンプルでは、<xref:System.Printing.PrintQueue.MergeAndValidatePrintTicket%2A> メソッドを呼び出して、新しい、最小限の <xref:System.Printing.PrintTicket> をユーザーの既定の <xref:System.Printing.PrintTicket>にマージしています。 これにより、そのプロパティの1つとして新しい <xref:System.Printing.PrintTicket> を含む <xref:System.Printing.ValidationResult> が返されます。  
  
5. このサンプルでは、新しい <xref:System.Printing.PrintTicket> が二重に要求することをテストします。 存在する場合は、サンプルによってユーザーの既定の印刷チケットが新しく作成されます。 上記の手順 2. を省略した場合、プリンターでは、長辺に沿った両面印刷がサポートされていないと、テストの結果として `false`が発生します。 (上記のメモを参照してください)。  
  
6. 最後の重要な手順は、<xref:System.Printing.PrintQueue.Commit%2A> メソッドを使用して、<xref:System.Printing.PrintQueue> の <xref:System.Printing.PrintQueue.UserPrintTicket%2A> プロパティに対する変更をコミットすることです。  
  
 [!code-csharp[PrintTicketManagment#UsingMergeAndValidate](~/samples/snippets/csharp/VS_Snippets_Wpf/PrintTicketManagment/CSharp/printticket.cs#usingmergeandvalidate)]
 [!code-vb[PrintTicketManagment#UsingMergeAndValidate](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PrintTicketManagment/visualbasic/printticket.vb#usingmergeandvalidate)]  
  
 この例を簡単にテストできるように、この例の残りの部分を以下に示します。 プロジェクトと名前空間を作成し、この記事の両方のコードスニペットを名前空間ブロックに貼り付けます。  
  
 [!code-csharp[PrintTicketManagment#UIForMergeAndValidatePTUtility](~/samples/snippets/csharp/VS_Snippets_Wpf/PrintTicketManagment/CSharp/printticket.cs#uiformergeandvalidateptutility)]
 [!code-vb[PrintTicketManagment#UIForMergeAndValidatePTUtility](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PrintTicketManagment/visualbasic/printticket.vb#uiformergeandvalidateptutility)]  
  
## <a name="see-also"></a>参照

- <xref:System.Printing.PrintCapabilities>
- <xref:System.Printing.PrintTicket>
- <xref:System.Printing.PrintServer.GetPrintQueues%2A>
- <xref:System.Printing.PrintServer>
- <xref:System.Printing.EnumeratedPrintQueueTypes>
- <xref:System.Printing.PrintQueue>
- <xref:System.Printing.PrintQueue.GetPrintCapabilities%2A>
- [WPF のドキュメント](documents-in-wpf.md)
- [印刷の概要](printing-overview.md)
- [スキーマの印刷](/windows/win32/printdocs/printschema)
