---
title: 共通 I/O タスク
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- I/O, common tasks
ms.assetid: bf00c380-706a-4e38-b829-454a480629fc
ms.openlocfilehash: 01e9d6b50bd7eeafea792a772ca86a81e40dafd4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708179"
---
# <a name="common-io-tasks"></a>共通 I/O タスク
<xref:System.IO> 名前空間には、読み取り、書き込みなどの各種アクションをファイル、ディレクトリ、およびストリーム上で実行できるようにするいくつかのクラスが用意されています。 詳細については、「[ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)」を参照してください。  
  
## <a name="common-file-tasks"></a>共通ファイル タスク  
  
|目的|参照項目|  
|-------------------|--------------------------------------|  
|テキスト ファイルの作成|<xref:System.IO.File.CreateText%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.CreateText%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.File.Create%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.Create%2A?displayProperty=nameWithType> メソッド|  
|テキスト ファイルへの書き込み|[方法: ファイルにテキストを書き込む](../../../docs/standard/io/how-to-write-text-to-a-file.md)<br /><br /> [方法: テキスト ファイルを記述する (C++/CLI)](/cpp/dotnet/how-to-write-a-text-file-cpp-cli)|  
|テキスト ファイルからの読み取り|[方法: ファイルからテキストを読み取る](../../../docs/standard/io/how-to-read-text-from-a-file.md)|  
|ファイルへのテキストの追加|[方法: ログ ファイルを開いて情報を追加する](../../../docs/standard/io/how-to-open-and-append-to-a-log-file.md)<br /><br /> <xref:System.IO.File.AppendText%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.AppendText%2A?displayProperty=nameWithType> メソッド|  
|ファイル名の変更またはファイルの移動|<xref:System.IO.File.Move%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.MoveTo%2A?displayProperty=nameWithType> メソッド|  
|ファイルの削除|<xref:System.IO.File.Delete%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.Delete%2A?displayProperty=nameWithType> メソッド|  
|ファイルのコピー|<xref:System.IO.File.Copy%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.CopyTo%2A?displayProperty=nameWithType> メソッド|  
|ファイルのサイズの取得|<xref:System.IO.FileInfo.Length%2A?displayProperty=nameWithType> プロパティ|  
|ファイルの属性の取得|<xref:System.IO.File.GetAttributes%2A?displayProperty=nameWithType> メソッド|  
|ファイルの属性の設定|<xref:System.IO.File.SetAttributes%2A?displayProperty=nameWithType> メソッド|  
|ファイルが存在するかどうかの確認|<xref:System.IO.File.Exists%2A?displayProperty=nameWithType> メソッド|  
|バイナリ ファイルからの読み取り|[方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)|  
|バイナリ ファイルへの書き込み|[方法: 新しく作成されたデータ ファイルに対して読み書きする](../../../docs/standard/io/how-to-read-and-write-to-a-newly-created-data-file.md)|  
|ファイル名拡張子の取得|<xref:System.IO.Path.GetExtension%2A?displayProperty=nameWithType> メソッド|  
|ファイルの絶対パスの取得|<xref:System.IO.Path.GetFullPath%2A?displayProperty=nameWithType> メソッド|  
|パスからのファイル名と拡張子の取得|<xref:System.IO.Path.GetFileName%2A?displayProperty=nameWithType> メソッド|  
|ファイルの拡張子の変更|<xref:System.IO.Path.ChangeExtension%2A?displayProperty=nameWithType> メソッド|  
  
## <a name="common-directory-tasks"></a>共通ディレクトリ タスク  
  
|目的|参照項目|  
|-------------------|--------------------------------------|  
|My Documents などの特別なフォルダー内のファイルへのアクセス|[方法: ファイルにテキストを書き込む](../../../docs/standard/io/how-to-write-text-to-a-file.md)|  
|ディレクトリを作成する|<xref:System.IO.Directory.CreateDirectory%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.FileInfo.Directory%2A?displayProperty=nameWithType> プロパティ|  
|サブディレクトリの作成|<xref:System.IO.DirectoryInfo.CreateSubdirectory%2A?displayProperty=nameWithType> メソッド|  
|ディレクトリの名前変更または移動|<xref:System.IO.Directory.Move%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.DirectoryInfo.MoveTo%2A?displayProperty=nameWithType> メソッド|  
|ディレクトリをコピーする|[方法: ディレクトリをコピーする](../../../docs/standard/io/how-to-copy-directories.md)|  
|ディレクトリを削除する|<xref:System.IO.Directory.Delete%2A?displayProperty=nameWithType> メソッド<br /><br /> <xref:System.IO.DirectoryInfo.Delete%2A?displayProperty=nameWithType> メソッド|  
|ディレクトリ内のファイルとサブディレクトリの確認|[方法: ディレクトリとファイルを列挙する](../../../docs/standard/io/how-to-enumerate-directories-and-files.md)|  
|ディレクトリのサイズの確認|<xref:System.IO.Directory?displayProperty=nameWithType> クラス|  
|ディレクトリが存在するかどうかの確認|<xref:System.IO.Directory.Exists%2A?displayProperty=nameWithType> メソッド|  
  
## <a name="see-also"></a>参照

- [ファイルおよびストリーム入出力](../../../docs/standard/io/index.md)
- [ストリームの構成](../../../docs/standard/io/composing-streams.md)
- [非同期ファイル I/O](../../../docs/standard/io/asynchronous-file-i-o.md)
