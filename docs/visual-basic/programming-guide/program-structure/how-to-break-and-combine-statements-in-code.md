---
title: '方法 : コード内でステートメントを分割および連結する'
ms.date: 07/20/2015
f1_keywords:
- vb._
helpviewer_keywords:
- colons (:)
- line continuation
- _ line-continuation character
- ': line separator character'
- Visual Basic code, line breaks in
- Visual Basic code, line breaks
- Visual Basic code, line continuation
- long lines of code
- line terminator
- line-continuation sequence
- underscores [Visual Basic], in code
- statements [Visual Basic], line continuation in
- line breaks [Visual Basic], in code
- line-continuation character [Visual Basic]
- Visual Basic code, line continuation in
- statements [Visual Basic], line breaks in
ms.assetid: dea01dad-a8ac-484a-bb3a-8c45a1b1eccc
ms.openlocfilehash: f1a24c001cd20acc7663fb4cbe60e7e35a9c8fc3
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347434"
---
# <a name="how-to-break-and-combine-statements-in-code-visual-basic"></a>方法: コード内でステートメントを分割および連結する (Visual Basic)

コードを記述するときに、コードエディターで水平スクロールを必要とする長いステートメントを作成する場合があります。 これはコードの実行方法には影響しませんが、モニターに表示されるコードをユーザーまたは他のユーザーが読み取ることが困難になります。 このような場合は、1つの long ステートメントを複数の行に分割することを検討してください。

## <a name="to-break-a-single-statement-into-multiple-lines"></a>1つのステートメントを複数の行に分割するには

行連結文字を使用します。これは、改行する位置で、アンダースコア (`_`) となります。 アンダースコアは、直後にスペースを付け、その直後に行終端記号 (キャリッジリターン) を付けるか、または (バージョン16.0 以降) コメントの後に復帰を続けます。

  > [!NOTE]
  > 場合によっては、行連結文字を省略すると、Visual Basic コンパイラは、次のコード行でステートメントを暗黙的に続行します。 行連結文字を省略できる構文要素の一覧については、「[ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)」の「暗黙的な行の連結」を参照してください。

  次の例では、ステートメントは、行連結文字が最後の行以外のすべてを終了する4行に分割されます。

  [!code-vb[VbVbcnConventions#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#20)]

  このシーケンスを使用すると、コードがオンラインでも印刷時でも読みやすくなります。

  行連結文字は、行の最後の文字である必要があります。 同じ行の他の何にも従うことはできません。

  行連結文字を使用できる場所については、いくつかの制限があります。たとえば、引数名の途中で使用することはできません。 行連結文字を使用して引数リストを分割することはできますが、引数の個々の名前はそのままにしておく必要があります。

  行連結文字を使用してコメントを続行することはできません。 コンパイラは、コメント内の文字が特別な意味を持つかどうかを確認しません。 複数行のコメントの場合は、各行にコメント記号 (`'`) を繰り返します。

 各ステートメントを別々の行に配置することをお勧めしますが、Visual Basic 複数のステートメントを同じ行に配置することもできます。

## <a name="to-place-multiple-statements-on-the-same-line"></a>複数のステートメントを同じ行に配置するには

次の例のように、ステートメントをコロン (`:`) で区切ります。

  [!code-vb[VbVbcnConventions#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnConventions/VB/Class1.vb#10)]

## <a name="see-also"></a>参照

- [プログラム構造とコード規則](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
- [ステートメント](../../../visual-basic/programming-guide/language-features/statements.md)
