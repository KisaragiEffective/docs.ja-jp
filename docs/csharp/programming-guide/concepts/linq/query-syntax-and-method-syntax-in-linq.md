---
title: LINQ でのクエリ構文とメソッド構文 (C#)
ms.date: 07/20/2015
helpviewer_keywords:
- LINQ [C#], query syntax vs. method syntax
- queries [LINQ in C#], syntax comparisons
ms.assetid: eedd6dd9-fec2-428c-9581-5b8783810ded
ms.openlocfilehash: 17280daaf98010245bbd019652a2a46d7f66ab59
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2020
ms.locfileid: "75635497"
---
# <a name="query-syntax-and-method-syntax-in-linq-c"></a>LINQ でのクエリ構文とメソッド構文 (C#)
統合言語クエリ (LINQ) の入門的なドキュメントでは、ほとんどのクエリが、LINQ の宣言型クエリ構文を使用して記述されています。 ただし、クエリ構文は、コードのコンパイル時に、.NET 共通言語ランタイム (CLR) 用のメソッド呼び出しに変換する必要があります。 これらのメソッド呼び出しが、標準クエリ演算子 (`Where`、`Select`、`GroupBy`、`Join`、`Max`、`Average` など) を呼び出します。 これらは、クエリ構文ではなくメソッド構文を使用して直接呼び出すことができます。  
  
 クエリ構文とメソッド構文は意味的には同じものですが、多くの人は、クエリ構文のほうがシンプルで読みやすいと感じます。 一部のクエリは、メソッド呼び出しとして表現する必要があります。 たとえば、指定した条件に一致する要素の数を取得するクエリを表すには、メソッド呼び出しを使用する必要があります。 また、ソース シーケンスで最大の値を持つ要素を取得するクエリにも、メソッド呼び出しを使用する必要があります。 <xref:System.Linq> 名前空間の標準クエリ演算子のリファレンス ドキュメントでは、通常、メソッド構文が使用されます。 そのため、LINQ クエリの記述をこれから学習する初心者にとっても、クエリやクエリ式自体の中でメソッド構文をどのように使用すればよいか理解しておくことは有用です。  
  
## <a name="standard-query-operator-extension-methods"></a>標準クエリ演算子の拡張メソッド  
 次の例は、シンプルな*クエリ式*と、*メソッド ベースのクエリ*として記述された、意味的に同等のクエリを示したものです。  
  
 [!code-csharp[csLINQGettingStarted#22](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsLINQGettingStarted/CS/Class1.cs#22)]  
  
 2 つの例からの出力は同じです。 クエリ変数の型は、どちらの形式でも同じです: <xref:System.Collections.Generic.IEnumerable%601>。  
  
 メソッド ベースのクエリを理解するために、より詳しく調べていきましょう。 式の右側を見ると、`where` 句が `numbers` オブジェクトのインスタンス メソッドとして表されていることがわかります。これは、既におわかりのように、`IEnumerable<int>` の型を持っています。 ジェネリック型の <xref:System.Collections.Generic.IEnumerable%601> インターフェイスについて知識があれば、これが `Where` メソッドではないことがわかるでしょう。 しかし、Visual Studio IDE で IntelliSense の入力補完リストを呼び出すと、`Where` メソッドだけでなく、`Select`、`SelectMany`、`Join`、`Orderby` など、他にも多くのメソッドが表示されます。 これらはすべて、標準クエリ演算子です。  
  
 ![Intellisense の標準クエリ演算子をすべて示すスクリーンショット。](./media/query-syntax-and-method-syntax-in-linq/standard-query-operators.png)  
  
 一見、<xref:System.Collections.Generic.IEnumerable%601> が再定義され、これらのメソッドが追加されたかのように見えますが、実際にはそうではありません。 標準クエリ演算子は、*拡張メソッド*という新しい種類のメソッドとして実装されています。 拡張メソッドは、既存の型を "拡張" します。これらは、あたかもその型のインスタンス メソッドであるかのように呼び出すことができます。 標準クエリ演算子が <xref:System.Collections.Generic.IEnumerable%601> を拡張しているため、`numbers.Where(...)` を書き込むことができます。  
  
 LINQ の初心者が拡張メソッドについて知っておくべき最も重要なことは、適切な `using` ディレクティブを使用して、アプリケーションのスコープ内にそれらを取り込む方法です。 アプリケーションの観点から見れば、拡張メソッドは通常のインスタンス メソッドと同じものです。  
  
 拡張メソッドについて詳しくは、「[拡張メソッド](../../classes-and-structs/extension-methods.md)」をご覧ください。 標準クエリ演算子について詳しくは、「[標準クエリ演算子の概要 (C#)](./standard-query-operators-overview.md)」をご覧ください。 一部の LINQ プロバイダー ([!INCLUDE[vbtecdlinq](~/includes/vbtecdlinq-md.md)] や [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] など) では、<xref:System.Collections.Generic.IEnumerable%601> 以外の型に対応するため、独自の標準クエリ演算子と追加の拡張メソッドを実装しています。  
  
## <a name="lambda-expressions"></a>ラムダ式  
 上記の例では、条件式 (`num % 2 == 0`) がインライン引数として `Where` メソッドに渡さています: `Where(num => num % 2 == 0).` このインライン式は、ラムダ式と呼ばれます。 これを使用すると、本来であれば、匿名メソッド、ジェネリック デリゲート、式ツリーなど、より複雑な形式で記述しなければならないコードを、簡単に記述できます。 C# では、`=>` がラムダ演算子で、"goes to" という読み方をします。 演算子の左側にある `num` は、クエリ式の `num` に対応する入力変数です。 コンパイラは、`num` がジェネリック `numbers` 型であることがわかっているため、<xref:System.Collections.Generic.IEnumerable%601> の型を推論できます。 ラムダの本体は、クエリ構文や、C# のその他の式やステートメントの式と同じです。これには、メソッド呼び出しやその他の複雑なロジックを含めることができます。 "戻り値" は、式の結果だけです。  
  
 LINQ の初心者の場合、ラムダを広範に使用する必要はありません。 ただし、一部のクエリはメソッド構文でしか表現できず、ラムダ式が必須となるものもあります。 ラムダに慣れてきたら、これが LINQ のツールボックスで使用できる強力で柔軟なツールであることがおわかりいただけるでしょう。 詳しくは、「[ラムダ式](../../statements-expressions-operators/lambda-expressions.md)」をご覧ください。  
  
## <a name="composability-of-queries"></a>クエリの構成可能性  
 上記の例で、`OrderBy` メソッドは `Where` への呼び出しでドット演算子を使用して起動されています。 `Where` は、フィルター処理されたシーケンスを生成し、その後 `Orderby` は、そのシーケンスをソートして操作しています。 クエリが `IEnumerable` を返すので、開発者は、メソッド呼び出しをつないでいきながら、メソッド構文でそれらを編成します。 これが、クエリ構文を使ってクエリを記述する際に、コンパイラがバック グラウンドで行っていることなのです。 また、クエリ変数にはクエリの結果が格納されないので、開発者はそれが実行された後でも、それを随時変更したり、新しいクエリのベースとして使用することができます。  
