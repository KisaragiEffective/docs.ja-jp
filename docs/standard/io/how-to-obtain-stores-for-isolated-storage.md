---
title: '方法 : 分離ストレージでストアを取得する'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- stores, obtaining
- storing data using isolated storage, obtaining stores
- isolated storage, obtaining stores
- data stores, obtaining
- data storage using isolated storage, obtaining stores
ms.assetid: fcb6b178-d526-47c4-b029-e946f880f9db
ms.openlocfilehash: 7104ba665f60c2d55217a2d8628c85f6e469ad6f
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75706932"
---
# <a name="how-to-obtain-stores-for-isolated-storage"></a>方法 : 分離ストレージでストアを取得する
分離ストアでは、データ コンパートメント内の仮想ファイル システムを公開します。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile> クラスでは、分離ストアと対話するためのいくつかのメソッドが提供されます。 ストアを作成して取得するために、<xref:System.IO.IsolatedStorage.IsolatedStorageFile> では次の 3 つの静的メソッドが提供されます。  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A> は、ユーザーおよびアセンブリ別に分離されるストレージを返します。  
  
- <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> は、ドメインおよびアセンブリ別に分離されるストレージを返します。  
  
     両方のメソッドで、呼び出し元のコードに属するストアが取得されます。  
  
- 静的メソッド <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> は、スコープ パラメーターの組み合わせを渡すことで指定される分離ストアを返します。  
  
 次のコードでは、ユーザー、アセンブリ、およびドメイン別に分離されるストアを返します。  
  
 [!code-cpp[Conceptual.IsolatedStorage#6](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#6)]
 [!code-csharp[Conceptual.IsolatedStorage#6](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#6)]
 [!code-vb[Conceptual.IsolatedStorage#6](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#6)]  
  
 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> メソッドを使用して、ストアが移動ユーザー プロファイルと共にローミングするように指定することができます。 この設定方法の詳細については、「[分離のタイプ](../../../docs/standard/io/types-of-isolation.md)」を参照してください。  
  
 異なるアセンブリ内から取得される分離ストアは、既定では異なるストアとなります。 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> メソッドのパラメーターでアセンブリまたはドメインの証拠を渡すことで、異なるアセンブリまたはドメインのストアにアクセスできます。 この場合、アプリケーション ドメイン ID で分離ストレージにアクセスするアクセス許可が必要になります。 詳細については、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> メソッドのオーバーロードに関するページを参照してください。  
  
 <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A>、および <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> メソッドは <xref:System.IO.IsolatedStorage.IsolatedStorageFile> オブジェクトを返します。 状況に最適な分離タイプの判別に役立つ「[分離のタイプ](../../../docs/standard/io/types-of-isolation.md)」を参照してください。 分離ストレージ ファイル オブジェクトがある場合は、分離ストレージ メソッドを使用して、ファイルとディレクトリの読み取り、書き込み、作成、削除を行うことができます。  
  
 ストア自体を取得するのに十分なアクセス権がないコードに <xref:System.IO.IsolatedStorage.IsolatedStorageFile> オブジェクトを渡さないようにするためのメカニズムはありません。 ドメインとアセンブリの ID と分離ストレージのアクセス許可が確認されるのは、通常、<xref:System.IO.IsolatedStorage.IsolatedStorage>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>、または <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> メソッドで <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> オブジェクトへの参照が取得される場合のみです。 したがって、<xref:System.IO.IsolatedStorage.IsolatedStorageFile> への参照を保護するのは、これらの参照を使用するコードの役目です。  
  
## <a name="example"></a>例  
 次のコードでは、ユーザーとアセンブリ別に分離されるストアを取得するクラスの単純な例を示します。 <xref:System.IO.IsolatedStorage.IsolatedStorageScope.Domain?displayProperty=nameWithType> メソッドが渡す引数に <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> を追加することで、ユーザー、ドメイン、およびアセンブリ別に分離されるストアを取得するようにコードを変更できます。  
  
 コードを実行したら、コマンド ラインで「**StoreAdm /LIST**」と入力して、ストアが作成されたことを確認することができます。 これにより、[分離ストレージ ツール (Storeadm.exe)](../../../docs/framework/tools/storeadm-exe-isolated-storage-tool.md) が実行され、ユーザーの現在の分離ストアがすべてリストされます。  
  
 [!code-cpp[Conceptual.IsolatedStorage#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source6.cpp#7)]
 [!code-csharp[Conceptual.IsolatedStorage#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source6.cs#7)]
 [!code-vb[Conceptual.IsolatedStorage#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source6.vb#7)]  
  
## <a name="see-also"></a>参照

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- <xref:System.IO.IsolatedStorage.IsolatedStorageScope>
- [分離ストレージ](../../../docs/standard/io/isolated-storage.md)
- [分離のタイプ](../../../docs/standard/io/types-of-isolation.md)
- [.NET のアセンブリ](../assembly/index.md)
