---
title: <exception>
ms.date: 07/20/2015
helpviewer_keywords:
- <exception> XML tag
- exception XML tag
ms.assetid: c0517549-171e-4dae-ab88-a9c1700b6eee
ms.openlocfilehash: e1e7f2d0fb06599f83ba224ed52a10429d9b11fe
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/22/2019
ms.locfileid: "74346957"
---
# <a name="exception-visual-basic"></a>\<exception> (Visual Basic)
スローできる例外を指定します。  
  
## <a name="syntax"></a>構文  
  
```xml  
<exception cref="member">description</exception>  
```  
  
## <a name="parameters"></a>パラメーター  
 `member`  
 現在のコンパイル環境から使用できる例外の参照。 コンパイラは、指定された例外が存在し、出力の XML で `member` が正規要素名に変換されることを確認します。 `member` は、二重引用符 (" ") で囲む必要があります。  
  
 `description`  
 説明です。  
  
## <a name="remarks"></a>コメント  
 `<exception>` タグを使用して、スローできる例外を指定します。 このタグは、メソッドの定義に適用されます。  
  
 コンパイル時に [-doc](../../../visual-basic/reference/command-line-compiler/doc.md) を指定して、ドキュメント コメントをファイルに出力します。  
  
## <a name="example"></a>例  
 この例では、`<exception>` タグを使用して、`IntDivide` 関数がスローできる例外を記述します。  
  
 [!code-vb[VbVbcnXmlDocComments#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnXmlDocComments/VB/Class1.vb#3)]  
  
## <a name="see-also"></a>関連項目

- [XML のコメント用タグ](../../../visual-basic/language-reference/xmldoc/index.md)
