---
title: '方法 : ICommandSource を実装する'
ms.date: 12/05/2019
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ICommandSource interfaces [WPF], implementing
ms.assetid: 7452dd39-6e11-44bf-806a-31d87f3772ac
ms.openlocfilehash: 755377d25a2deb48aa9da86d4182075e30763530
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75345098"
---
# <a name="how-to-implement-icommandsource"></a>方法 : ICommandSource を実装する

この例では、<xref:System.Windows.Input.ICommandSource>を実装してコマンドソースを作成する方法を示します。 コマンドソースは、コマンドの呼び出し方法を認識しているオブジェクトです。 <xref:System.Windows.Input.ICommandSource> インターフェイスは、次の3つのメンバーを公開します。

- <xref:System.Windows.Input.ICommandSource.Command%2A>: 呼び出されるコマンド。
- <xref:System.Windows.Input.ICommandSource.CommandParameter%2A>: コマンドソースからコマンドを処理するメソッドに渡されるユーザー定義データ型。
- <xref:System.Windows.Input.ICommandSource.CommandTarget%2A>: コマンドが実行されているオブジェクト。

この例では、<xref:System.Windows.Controls.Slider> コントロールから継承し、<xref:System.Windows.Input.ICommandSource> インターフェイスを実装するクラスが作成されます。
  
## <a name="example"></a>使用例

WPF には、<xref:System.Windows.Controls.Button>、<xref:System.Windows.Controls.MenuItem>、<xref:System.Windows.Documents.Hyperlink>などの <xref:System.Windows.Input.ICommandSource>を実装するクラスが多数用意されています。 コマンドソースは、コマンドの呼び出し方法を定義します。 これらのクラスは、クリックされたときにコマンドを呼び出し、<xref:System.Windows.Input.ICommandSource.Command%2A> プロパティが設定されている場合にのみコマンドソースになります。

この例では、<xref:System.Windows.Controls.Primitives.RangeBase.Value%2A> プロパティが変更されたときに、スライダーが移動されたとき、またはより正確にコマンドを呼び出します。

クラス定義を次に示します。

[!code-csharp[ImplementICommandSource#ImplementICommandSourceClassDefinition](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandsourceclassdefinition)]
[!code-vb[ImplementICommandSource#ImplementICommandSourceClassDefinition](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandsourceclassdefinition)]

次の手順では、<xref:System.Windows.Input.ICommandSource> メンバーを実装します。 この例では、プロパティは <xref:System.Windows.DependencyProperty> オブジェクトとして実装されています。 これにより、プロパティでデータバインディングを使用できるようになります。 <xref:System.Windows.DependencyProperty> クラスの詳細については、「[依存関係プロパティの概要](dependency-properties-overview.md)」を参照してください。 データバインディングの詳細については、「[データバインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)」を参照してください。

ここでは、<xref:System.Windows.Input.ICommandSource.Command%2A> プロパティのみを示しています。
 
[!code-csharp[ImplementICommandSource#ImplementICommandSourceCommandPropertyDefinition](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandsourcecommandpropertydefinition)]
[!code-vb[ImplementICommandSource#ImplementICommandSourceCommandPropertyDefinition](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandsourcecommandpropertydefinition)]  
  
次に、<xref:System.Windows.DependencyProperty> の変更コールバックを示します。

[!code-csharp[ImplementICommandSource#ImplementICommandSourceCommandChanged](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandsourcecommandchanged)]
[!code-vb[ImplementICommandSource#ImplementICommandSourceCommandChanged](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandsourcecommandchanged)]

次の手順では、コマンドソースに関連付けられているコマンドを追加および削除します。 前のコマンドに関連付けられているイベントハンドラー (存在する場合) を最初に削除する必要があるため、新しいコマンドが追加されたときには、<xref:System.Windows.Input.ICommandSource.Command%2A> プロパティを単に上書きすることはできません。

[!code-csharp[ImplementICommandSource#ImplementICommandSourceHookUnHookCommands](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandsourcehookunhookcommands)]
[!code-vb[ImplementICommandSource#ImplementICommandSourceHookUnHookCommands](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandsourcehookunhookcommands)]

次の手順では、<xref:System.Windows.Input.ICommand.CanExecuteChanged> ハンドラーのロジックを作成します。

<xref:System.Windows.Input.ICommand.CanExecuteChanged> イベントは、現在のコマンドターゲットで実行するコマンドの機能が変更された可能性があることをコマンドソースに通知します。 コマンドソースがこのイベントを受け取ると、通常はコマンドの <xref:System.Windows.Input.ICommand.CanExecute%2A> メソッドを呼び出します。 コマンドが現在のコマンドターゲットで実行できない場合、コマンドソースは通常、それ自体を無効にします。 コマンドが現在のコマンドターゲットで実行できる場合、コマンドソースは通常、それ自体を有効にします。

[!code-csharp[ImplementICommandSource#ImplementICommandCanExecuteChanged](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandcanexecutechanged)]
[!code-vb[ImplementICommandSource#ImplementICommandCanExecuteChanged](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandcanexecutechanged)]

最後の手順は <xref:System.Windows.Input.ICommand.Execute%2A> メソッドです。 コマンドが <xref:System.Windows.Input.RoutedCommand>の場合、<xref:System.Windows.Input.RoutedCommand> <xref:System.Windows.Input.RoutedCommand.Execute%2A> メソッドが呼び出されます。それ以外の場合は、<xref:System.Windows.Input.ICommand> <xref:System.Windows.Input.ICommand.Execute%2A> メソッドが呼び出されます。

[!code-csharp[ImplementICommandSource#ImplementICommandExecute](~/samples/snippets/csharp/VS_Snippets_Wpf/ImplementICommandSource/CSharp/CommandSlider.cs#implementicommandexecute)]
[!code-vb[ImplementICommandSource#ImplementICommandExecute](~/samples/snippets/visualbasic/VS_Snippets_Wpf/ImplementICommandSource/visualbasic/commandslider.vb#implementicommandexecute)]

## <a name="see-also"></a>関連項目

- <xref:System.Windows.Input.ICommandSource>
- <xref:System.Windows.Input.ICommand>
- <xref:System.Windows.Input.RoutedCommand>
- [コマンド実行の概要](commanding-overview.md)
