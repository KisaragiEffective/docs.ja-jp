---
title: コントロールに属性を適用する
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- controls [Windows Forms], applying attributes
- attributes [Windows Forms], applying
- Windows Forms controls, applying attributes
ms.assetid: af0a3f7f-155b-4ba1-83c4-9cf721331a06
ms.openlocfilehash: b8ecd516cf6bb189c6ad1b208dd8e3a5444f001c
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76741493"
---
# <a name="how-to-apply-attributes-in-windows-forms-controls"></a>方法 : Windows フォーム コントロールに属性を適用する
デザイン環境と正しく対話し、実行時に正常に実行されるコンポーネントとコントロールを開発するには、クラスおよびメンバーに属性を正しく適用する必要があります。  
  
## <a name="example"></a>例  
 次のコード例は、カスタムコントロールでいくつかの属性を使用する方法を示しています。 このコントロールは、単純なログ記録機能を示しています。 コントロールがデータソースにバインドされると、<xref:System.Windows.Forms.DataGridView> コントロールにデータソースから送信された値が表示されます。 値が `Threshold` プロパティによって指定された値を超えると、`ThresholdExceeded` イベントが発生します。  
  
 `AttributesDemoControl` は、`LogEntry` クラスを使用して値をログに記録します。 `LogEntry` クラスはテンプレートクラスであり、これはログに記録される型でパラメーター化されることを意味します。 たとえば、`AttributesDemoControl` が `float`型の値をログに記録している場合、各 `LogEntry` インスタンスは次のように宣言されて使用されます。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#110](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/form1.cs#110)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#110](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/form1.vb#110)]  
  
> [!NOTE]
> `LogEntry` は任意の型によってパラメーター化されるため、パラメーターの型を操作するにはリフレクションを使用する必要があります。 しきい値機能を機能させるには、パラメーターの型 `T` が <xref:System.IComparable> インターフェイスを実装する必要があります。  
  
 `AttributesDemoControl` をホストするフォームは、パフォーマンスカウンターを定期的に照会します。 各値は、適切な型の `LogEntry` にパッケージ化され、フォームの <xref:System.Windows.Forms.BindingSource>に追加されます。 `AttributesDemoControl` は、データバインディングを介して値を受け取り、<xref:System.Windows.Forms.DataGridView> コントロールに値を表示します。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#1)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#1)]  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#100](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/form1.cs#100)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#100](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/form1.vb#100)]  
  
 最初のコード例は、`AttributesDemoControl` の実装です。 2番目のコード例は、`AttributesDemoControl`を使用するフォームを示しています。  
  
## <a name="class-level-attributes"></a>クラスレベルの属性  
 一部の属性はクラスレベルで適用されます。 次のコード例は、Windows フォームコントロールに一般的に適用される属性を示しています。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#20](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#20)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#20](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#20)]  
  
### <a name="typeconverter-attribute"></a>TypeConverter 属性  
 <xref:System.ComponentModel.TypeConverterAttribute> は、一般的に使用されるもう1つのクラスレベルの属性です。 次のコード例は、`LogEntry` クラスの使用方法を示しています。 この例では、`LogEntryTypeConverter`と呼ばれる `LogEntry` 型の <xref:System.ComponentModel.TypeConverter> の実装も示しています。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#5](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#5)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#5](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#5)]  
  
## <a name="member-level-attributes"></a>メンバーレベルの属性  
 一部の属性は、メンバーレベルで適用されます。 次のコード例は、Windows フォームコントロールのプロパティに一般的に適用されるいくつかの属性を示しています。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#21](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#21)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#21](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#21)]  
  
### <a name="ambientvalue-attribute"></a>AmbientValue 属性  
 次の例では、<xref:System.ComponentModel.AmbientValueAttribute> を示し、デザイン環境との対話をサポートするコードを示します。 この相互作用は、"*アンビエント*" と呼ばれます。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#23](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#23)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#23](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#23)]  
  
### <a name="databinding-attributes"></a>Databinding 属性  
 次の例は、複合データバインディングの実装を示しています。 前に示したクラスレベルの <xref:System.ComponentModel.ComplexBindingPropertiesAttribute>では、データバインディングに使用する `DataSource` と `DataMember` のプロパティを指定します。 <xref:System.ComponentModel.AttributeProviderAttribute> は、`DataSource` プロパティのバインド先となる型を指定します。  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#25](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#25)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#25](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#25)]  
  
 [!code-csharp[System.ComponentModel.AttributesDemoControl#26](~/samples/snippets/csharp/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/CS/attributesdemocontrol.cs#26)]
 [!code-vb[System.ComponentModel.AttributesDemoControl#26](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.ComponentModel.AttributesDemoControl/VB/attributesdemocontrol.vb#26)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  
  
- `AttributesDemoControl` をホストするフォームは、ビルドするために `AttributesDemoControl` アセンブリへの参照を必要とします。  
  
## <a name="see-also"></a>参照

- <xref:System.IComparable>
- <xref:System.Windows.Forms.DataGridView>
- [.NET Framework を使用したカスタム Windows フォーム コントロールの開発](developing-custom-windows-forms-controls.md)
- [Windows フォーム コントロールの属性](attributes-in-windows-forms-controls.md)
- [方法: DesignerSerializationVisibilityAttribute を使用して標準型のコレクションをシリアル化する](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/ms171833(v=vs.120))
