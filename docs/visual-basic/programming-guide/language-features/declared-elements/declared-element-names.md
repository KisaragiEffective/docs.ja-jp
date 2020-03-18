---
title: 宣言された要素の名前 (Visual Basic)
ms.date: 07/20/2015
helpviewer_keywords:
- declared elements [Visual Basic], case sensitivity
- names [Visual Basic], Visual Basic rules
- naming conventions [Visual Basic]
- element names [Visual Basic]
- declared elements [Visual Basic], identifiers
- declarations [Visual Basic], elements
- declared elements [Visual Basic], valid names
- '[] escape characters [Visual Basic]'
- names [Visual Basic], elements
- declaration statements [Visual Basic], declared elements
- declaring elements [Visual Basic]
- identifiers [Visual Basic], declared elements
- case sensitivity, declared element names
- escape characters [Visual Basic]
- names [Visual Basic], declared elements
- declared elements [Visual Basic], about declared elements
- escaped names [Visual Basic]
- declared elements [Visual Basic], names
- names [Visual Basic], naming conventions
- identifiers [Visual Basic], elements
ms.assetid: 09d8843b-c0dc-4afe-9dab-87c439a69e66
ms.openlocfilehash: e8620517b934a5f1a97ea25c5a94c8b932bb47b2
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74345427"
---
# <a name="declared-element-names-visual-basic"></a>宣言された要素の名前 (Visual Basic)
宣言されたすべての要素には、*識別子*とも呼ばれる名前が付いています。これは、コードがそれを参照するために使用します。  
  
## <a name="rules"></a>ルール  
 Visual Basic 内の要素名は、次の規則を満たす必要があります。  
  
- 先頭には、アルファベット文字またはアンダースコア (`_`) を使用する必要があります。  
  
- アルファベット、数字、およびアンダースコアのみを含める必要があります。  
  
- アンダースコアで始まる場合は、少なくとも1つのアルファベット文字または10進数が含まれている必要があります。  
  
- 長さは1023文字以下でなければなりません。  
  
 1023文字の長さの制限は、`outerNamespace.middleNamespace.innerNamespace.thisClass.thisElement`など、完全修飾名の文字列全体にも適用されます。  
  
 次の例は、有効な要素名を示しています。  
  
 `aB123__45`  
  
 `_567`  
  
 次の例は、無効な要素名を示しています。 最初のはアンダースコアのみを含み、2番目のは10進数の数字で始まり、3番目の文字には無効な文字 ($) が含まれます。  
  
 `' Three INVALID element names`  
  
 `_`  
  
 `12ABC`  
  
 `xyz$wv`  
  
> [!CAUTION]
> アンダースコア (`_`) で始まる要素名は、[言語に依存しないコンポーネント](../../../../standard/language-independence-and-language-independent-components.md)(cls) の一部ではないため、このような名前を定義するコンポーネントを cls 準拠のコードで使用することはできません。 ただし、要素名の他の位置にあるアンダースコアは CLS に準拠しています。  
  
### <a name="name-length-guidelines"></a>名前の長さのガイドライン  
 実際には、要素の性質を明確に識別しながら、可能な限り短い名前を使用する必要があります。 これにより、コードの読みやすさが向上し、行の長さとソースファイルのサイズが減少します。  
  
 一方、要素が表す内容と、コードでその要素がどのように使用されているかを正確に記述していない場合は、名前を短くするべきではありません。 これは、コードを読みやすくするために重要です。 他のユーザーがそれを理解しようとしている場合、または記述した後に長い時間を見ている場合は、適切な要素名を使用するとかなりの時間を節約できます。  
  
## <a name="escaped-names"></a>エスケープされた名前  
 一般に、要素名は、Visual Basic によって予約されているキーワード (`Case` や `Friend`など) と一致しないようにする必要があります。 ただし、エスケープされた*名前*を定義することもできます。これは、角かっこ (`[ ]`) で囲みます。 エスケープされた名前は、角かっこによってあいまいさが解消されるため、任意の Visual Basic キーワードと一致させることができます。 また、コード内で後で名前を参照するときにも、角かっこを使用します。  
  
 一般に、次の場合にのみ、エスケープされた名前を使用する必要があります。  
  
- コードは、名前として使用されているキーワードを予約していない以前のバージョンの Visual Basic から移行されました。もしくは  
  
- 指定されたキーワードが予約されていない別の言語で記述されたコードを操作しています。  
  
 それ以外の場合は、名前がキーワードと競合する場合は、要素の名前を変更することを検討してください。 統合開発環境 (IDE: integrated development environment) を使用すると、簡単にこの操作を行うことができます。 詳細については、「[リファクタリング](/visualstudio/ide/refactoring-in-visual-studio)」を参照してください。  
  
## <a name="case-sensitivity-in-names"></a>名前の大文字と小文字の区別  
 Visual Basic の要素名は大文字と小文字が区別されます。 つまり、コンパイラは、アルファベットの大文字と小文字が異なる2つの名前を比較するときに、同じ名前として解釈します。 たとえば、 `ABC` と `abc` は、宣言された同じ要素を参照していると見なされます。  
  
 しかし、共通言語ランタイム (CLR) のバインディングでは大文字と小文字が区別されます。 このため、アセンブリまたは DLL を作成し、他のアセンブリで使用できるようにすると、名前の大文字と小文字が区別されるようになります。 たとえば、 `ABC`という名前の要素を持つクラスを定義し、他のアセンブリから共通言語ランタイムを通じてこのクラスを使用する場合は、この要素を `ABC`として参照する必要があります。 後でクラスを再コンパイルし、要素の名前を `abc`に変更した場合、クラスを使用している他のアセンブリは、その要素にアクセスできなくなります。 したがって、アセンブリを更新してリリースするときは、パブリックな要素の名前の大文字と小文字を変更しないでください。  
  
## <a name="names-and-locales"></a>名前とロケール  
 名前の比較は、ロケールに依存しません。 2つの名前が1つのロケールで一致する場合は、すべてのロケールで一致することが保証されます。  
  
## <a name="see-also"></a>参照

- [宣言された要素](../../../../visual-basic/programming-guide/language-features/declared-elements/index.md)
- [宣言された要素の特性](../../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-characteristics.md)
- [宣言された要素の参照](../../../../visual-basic/programming-guide/language-features/declared-elements/references-to-declared-elements.md)
- [ステートメント](../../../../visual-basic/language-reference/statements/index.md)
