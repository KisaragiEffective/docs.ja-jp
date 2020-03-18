---
title: クリップボードのデータの格納と読み取り
ms.date: 07/20/2015
helpviewer_keywords:
- Clipboard, storing data to (My.Computer.Clipboard)
- Clipboard, reading from (My.Computer.Clipboard)
- Clipboard
- My.Computer.Clipboard object, tasks
- data [Visual Basic], Clipboard
- reading data, from Clipboard
ms.assetid: f690119a-4378-4f7d-b20e-d9377ef49496
ms.openlocfilehash: 243fb237f3f9ba53f8b29079df08531c102c78dd
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74349734"
---
# <a name="storing-data-to-and-reading-from-the-clipboard-visual-basic"></a>クリップボードのデータの格納と読み取り (Visual Basic)

クリップボードは、テキスト、イメージなどのデータの格納に使用できます。 アクティブなプロセスすべてがこのクリップボードを共有しているため、クリップボードを使ってデータをプロセス間で転送することができます。 `My.Computer.Clipboard` オブジェクトを使用すると、クリップボードに簡単にアクセスして、読み込みや書き込みを実行できます。  
  
## <a name="reading-from-the-clipboard"></a>クリップボードからの読み取り  

 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetText%2A> メソッドを使用すると、クリップボードからテキストを読み取ることができます。 次のコードはテキストを読み取って、メッセージ ボックスに表示します。 この例を正しく動作させるには、クリップボードにテキストが格納されている必要があります。  
  
 [!code-vb[VbVbcnMyClipboard#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#4)]  
  
 このコード例は、IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでこのスニペットにアクセスするには、 **[Windows フォーム アプリケーション]、[クリップボード]** の順に移動します。 詳細については、「[Code Snippets](/visualstudio/ide/code-snippets)」を参照してください。  
  
 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetImage%2A> メソッドを使用すると、クリップボードからイメージを取得できます。 この例では、イメージを取得して `PictureBox1` に割り当てる前に、そのイメージがクリップボードにあるかどうかを確認します。  
  
 [!code-vb[VbResourceTasks#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#16)]  
  
 このコード例は、IntelliSense コード スニペットとしても利用できます。 コード スニペット ピッカーでこのスニペットにアクセスするには、 **[Windows フォーム アプリケーション]、[クリップボード]** の順に移動します。詳細については、「[コード スニペット](/visualstudio/ide/code-snippets)」を参照してください。  
  
 クリップボード内のアイテムは、アプリケーションの終了後も保持されます。  
  
## <a name="determining-the-type-of-file-stored-in-the-clipboard"></a>クリップボードに格納されているファイルの種類の確認  

 クリップボードには、テキスト、オーディオ ファイル、イメージなど、さまざまな形式のデータを格納できます。 クリップボード内のファイルの種類を確認するには、<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsAudio%2A>、<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsFileDropList%2A>、<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsImage%2A>、<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsText%2A> などのメソッドを使用します。 確認対象にカスタム形式が含まれる場合は、<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.ContainsData%2A> メソッドを使用できます。  
  
 `ContainsImage` 関数を使用すると、クリップボードに含まれるデータがイメージかどうかを確認できます。 次のコードでは、データがイメージかどうかを確認し、それに応じて報告します。  
  
 [!code-vb[VbResourceTasks#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#13)]  
  
## <a name="clearing-the-clipboard"></a>クリップボードのクリア  

 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.Clear%2A> メソッドはクリップボードをクリアします。 クリップボードは他のプロセスによって共有されているため、クリップボードをクリアすると、こうしたプロセスに影響することがあります。  
  
 `Clear` メソッドの使用例を次のコードに示します。  
  
 [!code-vb[VbVbcnMyClipboard#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#3)]  
  
## <a name="writing-to-the-clipboard"></a>クリップボードへの書き込み  

 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetText%2A> メソッドを使用して、クリップボードにテキストを書き込みます。 次のコードでは、"This is a test string" という文字列をクリップボードに書き込みます。  
  
 [!code-vb[VbVbcnMyClipboard#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#1)]  
  
 `SetText` メソッドは、<xref:System.Windows.Forms.TextDataFormat> 型を含む書式パラメーターを受け取ることができます。 次のコードでは、"This is a test string" という文字列を RTF テキストとしてクリップボードに書き込みます。  
  
 [!code-vb[VbVbcnMyClipboard#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#2)]  
  
 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetData%2A> メソッドを使用して、クリップボードにデータを書き込みます。 この例では、`DataObject` `dataChunk`をカスタム書式 `specialFormat` でクリップボードに書き込みます。  
  
 [!code-vb[VbVbcnMyClipboard#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyClipboard/VB/Class1.vb#7)]  
  
 <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetAudio%2A> メソッドを使用して、クリップボードにオーディオ データを書き込みます。 この例では、バイト配列 `musicReader` を作成し、`cool.wav` ファイルを読み込んでから、それをクリップボードに書き込みます。  
  
 [!code-vb[VbResourceTasks#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbResourceTasks/VB/Class1.vb#5)]  
  
> [!IMPORTANT]
> クリップボードには他のユーザーもアクセスできるため、パスワード、機密データなどの機密情報は格納しないでください。  
  
## <a name="see-also"></a>関連項目

- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy>
- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.GetAudioStream%2A>
- <xref:Microsoft.VisualBasic.MyServices.ClipboardProxy.SetDataObject%2A>
- [方法: XML ファイルからオブジェクト データを読み込む](../../../programming-guide/concepts/serialization/how-to-read-object-data-from-an-xml-file.md)
- [方法: XML ファイルにオブジェクト データを書き込む](../../../programming-guide/concepts/serialization/how-to-write-object-data-to-an-xml-file.md)
