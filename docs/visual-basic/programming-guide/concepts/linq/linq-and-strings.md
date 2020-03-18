---
title: LINQ と文字列
ms.date: 07/20/2015
ms.assetid: 75ddb201-d97a-4f98-8cdf-4ad51714529a
ms.openlocfilehash: 73ce4bf5586f1f9ff4995ea6f425b90744b7e333
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353281"
---
# <a name="linq-and-strings-visual-basic"></a>LINQ と文字列 (Visual Basic)
文字列やそのコレクションは、LINQ を使って照会したり変換したりすることができます。 特に、テキスト ファイル内の半構造化されたデータでその利便性が発揮されます。 LINQ クエリは、従来の文字列関数や正規表現と組み合わせることができます。 たとえば、<xref:System.String.Split%2A> または <xref:System.Text.RegularExpressions.Regex.Split%2A> メソッドを使用して、文字列の配列を作成し、その後で LINQ を使用してクエリを実行したり変更したりすることができます。 LINQ クエリの <xref:System.Text.RegularExpressions.Regex.IsMatch%2A> 句で `where` メソッドを使用できます。 LINQ を使用して、正規表現によって返される <xref:System.Text.RegularExpressions.MatchCollection> の結果に対してクエリを実行したり変更したりすることができます。  
  
 このセクションで説明する手法を使えば、半構造化されたテキスト データを XML に変換することもできます。 詳細については、「[方法: CSV ファイルから XML を生成する](how-to-generate-xml-from-csv-files.md)」を参照してください。  
  
 このセクションの例は、次の 2 つのカテゴリに分かれています。  
  
## <a name="querying-a-block-of-text"></a>テキスト ブロックの照会  
 <xref:System.String.Split%2A> メソッドまたは <xref:System.Text.RegularExpressions.Regex.Split%2A> メソッドを使用して、テキスト ブロックをクエリ可能な小さな文字列の配列に分割することによって、テキスト ブロックのクエリ、分析、および変更を実行できます。 単語や文、段落、ページなどの単位にソース テキストを分割できるほか、クエリ内で必要であれば、さらに細かく分割することもできます。  
  
 [方法: 文字列内の単語の出現回数をカウントする (LINQ) (Visual Basic)](how-to-count-occurrences-of-a-word-in-a-string-linq.md)  
 テキストに対する単純なクエリを LINQ で行う方法が紹介されています。  
  
 [方法: 指定された単語のセットを含む文章を照会する (LINQ) (Visual Basic)](how-to-query-for-sentences-that-contain-a-specified-set-of-words.md)

 区切りを指定してテキスト ファイルを分割する方法やその各構成要素に対してクエリを実行する方法が紹介されています。  
  
 [方法: 文字列内の文字を照会する (LINQ) (Visual Basic)](how-to-query-for-characters-in-a-string-linq.md)  
 文字列がクエリ可能型であることを実証します。  
  
 [LINQ クエリと正規表現を組み合わせる方法 (Visual Basic)](how-to-combine-linq-queries-with-regular-expressions.md)  
 LINQ クエリで正規表現を使い、フィルター処理されたクエリ結果に対して複雑なパターン マッチを行う方法が紹介されています。  
  
## <a name="querying-semi-structured-data-in-text-format"></a>半構造化されたテキスト形式データのクエリ  
 テキスト ファイルにはさまざまな種類がありますが、タブ区切りファイルやコンマ区切りファイル、固定長行など同様の形式を持った一連の行で構成されていることは少なくありません。 そのようなテキスト ファイルをメモリに読み込んだ後、LINQ を使って、必要な行を照会したり編集したりすることができます。 複数ソースからのデータを組み合わせる作業も LINQ クエリなら簡単に行うことができます。  
  
 [方法: 2 つのリストの差集合を検索する (LINQ) (Visual Basic)](how-to-find-the-set-difference-between-two-lists-linq.md)  
 ある特定のリストには存在しているものの、それ以外には存在しない文字列をすべて探す方法が紹介されています。  
  
 [方法: 任意の単語またはフィールドを基準にテキストデータの並べ替えまたはフィルター処理を実行する (LINQ) (Visual Basic)](how-to-sort-or-filter-text-data-by-any-word-or-field-linq.md)  
 単語やフィールドに基づいてテキスト行を並べ替える方法が紹介されています。  
  
 [方法: 区切られたファイルのフィールドの順序を変更する (LINQ) (Visual Basic)](how-to-reorder-the-fields-of-a-delimited-file.md)  
 .csv ファイルの行に含まれるフィールドを並べ替える方法が紹介されています。  
  
 [方法: 文字列コレクションを結合および比較する (LINQ) (Visual Basic)](how-to-combine-and-compare-string-collections-linq.md)  
 文字列リストをさまざまな方法で結合する方法が紹介されています。  
  
 [方法: 複数のソースからオブジェクトコレクションにデータを設定する (LINQ) (Visual Basic)](how-to-populate-object-collections-from-multiple-sources-linq.md)  
 複数のテキスト ファイルをデータ ソースとしてオブジェクトのコレクションを作成する方法が紹介されています。  
  
 [方法: 異種ファイルのコンテンツを結合する (LINQ) (Visual Basic)](how-to-join-content-from-dissimilar-files-linq.md)  
 2 つのリストに含まれる文字列を、一致するキーを使って 1 つの文字列に結合する方法が紹介されています。  
  
 [方法: グループを使用してファイルを複数のファイルに分割する (LINQ) (Visual Basic)](how-to-split-a-file-into-many-files-by-using-groups-linq.md)  
 1 つのファイルをデータ ソースとして新しいファイルを作成する方法が紹介されています。  
  
 [方法: CSV テキストファイルの列値を計算する (LINQ) (Visual Basic)](how-to-compute-column-values-in-a-csv-text-file-linq.md)  
 .csv ファイルでテキスト データに対して数学的計算を実行する方法が紹介されています。  
  
## <a name="see-also"></a>参照

- [統合言語クエリ (LINQ) (Visual Basic)](index.md)
- [方法: CSV ファイルから XML を生成する](how-to-generate-xml-from-csv-files.md)
