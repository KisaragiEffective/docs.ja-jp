---
title: '方法: 分離ストレージの領域不足状態に備える'
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- data stores, quotas
- isolated storage, quotas
- quantity of isolated storage used
- limit on isolated storage used
- stores, quotas
- stores, out of space conditions
- data storage using isolated storage, quotas
- storing data using isolated storage, quotas
- space remaining in isolated storage
- data stores, out of space conditions
- storing data using isolated storage, out of space conditions
- quotas for isolated storage
- isolated storage, out of space conditions
- data storage using isolated storage, out of space conditions
ms.assetid: e35d4535-3732-421e-b1a3-37412e036145
ms.openlocfilehash: 5666019e1a65880221261ef5ad704f82c37263b2
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2020
ms.locfileid: "75708116"
---
# <a name="how-to-anticipate-out-of-space-conditions-with-isolated-storage"></a>方法: 分離ストレージの領域不足状態に備える

分離ストレージを使用するコードは、分離ストレージ ファイルとディレクトリが存在するデータ コンパートメントの最大サイズを規定する[クォータ](../../../docs/standard/io/isolated-storage.md#quotas)の制約を受けます。 クォータはセキュリティ ポリシーによって定義され、管理者が構成できます。 データの書き込み時に許容される最大サイズを超えると、<xref:System.IO.IsolatedStorage.IsolatedStorageException> 例外がスローされ、操作は失敗します。 データ ストレージを容量不足にしてアプリケーションの要求拒否を引き起こす可能性がある悪意のあるサービス拒否攻撃を、この方法で防ぐことができます。

この理由で特定の書き込み試行が失敗する可能性があるかどうかを判断できるように、<xref:System.IO.IsolatedStorage.IsolatedStorage> クラスには 3 つの読み取り専用のプロパティ <xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorage.UsedSize%2A>、<xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A> が用意されています。 これらのプロパティを使用して、ストアへの書き込みによってストアの最大許容サイズを超えるかどうかを判断できます。 分離ストレージには同時にアクセスできます。そのため、残りのストレージ容量を計算しても、ストアに書き込もうとするまでにストレージ領域が使用される可能性がある点に注意してください。 ただし、ストアの最大サイズを使用すると、使用できるストレージの上限に近づいているかどうかを判断できます。

<xref:System.IO.IsolatedStorage.IsolatedStorage.Quota%2A> プロパティはアセンブリからの証拠に応じて適切に動作します。 この理由から、<xref:System.IO.IsolatedStorage.IsolatedStorageFile>、<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForAssembly%2A>、または <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetUserStoreForDomain%2A> メソッドを使用して作成された <xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetStore%2A> オブジェクトでのみ、このプロパティを取得するようにします。 その他の方法で作成された <xref:System.IO.IsolatedStorage.IsolatedStorageFile> オブジェクト (<xref:System.IO.IsolatedStorage.IsolatedStorageFile.GetEnumerator%2A> メソッドから返されたオブジェクトなど) は正確な最大サイズを返しません。

## <a name="example"></a>例

次のコード例は、分離ストアを取得し、いくつかのファイルを作成し、<xref:System.IO.IsolatedStorage.IsolatedStorage.AvailableFreeSpace%2A> プロパティを取得します。 残りの容量はバイト単位で報告されます。

[!code-cpp[Conceptual.IsolatedStorage#8](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.isolatedstorage/cpp/source7.cpp#8)]
[!code-csharp[Conceptual.IsolatedStorage#8](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.isolatedstorage/cs/source7.cs#8)]
[!code-vb[Conceptual.IsolatedStorage#8](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.isolatedstorage/vb/source7.vb#8)]

## <a name="see-also"></a>参照

- <xref:System.IO.IsolatedStorage.IsolatedStorageFile>
- [分離ストレージ](../../../docs/standard/io/isolated-storage.md)
- [方法: 分離ストレージでストアを取得する](../../../docs/standard/io/how-to-obtain-stores-for-isolated-storage.md)
