---
title: '方法: 結合を使用して LINQ とデータを結合する'
ms.date: 07/20/2015
helpviewer_keywords:
- queries [LINQ in Visual Basic], joins
- joins [LINQ in Visual Basic]
- Join clause [LINQ in Visual Basic]
- Group Join clause [Visual Basic]
- joining [LINQ in Visual Basic]
- queries [LINQ in Visual Basic], how-to topics
ms.assetid: 5b00a478-035b-41c6-8918-be1a97728396
ms.openlocfilehash: 7279908c5d262b65f4c4da9cd9b6c1b4117bc402
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345002"
---
# <a name="how-to-combine-data-with-linq-by-using-joins-visual-basic"></a>方法 : LINQ の結合を使用してデータを結合する (Visual Basic)
Visual Basic には、コレクション間の共通値に基づいて複数のコレクションの内容を結合できるようにするための `Join` および `Group Join` クエリ句が用意されています。 これらの値は*キー*値として知られています。 リレーショナルデータベースの概念を理解している開発者は、`Join` 句を内部結合として認識し、`Group Join` 句を、実質的に左外部結合として認識します。  
  
 このトピックの例では、`Join` と `Group Join` のクエリ句を使用してデータを結合するいくつかの方法を示します。  
  
## <a name="create-a-project-and-add-sample-data"></a>プロジェクトを作成してサンプルデータを追加する  
  
#### <a name="to-create-a-project-that-contains-sample-data-and-types"></a>サンプルデータとデータ型を含むプロジェクトを作成するには  
  
1. このトピックのサンプルを実行するには、Visual Studio を開き、新しい Visual Basic コンソールアプリケーションプロジェクトを追加します。 Visual Basic によって作成された module1.vb ファイルをダブルクリックします。  
  
2. このトピックのサンプルでは、次のコード例の `Person` と `Pet` 型とデータを使用します。 Visual Basic によって作成された既定の `Module1` モジュールにこのコードをコピーします。  
  
     [!code-vb[VbLINQHowTos#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#1)]  
    [!code-vb[VbLINQHowTos#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#2)]  
  
## <a name="perform-an-inner-join-by-using-the-join-clause"></a>Join 句を使用して内部結合を実行する  
 内部結合では、2つのコレクションのデータを結合します。 指定されたキー値が一致する項目が含まれます。 いずれかのコレクションの項目のうち、他のコレクションに一致する項目がない項目は除外されます。  
  
 Visual Basic では、LINQ で内部結合を実行するためのオプションとして、暗黙の結合と明示的な結合の2つのオプションが用意されています。  
  
 暗黙の結合では、`From` 句で結合するコレクションを指定し、`Where` 句で一致するキーフィールドを識別します。 Visual Basic は、指定されたキーフィールドに基づいて、2つのコレクションを暗黙的に結合します。  
  
 結合で使用するキーフィールドを特定する場合は、`Join` 句を使用して明示的な結合を指定できます。 この場合でも、`Where` 句を使用してクエリ結果をフィルター処理できます。  
  
#### <a name="to-perform-an-inner-join-by-using-the-join-clause"></a>Join 句を使用して内部結合を実行するには  
  
1. 次のコードをプロジェクトの `Module1` モジュールに追加して、暗黙的および明示的な内部結合の例を確認します。  
  
     [!code-vb[VbLINQHowTos#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#4)]  
  
## <a name="perform-a-left-outer-join-by-using-the-group-join-clause"></a>Group Join 句を使用して左外部結合を実行する  
 左外部結合には、結合の左側のコレクションのすべての項目が含まれ、結合の右側のコレクションの値のみが一致します。 左側のコレクションに一致する項目がない結合の右側のコレクションの項目は、クエリ結果から除外されます。  
  
 `Group Join` 句は、左外部結合を実行します。 通常、左外部結合と呼ばれるものと、`Group Join` 句が返すものの違いは、`Group Join` 句が左側のコレクションの各項目について、結合の右側のコレクションから結果をグループ化することです。 リレーショナルデータベースでは、左外部結合はグループ化されていない結果を返します。この場合、クエリ結果の各項目には、結合内の両方のコレクションの一致する項目が含まれます。 この場合、結合の左側のコレクションの項目は、右側のコレクションの一致する項目ごとに繰り返されます。 次の手順を完了すると、次のような内容が表示されます。  
  
 クエリを拡張し、グループ化されたクエリ結果ごとに1つの項目を返すようにすることで、`Group Join` クエリの結果をグループ化されていない結果として取得できます。 これを実現するには、グループ化されたコレクションの `DefaultIfEmpty` メソッドに対してクエリを実行する必要があります。 これにより、右側のコレクションと一致する結果がない場合でも、結合の左側のコレクションの項目がクエリ結果に引き続き含まれるようになります。 結合の右側のコレクションから一致する値がない場合は、クエリにコードを追加して既定の結果値を指定できます。  
  
#### <a name="to-perform-a-left-outer-join-by-using-the-group-join-clause"></a>Group Join 句を使用して左外部結合を実行するには  
  
1. 次のコードをプロジェクトの `Module1` モジュールに追加して、グループ化された左外部結合とグループ化されていない左外部結合の両方の例を確認します。  
  
     [!code-vb[VbLINQHowTos#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#3)]  
  
## <a name="perform-a-join-by-using-a-composite-key"></a>複合キーを使用して結合を実行する  
 `Join` または `Group Join` 句で `And` キーワードを使用すると、結合されているコレクションの値を照合するときに使用する複数のキーフィールドを識別できます。 `And` キーワードは、指定されたすべてのキーフィールドが結合される項目に一致する必要があることを指定します。  
  
#### <a name="to-perform-a-join-by-using-a-composite-key"></a>複合キーを使用して結合を実行するには  
  
1. 次のコードをプロジェクトの `Module1` モジュールに追加して、複合キーを使用する結合の例を確認します。  
  
     [!code-vb[VbLINQHowTos#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#5)]  
  
## <a name="run-the-code"></a>コードを実行する  
  
#### <a name="to-add-code-to-run-the-examples"></a>例を実行するコードを追加するには  
  
1. プロジェクトの `Module1` モジュールの `Sub Main` を次のコードに置き換えて、このトピックの例を実行します。  
  
     [!code-vb[VbLINQHowTos#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbLINQHowTos/VB/Module1.vb#6)]  
  
2. F5 キーを押して、例を実行します。  
  
## <a name="see-also"></a>関連項目

- [LINQ](../../../../visual-basic/programming-guide/language-features/linq/index.md)
- [Visual Basic における LINQ の概要](../../../../visual-basic/programming-guide/language-features/linq/introduction-to-linq.md)
- [Join 句](../../../../visual-basic/language-reference/queries/join-clause.md)
- [Group Join 句](../../../../visual-basic/language-reference/queries/group-join-clause.md)
- [From 句](../../../../visual-basic/language-reference/queries/from-clause.md)
- [WHERE 句](../../../../visual-basic/language-reference/queries/where-clause.md)
- [クエリ](../../../../visual-basic/language-reference/queries/index.md)
- [LINQ によるデータ変換 (C#)](../../../../csharp/programming-guide/concepts/linq/data-transformations-with-linq.md)
