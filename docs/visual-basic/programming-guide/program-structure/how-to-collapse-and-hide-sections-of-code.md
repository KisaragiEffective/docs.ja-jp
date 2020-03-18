---
title: '方法 : コードのセクションを折りたたんで非表示にする'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, code collapsing
- Visual Basic, code hiding
- Visual Basic code, collapsing and hiding
ms.assetid: b770e8f5-e07d-491a-ab4b-a977980f9ba2
ms.openlocfilehash: e7aacdc3f41199127b00d276b382ec4a5f258da0
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347409"
---
# <a name="how-to-collapse-and-hide-sections-of-code-visual-basic"></a>方法: コードのセクションを折りたたんで非表示にする (Visual Basic)

`#Region` ディレクティブを使用すると、Visual Basic ファイル内のコードのセクションを折りたたんで非表示にすることができます。 `#Region` ディレクティブを使用すると、Visual Studio コードエディターを使用して展開または折りたたむことができるコードブロックを指定できます。 コードを選択的に非表示にする機能により、ファイルの管理が容易になり、読みやすくなります。 詳細については、「[アウトライン](/visualstudio/ide/outlining)」を参照してください。

`#Region` ディレクティブは、`#If...#End If`などのコードブロックのセマンティクスをサポートします。 つまり、1つのブロックで開始し、別のブロックで終了することはできません。start と end は同じブロック内になければなりません。 `#Region` ディレクティブは、関数内ではサポートされていません。

## <a name="to-collapse-and-hide-a-section-of-code"></a>コードのセクションを折りたたんで非表示にするには

次の例に示すように、`#Region` と `#End Region` のステートメントの間にコードのセクションを配置します。

[!code-vb[VbVbalrConditionalComp#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#6)]

`#Region` ブロックは、コードファイル内で複数回使用できます。そのため、ユーザーは、折りたたまれる可能性があるプロシージャとクラスの独自のブロックを定義できます。 `#Region` ブロックは、他の `#Region` ブロック内で入れ子にすることもできます。

> [!NOTE]
> コードを非表示にしても、コンパイルが妨げられることはなく、`#If...#End If` のステートメントには影響しません。

## <a name="see-also"></a>参照

- [条件付きコンパイル](../../../visual-basic/programming-guide/program-structure/conditional-compilation.md)
- [#Region ディレクティブ](../../../visual-basic/language-reference/directives/region-directive.md)
- [#If...Then...#Else ディレクティブ](../../../visual-basic/language-reference/directives/if-then-else-directives.md)
- [アウトライン](/visualstudio/ide/outlining)
