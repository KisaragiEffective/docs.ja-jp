---
title: 表記規則とコード規則
ms.date: 07/20/2015
helpviewer_keywords:
- coding conventions [Visual Basic], Visual Basic
- best practices [Visual Basic], coding conventions
- conventions [Visual Basic], Visual Basic coding
- typographic conventions [Visual Basic]
- document conventions [Visual Basic]
- conventions [Visual Basic], documentation
- Visual Basic code, conventions
ms.assetid: 1916cd81-ea9d-4faa-81f7-4a0d864b60f4
ms.openlocfilehash: 4906c5ebadb7ce77f2d0e53b2fc5dbab69c5b41f
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74352711"
---
# <a name="typographic-and-code-conventions-visual-basic"></a>表記規則とコード規則 (Visual Basic)

Visual Basic のドキュメントでは、次の表記規則とコード規則に従って記述されています。  
  
## <a name="typographic-conventions"></a>表記規則  
  
|例|説明|  
|-------------|-----------------|  
|`Sub`, `If`, `ChDir`, `Print`, `True`, `Debug`|Visual Basic 特有のキーワードやランタイム メンバは、太字で先頭を大文字にして表記します。|  
|**SmallProject**、 **buttoncollection**|入力する語句は、次の例に示すように書式設定されます。|  
|[Module ステートメント](../../visual-basic/language-reference/statements/module-statement.md)|別のヘルプページにアクセスするためにクリックできるリンクは、この例に示すように書式設定されています。|  
|*object*、 *variableName*、`argumentList`|指定する情報のプレースホルダーは、次の例に示すように書式設定されます。|  
|[影]、[*式の一覧*]|構文では、省略可能な項目が角かっこで囲まれています。|  
|{`Public` &#124; `Friend` &#124; `Private`}|構文では、2つ以上の項目のいずれかを選択する必要がある場合、項目は中かっこで囲まれ、縦棒で区切られます。<br /><br /> 項目を1つだけ選択する必要があります。|  
|[`Protected` &#124; `Friend`]|構文では、2つ以上の項目を選択するオプションがある場合、項目は角かっこで囲まれ、縦棒で区切られます。<br /><br /> 項目の任意の組み合わせを選択することも、項目を使用しないこともできます。|  
|[{`ByVal` &#124; `ByRef`}]|構文では、複数の項目を選択できますが、項目を完全に省略することもできます。これらの項目は、中かっこで囲まれ、縦棒で区切られて角かっこで囲まれます。|  
|*membername*1、 *Membername*2、 *membername*3|同じプレースホルダーの複数のインスタンスは、例に示すように、添字によって区別されます。|  
|*memberName1*<br /><br /> ...<br /><br /> *memberNameN*|構文では、省略記号 (...) を使用して、省略記号の直前にある種類の項目の不定数を示します。<br /><br /> コードでは、省略記号はわかりやすくするために省略されていることを示します。|  
|ESC, ENTER|キーボードのキー名とキーシーケンスは、すべて大文字で表示されます。|  
|ALT + F1|プラス記号 (+) は、キーの組み合わせを表します。 たとえば、Alt + F1 は Alt キーを押しながら、F1 キーを押すことを表します。|  
  
## <a name="code-conventions"></a>コード規則  
  
|例|説明|  
|-------------|-----------------|  
|`sampleString = "Hello, world!"`|コードサンプルは固定ピッチフォントで表示され、次の例に示すように書式設定されます。|  
|前のステートメントにより、`sampleString` の値が "Hello, world!" に設定されます。|説明のテキスト内のコード要素は、この例で示すように固定ピッチ フォントで表示されます。|  
|`' This is a comment.`<br /><br /> `REM This is also a comment.`|コードコメントは、アポストロフィ (') または REM キーワードによって導入されます。|  
|`sampleVar = "This is an " _`<br /><br /> `& "example" _`<br /><br /> `& " of how to continue code."`|行の末尾にスペースとアンダースコア (_) が続く場合は、ステートメントが次の行で続行されることを示します。|  
  
## <a name="see-also"></a>参照

- [Visual Basic の言語リファレンス](../../visual-basic/language-reference/index.md)
- [キーワード](../../visual-basic/language-reference/keywords/index.md)
- [Visual Basic ランタイム ライブラリのメンバー](../../visual-basic/language-reference/runtime-library-members.md)
- [Visual Basic 名前付け規則](../../visual-basic/programming-guide/program-structure/naming-conventions.md)
- [方法 : コード内でステートメントを分割および連結する](../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
- [コード内のコメント](../../visual-basic/programming-guide/program-structure/comments-in-code.md)
