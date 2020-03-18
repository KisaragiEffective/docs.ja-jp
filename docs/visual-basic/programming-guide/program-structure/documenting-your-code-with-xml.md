---
title: XML の使用によるコードのドキュメントの作成
ms.date: 07/20/2015
helpviewer_keywords:
- XML [Visual Basic], documenting code
- XML comments, Visual Basic
- Visual Basic code, documenting with XML
ms.assetid: a0d35dc7-c5f9-4d74-92ff-a1c6f28d5235
ms.openlocfilehash: bdf0da7a8acc919e4a1d66b81e30c9ed912dd321
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347441"
---
# <a name="documenting-your-code-with-xml-visual-basic"></a>XML の使用によるコードのドキュメントの作成 (Visual Basic)

Visual Basic では、XML を使用してコードをドキュメント化できます。

## <a name="xml-documentation-comments"></a>XML ドキュメントのコメント

Visual Basic は、プロジェクトの XML ドキュメントを自動的に作成する簡単な方法を提供します。 型とメンバーの XML スケルトンを自動的に生成し、各パラメーターの概要、説明ドキュメント、およびその他の解説を提供できます。 適切な設定を使用すると、XML ドキュメントはプロジェクトと同じ名前の xml ファイルに自動的に出力され、.xml 拡張子が付けられます。 詳細については、「[-doc](../../../visual-basic/reference/command-line-compiler/doc.md)」を参照してください。

XML ファイルは、XML として使用することも、それ以外の方法で操作することもできます。 このファイルは、プロジェクトの出力 .exe ファイルまたは .dll ファイルと同じディレクトリにあります。

XML ドキュメントは `'''`から始まります。 これらのコメントの処理にはいくつか制限があります。

- ドキュメントは整形式の XML である必要があります。 XML が整形式でない場合は、警告が生成され、ドキュメントファイルにはエラーが発生したことを示すコメントが含まれます。

- 開発者は、独自のタグ セットを自由に作成できます。 推奨される一連のタグがあります (このトピックの「関連セクション」を参照してください)。 推奨されるタグの一部には特別な意味があります。

  - \<param> タグは、パラメーターの記述に使われます。 このタグがあると、コンパイラは、パラメーターが存在すること、およびすべてのパラメーターがドキュメントで記述されていることを確認します。 検証が失敗した場合、コンパイラは警告を発行します。

  - `cref` 属性は任意のタグにアタッチでき、コード要素への参照を提供します。 コンパイラは、このコード要素が存在することを確認します。 検証が失敗した場合、コンパイラは警告を発行します。 また、コンパイラは、`cref` 属性で記述されている型を検索するときに、`Imports` のすべてのステートメントを尊重します。

  - \<summary > タグは、型またはメンバーに関する追加情報を表示するために Visual Studio の IntelliSense によって使用されます。

## <a name="related-sections"></a>関連項目

ドキュメントコメントを含む XML ファイルの作成の詳細については、次のトピックを参照してください。

- [-doc](../../../visual-basic/reference/command-line-compiler/doc.md)

- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)

- [XML ファイルの処理](../../../visual-basic/programming-guide/program-structure/processing-the-xml-file.md)

- [方法: XML ドキュメントを作成する](../../../visual-basic/programming-guide/program-structure/how-to-create-xml-documentation.md)

- [Visual Studio の XML ツール](/visualstudio/xml-tools/xml-tools-in-visual-studio)

## <a name="see-also"></a>関連項目

- [Visual Basic でのアプリケーションの開発](../../../visual-basic/developing-apps/index.md)
- [Visual Basic のプログラミング ガイド](../../../visual-basic/programming-guide/index.md)
