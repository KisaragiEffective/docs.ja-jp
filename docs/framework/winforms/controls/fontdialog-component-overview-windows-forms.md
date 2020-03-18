---
title: FontDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- FontDialog
helpviewer_keywords:
- Font dialog box [Windows Forms], Windows Forms
- Font dialog box
- FontDialog component [Windows Forms], about FontDialog component
ms.assetid: daf46e57-1b4b-4b7a-bad0-b50ca7ba75dc
ms.openlocfilehash: 664b756dc068ca283e4f43edbdd0f3266f5d1142
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745704"
---
# <a name="fontdialog-component-overview-windows-forms"></a>FontDialog コンポーネントの概要 (Windows フォーム)
Windows フォーム <xref:System.Windows.Forms.FontDialog> コンポーネントは、事前に構成されたダイアログボックスです。これは、システムに現在インストールされているフォントを公開するために使用される標準の Windows**フォント**ダイアログボックスです。 独自のダイアログボックスを構成する代わりに、フォントを選択するための簡単なソリューションとして、Windows ベースのアプリケーション内で使用します。  
  
 既定では、ダイアログボックスには、フォント、フォントスタイル、およびサイズのリストボックスが表示されます。取り消し線や下線などの効果のチェックボックススクリプトのドロップダウンリストフォントの表示方法を示すサンプルがあります。 (スクリプトは、ヘブライ語や日本語など、特定のフォントで使用できるさまざまな文字スクリプトを参照します。)[フォント] ダイアログボックスを表示するには、<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを呼び出します。  
  
## <a name="key-properties"></a>キー プロパティ  
 コンポーネントには、その外観を構成する多数のプロパティがあります。 ダイアログボックスの選択項目を設定するプロパティは、<xref:System.Windows.Forms.FontDialog.Font%2A> と <xref:System.Windows.Forms.FontDialog.Color%2A>です。 <xref:System.Windows.Forms.FontDialog.Font%2A> プロパティは、フォント、スタイル、サイズ、スクリプト、および効果を設定します。たとえば、`Arial, 10pt, style=Italic, Strikeout`のようにします。  
  
## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.FontDialog>
- [FontDialog コンポーネント](fontdialog-component-windows-forms.md)
