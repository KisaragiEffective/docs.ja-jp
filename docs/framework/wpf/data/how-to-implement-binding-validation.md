---
title: '方法 : バインディングの検証の実装'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- validation of binding [WPF]
- data binding [WPF], validation of binding
- binding [WPF], validation of
ms.assetid: eb98b33d-9866-49ae-b981-bc5ff20d607a
ms.openlocfilehash: 245b05d9cfa7ca66dec310bd9a5291def0101d19
ms.sourcegitcommit: 944ddc52b7f2632f30c668815f92b378efd38eea
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/03/2019
ms.locfileid: "73459110"
---
# <a name="how-to-implement-binding-validation"></a>方法 : バインディングの検証の実装

この例では、カスタム検証規則に基づいて、<xref:System.Windows.Controls.Validation.ErrorTemplate%2A> とスタイルトリガーを使用して、無効な値が入力されたときにユーザーに通知するための視覚的なフィードバックを提供する方法を示します。

## <a name="example"></a>例

次の例の <xref:System.Windows.Controls.TextBox> のテキストコンテンツは、`ods`という名前のバインディングソースオブジェクトの `Age` プロパティ (int 型) にバインドされています。 バインディングは、`AgeRangeRule` という名前の検証規則を使用するよう設定されているため、ユーザーが数字以外の文字、または 21 から 130 の範囲外の値を入力すると、テキスト ボックスの横に赤の感嘆符が表示され、ユーザーがテキスト ボックス上にマウスを置くとエラー メッセージを含んだツール ヒントが示されます。

[!code-xaml[BindValidation#2](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#2)]

次の例は、<xref:System.Windows.Controls.ValidationRule> から継承し、<xref:System.Windows.Controls.ValidationRule.Validate%2A> メソッドをオーバーライドする `AgeRangeRule`の実装を示しています。 `Int32.Parse` メソッドは、無効な文字が含まれていないことを確認するために、値に対して呼び出されます。 <xref:System.Windows.Controls.ValidationRule.Validate%2A> メソッドは、解析中に例外がキャッチされたかどうか、および age 値が下限と上限の範囲外であるかどうかに基づいて、値が有効かどうかを示す <xref:System.Windows.Controls.ValidationResult> を返します。

[!code-csharp[BindValidation#3](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/AgeRangeRule.cs#3)]
[!code-vb[BindValidation#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BindValidation/VisualBasic/AgeRangeRule.vb#3)]

次の例は、ユーザーに検証エラーを通知する赤色の感嘆符を作成するカスタム <xref:System.Windows.Controls.ControlTemplate> `validationTemplate` を示しています。 コントロール テンプレートは、コントロールの外観を再定義するために使用されます。

[!code-xaml[BindValidation#4](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#4)]

次の例に示すように、エラーメッセージを示す <xref:System.Windows.Controls.ToolTip> は `textBoxInError`という名前のスタイルを使用して作成されます。 <xref:System.Windows.Controls.Validation.HasError%2A> の値が `true`場合、トリガーにより、現在の <xref:System.Windows.Controls.TextBox> のツールヒントが最初の検証エラーに設定されます。 <xref:System.Windows.Data.Binding.RelativeSource%2A> は <xref:System.Windows.Data.RelativeSourceMode.Self>に設定され、現在の要素を参照しています。

[!code-xaml[BindValidation#5](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#5)]

完全な例については、「[バインドの検証のサンプル](https://github.com/Microsoft/WPF-Samples/tree/master/Data%20Binding/BindValidation)」を参照してください。
  
カスタム <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> を指定しない場合、検証エラーが発生したときにユーザーに視覚的なフィードバックを提供するために、既定のエラーテンプレートが表示されることに注意してください。 詳しくは、「[データ バインドの概要](../../../desktop-wpf/data/data-binding-overview.md)」の「データの検証」をご覧ください。 さらに [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] は、バインディング ソース プロパティの更新中にスローされる例外をキャッチするための、組み込みの検証規則を提供します。 詳細については、「<xref:System.Windows.Controls.ExceptionValidationRule>」を参照してください。

## <a name="see-also"></a>関連項目

- [データ バインディングの概要](../../../desktop-wpf/data/data-binding-overview.md)
- [方法トピック](data-binding-how-to-topics.md)
