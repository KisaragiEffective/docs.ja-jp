---
title: '方法: StreamReader を使用してファイルからテキストを読み取る'
ms.date: 07/20/2015
helpviewer_keywords:
- reading files [Visual Basic], text
- text, reading from files
- reading text from files [Visual Basic]
- files [Visual Basic], reading
ms.assetid: 384033c6-18f9-4d59-9610-36371226558f
ms.openlocfilehash: 572463d1f03d768fb133f2dac59b012051f053bb
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334560"
---
# <a name="how-to-read-text-from-files-with-a-streamreader-visual-basic"></a>方法: StreamReader を使用してファイルからテキストを読み取る (Visual Basic)

`My.Computer.FileSystem` オブジェクトには、<xref:System.IO.TextReader> および <xref:System.IO.TextWriter> を開くためのメソッドがあります。 これらのメソッド (`OpenTextFileWriter` メソッドと `OpenTextFileReader` メソッド) は、高度なメソッドで、 **[すべて]** タブを選択しないと IntelliSense で表示されません。  
  
### <a name="to-read-a-line-from-a-file-with-a-text-reader"></a>テキスト リーダーを使用してファイルから行を読み取るには  
  
- `OpenTextFileReader` メソッドにファイルを指定して <xref:System.IO.TextReader> を開きます。 この例では、`testfile.txt` という名前のファイルを開き、1 行を読み取って、その行をメッセージ ボックスに表示します。  
  
     [!code-vb[VbFileIORead#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIORead/VB/Class1.vb#1)]  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 読み取るファイルは、テキスト ファイルである必要があります。  
  
 ファイル名からファイルの内容を判断しないでください。 たとえば、Form1.vb というファイルは Visual Basic のソース ファイルではない可能性もあります。  
  
 アプリケーションでデータを使用する前に、入力をすべて検証してください。 ファイルの内容が予想どおりでないことがあり、ファイルの内容を読み取るメソッドが失敗する可能性があります。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  

 ファイルを読み取るには、アセンブリに対して <xref:System.Security.Permissions.FileIOPermission> クラスで特権レベルが許可されている必要があります。 部分的に信頼されているコンテキストで実行している場合は、特権不足のため例外がスローされることがあります。 詳しくは、「[コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)」をご覧ください。 また、ユーザーはファイルへのアクセス許可も必要です。 詳しくは、「[アクセス制御リスト (ACL: Access Control List) 技術の概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229742(v=vs.100))」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:System.Windows.Forms.OpenFileDialog>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileWriter%2A>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.OpenTextFileReader%2A>
- [SaveFileDialog コンポーネント](../../../../framework/winforms/controls/savefiledialog-component-windows-forms.md)
- [ファイルの読み取り](../../../../visual-basic/developing-apps/programming/drives-directories-files/reading-from-files.md)
