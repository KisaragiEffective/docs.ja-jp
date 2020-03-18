---
title: 連続するキーで結果をグループ化する (C# での LINQ)
description: C# で LINQ を使用して、連続するキーで結果をグループ化する方法について説明します。
ms.date: 08/14/2018
ms.assetid: cbda9c08-151b-4c9e-82f7-c3d7f3dac66b
ms.openlocfilehash: b5753c85bb07be4fc84b78a299eece961969ff9d
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659905"
---
# <a name="group-results-by-contiguous-keys"></a>連続するキーで結果をグループ化する

要素をグループ化し、連続するキーのサブシーケンスを表すチャンクにする方法を次の例に示します。 たとえば、次の一連のキーと値のペアがあるとします。

|キー|値|
|---------|-----------|
|A|水|
|A|think|
|A|that|
|B|Linq|
|C|is|
|A|really|
|B|cool|
|B|!|

次のグループがこの順序で作成されます。

1. We, think, that

2. Linq

3. is

4. really

5. cool, !

ソリューションは、結果をストリーミングで返すスレッド セーフな拡張メソッドとして実装されます。 つまり、ソース シーケンス内を移動するときにグループが作成されます。 `group` 演算子や `orderby` 演算子とは異なり、すべてのシーケンスの読み取りが終わる前に、呼び出し元にグループを返し始めることができます。

ソース コードのコメントで説明されているように、ソース シーケンスが反復処理されるときに各グループまたはチャンクのコピーを作成することで、スレッド セーフが実現されます。 ソース シーケンスに連続するアイテムの大きなシーケンスがある場合、共通言語ランタイムにより <xref:System.OutOfMemoryException> がスローされる可能性があります。

## <a name="example"></a>例

拡張メソッドと、それを使用するクライアント コードの両方を次の例に示します。

[!code-csharp[cscsrefContiguousGroups#1](~/samples/snippets/csharp/concepts/linq/how-to-group-results-by-contiguous-keys_1.cs)]

プロジェクトで拡張メソッドを使用するには、`MyExtensions` 静的クラスを新規または既存のソース コード ファイルにコピーし、必要に応じて、配置されている名前空間の `using` ディレクティブを追加します。

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
