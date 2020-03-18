---
title: '方法 : ステートメントへのラベル付け'
ms.date: 07/20/2015
helpviewer_keywords:
- colons (:)
- statements [Visual Basic], labels
- ': separator character'
- Visual Basic code, labeling statements
ms.assetid: 38f1ff43-2054-42cb-963b-1998e60c6ed4
ms.openlocfilehash: be116ac8046c43e89e44c2d9127c6131e4dfaa52
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347379"
---
# <a name="how-to-label-statements-visual-basic"></a>方法: ステートメントへのラベル付け (Visual Basic)

ステートメントブロックは、コロンで区切られたコード行で構成されます。 識別文字列または整数で始まるコード行には、ラベルが*付け*られています。 ステートメントラベルを使用して、コード行をマークし、`On Error Goto`などのステートメントで使用するように指定します。

ラベルは、プログラミング要素を識別する Visual Basic 識別子や、整数リテラルなど、有効な識別子である場合があります。 ラベルは、ソースコードの行の先頭に記述する必要があります。また、同じ行にステートメントが続くかどうかに関係なく、コロンで続ける必要があります。

コンパイラは、行の先頭が既に定義されている識別子と一致するかどうかをチェックすることによって、ラベルを識別します。 そうでない場合、コンパイラはこれがラベルであると見なします。

ラベルには独自の宣言領域があり、他の識別子に干渉することはありません。 ラベルのスコープは、メソッドの本体です。 ラベル宣言は、あいまいな状況で優先されます。

> [!NOTE]
> ラベルは、メソッド内の実行可能なステートメントでのみ使用できます。

## <a name="to-label-a-line-of-code"></a>コード行にラベルを付けるには

ソースコード行の先頭に、識別子、コロン、コロンの順に配置します。

たとえば、次のコード行には、`Jump` と `120`のラベルが付けられています。

[!code-vb[VbVbalrStatements#708](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#708)]

## <a name="see-also"></a>参照

- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
- [宣言された要素の名前](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)
- [プログラム構造とコード規則](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
