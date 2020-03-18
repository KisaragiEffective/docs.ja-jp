---
title: '方法: My Documents ディレクトリのファイルにテキストを書き込む'
ms.date: 07/20/2015
helpviewer_keywords:
- files [Visual Basic], writing to
- text, writing to files
- examples [Visual Basic], text files
- writing to files [Visual Basic], in My Documents
ms.assetid: 1c726124-781d-4976-9baa-ed46814ff3fe
ms.openlocfilehash: bc62f2bc63a2ea185b8ea4c8d271dd28d347d6f0
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74334523"
---
# <a name="how-to-write-text-to-files-in-the-my-documents-directory-in-visual-basic"></a>方法: My Documents ディレクトリのファイルにテキストを書き込む (Visual Basic)

`My.Computer.FileSystem.SpecialDirectories` オブジェクトを使うと、 **[MyDocuments]** ディレクトリなどの特別なディレクトリにアクセスできます。  
  
## <a name="procedure"></a>プロシージャ  
  
#### <a name="to-write-new-text-files-in-the-my-documents-directory"></a>[MyDocuments] ディレクトリに新しいテキスト ファイルを書き込むには  
  
1. `My.Computer.FileSystem.SpecialDirectories.MyDocuments` プロパティを使ってパスを指定します。  
  
     [!code-vb[VbFileIOWrite#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#1)]  
  
2. `WriteAllText` メソッドを使って、指定したファイルにテキストを書き込みます。  
  
     [!code-vb[VbVbcnMyFileSystem#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnMyFileSystem/VB/Class1.vb#14)]  
  
## <a name="example"></a>例  

 [!code-vb[VbFileIOWrite#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbFileIOWrite/VB/Class1.vb#2)]  
  
## <a name="compiling-the-code"></a>コードのコンパイル  

 `test.txt` を、実際に書き込むファイルの名前に置き換えます。  
  
## <a name="robust-programming"></a>信頼性の高いプログラミング  

 このコードでは、ファイルにテキストを書き込むときに発生する可能性のあるすべての例外が再スローされます。 ユーザーの選択肢を有効なファイル名に制限する [OpenFileDialog](../../../../framework/winforms/controls/openfiledialog-component-windows-forms.md) コンポーネントや [SaveFileDialog](../../../../framework/winforms/controls/savefiledialog-component-windows-forms.md) コンポーネントなどの Windows フォーム コントロールを使うことで、例外が発生する可能性を減らすことができます。 ただし、これらのコントロールを使ったからといって例外がまったくなくなるわけではありません。 ユーザーがファイルを選んだ時点から、コードが実行される時点までの間に、ファイル システムが変化する可能性があります。 ですから、ファイルを操作するときはほとんど常に、例外処理が必要になります。  
  
## <a name="net-framework-security"></a>.NET Framework のセキュリティ  

 部分的に信頼されているコンテキストで実行している場合は、特権不足のため例外がスローされることがあります。 詳しくは、「[コード アクセス セキュリティの基礎](../../../../framework/misc/code-access-security-basics.md)」をご覧ください。  
  
 この例では、新しいファイルを作成します。 アプリケーションでファイルを作成する必要がある場合、そのアプリケーションにはフォルダーに対する Create アクセス許可が必要です。 アクセス許可は、アクセス制御リストを使って設定します。 ファイルが既に存在する場合、アプリケーションに必要なのは低い権限の Write アクセス許可だけです。 可能な場合は、フォルダーに対する Create アクセス許可を付与するのではなく、展開の間にファイルを作成しておき、1 つのファイルの Read アクセス許可を付与するだけの方が安全です。 また、ルート フォルダーや **[Program Files]** フォルダーにデータを書き込むより、ユーザー フォルダーに書き込む方が安全です。 詳しくは、「[アクセス制御リスト (ACL: Access Control List) 技術の概要](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms229742(v=vs.100))」をご覧ください。  
  
## <a name="see-also"></a>参照

- <xref:System.IO.Path.Combine%2A?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Devices.Computer>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem>
- <xref:Microsoft.VisualBasic.FileIO.FileSystem.WriteAllText%2A>
- <xref:Microsoft.VisualBasic.FileIO.SpecialDirectories>
