---
title: PageSetupDialog コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- PageSetupDialog
helpviewer_keywords:
- Page Setup dialog box [Windows Forms], displaying
- PageSetupDialog component
ms.assetid: 791caacb-a5ca-4fca-bad9-1a5721ad697c
ms.openlocfilehash: a891cb8cc77007d7591d41461c94f61c077eb300
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76744333"
---
# <a name="pagesetupdialog-component-overview-windows-forms"></a>PageSetupDialog コンポーネントの概要 (Windows フォーム)

Windows フォーム <xref:System.Windows.Forms.PageSetupDialog> コンポーネントは、Windows ベースのアプリケーションで印刷するページの詳細を設定するための構成済みダイアログボックスです。 ユーザーが独自のダイアログボックスを構成する代わりにページ設定を設定するための簡単なソリューションとして、Windows ベースのアプリケーション内で使用します。 ユーザーが罫線と余白の調整、ヘッダーとフッター、縦または横の向きを設定できるようにすることができます。 Windows の標準のダイアログ ボックスを使用して、ユーザーがすぐに慣れる基本的な機能を持つアプリケーションを作成します。

## <a name="key-properties-and-methods"></a>キーのプロパティとメソッド

実行時にダイアログを表示するには、<xref:System.Windows.Forms.CommonDialog.ShowDialog%2A> メソッドを使用します。 このコンポーネントには、1つのページ (<xref:System.Drawing.Printing.PrintDocument> クラス) または任意のドキュメント (<xref:System.Drawing.Printing.PageSettings> クラス) に関連する設定可能なプロパティがあります。 また、<xref:System.Windows.Forms.PageSetupDialog> コンポーネントを使用して、<xref:System.Drawing.Printing.PrinterSettings> クラスに格納されている特定のプリンター設定を決定することもできます。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Windows.Forms.PageSetupDialog> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Windows.Forms.PageSetupDialog>
- [PageSetupDialog コンポーネント](pagesetupdialog-component-windows-forms.md)
