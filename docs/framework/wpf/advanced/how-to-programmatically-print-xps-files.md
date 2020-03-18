---
title: '方法 : XPS ファイルをプログラムにより印刷する'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- printing XPS files programmatically [WPF]
- XPS files [WPF], printing programmatically
ms.assetid: 0b1c0a3f-b19e-43d6-bcc9-eb3ec4e555ad
ms.openlocfilehash: cc86a7e7c6a816af37c3d063825ed62583afa78a
ms.sourcegitcommit: f348c84443380a1959294cdf12babcb804cfa987
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/12/2019
ms.locfileid: "73966996"
---
# <a name="how-to-programmatically-print-xps-files"></a>方法 : XPS ファイルをプログラムにより印刷する

<xref:System.Printing.PrintQueue.AddJob%2A> メソッドの1つのオーバーロードを使用して、<xref:System.Windows.Controls.PrintDialog> を開かずに (XPS) ファイルを印刷することができます。また、原則として [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)] を使用することもできます。

多数の <xref:System.Windows.Xps.XpsDocumentWriter.Write%2A?displayProperty=nameWithType> および <xref:System.Windows.Xps.XpsDocumentWriter.WriteAsync%2A?displayProperty=nameWithType> 方法を使用して XPS ファイルを印刷することもできます。 詳細については、「 [XPS ドキュメントの印刷](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771525(v=vs.90))」を参照してください。

XPS を印刷するもう1つの方法は、<xref:System.Windows.Controls.PrintDialog.PrintDocument%2A?displayProperty=nameWithType> または <xref:System.Windows.Controls.PrintDialog.PrintVisual%2A?displayProperty=nameWithType> の方法を使用することです。 [印刷ダイアログ ボックスの呼び出し](how-to-invoke-a-print-dialog.md)に関するページをご覧ください。

## <a name="example"></a>例

3パラメーター <xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> メソッドを使用する主な手順は次のとおりです。 詳細は、下記の例に示します。

1. プリンターが XPSDrv プリンターかどうかを確認します XPSDrv の詳細については、「[印刷の概要](printing-overview.md)」を参照してください。

2. プリンターが XPSDrv プリンターでない場合は、スレッドのアパートメントをシングル スレッドに設定します。

3. プリント サーバーと印刷キューのオブジェクトをインスタンス化します。

4. ジョブ名、印刷するファイル、およびプリンターが XPSDrv プリンターであるかどうかを示す <xref:System.Boolean> フラグを指定して、メソッドを呼び出します。

次の例は、ディレクトリ内のすべての XPS ファイルをバッチ印刷する方法を示しています。 アプリケーションはディレクトリを指定するようにユーザーに要求しますが、3つのパラメーター <xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> メソッドには [!INCLUDE[TLA#tla_ui](../../../../includes/tlasharptla-ui-md.md)]は必要ありません。 これは、XPS ファイル名とパスが含まれているすべてのコードパスで使用できます。

<xref:System.Printing.PrintQueue.AddJob%2A> の3つのパラメーター <xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> オーバーロードは、<xref:System.Boolean> パラメーターが `false`ときは常に1つのスレッドアパートメントで実行する必要があります。これは、非 XPSDrv プリンターが使用されているときに必要です。 ただし、.NET の既定のアパートメント状態は、複数のスレッドになります。 この例では XPSDrv 以外のプリンターを前提にしているため、この既定の状態を逆にする必要があります。

既定の状態を変更する方法は 2 つあります。 1つの方法として、アプリケーションの `Main` メソッドの最初の行のすぐ上に <xref:System.STAThreadAttribute> (つまり、"`[System.STAThreadAttribute()]`") を追加する方法があります (通常は "`static void Main(string[] args)`")。 ただし、多くのアプリケーションでは、`Main` メソッドにマルチスレッドアパートメント状態が必要であるため、2番目のメソッドがあります。 <xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> の呼び出しを、<xref:System.Threading.Thread.SetApartmentState%2A>で <xref:System.Threading.ApartmentState.STA> に設定されたアパートメント状態を持つ別のスレッドに配置します。 次の例では、この 2 番目の方法を使用します。

したがって、この例では、まず <xref:System.Threading.Thread> オブジェクトをインスタンス化し、それに**Printxps**メソッドを <xref:System.Threading.ThreadStart> パラメーターとして渡します。 ( **Printxps**メソッドは、この例の後半で定義されています)。次に、スレッドは1つのスレッドアパートメントに設定されます。 それ以降の `Main` メソッドのコードは、この新しいスレッドを開始します。

この例の要点は、`static`**staticBatchXPSPrinter.PrintXPS** メソッド内にあります。 プリントサーバーとキューを作成した後、メソッドは XPS ファイルを含むディレクトリの入力をユーザーに求めます。 ディレクトリの存在と \*のファイルが存在することを検証した後、メソッドは、このようなファイルを印刷キューに追加します。 この例では、プリンターが非 XPSDrv であることを前提としているため、<xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> メソッドの最後のパラメーターに `false` を渡しています。 このため、メソッドは、ファイル内の XPS マークアップを、プリンターのページ記述言語への変換を試みる前に検証します。 検証に失敗すると、例外がスローされます。 このコード例では、例外をキャッチし、そのことをユーザーに通知した後、次の XPS ファイルの処理に移ります。

[!code-csharp[BatchPrintXPSFiles#BatchPrintXPSFiles](~/samples/snippets/csharp/VS_Snippets_Wpf/BatchPrintXPSFiles/CSharp/Program.cs#batchprintxpsfiles)]
[!code-vb[BatchPrintXPSFiles#BatchPrintXPSFiles](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BatchPrintXPSFiles/visualbasic/program.vb#batchprintxpsfiles)]

XPSDrv プリンターを使用している場合は、最後のパラメーターを `true` に設定できます。 この場合、XPS はプリンターのページ記述言語であるため、このメソッドは、ファイルを検証したり、別のページ記述言語に変換したりせずに、そのファイルをプリンターに送信します。 アプリケーションが XPSDrv プリンターを使用するかどうかがデザイン時にわからない場合は、アプリケーションを変更して、<xref:System.Printing.PrintQueue.IsXpsDevice%2A> プロパティを読み、その内容に従って分岐するようにすることができます。

Windows Vista と Microsoft .NET Framework のリリース直後に使用できる XPSDrv プリンターはほとんどないため、XPSDrv 以外のプリンターを XPSDrv プリンターとして偽装することが必要になる場合があります。 そのためには、アプリケーションを実行しているコンピューターで、以下のレジストリ キーのファイルの一覧に Pipelineconfig.xml を追加します。

HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Print\Environments\Windows NT x86\Drivers\Version-3\\ *\<PseudoXPSPrinter>* \DependentFiles

ここで、 *\<PseudoXPSPrinter* は任意の印刷キューです。 その後、コンピューターを再起動する必要があります。

このような偽装により、`true` を <xref:System.Printing.PrintQueue.AddJob%28System.String%2CSystem.String%2CSystem.Boolean%29> の最後のパラメーターとして渡すことができますが、例外は発生しませんが *\<PseudoXPSPrinter >* は実際には XPSDrv プリンターではないため、出力されるのはガベージのみです。

> [!NOTE]
> わかりやすくするために、上記の例では、ファイルが XPS であることをテストする際に、\*の拡張子を使用しています。 ただし、XPS ファイルにはこの拡張機能は必要ありません。 [Isxps (Isxps 適合性ツール)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aa348104(v=vs.100))は、XPS の有効性をテストするための1つの方法です。

## <a name="see-also"></a>関連項目

- <xref:System.Printing.PrintQueue>
- <xref:System.Printing.PrintQueue.AddJob%2A>
- <xref:System.Threading.ApartmentState>
- <xref:System.STAThreadAttribute>
- [XPS ドキュメント](/windows/desktop/printdocs/documents)
- [XPS ドキュメントの印刷](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms771525(v=vs.90))
- [マネージスレッド処理とアンマネージスレッド処理](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5s8ee185(v=vs.100))
- [isXPS.exe (isXPS 適合性ツール)](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/aa348104(v=vs.100))
- [WPF のドキュメント](documents-in-wpf.md)
- [印刷の概要](printing-overview.md)
