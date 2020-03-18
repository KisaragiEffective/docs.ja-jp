---
title: .NET Framework のファイル I/O とファイル システムの基礎
ms.date: 07/20/2015
helpviewer_keywords:
- file access, file I/O in Visual Basic
- file attributes, determining
- streams, fundamental operations
- file permissions
- streams
- streams, definition
ms.assetid: 49d837c0-cf28-416f-8606-4d83d7b479ef
ms.openlocfilehash: 5d60d0089d042c0be343c741c26de0b4b7778d6d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348945"
---
# <a name="basics-of-net-framework-file-io-and-the-file-system-visual-basic"></a>.NET Framework のファイル I/O とファイル システムの基礎 (Visual Basic)

<xref:System.IO> 名前空間のクラスは、ドライブ、ファイル、ディレクトリの操作に使用されます。

<xref:System.IO> 名前空間には <xref:System.IO.File> クラスと <xref:System.IO.Directory> クラスが含まれています。これらのクラスを使用すると、.NET Framework でファイルとディレクトリを操作できます。 これらのオブジェクトのメソッドは静的メンバーまたは共有メンバーなので、あらかじめクラスのインスタンスを作成しなくてもメンバーを直接使用できます。 これらのクラスに関連するクラスとして、<xref:System.IO.FileInfo> クラスと <xref:System.IO.DirectoryInfo> クラスがあります。`My` 機能を使用しているユーザーには使い慣れたクラスです。 これらのクラスを使用するには、名前を完全修飾するか、または、関係するコードの先頭に `Imports` ステートメントを記述して、適切な名前空間をインポートする必要があります。 詳細については、「[Imports ステートメント (.NET 名前空間および型)](../../../../visual-basic/language-reference/statements/imports-statement-net-namespace-and-type.md)」を参照してください。

> [!NOTE]
> このセクションのそのほかのトピックでは `My.Computer.FileSystem` のクラスではなく、 `System.IO` オブジェクトを使用して、ドライブ、ファイル、およびディレクトリを操作します。 `My.Computer.FileSystem` オブジェクトは主に Visual Basic のプログラムで使用することを目的としています。 `System.IO` のクラスは .NET Framework をサポートする Visual Basic などの言語で使用するためのものです。

## <a name="definition-of-a-stream"></a>ストリームの定義

.NET Framework では、ファイルに対する読み取りと書き込みをサポートするストリームを使用します。 ストリームとは、1 次元の連続したデータの集まりと考えることができます。ストリームには先頭と末尾があり、カーソルでストリーム内での現在の位置を示します。

![Filestream 内の現在の位置を示すカーソル](./media/basics-of-net-framework-file-io-and-the-file-system/filestream-cursor-position.gif)

## <a name="stream-operations"></a>ストリームの操作

ストリームに格納されているデータは、メモリ、ファイル、または TCP/IP ソケットから取得したものです。 ストリームに対しては、次の基本操作を実行できます。

- **読み取り**。 ストリームを読み取ったり、文字列やバイト配列などのデータ構造にストリームからデータを転送したりできます。

- **書き込み**。 ストリームに書き込んだり、データ ソースからストリームにデータを転送したりできます。

- **シーク**。 ストリーム内の現在の位置をクエリおよび変更できます。

詳細については、「 [ストリームの作成](../../../../standard/io/composing-streams.md)」を参照してください。

## <a name="types-of-streams"></a>ストリームの種類

.NET Framework では、ストリームは <xref:System.IO.Stream> クラスで表されます。これは、その他のすべてのストリームのための抽象クラスです。 <xref:System.IO.Stream> クラスのインスタンスを直接作成することはできません。これを実装するいずれかのクラスを使用する必要があります。

ストリームにはさまざまな種類がありますが、ファイルの入出力 (I/O) を処理するという目的のために最も重要なのは、ファイルに対する読み取りと書き込みを実現する <xref:System.IO.FileStream> クラスと、分離ストレージに対するファイルやディレクトリの作成を実現する <xref:System.IO.IsolatedStorage.IsolatedStorageFileStream> クラスです。 この他、ファイル I/O を処理する場合、以下のようなストリームを使用できます。

- <xref:System.IO.BufferedStream>

- <xref:System.Security.Cryptography.CryptoStream>

- <xref:System.IO.MemoryStream>

- [https://login.microsoftonline.com/consumers/](<xref:System.Net.Sockets.NetworkStream>)

次の表は、ストリームで一般的に実行するタスクの一覧です。

|ターゲット|参照先|
|---|---|
|データ ファイルに対する読み取りと書き込み|[方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../../standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)|
|ファイルのテキストの読み取り|[方法: ファイルからテキストを読み取る](../../../../standard/io/how-to-read-text-from-a-file.md)|
|テキストのファイルへの書き込み|[方法: ファイルにテキストを書き込む](../../../../standard/io/how-to-write-text-to-a-file.md)|
|文字列からの文字の読み取り|[方法: 文字列から文字を読み取る](../../../../standard/io/how-to-read-characters-from-a-string.md)|
|文字列への文字の書き込み|[方法: 文字列に文字を書き込む](../../../../standard/io/how-to-write-characters-to-a-string.md)|
|データを暗号化する|[データの暗号化](../../../../standard/security/encrypting-data.md)|
|データの復号化|[データの復号化](../../../../standard/security/decrypting-data.md)|

## <a name="file-access-and-attributes"></a>ファイル アクセスと属性

ファイルの作成、オープン、および共有の方法は、<xref:System.IO.FileAccess>、<xref:System.IO.FileMode>、および <xref:System.IO.FileShare> の各列挙体で制御できます。これらの列挙体には、<xref:System.IO.FileStream> クラスのコンストラクターで使用するフラグが含まれています。 たとえば、<xref:System.IO.FileStream> を開くかまたは新規作成するときに、ファイルを追加書き込み用に開くかどうか、指定のファイルが存在しない場合にファイルを新規作成するかどうか、ファイルを上書きするかどうか、などを <xref:System.IO.FileMode> 列挙体で指定できます。

<xref:System.IO.FileAttributes> 列挙体を使用すると、ファイル固有の情報を収集できます。 <xref:System.IO.FileAttributes> 列挙体は、格納されているファイルの属性を返します。これらの属性によって、圧縮ファイル、暗号化されたファイル、隠しファイル、読み取り専用ファイル、アーカイブ、ディレクトリ、システム ファイル、一時ファイルであるかどうかがわかります。

次の表は、ファイル アクセスとファイル属性に関連するタスクの一覧です。

|ターゲット|参照先|
|---|---|
|ログ ファイルのオープンとテキストの追加|[方法: ログ ファイルを開いて情報を追加する](../../../../standard/io/how-to-open-and-append-to-a-log-file.md)|
|ファイルの属性の判断|<xref:System.IO.FileAttributes>|

## <a name="file-permissions"></a>ファイルのアクセス許可

ファイルおよびディレクトリに対するアクセスの制御は、<xref:System.Security.Permissions.FileIOPermission> クラスで行うことができます。 これは、Web フォームの開発者には特に重要な場合があります。既定では、Web フォームは、ASPNET という名前の特別なローカル ユーザー アカウントのコンテキストで実行されます。ASPNET は、ASP.NET および .NET Framework のインストール時に作成されます。 ASPNET ユーザー アカウントはアクセス許可が制限されているため、アプリケーションがリソースへのアクセスを要求したときに、ユーザーが処理を実行できない場合があります (たとえば、Web アプリケーションからファイルへの書き込みなど)。 詳細については、<xref:System.Security.Permissions.FileIOPermission> を参照してください。

## <a name="isolated-file-storage"></a>分離ファイル ストレージ

分離ストレージとは、ファイルを扱うときに、必要なアクセス許可をユーザーまたはコードが持っていないために生じる問題を解決するためのものです。 分離ストレージでは、各ユーザーにデータ コンパートメントが割り当てられます。このデータ コンパートメントには、1 つまたは複数のストアを保持できます。 ストア間は、ユーザーおよびアセンブリごとに分離できます。 ストアにアクセスできるのは、それを作成したユーザーおよびアセンブリのみです。 ストアは、完全な仮想ファイル システムとして機能します。つまり、ストア内でディレクトリやファイルを作成および操作できます。

次の表は、分離ファイル ストレージに一般に関連するタスクの一覧です。

|ターゲット|参照先|
|---|---|
|分離ストアの作成|[方法: 分離ストレージでストアを取得する](../../../../standard/io/how-to-obtain-stores-for-isolated-storage.md)|
|分離ストアの列挙|[方法: 分離ストレージでストアを列挙する](../../../../standard/io/how-to-enumerate-stores-for-isolated-storage.md)|
|分離ストアの削除|[方法: 分離ストレージでストアを削除する](../../../../standard/io/how-to-delete-stores-in-isolated-storage.md)|
|分離ストレージのファイルまたはディレクトリの作成|[方法: 分離ストレージでファイルおよびディレクトリを作成する](../../../../standard/io/how-to-create-files-and-directories-in-isolated-storage.md)|
|分離ストレージのファイルの検索|[方法: 分離ストレージ内でファイルおよびディレクトリを検索する](../../../../standard/io/how-to-find-existing-files-and-directories-in-isolated-storage.md)|
|分離ストレージのファイルに対する読み取りと書き込み|[方法: 分離ストレージ内でファイルの読み取りと書き込みを行う](../../../../standard/io/how-to-read-and-write-to-files-in-isolated-storage.md)|
|分離ストレージのファイルまたはディレクトリの削除|[方法: 分離ストレージでファイルおよびディレクトリを削除する](../../../../standard/io/how-to-delete-files-and-directories-in-isolated-storage.md)|

## <a name="file-events"></a>ファイルのイベント

<xref:System.IO.FileSystemWatcher> コンポーネントを使用すると、自システム上のファイルとディレクトリ、またはネットワークでアクセスできる任意のコンピューター上のファイルとディレクトリの変更を監視できます。 たとえば、ファイルが変更されたときに、その旨をユーザーに警告することが必要な場合があります。 変更が行われると、1 つまたは複数のイベントが発生し、バッファーに格納され、<xref:System.IO.FileSystemWatcher> コンポーネントに渡されて処理されます。

## <a name="see-also"></a>参照

- [ストリームの構成](../../../../standard/io/composing-streams.md)
- [ファイルおよびストリーム入出力](../../../../standard/io/index.md)
- [非同期ファイル I/O](../../../../standard/io/asynchronous-file-i-o.md)
- [.NET Framework のファイル I/O とファイル システムで使用するクラス (Visual Basic)](../../../../visual-basic/developing-apps/programming/drives-directories-files/classes-used-in-net-framework-file-io-and-the-file-system.md)
