---
title: '方法 : 分離ストレージ内でファイルおよびディレクトリを検索する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, finding files and directories
- locating files in isolated storage file
- directories [.NET Framework], isolated storage
- isolated storage, finding files and directories
- data storage using isolated storage, finding files and directories
- files [.NET Framework], isolated storage
- data stores, finding files and directories
- locating directories in isolated storage file
- storing data using isolated storage, finding files and directories
ms.assetid: eb28458a-6161-4e7a-9ada-30ef93761b5c
ms.openlocfilehash: dfebcc9acf0b699f898c106921d15ce0294bc5d2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75707134"
---
# <a name="how-to-find-existing-files-and-directories-in-isolated-storage"></a>方法 : 分離ストレージ内でファイルおよびディレクトリを検索する

分離ストレージ内のディレクトリを検索するには、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A?displayProperty=nameWithType> メソッドを使用します。 このメソッドは、検索パターンを表す文字列を取得します。 検索パターンでは、1 文字を表すワイルドカード文字 (?) と複数の文字を表すワイルドカード文字 (\*) の両方を使用できます。しかし、これらのワイルドカード文字は、名前の最後の部分で使用する必要があります。 たとえば、`directory1/*ect*` は有効な検索文字列ですが、`*ect*/directory2` は無効です。  
  
 ファイルを検索するには、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A?displayProperty=nameWithType> メソッドを使用します。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> に適用される、検索文字列内のワイルドカード文字の制約と同じ制約が <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A> にも適用されます。  
  
 これらはいずれも再帰的メソッドではありません。つまり、<xref:System.IO.IsolatedStorage.IsolatedStorageFile> には、ストア内のすべてのディレクトリまたはファイルを一覧表示するためのメソッドは用意されていません。 ただし、次のコード例には、再帰的メソッドの例が含まれています。  
  
## <a name="example"></a>例  
 分離ストア内にファイルおよびディレクトリを作成する方法を次のコード例で示します。 最初に、ユーザー、ドメイン、アセンブリ別に分離されたストアを取得し、`isoStore` 変数に格納します。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.CreateDirectory%2A> メソッドは複数の異なるディレクトリをセットアップするために使用され、<xref:System.IO.IsolatedStorage.IsolatedStorageFileStream.%23ctor%28System.String%2CSystem.IO.FileMode%2CSystem.IO.IsolatedStorage.IsolatedStorageFile%29> コンストラクターではそれらのディレクトリ内にいくつかのファイルを作成します。 次に、`GetAllDirectories` メソッドの結果内をループします。 このメソッドは、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetDirectoryNames%2A> を使用して現在のディレクトリ内のすべてのディレクトリ名を検索します。 これらのディレクトリ名は配列で格納されているので、`GetAllDirectories` は自分自身を呼び出し、検出したそれぞれのディレクトリを渡します。 結果として、すべてのディレクトリ名が配列に返されます。 次に、`GetAllFiles` メソッドが呼び出されます。 このメソッドは、`GetAllDirectories` を呼び出してすべてのディレクトリの名前を検索した後、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetFileNames%2A> メソッドを使用して、各ディレクトリにファイルが存在するかどうかをチェックします。 結果は、表示のために配列で返されます。  
  
 [!code-cpp[Conceptual.IsolatedStorage#9](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source8.cpp#9)]
 [!code-csharp[Conceptual.IsolatedStorage#9](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source8.cs#9)]
 [!code-vb[Conceptual.IsolatedStorage#9](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source8.vb#9)]  
  
## <a name="see-also"></a>参照

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [分離ストレージ](../../../docs/standard/io/isolated-storage.md)
