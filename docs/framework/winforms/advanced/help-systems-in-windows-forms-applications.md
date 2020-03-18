---
title: ヘルプシステム
ms.date: 03/30/2017
helpviewer_keywords:
- Help [Windows Forms], adding to Windows applications
- Windows applications [Windows Forms], providing Help systems
- What's This? Help
- Help [Windows Forms], Windows Forms
- HelpProvider component [Windows Forms], providing Help in Windows applications
ms.assetid: 2a96a278-432c-41fc-9e3c-5bfedf5e1267
ms.openlocfilehash: c97a22dbdbdcc0eb282b52e16c4ef40914b1d9e7
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76742249"
---
# <a name="help-systems-in-windows-forms-applications"></a>Windows フォーム アプリケーションのヘルプ システム
アプリケーションの開発者として最も重要な courtesies の1つは、ユーザーがスキルの高いヘルプシステムを提供できることです。 ここで、混乱または迷子が行われたときに、この場所が有効になります。 Windows ベースのアプリケーションでヘルプシステムを提供するには、 [HelpProvider コンポーネント](../controls/helpprovider-component-windows-forms.md)を使用します。  
  
## <a name="different-types-of-help"></a>さまざまな種類のヘルプ  
 Windows フォーム <xref:System.Windows.Forms.HelpProvider> コンポーネントは、Windows ベースのアプリケーションで HTML ヘルプ 1.x のヘルプ ファイル (HTML Help Workshop で生成された .chm ファイル、または .htm ファイル) を関連付けるために使用します。 <xref:System.Windows.Forms.HelpProvider> コンポーネントは、Windows フォームまたは特定のコントロールのコントロールに対して、状況依存のヘルプを提供するために使用できます。 また、<xref:System.Windows.Forms.HelpProvider> コンポーネントでは、目次のメインページ、インデックス、検索機能など、特定の領域に対してヘルプファイルを開くことができます。 <xref:System.Windows.Forms.HelpProvider> コンポーネントに関する一般的な情報については、「 [HelpProvider コンポーネントの概要](../controls/helpprovider-component-overview-windows-forms.md)」を参照してください。 <xref:System.Windows.Forms.HelpProvider> コンポーネントを使用して Windows フォームのポップアップヘルプを表示する方法については、「[方法: ポップアップヘルプを表示](how-to-display-pop-up-help.md)する」を参照してください。 <xref:System.Windows.Forms.ToolTip> コンポーネントを使用してコントロール固有のヘルプを表示する方法の詳細については、「[ツールヒントを使用したコントロールのヘルプ](control-help-using-tooltips.md)」を参照してください。  
  
 Html ヘルプ 1. x ファイルは、HTML ヘルプワークショップを使用して生成できます。 HTML ヘルプの詳細については、MSDN の「HTML ヘルプワークショップ」またはその他の「HTML ヘルプ」を参照してください。  
  
## <a name="see-also"></a>参照

- [Windows フォームでのヘルプの統合](integrating-user-help-in-windows-forms.md)
- [HelpProvider コンポーネント](../controls/helpprovider-component-windows-forms.md)
- [ToolTip コンポーネント](../controls/tooltip-component-windows-forms.md)
- [Windows フォームの概要](../windows-forms-overview.md)
- [Windows フォーム](../index.md)
