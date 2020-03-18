---
title: PrintDocument コンポーネントの概要
ms.date: 03/30/2017
f1_keywords:
- PrintDocument
helpviewer_keywords:
- PrintDocument component [Windows Forms], about PrintDocument component
- printing [Windows Forms], PrintDocument component
ms.assetid: b59b4b60-dce5-42ca-8421-3a54a2f7bab0
ms.openlocfilehash: a82cc0cdcb8cfae796c9c6bf60ab73873f85a291
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2020
ms.locfileid: "76728544"
---
# <a name="printdocument-component-overview-windows-forms"></a>PrintDocument コンポーネントの概要 (Windows フォーム)

Windows フォーム [PrintDocument](printdocument-component-windows-forms.md) コンポーネントを使用して、印刷対象を示すプロパティを設定し、Windows ベースのアプリケーション内でドキュメントを印刷できます。 このコンポーネントは、ドキュメント印刷のあらゆる側面を制御するために、[PrintDialog](printdialog-component-windows-forms.md) コンポーネントと組み合わせて使用できます。

## <a name="working-with-the-printdocument-component"></a>PrintDocument コンポーネントの操作

<xref:System.Drawing.Printing.PrintDocument> コンポーネントに関連する主なシナリオの2つを次に示します。

- 簡易印刷ジョブ (個々のテキスト ファイルを印刷する場合など)。 このような場合は、<xref:System.Drawing.Printing.PrintDocument> コンポーネントを Windows フォームに追加し、<xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントハンドラーでファイルを出力するプログラミングロジックを追加します。 プログラミングロジックは、ドキュメントを印刷するために <xref:System.Drawing.Printing.PrintDocument.Print%2A> メソッドとなする必要があります。 このメソッドは、<xref:System.Drawing.Printing.PrintPageEventArgs> クラスの <xref:System.Drawing.Printing.PrintPageEventArgs.Graphics%2A> プロパティに含まれる <xref:System.Drawing.Graphics> オブジェクトをプリンターに送信します。 <xref:System.Drawing.Printing.PrintDocument> コンポーネントを使用してテキストドキュメントを印刷する方法を示す例については、「[方法: 複数ページのテキストファイルを Windows フォームで印刷](../advanced/how-to-print-a-multi-page-text-file-in-windows-forms.md)する」を参照してください。

- より複雑な印刷ジョブ (作成した印刷ロジックを再利用する場合など)。 このような場合は、<xref:System.Drawing.Printing.PrintDocument> コンポーネントから新しいコンポーネントを派生させ、<xref:System.Drawing.Printing.PrintDocument.PrintPage> イベントをオーバーライドします (Visual Basic C#または[オーバーライド](../../../csharp/language-reference/keywords/override.md)のオーバーライドに関する部分[を参照し](../../../visual-basic/language-reference/modifiers/overrides.md)てください)。

フォームに追加されると、Visual Studio の Windows フォームデザイナーの下部にあるトレイに <xref:System.Drawing.Printing.PrintDocument> コンポーネントが表示されます。

## <a name="see-also"></a>参照

- <xref:System.Drawing.Graphics>
- <xref:System.Drawing.Printing.PrintDocument>
- [Windows フォームにおける印刷のサポート](../advanced/windows-forms-print-support.md)
- [PrintDocument コンポーネント](printdocument-component-windows-forms.md)
