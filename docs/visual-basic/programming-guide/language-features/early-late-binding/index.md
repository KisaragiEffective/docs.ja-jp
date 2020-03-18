---
title: 事前バインディングと遅延バインディング
ms.date: 07/20/2015
helpviewer_keywords:
- early binding [Visual Basic]
- objects [Visual Basic], late-bound
- objects [Visual Basic], early-bound
- objects [Visual Basic], late bound
- early binding [Visual Basic], Visual Basic compiler
- binding [Visual Basic], late and early
- objects [Visual Basic], early bound
- Visual Basic compiler, early and late binding
- late binding [Visual Basic]
- late binding [Visual Basic], Visual Basic compiler
ms.assetid: d6ff7f1e-b94f-4205-ab8d-5cfa91758724
ms.openlocfilehash: bd70d8642c18e9bc2baba8128ec908c88e0477ce
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345181"
---
# <a name="early-and-late-binding-visual-basic"></a>事前バインディングと遅延バインディング (Visual Basic)
Visual Basic コンパイラは、オブジェクトがオブジェクト変数に割り当てられるときに `binding` と呼ばれるプロセスを実行します。 オブジェクトが特定のオブジェクト型として宣言された変数に代入される場合、オブジェクトは*事前バインディング*されます。 事前バインディングされたオブジェクトを使用すると、コンパイラは、アプリケーションを実行する前に、メモリの割り当てとその他の最適化を実行することができます。 たとえば、次のコードは、<xref:System.IO.FileStream> 型の変数を宣言します。  
  
 [!code-vb[VbVbalrOOP#90](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#90)]  
  
 <xref:System.IO.FileStream> は特定のオブジェクト型であるため、`FS` に代入されるインスタンスは事前バインディングされます。  
  
 対照的に、オブジェクトが  *型として宣言された変数に代入される場合、オブジェクトは*遅延バインディング`Object`されます。 この型のオブジェクトは、任意のオブジェクトへの参照を保持できますが、事前バインディングされたオブジェクトが持っている多くの利点を欠いています。 たとえば、次のコードは、`CreateObject` 関数によって返されたオブジェクトを保持するオブジェクト変数を宣言します。  
  
 [!code-vb[VbVbalrOOP#91](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/LateBinding.vb#91)]  
  
## <a name="advantages-of-early-binding"></a>事前バインディングの利点  
 コンパイラが効率の高いアプリケーションを生成するための重要な最適化を行うことができるため、可能であれば、事前バインディングされたオブジェクトを使用する必要があります。 事前バインディングされたオブジェクトは遅延バインディング オブジェクトよりもはるかに高速であり、使用されるオブジェクトの種類を正確に記述することでコードを読みやすくして管理を容易にします。 事前バインディングのもう1つの利点は、自動コード補完やダイナミックヘルプなどの便利な機能を有効にすることです。これは、Visual Studio 統合開発環境 (IDE: integrated development environment) では、使用しているオブジェクトの種類を編集時に正確に判別できるためです。コード. 事前バインディングを使用すると、コンパイラがプログラムのコンパイル時にエラーを報告できるため、ランタイム エラーの数が減少し、重大度が低下します。  
  
> [!NOTE]
> 遅延バインディングは、`Public` として宣言されている型メンバーにアクセスするためにのみ使用できます。 `Friend` または `Protected Friend` として宣言されているメンバーにアクセスすると、ランタイム エラーが発生します。  
  
## <a name="see-also"></a>参照

- <xref:Microsoft.VisualBasic.Interaction.CreateObject%2A>
- [オブジェクトの有効期間 : オブジェクトの作成と破棄](../../../../visual-basic/programming-guide/language-features/objects-and-classes/object-lifetime-how-objects-are-created-and-destroyed.md)
- [Object Data Type](../../../../visual-basic/language-reference/data-types/object-data-type.md)
