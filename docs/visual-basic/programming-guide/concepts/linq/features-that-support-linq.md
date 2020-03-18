---
title: LINQ をサポートする機能
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, LINQ features
- LINQ [Visual Basic], features supporting LINQ
ms.assetid: c821bb50-b6f6-4cf9-8aba-2717e465bd3a
ms.openlocfilehash: 57c0566f9a76715e48b20f2e6493aa1a506c64be
ms.sourcegitcommit: 7bc6887ab658550baa78f1520ea735838249345e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/03/2020
ms.locfileid: "75636862"
---
# <a name="visual-basic-features-that-support-linq"></a>LINQ をサポートする Visual Basic の機能
統合言語クエリ (LINQ) とは、クエリ構文やその他の言語構成要素を言語で直接サポートする Visual Basic のテクノロジを指します。 LINQ では、外部データソースに対してクエリを実行するための新しい言語を習得する必要はありません。 Visual Basic を使用すると、リレーショナルデータベース、XML ストア、またはオブジェクトのデータに対してクエリを実行できます。 この言語へのクエリ機能の統合により、コンパイル時に構文エラーやタイプセーフをチェックできるようになります。 また、この統合により、Visual Basic で豊富な多様なクエリを作成するために必要な知識のほとんどを把握しておくことができます。  
  
 以下のセクションでは、入門ドキュメント、コード例、およびサンプルアプリケーションの概要を理解できるように、LINQ をサポートするための詳細な言語構成要素について説明します。 また、リンクをクリックして、言語機能の詳細な説明を参照して、統合言語クエリを有効にすることもできます。 まず、 [「チュートリアル: Visual Basic でのクエリの作成」](../../../../visual-basic/programming-guide/concepts/linq/walkthrough-writing-queries.md)をお勧めします。  
  
## <a name="query-expressions"></a>クエリ式  
 Visual Basic のクエリ式は、SQL または XQuery と同様の宣言構文で表現できます。 コンパイル時に、クエリ構文は、LINQ プロバイダーによる標準クエリ演算子の拡張メソッドの実装に対するメソッド呼び出しに変換されます。 アプリケーションでは、`Imports` ステートメントを使用して適切な名前空間を指定することによって、スコープ内の標準クエリ演算子を制御します。 Visual Basic のクエリ式の構文は次のようになります。  
  
 [!code-vb[VbLINQVbFeatures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#1)]  
  
 詳細については、「 [Visual Basic での LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)」を参照してください。  
  
## <a name="implicitly-typed-variables"></a>暗黙的に型指定された変数  
 変数を宣言して初期化するときに型を明示的に指定する代わりに、コンパイラが型を推論して割り当てられるようにすることができます。 これは、*ローカル型の推論*と呼ばれます。  
  
 型が推論される変数は、明示的に指定した型を持つ変数と同じように、厳密に型指定されます。 ローカル型の推論は、メソッド本体内でローカル変数を定義する場合にのみ機能します。 詳細については、「[オプション推論ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)と[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。  
  
 ローカル型の推論の例を次に示します。 この例を使用するには、`Option Infer` を `On`に設定する必要があります。  
  
 [!code-vb[VbLINQVbFeatures#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#2)]  
  
 ローカル型の推論では、匿名型を作成することもできます。これについては、このセクションの後半で説明します。 LINQ クエリに必要です。  
  
 次の LINQ の例では、`Option Infer` が `On` または `Off`の場合、型の推定が行われます。 `Option Infer` が `Off`、`Option Strict` が `On`場合、コンパイル時エラーが発生します。  
  
 [!code-vb[VbLINQVbFeatures#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#3)]  
  
## <a name="object-initializers"></a>オブジェクト初期化子  
 オブジェクト初期化子は、クエリの結果を保持する匿名型を作成する必要がある場合に、クエリ式で使用されます。 また、クエリの外部で名前付きの型のオブジェクトを初期化するために使用することもできます。 オブジェクト初期化子を使用すると、コンストラクターを明示的に呼び出すことなく、1行でオブジェクトを初期化できます。 パブリック `Name` と `Phone` プロパティを持つ `Customer` という名前のクラスと、その他のプロパティがあると仮定すると、オブジェクト初期化子を次のように使用できます。  
  
 [!code-vb[VbLINQVbFeatures#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#4)]  
  
 詳細については、「[オブジェクト初期化子: 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)」を参照してください。  
  
## <a name="anonymous-types"></a>匿名型  
 匿名型を使用すると、一連のプロパティを一時的にクエリ結果に含める要素にグループ化することができます。 これにより、クエリ内の使用可能なフィールドの任意の組み合わせを任意の順序で選択できます。要素の名前付きデータ型を定義する必要はありません。  
  
 *匿名型*は、コンパイラによって動的に構築されます。 型の名前はコンパイラによって割り当てられ、新しいコンパイルのたびに変更される可能性があります。 そのため、名前を直接使用することはできません。 匿名型は、次の方法で初期化されます。  
  
 [!code-vb[VbLINQVbFeatures#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#5)]  
  
 詳細については、「[匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)」を参照してください。  
  
## <a name="extension-methods"></a>Extension のメソッド  
 拡張メソッドを使用すると、定義の外部からデータ型またはインターフェイスにメソッドを追加できます。 この機能を使用すると、実際に型を変更することなく、既存の型に新しいメソッドを追加できます。 標準クエリ演算子は、それ自体が、<xref:System.Collections.Generic.IEnumerable%601>を実装する任意の型に対して LINQ クエリ機能を提供する拡張メソッドのセットです。 その他の <xref:System.Collections.Generic.IEnumerable%601> の拡張機能には、<xref:System.Linq.Enumerable.Count%2A>、<xref:System.Linq.Enumerable.Union%2A>、および <xref:System.Linq.Enumerable.Intersect%2A>が含まれます。  
  
 次の拡張メソッドは、print メソッドを <xref:System.String> クラスに追加します。  
  
 [!code-vb[VbLINQVbFeatures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#6)]  
  
 メソッドは、<xref:System.String>の通常のインスタンスメソッドと同様に呼び出されます。  
  
 [!code-vb[VbLINQVbFeatures#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#7)]  
  
 詳細については、「[拡張メソッド](../../../../visual-basic/programming-guide/language-features/procedures/extension-methods.md)」を参照してください。  
  
## <a name="lambda-expressions"></a>ラムダ式  
 ラムダ式は、1つの値を計算して返す名前のない関数です。 名前付き関数とは異なり、ラムダ式は同時に定義および実行できます。 次の例では、4が表示されます。  
  
 [!code-vb[VbLINQVbFeatures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#8)]  
  
 ラムダ式の定義を変数名に割り当ててから、その名前を使用して関数を呼び出すことができます。 次の例では、4も表示されます。  
  
 [!code-vb[VbLINQVbFeatures#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#12)]  
  
 LINQ では、ラムダ式は多くの標準クエリ演算子を基にしています。 コンパイラは、`Where`、`Select`、`Order By`、`Take While`などの基本的なクエリメソッドで定義されている計算をキャプチャするラムダ式を作成します。  
  
 たとえば、次のコードでは、学生のリストからすべての上級学生を返すクエリを定義しています。  
  
 [!code-vb[VbLINQVbFeatures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#9)]  
  
 クエリ定義は、次の例のようなコードにコンパイルされます。この例では、2つのラムダ式を使用して `Where` と `Select`の引数を指定しています。  
  
 [!code-vb[VbLINQVbFeatures#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#10)]  
  
 どちらのバージョンも、`For Each` ループを使用して実行できます。  
  
 [!code-vb[VbLINQVbFeatures#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQVbFeatures/VB/Class1.vb#11)]  
  
 詳しくは、「[ラムダ式](../../../../visual-basic/programming-guide/language-features/procedures/lambda-expressions.md)」をご覧ください。  
  
## <a name="see-also"></a>関連項目

- [統合言語クエリ (LINQ) (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/index.md)
- [Visual Basic の LINQ の概要](../../../../visual-basic/programming-guide/concepts/linq/getting-started-with-linq.md)
- [LINQ と文字列 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-and-strings.md)
- [Option Infer ステートメント](../../../../visual-basic/language-reference/statements/option-infer-statement.md)
- [Option Strict ステートメント](../../../../visual-basic/language-reference/statements/option-strict-statement.md)
