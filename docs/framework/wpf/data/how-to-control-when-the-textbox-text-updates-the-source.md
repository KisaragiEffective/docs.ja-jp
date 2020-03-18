---
title: '方法 : TextBox テキストでソースを更新するタイミングを制御する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- source updates [WPF], timing of
- data binding [WPF], timing of source updates
- timing of source updates [WPF]
ms.assetid: ffb7b96a-351d-4c68-81e7-054033781c64
ms.openlocfilehash: b211eb67e3bac95f74255935a39cc0d6aec61531
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73974781"
---
# <a name="how-to-control-when-the-textbox-text-updates-the-source"></a>方法 : TextBox テキストでソースを更新するタイミングを制御する
このトピックでは、<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> プロパティを使用して、バインディングソースの更新のタイミングを制御する方法について説明します。 このトピックでは、例として <xref:System.Windows.Controls.TextBox> コントロールを使用します。

## <a name="example"></a>例
 <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType> プロパティには、<xref:System.Windows.Data.UpdateSourceTrigger.LostFocus>の既定の <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> 値が設定されています。 つまり、アプリケーションにデータバインド <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType> プロパティを持つ <xref:System.Windows.Controls.TextBox> がある場合、<xref:System.Windows.Controls.TextBox> に入力したテキストは、<xref:System.Windows.Controls.TextBox> がフォーカスを失うまで (たとえば、<xref:System.Windows.Controls.TextBox>から離れたときに) ソースを更新しません。

 入力時にソースを更新する場合は、バインドの <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> を <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged>に設定します。 次の例では、強調表示されているコード行は、<xref:System.Windows.Controls.TextBox> と <xref:System.Windows.Controls.TextBlock> の両方の `Text` プロパティが同じソースプロパティにバインドされていることを示しています。 <xref:System.Windows.Controls.TextBox> binding の <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> プロパティは <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged>に設定されています。

 [!code-xaml[SimpleBinding#USTHowTo](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Page1.xaml?highlight=33-39,41-42)]

 その結果、次のサンプルのスクリーンショットに示すように、ユーザーが <xref:System.Windows.Controls.TextBox>にテキストを入力すると、<xref:System.Windows.Controls.TextBlock> に同じテキスト (ソースが変更されたため) が表示されます。

 ![単純なデータバインディングを示すスクリーンショット。](./media/how-to-control-when-the-textbox-text-updates-the-source/data-binding-simple-binding-sample.png)

 ダイアログまたはユーザーが編集可能なフォームがあり、ユーザーがフィールドの編集を完了して [OK] をクリックするまでソースの更新を延期する場合は、次の例に示すように、バインドの <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> 値を <xref:System.Windows.Data.UpdateSourceTrigger.Explicit>に設定できます。

 [!code-xaml[UpdateSource#2](~/samples/snippets/csharp/VS_Snippets_Wpf/UpdateSource/CSharp/Window1.xaml#2)]

 <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> 値を <xref:System.Windows.Data.UpdateSourceTrigger.Explicit>に設定すると、アプリケーションが <xref:System.Windows.Data.BindingExpression.UpdateSource%2A> メソッドを呼び出した場合にのみ、ソース値が変更されます。 次の例は、`itemNameTextBox`に対して <xref:System.Windows.Data.BindingExpression.UpdateSource%2A> を呼び出す方法を示しています。

 [!code-csharp[UpdateSource#1](~/samples/snippets/csharp/VS_Snippets_Wpf/UpdateSource/CSharp/Window1.xaml.cs#1)]
 [!code-vb[UpdateSource#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UpdateSource/VisualBasic/Window1.xaml.vb#1)]

> [!NOTE]
> 他のコントロールのプロパティにも同じ手法を使用できますが、その他のほとんどのプロパティには <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged>の既定の <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> 値があることに注意してください。 詳細については、<xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> のプロパティページを参照してください。

> [!NOTE]
> <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> プロパティはソースの更新を処理するため、<xref:System.Windows.Data.BindingMode.TwoWay> または <xref:System.Windows.Data.BindingMode.OneWayToSource> のバインドにのみ関連します。 バインディングを機能させるには <xref:System.Windows.Data.BindingMode.TwoWay> と <xref:System.Windows.Data.BindingMode.OneWayToSource> のバインドが必要です。ソースオブジェクトでは、プロパティの変更通知を提供する必要があります。 詳しくは、このトピック内にあるサンプルをご覧ください。 また、「[方法 : プロパティの変更通知を実装する](how-to-implement-property-change-notification.md)」もご覧ください。

## <a name="see-also"></a>関連項目

- [方法トピック](data-binding-how-to-topics.md)
