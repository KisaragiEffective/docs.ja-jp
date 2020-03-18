---
title: '方法 : バインディングをクリアする'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- bindings [WPF], clearing
- clearing bindings [WPF]
- data binding [WPF], clearing bindings
ms.assetid: 73962a93-32a9-4bcd-9240-bcfbb239093a
ms.openlocfilehash: 66f7eb0209d23660b9c7351ca793f509b2f4bb8d
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73458880"
---
# <a name="how-to-clear-bindings"></a>方法 : バインディングをクリアする
この例では、オブジェクトからバインディングをクリアする方法を示します。  
  
## <a name="example"></a>例  
 オブジェクトの個々のプロパティからバインディングをクリアするには、次の例に示すように <xref:System.Windows.Data.BindingOperations.ClearBinding%2A> を呼び出します。 次の例では、<xref:System.Windows.Controls.TextBlock> オブジェクトである*mytext*の <xref:System.Windows.Controls.TextBlock.TextProperty> からバインドを削除します。  
  
 [!code-csharp[CodeOnlyBinding#ClearBinding](~/samples/snippets/csharp/VS_Snippets_Wpf/CodeOnlyBinding/CSharp/binding.cs#clearbinding)]
 [!code-vb[CodeOnlyBinding#ClearBinding](~/samples/snippets/visualbasic/VS_Snippets_Wpf/CodeOnlyBinding/VisualBasic/App.vb#clearbinding)]  
  
 バインディングをクリアすると、バイディングが削除され、依存関係プロパティの値は、バインディングが存在していなかったときの値に変更されます。 この値は、既定値、継承された値、データ テンプレート バインディングの値などです。  
  
 オブジェクトのすべての可能なプロパティからバインディングをクリアするには、<xref:System.Windows.Data.BindingOperations.ClearAllBindings%2A>を使用します。  
  
## <a name="see-also"></a>関連項目

- <xref:System.Windows.Data.BindingOperations>
- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
