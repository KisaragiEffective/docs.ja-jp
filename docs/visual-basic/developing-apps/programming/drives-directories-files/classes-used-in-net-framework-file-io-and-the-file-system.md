---
title: .NET Framework のファイル I/O とファイル システムで使用するクラス
ms.date: 07/20/2015
helpviewer_keywords:
- file I/O classes
ms.assetid: 4a5ca924-eea8-4a95-a5f0-6ac10de276a3
ms.openlocfilehash: fe70f8fb655579049bb36fc324d04530259d25f2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "74348924"
---
# <a name="classes-used-in-net-framework-file-io-and-the-file-system-visual-basic"></a>.NET Framework のファイル I/O とファイル システムで使用するクラス (Visual Basic)

以下の表は、.NET Framework のファイル I/O で一般的に使用するクラスの一覧です。ファイル I/O クラス、ストリームの作成に使用するクラス、ストリームの読み取りと書き込みに使用するクラスに分類されています。  
  
詳細な一覧を参照するには、[クラス ライブラリの概要](../../../../standard/class-library-overview.md)に関するページをご覧ください。  
  
## <a name="basic-io-classes-for-files-drives-and-directories"></a>ファイル、ドライブ、およびディレクトリ用の基本 I/O クラス  

 次の表は、ファイル I/O に使用する主要なクラスの一覧と各クラスの説明です。  
  
|クラス|[説明]|  
|-----------|-----------------|  
|<xref:System.IO.Directory?displayProperty=nameWithType>|ディレクトリやサブディレクトリを作成、移動、および反復処理するための静的メソッドを提供します。|  
|<xref:System.IO.DirectoryInfo?displayProperty=nameWithType>|ディレクトリやサブディレクトリを作成、移動、および反復処理するためのインスタンス メソッドを提供します。|  
|<xref:System.IO.DriveInfo?displayProperty=nameWithType>|ドライブを作成、移動、および反復処理するためのインスタンス メソッドを提供します。|  
|<xref:System.IO.File?displayProperty=nameWithType>|ファイルを作成、コピー、削除、移動、およびオープンするための静的メソッドを提供し、`FileStream`の作成を支援します。|  
|<xref:System.IO.FileAccess?displayProperty=nameWithType>|ファイルの読み取り、書き込み、または読み取り/書き込みアクセスのための定数を定義します。|  
|<xref:System.IO.FileAttributes?displayProperty=nameWithType>|`Archive`、`Hidden`、`ReadOnly` など、ファイルとディレクトリの属性を提供します。|  
|<xref:System.IO.FileInfo?displayProperty=nameWithType>|ファイルを作成、コピー、削除、移動、およびオープンするための静的メソッドを提供し、`FileStream`の作成を支援します。|  
|<xref:System.IO.FileMode?displayProperty=nameWithType>|ファイルを開く方法を制御します。 このパラメーターは、`FileStream` と `IsolatedStorageFileStream` の多数のコンストラクター、および `Open` と <xref:System.IO.File> の <xref:System.IO.FileInfo> メソッドで使用します。|  
|<xref:System.IO.FileShare?displayProperty=nameWithType>|他のファイル ストリームから同一のファイルに対して可能なアクセスの種類を制御する定数を定義します。|  
|<xref:System.IO.Path?displayProperty=nameWithType>|ディレクトリ文字列を処理するためのメソッドとプロパティを提供します。|  
|<xref:System.Security.Permissions.FileIOPermission?displayProperty=nameWithType>|<xref:System.Security.Permissions.FileIOPermissionAttribute.Read%2A>、<xref:System.Security.Permissions.FileIOPermissionAttribute.Write%2A>、<xref:System.Security.Permissions.FileIOPermissionAttribute.Append%2A>、<xref:System.Security.Permissions.FileIOPermissionAttribute.PathDiscovery%2A> の各アクセス許可を定義してファイルおよびフォルダーへのアクセスを制御します。|  
  
## <a name="classes-used-to-create-streams"></a>ストリームの作成に使用するクラス  

 次の表は、ストリームの作成に使用する主要なクラスの一覧と各クラスの説明です。  
  
|クラス|[説明]|  
|-----------|-----------------|  
|<xref:System.IO.BufferedStream?displayProperty=nameWithType>|他のストリームの読み取りおよび書き込み操作に対してバッファリング レイヤーを追加します。|  
|<xref:System.IO.FileStream?displayProperty=nameWithType>|<xref:System.IO.FileStream.Seek%2A> メソッドにより、ファイルへのランダム アクセスをサポートします。 <xref:System.IO.FileStream> は、既定では同期的にファイルを開きますが、非同期操作もサポートしています。|  
|<xref:System.IO.MemoryStream?displayProperty=nameWithType>|バッキング ストアがファイルではなくメモリであるストリームを作成します。|  
|<xref:System.Net.Sockets.NetworkStream?displayProperty=nameWithType>|ネットワーク アクセスの基になるデータ ストリームを提供します。|  
|<xref:System.Security.Cryptography.CryptoStream?displayProperty=nameWithType>|データ ストリームを暗号変換にリンクするストリームを定義します。|  
  
## <a name="classes-used-to-read-from-and-write-to-streams"></a>ストリームの読み取りと書き込みに使用するクラス  

 次の表は、ストリームによるファイルの読み取りと書き込みに使用する固有のクラスの一覧です。  
  
|**クラス**|**説明**|  
|---------------|---------------------|  
|<xref:System.IO.BinaryReader?displayProperty=nameWithType>|エンコードされた文字列とプリミティブ データ型を <xref:System.IO.FileStream> から読み取ります。|  
|<xref:System.IO.BinaryWriter?displayProperty=nameWithType>|エンコードされた文字列とプリミティブ データ型を <xref:System.IO.FileStream> に書き込みます。|  
|<xref:System.IO.StreamReader?displayProperty=nameWithType>|<xref:System.IO.FileStream> を使用して文字とバイト間の変換を行い、<xref:System.IO.StreamReader.CurrentEncoding%2A> から文字を読み取ります。 <xref:System.IO.StreamReader> には、指定したストリームに適した <xref:System.IO.StreamReader.CurrentEncoding%2A> を <xref:System.IO.StreamReader.CurrentEncoding%2A> 固有のプリアンブル (バイト オーダー マークなど) に基づいて確認するコンストラクターが用意されています。|  
|<xref:System.IO.StreamWriter?displayProperty=nameWithType>|`FileStream` を使用して文字をバイトに変換し、<xref:System.IO.StreamWriter.Encoding%2A> に文字を書き込みます。|  
|<xref:System.IO.StringReader?displayProperty=nameWithType>|`String` から文字を読み取ります。 出力は、任意のエンコーディングのストリームまたは `String` です。|  
|<xref:System.IO.StringWriter?displayProperty=nameWithType>|`String` に文字を書き込みます。 出力は、任意のエンコーディングのストリームまたは `String` です。|  
  
## <a name="see-also"></a>参照

- [ストリームの構成](../../../../standard/io/composing-streams.md)
- [ファイルおよびストリーム入出力](../../../../standard/io/index.md)
- [非同期ファイル I/O](../../../../standard/io/asynchronous-file-i-o.md)
- [.NET Framework のファイル I/O とファイル システムの基礎 (Visual Basic)](../../../../visual-basic/developing-apps/programming/drives-directories-files/basics-of-net-framework-file-io-and-the-file-system.md)
