---
title: バッチ
ms.date: 12/13/2019
description: 1つのコマンドで SQL ステートメントのバッチを実行する方法について説明します。
ms.openlocfilehash: 0799784471520bb279db6a4b5879ad30945bdebb
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/25/2019
ms.locfileid: "75450310"
---
# <a name="batching"></a>バッチ

SQLite は、SQL ステートメントのバッチ処理を1つのコマンドにネイティブではサポートしていません。 しかし、ネットワークが関係していないので、パフォーマンスを向上させることはできません。 ただし、他の ADO.NET プロバイダーと同じように動作させるために、ステートメントのバッチ処理が便宜的に実装されます。

<xref:System.Data.Common.DbCommand.ExecuteReader%2A?displayProperty=nameWithType>を呼び出すと、ステートメントは、結果を返す最初のステートメントまで実行されます。 <xref:System.Data.Common.DbDataReader.NextResult%2A?displayProperty=nameWithType> を呼び出すと、ステートメントは、結果を返す次のステートメントまたはバッチの末尾に到達するまで実行を続けます。 <xref:System.Data.Common.DbDataReader.Dispose%2A?displayProperty=nameWithType> または <xref:System.Data.Common.DbDataReader.Close%2A> を呼び出すと、`NextResult()`によって使用されていない残りのステートメントが実行されます。 データリーダーを破棄しない場合、ファイナライザーは残りのステートメントを実行しようとしますが、エラーが発生した場合は無視されます。 このため、バッチを使用するときに `DbDataReader` オブジェクトを破棄することが重要です。

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/BatchingSample/Program.cs?name=snippet_Batching)]

## <a name="see-also"></a>関連項目

* [一括挿入](bulk-insert.md)
