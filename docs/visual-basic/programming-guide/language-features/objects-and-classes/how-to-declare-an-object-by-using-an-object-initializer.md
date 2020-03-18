---
title: '方法 : オブジェクト初期化子を使用してオブジェクトを宣言する'
ms.date: 07/20/2015
helpviewer_keywords:
- declaring objects using object initializer
- object initializers [Visual Basic]
- initializers [Visual Basic]
- Video How tos, Visual Basic
ms.assetid: 0f53a553-efd6-466d-80bf-6b679e5cd174
ms.openlocfilehash: ae04d338b61027c3917ad3a7f62ff40f0a95e53e
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347137"
---
# <a name="how-to-declare-an-object-by-using-an-object-initializer-visual-basic"></a>方法: オブジェクト初期化子を使用してオブジェクトを宣言する (Visual Basic)
オブジェクト初期化子を使用すると、1つのステートメントでクラスのインスタンスを宣言およびインスタンス化できます。 また、パラメーター化されたコンストラクターを呼び出さずに、インスタンスの1つ以上のメンバーを同時に初期化できます。  
  
 オブジェクト初期化子を使用して名前付きの型のインスタンスを作成すると、クラスのパラメーターなしのコンストラクターが呼び出され、指定された順序で指定されたメンバーの初期化が行われます。  
  
 次の手順は、3つの異なる方法で `Student` クラスのインスタンスを作成する方法を示しています。 クラスには、姓、名、およびクラス year プロパティがあります。 3つの各宣言は、`Student`の新しいインスタンスを作成し、プロパティ `First` を "Michael" に、プロパティ `Last` を "Tucker" に設定し、その他のすべてのメンバーを既定値に設定します。 プロシージャ内の各宣言の結果は、オブジェクト初期化子を使用しない次の例と同じです。  
  
 [!code-vb[VbVbalrObjectInit#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#20)]  
  
 `Student` クラスの実装については、「[方法: 項目のリストを作成](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)する」を参照してください。 このトピックのコードをコピーして、クラスを設定し、操作する `Student` オブジェクトの一覧を作成することができます。  
  
### <a name="to-create-an-object-of-a-named-class-by-using-an-object-initializer"></a>オブジェクト初期化子を使用して名前付きクラスのオブジェクトを作成するには  
  
1. コンストラクターを使用する予定の場合と同様に、宣言を開始します。  
  
     `Dim student1 As New Student`  
  
2. キーワード `With`を入力し、その後に中かっこで囲まれた初期化リストを入力します。  
  
     `Dim student1 As New Student With { <initialization list> }`  
  
3. 初期化の一覧に、初期化する各プロパティを含め、初期値を割り当てます。 プロパティの名前の前にピリオドが付きます。  
  
     [!code-vb[VbVbalrObjectInit#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#21)]  
  
     クラスの1つ以上のメンバーを初期化できます。  
  
4. または、クラスの新しいインスタンスを宣言し、そのインスタンスに値を割り当てることもできます。 最初に、`Student`のインスタンスを宣言します。  
  
     `Dim student2 As Student`  
  
5. 通常の方法で `Student` のインスタンスの作成を開始します。  
  
     `Dim student2 As Student = New Student`  
  
6. 「`With`」と入力し、オブジェクト初期化子を入力して、新しいインスタンスの1つ以上のメンバーを初期化します。  
  
     [!code-vb[VbVbalrObjectInit#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#22)]  
  
7. `As Student`を省略すると、前の手順で定義を簡略化できます。 これを行うと、コンパイラは、ローカル型推論を使用して、`student3` が `Student` のインスタンスであると判断します。  
  
     [!code-vb[VbVbalrObjectInit#23](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrObjectInit/VB/Class2.vb#23)]  
  
     詳細については、「[ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [ローカル型の推論](../../../../visual-basic/programming-guide/language-features/variables/local-type-inference.md)
- [方法: 項目のリストを作成する](../../../../visual-basic/programming-guide/concepts/linq/how-to-create-a-list-of-items.md)
- [オブジェクト初期化子 : 名前付きの型と匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [匿名型](../../../../visual-basic/programming-guide/language-features/objects-and-classes/anonymous-types.md)
