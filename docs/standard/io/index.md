---
title: ファイルおよびストリーム入出力 - .NET
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- IO namespace
- files, I/O
- System.IO namespace
- I/O [.NET Framework]
- streams, I/O
- data streams, I/O
ms.assetid: 4f4a33a9-66b7-4cd7-a285-4ad3e4276cd2
ms.openlocfilehash: 3c69e0fd23b1f8bc11fe908c66ba492f31a53f30
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75706596"
---
# <a name="file-and-stream-io"></a>ファイルおよびストリーム入出力

ファイルおよびストリーム I/O (入出力) とは、ストレージ メディアとの間のデータの転送を指します。 .NET Framework では、`System.IO` 名前空間に、データ ストリームおよびファイルで同期的および非同期的に読み取りと書き込みを有効にする型が用意されています。 これらの名前空間には、ファイルを圧縮および圧縮解除する型、パイプとシリアル ポート経由の通信を有効にする型もあります。

ファイルとは、バイトを順序立てて格納する名前付きのコレクションであり、永続ストレージを保有します。 ファイルを操作するときには、ディレクトリ パス、ディスク ストレージ、ファイル名、ディレクトリ名を操作します。 これに対し、ストリームは、バッキング ストアの読み取りと書き込みに使用できるバイトのシーケンスです。バッキング ストアは、複数のストレージ メディアのいずれかになります (たとえば、ディスク、メモリ)。 ディスク以外にいくつかのバッキング ストアが存在するのと同じように、ファイル ストリーム以外にも、ネットワーク ストリーム、メモリ ストリーム、パイプ ストリームなど、さまざまなストリームが存在します。

## <a name="files-and-directories"></a>ファイルとディレクトリ

<xref:System.IO?displayProperty=nameWithType> 名前空間の型を使用して、ファイルとディレクトリを操作できます。 たとえば、ファイルとディレクトリに対するプロパティを取得および設定したり、検索条件に基づいて、ファイルとディレクトリのコレクションを取得したりできます。

.NET Core 1.1 以降および .NET Framework 4.6.2 でサポートされている DOS デバイスの構文を含む、パスの名前付け規則および Windows システムのファイル パスを表す方法の詳細については、「[Windows システムのファイル パスの形式](file-path-formats.md)」を参照してください。

一般的なファイルおよびディレクトリのクラスを次に示します。

- <xref:System.IO.File> - ファイルを作成、コピー、削除、移動、およびオープンするための静的メソッドを提供し、<xref:System.IO.FileStream> オブジェクトの作成を支援します。

- <xref:System.IO.FileInfo> - ファイルを作成、コピー、削除、移動、およびオープンするためのインスタンス メソッドを提供し、<xref:System.IO.FileStream> オブジェクトの作成を支援します。

- <xref:System.IO.Directory> - ディレクトリやサブディレクトリを作成、移動、および反復処理するための静的メソッドを提供します。

- <xref:System.IO.DirectoryInfo> - ディレクトリやサブディレクトリを作成、移動、および反復処理するためのインスタンス メソッドを提供します。

- <xref:System.IO.Path> - 複数のプラットフォームにまたがってディレクトリ文字列を処理するためのメソッドおよびプロパティを提供します。

ファイル システム メソッドを呼び出すときは、常に堅牢な例外処理を用意するようにします。 詳細については、[I/O エラーの処理](handling-io-errors.md)に関するページを参照してください。

これらのクラスに加えて、Visual Basic のユーザーは、ファイル I/O 用の <xref:Microsoft.VisualBasic.FileIO.FileSystem?displayProperty=nameWithType> クラスに用意されているメソッドとプロパティを使用できます。

「[方法: ディレクトリをコピーする](how-to-copy-directories.md)、「[方法: ディレクトリ一覧を作成する](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/5cf8zcfh(v=vs.100))」、「[方法: ディレクトリとファイルを列挙する](how-to-enumerate-directories-and-files.md)。

## <a name="streams"></a>ストリーム

抽象基底クラス <xref:System.IO.Stream> は、バイトの読み取りと書き込みをサポートします。 ストリームを表すすべてのクラスは、<xref:System.IO.Stream> クラスから継承されます。 <xref:System.IO.Stream> クラスとその派生クラスは、データ ソースやリポジトリの共通ビューを提供し、プログラマがオペレーティング システムや基になるデバイスに固有の詳細事項を考慮する必要をなくします。

ストリームには次の 3 つの基本的な操作が含まれます。

- 読み取り - ストリームからバイト配列などのデータ構造体にデータを転送します。

- 書き込み - データ ソースからストリームにデータを転送します。

- シーク - ストリーム内の現在位置を照会および変更します。

基になるデータ ソースまたはリポジトリによっては、ストリームがサポートできる機能が上記の一部に限られる場合があります。 たとえば、<xref:System.IO.Pipes.PipeStream> クラスは、シークをサポートしません。 ストリームの <xref:System.IO.Stream.CanRead%2A>、<xref:System.IO.Stream.CanWrite%2A>、および <xref:System.IO.Stream.CanSeek%2A> の各プロパティで、そのストリームがサポートする操作を指定します。

一般的なストリーム クラスを次に示します。

- <xref:System.IO.FileStream> – ファイルの読み取りと書き込みを実行します。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> – 分離ストレージのファイルの読み取りと書き込みを実行します。

- <xref:System.IO.MemoryStream> – バッキング ストアとしてメモリの読み取りと書き込みを実行します。

- <xref:System.IO.BufferedStream> – 読み取り操作と書き込み操作のパフォーマンスを向上します。

- <xref:System.Net.Sockets.NetworkStream> – ネットワーク ソケットを介して読み取りと書き込みを実行します。

- <xref:System.IO.Pipes.PipeStream> – 匿名パイプと名前付きパイプを介して読み取りと書き込みを実行します。

- <xref:System.Security.Cryptography.CryptoStream> – データ ストリームを暗号変換にリンクします。

ストリームを非同期的に操作する例については、「[非同期ファイル I/O](asynchronous-file-i-o.md)」を参照してください。

## <a name="readers-and-writers"></a>リーダーとライター

<xref:System.IO?displayProperty=nameWithType> 名前空間には、ストリームからエンコードされた文字を読み取ったり、ストリームに書き込んだりするための型も用意されています。 通常、ストリームはバイトの入出力用に設計されています。 リーダー型とライター型は、ストリームが操作を完了できるように、バイトとの間でエンコードされた文字の変換を処理します。 各リーダー クラスとライター クラスは、クラスの `BaseStream` プロパティから取得できるストリームに関連付けられます。

一般的なリーダー クラスとライター クラスを次に示します。

- <xref:System.IO.BinaryReader> および <xref:System.IO.BinaryWriter> – バイナリ値としてプリミティブ データ型の読み取りと書き込みを実行します。

- <xref:System.IO.StreamReader> および <xref:System.IO.StreamWriter> – 文字とバイトを相互変換するエンコーディング値を使用して、文字の読み取りと書き込みを実行します。

- <xref:System.IO.StringReader> および <xref:System.IO.StringWriter> – 文字列から文字を読み取り、文字列に文字を書き込みます。

- <xref:System.IO.TextReader> および <xref:System.IO.TextWriter> – バイナリ データを除く、文字と文字列の読み取りと書き込みを行う他のリーダーとライターの抽象基底クラスとして機能します。

「[方法: ファイルからテキストを読み取る](how-to-read-text-from-a-file.md)」、「[方法: ファイルにテキストを書き込む](how-to-write-text-to-a-file.md)」、「[方法: 文字列から文字を読み取る](how-to-read-characters-from-a-string.md)」、「[方法: 文字列への文字の書き込み](how-to-write-characters-to-a-string.md)」

## <a name="asynchronous-io-operations"></a>非同期 I/O 操作

大量のデータを読み取ったり書き込んだりすると、リソースが大量に消費されることがあります。 アプリケーションのユーザーに対する応答性を維持する必要がある場合は、これらのタスクを非同期的に実行する必要があります。 同期 I/O 操作の場合、大量のリソースを消費する操作が完了するまで UI スレッドがブロックされます。  Windows 8.x ストア アプリを開発するときには非同期 I/O 操作を使用して、アプリが動作しなくなったと受け取られないようにします。

`Async`、<xref:System.IO.Stream.CopyToAsync%2A>、<xref:System.IO.Stream.FlushAsync%2A>、および <xref:System.IO.Stream.ReadAsync%2A> の各メソッドのように、非同期メンバーの名前には <xref:System.IO.Stream.WriteAsync%2A> が含まれています。 これらのメソッドは、`async` キーワードおよび `await` キーワードと共に使用します。

詳細については、「[非同期ファイル I/O](asynchronous-file-i-o.md)」を参照してください。

## <a name="compression"></a>[圧縮]

圧縮とは、保管するためにファイルのサイズを小さくするプロセスのことです。 圧縮解除は、圧縮ファイルの内容を抽出し、その内容を使用可能な形式にするプロセスです。 <xref:System.IO.Compression?displayProperty=nameWithType> 名前空間には、ファイルおよびストリームを圧縮および圧縮解除するための型が含まれています。

次のクラスは、ファイルおよびストリームを圧縮および圧縮解除するときによく使用します。

- <xref:System.IO.Compression.ZipArchive> – ZIP アーカイブのエントリを作成および取得します。

- <xref:System.IO.Compression.ZipArchiveEntry> – 圧縮ファイルを表します。

- <xref:System.IO.Compression.ZipFile> – 圧縮パッケージの作成、抽出、およびオープンを行います。

- <xref:System.IO.Compression.ZipFileExtensions> – 圧縮パッケージのエントリを作成および抽出します。

- <xref:System.IO.Compression.DeflateStream> – Deflate アルゴリズムを使用してストリームを圧縮および圧縮解除します。

- <xref:System.IO.Compression.GZipStream> – gzip データ形式のストリームを圧縮および圧縮解除します。

「[方法:ファイルを圧縮して抽出する](how-to-compress-and-extract-files.md)」

## <a name="isolated-storage"></a>分離ストレージ

分離ストレージは、コードと保存データを関連付ける標準化された方法を定義することにより、分離性と安全性を提供するデータ ストレージ機構です。 ストレージは、ユーザー、アセンブリ、および (必要に応じて) ドメイン別に分離された仮想ファイル システムを提供します。 分離ストレージは、アプリケーションにユーザー ファイルへのアクセス許可がない場合に特に便利です。 コンピューターのセキュリティ ポリシーによって制御される方法でアプリケーションの設定またはファイルを保存できます。

分離ストレージは Windows 8.x ストア アプリには使用できません。代わりに、<xref:Windows.Storage?displayProperty=nameWithType> 名前空間のアプリケーション データ クラスを使用します。 詳細については、[アプリケーション データ](https://docs.microsoft.com/previous-versions/windows/apps/hh464917%28v=win.10%29)に関するページをご覧ください。

次のクラスは、分離ストレージを実装するときによく使用します。

- <xref:System.IO.IsolatedStorage.IsolatedStorage> – 分離ストレージの実装の基底クラスを提供します。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile> – ファイルとディレクトリを格納している分離ストレージ領域を提供します。

- <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> - 分離ストレージ内のファイルを公開します。

「[分離ストレージ](isolated-storage.md)」を参照してください。

## <a name="io-operations-in-windows-store-apps"></a>Windows ストア アプリの I/O 操作

Windows 8.x ストア アプリ用 .NET には、ストリームの読み取りと書き込みを行う型が多数あります。ただし、.NET Framework のすべての I/O 型がこのセットに含まれているわけではありません。

次に、I/O 操作を Windows 8.x ストア アプリで使用する場合に注意する必要がある重要な違いを示します。

- <xref:System.IO.File>、<xref:System.IO.FileInfo>、<xref:System.IO.Directory>、<xref:System.IO.DirectoryInfo> など、特にファイル操作に関連する型は、Windows 8.x ストア アプリ用 .NET には含まれていません。 代わりに、Windows ランタイムの名前空間 <xref:Windows.Storage?displayProperty=nameWithType> の型 (<xref:Windows.Storage.StorageFile> や <xref:Windows.Storage.StorageFolder> など) を使用します。

- 分離ストレージは使用できません。代わりに、[アプリケーション データ](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10))を使用します。

- UI スレッドをブロックしないように、<xref:System.IO.Stream.ReadAsync%2A>、<xref:System.IO.Stream.WriteAsync%2A> などの非同期メソッドを使用します。

- パス ベース圧縮の型 <xref:System.IO.Compression.ZipFile> と <xref:System.IO.Compression.ZipFileExtensions> は使用できません。 代わりに、名前空間 <xref:Windows.Storage.Compression?displayProperty=nameWithType> の型を使用します。

必要に応じて、.NET Framework ストリームと Windows ランタイム ストリームを変換できます。 詳細については、[.NET Framework ストリームと Windows ランタイム ストリームの間で変換を行う](how-to-convert-between-dotnet-streams-and-winrt-streams.md)」または <xref:System.IO.WindowsRuntimeStreamExtensions> を参照してください。

Windows 8.x ストア アプリでの I/O 操作の詳細については、「[Quickstart: Reading and writing files](https://docs.microsoft.com/previous-versions/windows/apps/hh758325(v=win.10))」 (クイック スタート: ファイルの読み取りと書き込み) を参照してください。

## <a name="io-and-security"></a>I/O とセキュリティ

<xref:System.IO?displayProperty=nameWithType> 名前空間のクラスを使用する場合、アクセス制御リスト (ACL: Access Control List) などのオペレーティング システムのセキュリティ要件に従い、ファイルとディレクトリへのアクセスを制御する必要があります。 この要件の他にも、<xref:System.Security.Permissions.FileIOPermission> で指定されている要件を満たす必要があります。 ACL はプログラムで管理できます。 詳細については、[ アクセス制御リスト エントリを追加または削除する](how-to-add-or-remove-access-control-list-entries.md)」を参照してください。

既定のセキュリティ ポリシーでは、インターネットまたはイントラネットのアプリケーションはユーザーのコンピューターのファイルにアクセスできません。 したがって、インターネットまたはイントラネットを経由してダウンロードされるコードを記述する場合に、物理ファイル パスを必要とする I/O クラスを使用しないでください。 代わりに、従来の .NET Framework アプリケーション用の[分離ストレージ](isolated-storage.md)を使用するか、Windows 8.x ストア アプリ用の[アプリケーション データ](https://docs.microsoft.com/previous-versions/windows/apps/hh464917(v=win.10))を使用します。

セキュリティ チェックが実行されるのは、ストリームが構築されている場合だけです。 したがって、ストリームを開いて、そのストリームを信頼度の低いコードやアプリケーション ドメインに渡さないでください。

## <a name="related-topics"></a>関連トピック

- [共通 I/O タスク](common-i-o-tasks.md)\
ファイル、ディレクトリ、およびストリームに関連する I/O タスクの一覧、および各タスクの関連するコンテンツと例へのリンクを示します。

- [非同期ファイル I/O](asynchronous-file-i-o.md)\
非同期 I/O のパフォーマンス上の利点と基本的な操作について説明します。

- [分離ストレージ](isolated-storage.md)\
コードと保存データを関連付ける標準的な方法を定義することにより、分離性と安全性を提供するデータ ストレージ機構について説明します。

- [パイプ](pipe-operations.md)\
.NET Framework での匿名パイプ操作と名前付きパイプ操作について説明します。

- [メモリ マップト ファイル](memory-mapped-files.md)\
ディスク上のファイルの内容を仮想メモリ内に格納するメモリ マップト ファイルについて説明します。 メモリ マップト ファイルは、非常に大きなファイルを編集したり、プロセス間通信用の共有メモリを作成したりするために使用できます。
