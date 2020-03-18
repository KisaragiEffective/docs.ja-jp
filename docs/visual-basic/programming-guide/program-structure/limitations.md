---
title: 制限事項
ms.date: 07/20/2015
helpviewer_keywords:
- limits
- limitations [Visual Basic]
- limitations
- limits, Visual Basic code
- Visual Basic code, limitations
ms.assetid: cf1646b7-5d24-48c6-9616-bda8a4849d91
ms.openlocfilehash: f7e19977a011565bb4b1536af5cab195f8a01017
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347366"
---
# <a name="visual-basic-limitations"></a>Visual Basic の制限事項
以前のバージョンの Visual Basic では、変数名の長さ、モジュールで許容される変数の数、モジュールのサイズなど、コードに境界が適用されていました。 Visual Basic .NET では、これらの制限は緩和されているため、コードの記述と配置をより自由に行うことができます。  
  
 物理的な制限は、コンパイル時の考慮事項に比べて、実行時のメモリに大きく依存します。 賢明なプログラミングプラクティスを使用し、大規模なアプリケーションを複数のクラスとモジュールに分割する場合、内部的な Visual Basic 制限が発生する可能性はほとんどありません。  
  
 極端な場合に発生する可能性があるいくつかの制限を次に示します。  
  
- **名前の長さ。** 宣言されたすべてのプログラミング要素の名前には最大文字数があります。 要素名が修飾されている場合、この最大値は修飾文字列全体に適用されます。 「 [Declared Element Names](../../../visual-basic/programming-guide/language-features/declared-elements/declared-element-names.md)」を参照してください。  
  
- **行の長さ。** ソースコードの物理的な行には、最大65535文字があります。 行連結文字を使用すると、論理ソースコード行が長くなる可能性があります。 「[方法: コード内のステートメントを分割して結合する](../../../visual-basic/programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)」を参照してください。  
  
- **配列の次元。** 配列に対して宣言できる次元の最大数があります。 これにより、配列要素を指定するために使用できるインデックスの数が制限されます。 [Visual Basic の配列ディメンションを](../../../visual-basic/programming-guide/language-features/arrays/array-dimensions.md)参照してください。  
  
- **文字列の長さ。** Unicode 文字の最大数は、1つの文字列に格納できます。 「 [String データ型](../../../visual-basic/language-reference/data-types/string-data-type.md)」を参照してください。  
  
- **環境文字列の長さ。** コマンドライン引数として使用される環境文字列には、最大32768文字があります。 これは、すべてのプラットフォームに関する制限です。  
  
## <a name="see-also"></a>参照

- [プログラム構造とコード規則](../../../visual-basic/programming-guide/program-structure/program-structure-and-code-conventions.md)
- [Visual Basic 名前付け規則](../../../visual-basic/programming-guide/program-structure/naming-conventions.md)
