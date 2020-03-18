---
title: '方法: コレクション初期化子で使用される拡張メソッドを作成または追加する'
ms.date: 07/20/2015
helpviewer_keywords:
- collection initializers [Visual Basic]
ms.assetid: f64b52c7-8b11-4410-93a6-cb3aeebcc772
ms.openlocfilehash: 6d5f9d38b413b79f111a14ec3829c57a9797ce54
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346719"
---
# <a name="how-to-create-an-add-extension-method-used-by-a-collection-initializer-visual-basic"></a>方法: コレクション初期化子で使用される拡張メソッドを作成または追加する (Visual Basic)
コレクション初期化子を使用してコレクションを作成すると、Visual Basic コンパイラは、`Add` メソッドのパラメーターがコレクション初期化子の値の型と一致するコレクション型の `Add` メソッドを検索します。 この `Add` メソッドを使用して、コレクション初期化子の値をコレクションに設定します。  
  
 一致する `Add` メソッドが存在せず、コレクションのコードを変更できない場合は、コレクション初期化子で必要なパラメーターを受け取る `Add` と呼ばれる拡張メソッドを追加できます。 これは通常、ジェネリックコレクションにコレクション初期化子を使用する場合に必要となります。  
  
## <a name="example"></a>例  
 次の例では、コレクション初期化子を使用して `Employee`型のオブジェクトを追加できるように、ジェネリック <xref:System.Collections.Generic.List%601> 型に拡張メソッドを追加する方法を示します。 拡張メソッドを使用すると、簡略化されたコレクション初期化子の構文を使用できます。  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo1#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo1/VB/Module1.vb#1)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo1#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo1/VB/Module1.vb#2)]  
  
 [!code-vb[VbVbalrCollectionInitializersHowTo1#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrCollectionInitializersHowTo1/VB/Module1.vb#3)]  
  
## <a name="see-also"></a>参照

- [コレクション初期化子](../../../../visual-basic/programming-guide/language-features/collection-initializers/index.md)
- [方法: コレクション初期化子を使用してコレクションを作成する](../../../../visual-basic/programming-guide/language-features/collection-initializers/how-to-create-a-collection-used-by-a-collection-initializer.md)
