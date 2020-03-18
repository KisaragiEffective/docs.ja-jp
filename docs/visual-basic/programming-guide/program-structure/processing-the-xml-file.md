---
title: XML ファイルの処理
ms.date: 07/20/2015
helpviewer_keywords:
- XML comments [Visual Basic], parsing [Visual Basic]
ms.assetid: 78a15cd0-7708-4e79-85d1-c154b7a14a8c
ms.openlocfilehash: 4230fd88b4b60c631135f5b7fb15f4b6272b5351
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74347299"
---
# <a name="processing-the-xml-file-visual-basic"></a>XML ファイルの処理 (Visual Basic)
コンパイラは、ドキュメントを生成するためにタグ付けされたコードのコンストラクトごとに、ID 文字列を生成します。 (コードにタグを付ける方法については、「 [XML コメントタグ](../../../visual-basic/language-reference/xmldoc/index.md)」を参照してください)。ID 文字列は、コンストラクトを一意に識別します。 XML ファイルを処理するプログラムでは、ID 文字列を使用して、対応する .NET Framework メタデータ/リフレクション項目を識別できます。  
  
 XML ファイルは、コードの階層的な表現ではありません。これは、要素ごとに生成された ID を持つ単純なリストです。  
  
 コンパイラは、次の規則に基づいて ID 文字列を生成します。  
  
- 文字列に空白は配置されません。  
  
- ID 文字列の最初の部分は、単一の文字とそれに続くコロンで識別されるメンバーの種類を示します。 次のメンバーの種類が使用されます。  
  
|文字|説明|  
|---|---|  
|N|namespace<br /><br /> ドキュメントのコメントを名前空間に追加することはできませんが、サポートされている場合には CREF 参照を行うことができます。|  
|T|種類: `Class`、`Module`、`Interface`、`Structure`、`Enum`、`Delegate`|  
|F|フィールド: `Dim`|  
|P|プロパティ: `Property` (既定のプロパティを含む)|  
|M|方法: `Sub`、`Function`、`Declare`、`Operator`|  
|E|イベント: `Event`|  
|!|エラー文字列<br /><br /> あとに続く文字列で、エラーの情報を示します。 Visual Basic コンパイラは、解決できないリンクのエラー情報を生成します。|  
  
- `String` の2番目の部分は、名前空間のルートから開始する項目の完全修飾名です。 項目の名前、それを囲む型、および名前空間は、ピリオドで区切られます。 アイテム自体の名前にピリオドが含まれている場合、それらはシャープ記号 (#) で置き換えられます。 名前に番号記号が付いている項目がないことを前提としています。 たとえば、`String` コンストラクターの完全修飾名は `System.String.#ctor`ます。  
  
- プロパティおよびメソッドについては、メソッドに引数がある場合は、引数のリストをかっこで囲み、メソッドに続けて指定します。 引数がない場合は、かっこはありません。 引数はコンマで区切られます。 各引数のエンコーディングは、.NET Framework シグネチャでのエンコード方法に直接従います。  
  
## <a name="example"></a>例  
 次のコードは、クラスとそのメンバーの ID 文字列が生成される方法を示しています。  
  
 [!code-vb[VbVbcnXmlDocComments#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#10)]  
  
## <a name="see-also"></a>参照

- [-doc](../../../visual-basic/reference/command-line-compiler/doc.md)
- [方法: XML ドキュメントを作成する](../../../visual-basic/programming-guide/program-structure/how-to-create-xml-documentation.md)
