---
title: '方法: ディレクトリとファイルを列挙する'
ms.date: 12/27/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- I/O [.NET Framework], enumerating directories and files
ms.assetid: 86b69a08-3bfa-4e5f-b4e1-3b7cb8478215
ms.openlocfilehash: 6a26d0ef529b81976c4d2caafed34bb5f08d8d46
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75707746"
---
# <a name="how-to-enumerate-directories-and-files"></a>方法: ディレクトリとファイルを列挙する
列挙可能なコレクションでは、ディレクトリとファイルの大きなコレクションを操作する際に配列よりも優れたパフォーマンスが得られます。 ディレクトリとファイルを列挙するには、列挙可能なディレクトリ名またはファイル名のコレクション、またはその <xref:System.IO.DirectoryInfo>、<xref:System.IO.FileInfo>、または <xref:System.IO.FileSystemInfo> オブジェクトを返すメソッドを使用します。  
  
ディレクトリまたはファイルの名前のみを検索して返す場合は、<xref:System.IO.Directory> クラスの列挙メソッドを使用します。 ディレクトリまたはファイルの他のプロパティを検索して返す場合は、<xref:System.IO.DirectoryInfo> および <xref:System.IO.FileSystemInfo> クラスを使用します。  
  
<xref:System.Collections.Generic.List%601> のようなコレクション クラスのコンストラクターの <xref:System.Collections.Generic.IEnumerable%601> パラメーターとして、このようなメソッドの列挙可能なコレクションを使用できます。  
  
次の表は、ファイルとディレクトリの列挙可能なコレクションを返すメソッドをまとめたものです。  
  
|検索して返すもの|使用するメソッド|  
|------------------|-------------------------------------|-------------------|  
|ディレクトリ名|<xref:System.IO.Directory.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|ディレクトリ情報 (<xref:System.IO.DirectoryInfo>)|<xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType>|  
|ファイル名|<xref:System.IO.Directory.EnumerateFiles%2A?displayProperty=nameWithType>|  
|ファイル情報 (<xref:System.IO.FileInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType>|  
|ファイル システムのエントリ名|<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  
|ファイル システムのエントリ情報 (<xref:System.IO.FileSystemInfo>)|<xref:System.IO.DirectoryInfo.EnumerateFileSystemInfos%2A?displayProperty=nameWithType>|  
|ディレクトリとファイルの名前 |<xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=nameWithType>|  

> [!NOTE]
> 省略可能な <xref:System.IO.SearchOption> 列挙型の <xref:System.IO.SearchOption.AllDirectories> オプションを使用すると、親ディレクトリのサブディレクトリ内にあるすべてのファイルをすぐに列挙できますが、<xref:System.UnauthorizedAccessException> のエラーによって列挙が不完全になる可能性があります。 このような例外をキャッチするには、まずディレクトリを列挙してからファイルを列挙します。  
  
## <a name="examples-use-the-directory-class"></a>次に例を示します。 Directory クラスを使用する  
  
次の例では、<xref:System.IO.Directory.EnumerateDirectories%28System.String%29?displayProperty=nameWithType> メソッドを使用して、指定されたパスの最上位レベルのディレクトリ名のリストを取得します。  

[!code-csharp[System.IO.EnumDirs1#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.enumdirs1/cs/program.cs#1)]
[!code-vb[System.IO.EnumDirs1#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.enumdirs1/vb/program.vb#1)]  

次の例では、<xref:System.IO.Directory.EnumerateFiles%28System.String%2CSystem.String%2CSystem.IO.SearchOption%29?displayProperty=nameWithType> メソッドを使用して、特定のパターンに一致するディレクトリとサブディレクトリ内にあるすべてのファイル名を再帰的に列挙します。 次に、各ファイルの各行を読み取り、指定された文字列を含む行をファイル名とパスと共に表示します。

[!code-csharp[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/cs/program.cs#1)]
[!code-vb[System.IO.Directory.EnumerateFiles#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directory.enumeratefiles/vb/program.vb#1)]  
  
## <a name="examples-use-the-directoryinfo-class"></a>次に例を示します。 DirectoryInfo クラスを使用する  
  
次の例では、<xref:System.IO.DirectoryInfo.EnumerateDirectories%2A?displayProperty=nameWithType> メソッドを使用して、<xref:System.IO.FileSystemInfo.CreationTimeUtc> が特定の <xref:System.DateTime> 値より前の最上位ディレクトリのコレクションを一覧表示します。  

[!code-csharp[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/cs/program.cs)]
[!code-vb[System.IO.DirectoryInfo.EnumDirs#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumdirs/vb/module1.vb)]  
  
次の例では、<xref:System.IO.DirectoryInfo.EnumerateFiles%2A?displayProperty=nameWithType> メソッドを使用して、<xref:System.IO.FileInfo.Length> が 10 MB を超えるすべてのファイルを一覧表示します。 この例では、まず、最上位レベルのディレクトリを列挙して考えられる未承認アクセス例外をキャッチしてから、ファイルを列挙します。  

[!code-csharp[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/cs/program.cs#1)]
[!code-vb[System.IO.DirectoryInfo.EnumerateDirectories#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.io.directoryinfo.enumeratedirectories/vb/program.vb#1)]  
  
## <a name="see-also"></a>関連項目

- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
