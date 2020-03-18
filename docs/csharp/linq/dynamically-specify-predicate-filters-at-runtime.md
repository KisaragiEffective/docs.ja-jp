---
title: 実行時における述語フィルターの動的指定 (C# での LINQ)
description: C# で LINQ を使用して、実行時に述語フィルターを動的に指定する方法について説明します。
ms.date: 12/01/2016
ms.assetid: 90238470-0767-497c-916c-52d0d16845e0
ms.openlocfilehash: 314be8f98b9ff014f14bef11a1f3581eff8574b4
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "61659944"
---
# <a name="dynamically-specify-predicate-filters-at-runtime"></a>実行時における述語フィルターの動的指定

`where` 句のソース要素に適用しなければならない述語の数が実行時までわからない場合があります。 複数の述語フィルターを動的に指定する方法として、次の例のように、<xref:System.Linq.Enumerable.Contains%2A> メソッドを使用する方法があります。 この例は 2 段階構築になっています。 最初に、プログラムで提供される値にフィルターを適用してプログラムを実行します。 次に、実行時に提供された入力を利用してプログラムをもう一度実行します。

## <a name="to-filter-by-using-the-contains-method"></a>Contains メソッドを使用してフィルター処理するには

1. 新しいコンソール アプリケーションを開き、それに `PredicateFilters` という名前を付けます。

2. 「`StudentClass`オブジェクトのコレクションを照会する[」から ](query-a-collection-of-objects.md) クラスをコピーし、クラス `PredicateFilters` の下の名前空間 `Program` に貼り付けます。 `StudentClass` は、`Student` オブジェクトの一覧を提供します。

3. `Main` で `StudentClass` メソッドをコメント アウトします。

4. クラス `Program` を次のコードで置き換えます。

     [!code-csharp[csProgGuideLINQ#26](~/samples/snippets/csharp/concepts/linq/how-to-dynamically-specify-predicate-filters-at-runtime_1.cs)]

5. 次の行をクラス `Main` の `DynamicPredicates` メソッドに追加します。`ids` の宣言の下です。

     ```csharp
     QueryById(ids);
     ```

6. プロジェクトを実行します。

7. 次の出力がコンソール ウィンドウに表示されます。

     Garcia: 114

     O'Donnell: 112

     Omelchenko: 111

8. 次の手順はプロジェクトをもう一度実行することですが、今度は配列 `ids` の代わりに実行時に提供された入力を使用します。 `QueryByID(ids)` メソッドで `QueryByID(args)` を `Main` に変更します。

9. コマンド ライン引数 `122 117 120 115` でプロジェクトを実行します。 プロジェクトが実行されると、これらの値が `args` メソッドのパラメーター、`Main` の要素になります。

10. 次の出力がコンソール ウィンドウに表示されます。

     Adams: 120

     Feng: 117

     Garcia: 115

     Tucker: 122

## <a name="to-filter-by-using-a-switch-statement"></a>switch ステートメントを使用してフィルター処理するには

1. `switch` ステートメントを使用し、あらかじめ決定されている代替クエリから選択できます。 次の例では、`studentQuery` は、実行時に指定された学年に基づき、別の `where` 句を使用します。

2. 次のメソッドをコピーし、クラス `DynamicPredicates` に貼り付けます。

     [!code-csharp[csProgGuideLINQ#27](~/samples/snippets/csharp/concepts/linq//how-to-dynamically-specify-predicate-filters-at-runtime_2.cs)]

3. `Main` メソッドで、`QueryByID` の呼び出しを次の呼び出しに置換します。この呼び出しは、`args` 配列の最初の要素をその引数として送信します (`QueryByYear(args[0])`)。

4. 1 から 4 の整数をコマンド ライン引数としてプロジェクトを実行します。

## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ)](index.md)
- [where 句](../language-reference/keywords/where-clause.md)
