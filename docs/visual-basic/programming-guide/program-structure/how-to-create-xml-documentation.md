---
title: '方法: XML ドキュメントを作成する'
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments
- XML documentation [Visual Basic], creating
ms.assetid: 27b5b06c-09b9-496a-8245-f9542d846230
ms.openlocfilehash: 41b7ef1f435fd0a4f20c4ca2936e2d91e155f7c5
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347417"
---
# <a name="how-to-create-xml-documentation-in-visual-basic"></a>方法 : Visual Basic で XML ドキュメントを作成する

この例では、XML ドキュメントコメントをコードに追加する方法を示します。

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

## <a name="to-create-xml-documentation-for-a-type-or-member"></a>型またはメンバーの XML ドキュメントを作成するには

1. **コードエディター**で、ドキュメントを作成する型またはメンバーの上の行にカーソルを置きます。

2. 「`'''` (3 つの単一引用符)」と入力します。

    型またはメンバーの XML スケルトンが**コードエディター**に追加されます。

3. 適切なタグの間に説明情報を追加します。

    > [!NOTE]
    > XML ドキュメントブロック内に行を追加する場合は、各行を `'''`で始める必要があります。

4. 新しい XML ドキュメントコメントと共に型またはメンバーを使用するコードを追加します。

    IntelliSense では、型またはメンバーの \<summary > タグのテキストが表示されます。

5. コードをコンパイルして、ドキュメントコメントを含む XML ファイルを生成します。 詳細については、「[-doc](../../../visual-basic/reference/command-line-compiler/doc.md)」を参照してください。

## <a name="see-also"></a>参照

- [XML の使用によるコードのドキュメントの作成](../../../visual-basic/programming-guide/program-structure/documenting-your-code-with-xml.md)
- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)
- [-doc](../../../visual-basic/reference/command-line-compiler/doc.md)
