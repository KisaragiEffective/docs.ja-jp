---
title: '方法: 文字列からの文字の読み取り'
ms.date: 01/21/2019
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- reading characters from strings
- data streams, reading characters from string
- I/O [.NET Framework], reading characters from strings
- reading data, strings
- streams, reading characters from string
ms.assetid: 27ea5e52-6db8-42d8-980a-50bcfc7fd270
ms.openlocfilehash: ed267ad62e46f6216c94906df1bcefb0684ab51b
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "78155764"
---
# <a name="how-to-read-characters-from-a-string"></a>方法: 文字列からの文字の読み取り
次のコード例は、文字列から同期的または非同期的に文字を読み取る方法を示しています。  
  
## <a name="example-read-characters-synchronously"></a>例:同期的に文字を読み取る
 この例では、文字列から同期的に 13 文字を読み取り、配列に格納し、これらを表示します。 次に、この例では、文字列の残りの文字を読み取り、6 番目の要素で始まる配列に格納し、配列の内容を表示します。  
  
 [!code-csharp[Conceptual.StringReader#1](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.stringreader/cs/source.cs#1)]
 [!code-vb[Conceptual.StringReader#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.stringreader/vb/source.vb#1)]  
  
## <a name="example-read-characters-asynchronously"></a>例:非同期的に文字を読み取る  
 次の例は WPF アプリの裏側にあるコードです。 ウィンドウを読み込むと、<xref:System.Windows.Controls.TextBox> コントロールから非同期的にすべての文字を読み取り、配列に格納します。 次に、<xref:System.Windows.Controls.TextBlock> コントロールの個別の行に各文字または空白文字が非同期で書き込まれます。  
  
 [!code-csharp[Conceptual.StringReader#2](../../../samples/snippets/csharp/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.cs)]
 [!code-vb[Conceptual.StringReader#2](../../../samples/snippets/visualbasic/VS_Snippets_Wpf/StringReaderWriter/MainWindow.xaml.vb)]  
  
## <a name="see-also"></a>関連項目

- <xref:System.IO.StringReader>  
- <xref:System.IO.StringReader.Read%2A?displayProperty=nameWithType>  
- [非同期ファイル I/O](../../../docs/standard/io/asynchronous-file-i-o.md)  
- [方法: ディレクトリ一覧を作成する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))  
- [方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)  
- [方法: ログ ファイルを開いて情報を追加する](../../../docs/standard/io/how-to-open-and-append-to-a-log-file.md)  
- [方法: ファイルからテキストを読み取る](../../../docs/standard/io/how-to-read-text-from-a-file.md)  
- [方法: テキストのファイルへの書き込み](../../../docs/standard/io/how-to-write-text-to-a-file.md)  
- [方法: 文字列への文字の書き込み](../../../docs/standard/io/how-to-write-characters-to-a-string.md)  
- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
