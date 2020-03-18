---
title: ErrorProvider コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- ErrorProvider
helpviewer_keywords:
- errors [Windows Forms], displaying to users
- error messages [Windows Forms], displaying
- ErrorProvider component [Windows Forms], about ErrorProvider component
ms.assetid: ced189f2-b5c8-46a7-a6f1-37f5af95dc99
ms.openlocfilehash: b66e97d97d0d8c2ea4e9bdba29f8ff35e486e1f6
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745793"
---
# <a name="errorprovider-component-overview-windows-forms"></a>ErrorProvider コンポーネントの概要 (Windows フォーム)
Windows フォーム[ErrorProvider](errorprovider-component-windows-forms.md)コンポーネントは、フォームまたはコントロールのユーザー入力を検証するために使用されます。 通常は、フォームでのユーザー入力の検証、またはデータセット内のエラーの表示と組み合わせて使用されます。 エラープロバイダーは、メッセージボックスにエラーメッセージを表示するよりも適しています。これは、メッセージボックスが閉じられると、エラーメッセージが表示されなくなるためです。 <xref:System.Windows.Forms.ErrorProvider> コンポーネントでは、テキストボックスなど、関連するコントロールの横にエラーアイコン (赤い丸の内側に白い感嘆符![](./media/errorprovider-component-overview-windows-forms/vb-error-provider-icon.gif)) が表示されます。ユーザーがマウスポインターをエラーアイコンの上に置くと、エラーメッセージ文字列を示すツールヒントが表示されます。  
  
## <a name="key-properties"></a>キー プロパティ  
 <xref:System.Windows.Forms.ErrorProvider> コンポーネントのキープロパティは、<xref:System.Windows.Forms.ErrorProvider.DataSource%2A>、<xref:System.Windows.Forms.ErrorProvider.ContainerControl%2A>、および <xref:System.Windows.Forms.ErrorProvider.Icon%2A>です。 データバインドコントロールで <xref:System.Windows.Forms.ErrorProvider> コンポーネントを使用する場合、コンポーネントでフォームにエラーアイコンが表示されるようにするには、<xref:System.Windows.Forms.ErrorProvider.ContainerControl%2A> プロパティを適切なコンテナー (通常は Windows フォーム) に設定する必要があります。 コンポーネントがデザイナーに追加されると、<xref:System.Windows.Forms.ErrorProvider.ContainerControl%2A> プロパティが、それを含むフォームに設定されます。コントロールをコードに追加する場合は、コントロールを自分で設定する必要があります。  
  
 <xref:System.Windows.Forms.ErrorProvider.Icon%2A> プロパティは、既定のの代わりにカスタムエラーアイコンに設定できます。 <xref:System.Windows.Forms.ErrorProvider.DataSource%2A> プロパティが設定されている場合、<xref:System.Windows.Forms.ErrorProvider> コンポーネントはデータセットのエラーメッセージを表示できます。 <xref:System.Windows.Forms.ErrorProvider> コンポーネントのキーメソッドは、エラーメッセージ文字列を指定し、エラーアイコンが表示される <xref:System.Windows.Forms.ErrorProvider.SetError%2A> メソッドです。  
  
> [!NOTE]
> <xref:System.Windows.Forms.ErrorProvider> コンポーネントには、ユーザー補助クライアントのサポートが組み込まれていません。 このコンポーネントを使用してアプリケーションにアクセスできるようにするには、追加のアクセス可能なフィードバックメカニズムを提供する必要があります。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.ErrorProvider>
- [方法: Windows フォーム ErrorProvider コンポーネントで DataSet 内にエラーを表示する](view-errors-within-a-dataset-with-wf-errorprovider-component.md)
- [方法: Windows フォーム ErrorProvider コンポーネントを使用してフォーム検証でエラー アイコンを表示する](display-error-icons-for-form-validation-with-wf-errorprovider.md)
