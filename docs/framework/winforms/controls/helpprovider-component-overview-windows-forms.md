---
title: HelpProvider コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- HelpProvider
helpviewer_keywords:
- HelpProvider component [Windows Forms], about HelpProvider component
- Help [Windows Forms], adding to Windows applications
- F1 Help [Windows Forms], adding to Windows Forms
- dialog boxes [Windows Forms], context-sensitive Help
- Windows Forms, context-sensitive Help
ms.assetid: 6b10c2cc-c577-4cb5-9669-e37b33416af9
ms.openlocfilehash: 74d35dfa39a605cb1e1e85cc3aeda834e1c60669
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76738716"
---
# <a name="helpprovider-component-overview-windows-forms"></a>HelpProvider コンポーネントの概要 (Windows フォーム)
Windows フォーム[HelpProvider](helpprovider-component-windows-forms.md)コンポーネントは、Windows アプリケーションと共に html ヘルプ1.x ヘルプファイル (Html ヘルプワークショップで生成された .chm ファイル、または .htm ファイル) を関連付けるために使用されます。 ヘルプは、次のさまざまな方法で提供できます。  
  
- Windows フォームのコントロールについて、状況依存のヘルプを提供します。  
  
- ダイアログボックスの特定のダイアログボックスまたは特定のコントロールについて、状況依存のヘルプを提供します。  
  
- 目次のメインページ、インデックス、検索機能など、特定の領域に対してヘルプファイルを開きます。  
  
## <a name="using-the-help-provider"></a>ヘルププロバイダーの使用  
 Windows フォームに <xref:System.Windows.Forms.HelpProvider> コンポーネントを追加すると、フォーム上の他のコントロールが <xref:System.Windows.Forms.HelpProvider> コンポーネントのヘルププロパティを公開できるようになります。 これにより、Windows フォーム上のコントロールにヘルプを提供できます。 <xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> プロパティを使用して、ヘルプファイルを <xref:System.Windows.Forms.HelpProvider> コンポーネントに関連付けることができます。 提供されるヘルプの種類を指定するには、<xref:System.Windows.Forms.HelpProvider.SetHelpNavigator%2A> を呼び出し、指定したコントロールの <xref:System.Windows.Forms.HelpNavigator> 列挙体の値を指定します。 <xref:System.Windows.Forms.HelpProvider.SetHelpKeyword%2A> メソッドを呼び出すことによって、ヘルプのキーワードまたはトピックを指定します。  
  
 必要に応じて、特定のヘルプ文字列を別のコントロールに関連付けるには、<xref:System.Windows.Forms.HelpProvider.SetHelpString%2A> メソッドを使用します。 コントロールにフォーカスがあるときにユーザーが F1 キーを押すと、このメソッドを使用してコントロールに関連付ける文字列がポップアップウィンドウに表示されます。  
  
 <xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> が設定されていない場合は、<xref:System.Windows.Forms.HelpProvider.SetHelpString%2A> を使用してヘルプテキストを指定する必要があります。 <xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> とヘルプ文字列の両方を設定した場合は、<xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> に基づくヘルプが優先されます。  
  
> [!NOTE]
> <xref:System.Windows.Forms.HelpProvider> コントロールの <xref:System.Windows.Forms.Help.ShowHelp%2A> メソッドまたは <xref:System.Windows.Forms.HelpProvider.HelpNamespace%2A> プロパティでヘルプファイルへのパスを指定するときに、相対パスを使用すると問題が発生することがあります。 そのため、絶対ファイルパスを使用してヘルプファイルを指定してください。  
  
## <a name="see-also"></a>参照

- [Windows フォーム アプリケーションのヘルプ システム](../advanced/help-systems-in-windows-forms-applications.md)
