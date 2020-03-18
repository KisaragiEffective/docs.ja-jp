---
title: クエリ キーワード - C# リファレンス
ms.date: 07/20/2015
helpviewer_keywords:
- query keywords [C#]
- LINQ [C#], query keywords
ms.assetid: 6c9bec16-dbd7-4a7c-a060-fe4600b2021f
ms.openlocfilehash: 01134eda540c5141afbd11b2c89ff779da495a9a
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "79173524"
---
# <a name="query-keywords-c-reference"></a>クエリ キーワード (C# リファレンス)

このセクションでは、クエリ式で使用するコンテキスト キーワードについて説明します。

## <a name="in-this-section"></a>このセクションの内容

|句|[説明]|
|------------|-----------------|
|[from](from-clause.md)|データ ソースおよび範囲変数 (反復変数に類似) を指定します。|
|[where](where-clause.md)|論理 AND 演算子 (`&&`) と論理 OR 演算子 (<code>&#124;&#124;</code>) で区切られた 1 つ以上のブール式に基づき、ソース要素をフィルター処理します。|
|[select](select-clause.md)|クエリ実行で返されるシーケンスに含まれる要素の型と形状を指定します。|
|[グループ](group-clause.md)|指定したキー値に基づき、クエリ結果をグループ化します。|
|[into](into.md)|join 句、group 句、または select 句の結果への参照として機能する識別子を指定します。|
|[orderby](orderby-clause.md)|要素型の既定の比較子に基づき、クエリ結果を昇順または降順で並べ替えます。|
|[join](join-clause.md)|指定した 2 つの一致基準の等価比較に基づき、2 つのデータ ソースを結合します。|
|[let](let-clause.md)|範囲変数を使用して、クエリ式のサブ式の結果を格納します。|
|[in](in.md)|[join](join-clause.md) 句のコンテキスト キーワードです。|
|[on](on.md)|[join](join-clause.md) 句のコンテキスト キーワードです。|
|[equals](equals.md)|[join](join-clause.md) 句のコンテキスト キーワードです。|
|[by](by.md)|[group](group-clause.md) 句のコンテキスト キーワードです。|
|[ascending](ascending.md)|[orderby](orderby-clause.md) 句のコンテキスト キーワードです。|
|[descending](descending.md)|[orderby](orderby-clause.md) 句のコンテキスト キーワードです。|

## <a name="see-also"></a>参照

- [C# のキーワード](index.md)
- [統合言語クエリ (LINQ)](../../programming-guide/concepts/linq/index.md)
- [C# での LINQ](../../linq/index.md)
