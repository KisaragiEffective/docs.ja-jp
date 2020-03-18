---
title: 概念と用語 (関数型変換)
ms.date: 07/20/2015
ms.assetid: 24fd244d-ebae-4721-8858-89bb544aea0b
ms.openlocfilehash: efc1fc5bb738e3d5d9d3fa2a8226c37da69c045c
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345707"
---
# <a name="concepts-and-terminology-functional-transformation-visual-basic"></a>概念と用語 (関数型変換) (Visual Basic)
このトピックでは、純粋関数型変換の概念と用語について説明します。 データの変換に対して関数型変換の方法を使用すると、多くの場合、従来の命令型のプログラミングよりすばやいプログラミングが可能になります。また、さまざまな表現を使用した、デバッグや保守の容易なコードが生成されます。

このセクションのトピックでは、関数型プログラミングのすべてを説明するのではなく、 XML の変換を容易にするいくつかの機能を紹介することを目的としています。

## <a name="what-is-pure-functional-transformation"></a>純粋関数型変換とは

"*純粋関数型変換*" では、"*純粋関数*" と呼ばれる一連の関数により、一連の構造化データを元の形式から別の形式に変換する方法が定義されます。 "純粋" という語は、それらの関数が "*コンポーザブル*" であることを示しています。関数がコンポーザブルであるためには、次の特性を備えている必要があります。

- "*自己完結している*"。このため、プログラムの他の部分との結び付きや相互依存を気にせずに、関数を自由に並べ替えることができます。 純粋変換では、その環境を考慮する必要はなく、また環境に対して影響を与えることもありません。 つまり、変換で使用される関数には "*副作用*" がありません。

- "*ステートレス*"。このため、同じ関数または関数のセットを同じ入力に対して実行すると、常に同じ出力が得られます。 純粋変換には、以前に使用されたときの情報は保持されません。

> [!IMPORTANT]
> このチュートリアルの以降では、"純粋関数" という用語を、特定の言語機能ではなくプログラミング方法を指す広い意味で使用します。
>
> 純粋関数は Visual Basic の関数として実装する必要があることに注意してください。
>
> また、純粋関数を C++ の純粋仮想メソッドと混同しないようにしてください。 純粋仮想メソッドとは、そのメソッドを含むクラスが抽象クラスであり、メソッドの本体が提供されないことを指します。

### <a name="functional-programming"></a>関数型プログラミング

"*関数型プログラミング*" とは、純粋関数型変換を直接サポートするプログラミング方法です。

これまで、ML、Scheme、Haskell、F# などの汎用関数型プログラミング言語に対する関心は、主に学術的な分野に限られていました。 Visual Basic では常に純粋関数型変換を作成できましたが、その難しさはほとんどのプログラマにとって魅力的な選択肢にはなりませんでした。 ただし、新しいバージョンの Visual Basic では、ラムダ式や型推論などの新しい言語構成要素によって、関数型プログラミングがはるかに簡単で生産性の高いものになります。

関数型プログラミングの詳細については、「[関数型プログラミングと命令型プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/functional-programming-vs-imperative-programming.md)」を参照してください。

#### <a name="domain-specific-fp-languages"></a>特定領域の FP 言語

広く採用されるに至らなかった汎用関数型プログラミング言語に比べると、特定領域の関数型プログラミング言語は成功していると言えます。 たとえば、カスケード スタイル シート (CSS) は多くの Web ページのルック アンド フィールの決定に利用されていますし、Extensible Stylesheet Language Transformations (XSLT) スタイル シートは XML データ操作に広く利用されています。 XSLT について詳しくは、「[XSLT 変換](../../../../standard/data/xml/xslt-transformations.md)」をご覧ください。

## <a name="terminology"></a>用語

次の表に、関数型変換に関連するいくつかの用語の定義を示します。

高階 (ファーストクラス) 関数 \
プログラム オブジェクトとして扱うことのできる関数です。 たとえば、他の関数に渡したり、他の関数から返したりすることができます。 Visual Basic では、デリゲートとラムダ式は、高階関数をサポートする言語機能です。 高階関数を記述するには、デリゲートを受け取る引数を 1 つ以上宣言し、通常はラムダ式を使用して呼び出します。 標準クエリ演算子の多くは高階関数です。

詳細については、「[標準クエリ演算子の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/standard-query-operators-overview.md)」を参照してください。

ラムダ式 \
基本的には、デリゲート型が必要とされる場所で使用できるインラインの匿名関数です。 これはラムダ式の簡略化した定義ですが、このチュートリアルの目的には十分です。

詳細については、「[ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」を参照してください。

コレクション \
データの構造化されたセットです。コレクション内のデータは同じ型であるのが一般的です。 コレクションで LINQ との互換性を確保するには、<xref:System.Collections.IEnumerable> インターフェイスか <xref:System.Linq.IQueryable> インターフェイス (または対応するジェネリック インターフェイスである <xref:System.Collections.Generic.IEnumerator%601> か <xref:System.Linq.IQueryable%601>) を実装する必要があります。

タプル (匿名型) \
タプルは数学的概念で、それぞれが特定の型を持つオブジェクトの有限のシーケンスを意味します。 順序付きリストとも呼ばれます。 匿名型は、この概念の言語実装です。匿名型を使用すると、名前のないクラス型を宣言し、同時にその型のオブジェクトをインスタンス化することができます。

詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。

型推論 (暗黙の型指定) \
明示的な型宣言がない場合に変数の型を特定するコンパイラの機能です。

詳細については、「[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。

遅延実行とレイジー評価 \
解決された値が実際に必要となるまで式の評価を遅らせることを意味します。 遅延実行はコレクションでサポートされています。

詳細については、「 [LINQ to XML (Visual Basic)」](../../../../visual-basic/programming-guide/concepts/linq/deferred-execution-and-lazy-evaluation-in-linq-to-xml.md)の「[基本的なクエリ操作 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md) 」および「遅延実行とレイジー評価」を参照してください。

これらの言語機能は、このセクション全体にわたってサンプル コードで使用されています。

## <a name="see-also"></a>参照

- [純粋関数型変換の概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/introduction-to-pure-functional-transformations.md)
- [関数型プログラミングと命令型プログラミング (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/functional-programming-vs-imperative-programming.md)
