---
title: 関数型プログラミングと手続き型プログラミング (LINQ to XML)
ms.date: 07/20/2015
ms.assetid: ea1015a5-d4c8-4d79-8e1e-ba17a40a4f39
ms.openlocfilehash: 3d1e3cf01b30454d29836f176afcd39cb2b55b73
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74353409"
---
# <a name="functional-vs-procedural-programming-linq-to-xml-visual-basic"></a>関数型プログラミングと手続き型プログラミング (LINQ to XML) (Visual Basic)
XML アプリケーションには、次のようにさまざまな種類があります。  
  
- ソース XML ドキュメントを受け取り、そのドキュメントとは構造の異なる新しい XML ドキュメントを生成するアプリケーション  
  
- ソース XML ドキュメントを受け取り、HTML や CSV テキスト ファイルなどのまったく異なる形式のドキュメントを生成するアプリケーション  
  
- ソース XML ドキュメントを受け取り、データベースにレコードを挿入するアプリケーション  
  
- データベースなどの別のソースからデータを受け取り、そのデータから XML ドキュメントを作成するアプリケーション  
  
 XML アプリケーションには他にも種類がありますが、上記のアプリケーションは XML プログラマが実装する必要がある代表的な機能です。  
  
 上記のすべてのアプリケーションで、開発者は 2 つの対照的な方法を使用できます。  
  
- 宣言型の方法を使用する関数型構築  
  
- プロシージャ コードを使用するメモリ内の XML ツリーの変更  
  
 LINQ to XML は両方の方法をサポートします。  
  
 関数型の方法を使用する場合は、ソース ドキュメントを受け取り、必要な構造を持つ完全に新しいドキュメントが生成される変換を記述します。  
  
 XML ツリーを直接変更する場合は、メモリ内の XML ツリーでノード間を移動し、必要に応じてノードを挿入、削除、変更します。  
  
 いずれの方法でも LINQ to XML を使用できます。 同じクラスを使用し、場合によっては同じメソッドを使用します。 ただし、2 つの方法の構造と目標はかなり異なります。 たとえば、これらの方法のうち、どちらのパフォーマンスが優れているか、また使用するメモリ量はどちらが多い (または少ない) かは、状況によって異なる場合があります。 また、どちらの方法が保守性に優れたコードをより簡単に記述し生成できるかも、状況によって異なります。  
  
 比較の2つの方法を確認するには、「[メモリ内の XML ツリーの変更と関数型構築 LINQ to XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/in-memory-xml-tree-modification-vs-functional-construction.md)」を参照してください。  
  
 関数型変換の記述に関するチュートリアルについては、「 [XML の純粋関数型変換 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/pure-functional-transformations-of-xml.md)」を参照してください。  
  
## <a name="see-also"></a>参照

- [LINQ to XML プログラミングの概要 (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/linq-to-xml-programming-overview.md)
